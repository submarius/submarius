---
title: Water-clarity model — methodology
description: How Submarius computes underwater visibility from satellite ocean colour, weather history, and physical penalties.
---

# Water-clarity model

This page describes the model that produces every visibility number
Submarius shows. The companion feature page is
[features/water-clarity.md](../features/water-clarity.md).

## Architecture in three layers

```
   Layer A — Day-1 physical model (always on)
       ↓
   Layer B — Crowd-sourced reports (calibrate + override)
       ↓
   Layer C — Per-region statistical refinement (as data accumulates)
```

The product **ships good on day one** — Layer A is engineered to be the
best-possible baseline using established oceanography, before any user
has submitted a single report. Layers B and C make it sharper over time
without ever discarding the physical grounding.

## Layer A — the physical model

The model takes a query `(lat, lon, time)` and aggregates a set of
physically-grounded sub-models, each returning a `(value, weight,
reason)` triple. The final estimate is a weighted average; each
contribution's pull is visible in the API response.

### Inputs

| Category | Signals |
|---|---|
| Atmospheric | Wind speed and direction (now / 24h max / 72h max), gust, wind direction 24h average, precipitation 24/48/72/168 h cumulative, air temperature |
| Marine | Wave height + period + direction, swell height + period + direction, wind-wave height, sea-surface temperature, sea level + 24 h mean (for tide range) |
| Tidal | Stage (rising / falling / slack), 24 h range, hours since last high/low, spring/neap state |
| Lunar | Phase, illumination |
| Hydrological (US) | Nearest river discharge, distance, turbidity, distance-weighted inflow index, 72 h discharge change |
| Satellite | Kd490 from CoastWatch (VIIRS, multi-mission DINEOF, GOES-16 ABI fused), chlorophyll-a, age in hours, cloud flag |
| HAB | Active flag, severity (0–3), distance to bloom centre |
| Static / contextual | Distance to coast, bottom depth (GEBCO), latitude band, month, hour of day local, enclosed-water classification (open / semi-open / enclosed, derived from coastline geometry — *not* place names) |

### Sub-models and weights

Each component below is weighted; the relative weights are tuned against
historical data. We don't publish the exact coefficients (they're
ongoing model parameters that get retuned), but the structure is open.

**Satellite Secchi from Kd490.** Apply the Lee 2015 mechanistic model
(see [kd490-explained.md](kd490-explained.md)) with an adaptive Case-1
vs Case-2 coefficient. Confidence decays with satellite age and is
zeroed when the cloud flag is set.

**Regional baseline.** A latitude-band × distance-from-coast lookup
populated from published coastal-water clarity ranges. Always present
as a low-weight prior — when satellite is fresh, it's overwhelmed by
the satellite signal; when satellite is stale, it carries more weight.

**Recent precipitation penalty.** Plumes scale with discharge and
distance, with a 1–2 day lag (Tagus, Yukon, Ebro studies — see
[water-clarity-research](../methodology/data-sources.md)). The penalty
is heavier nearshore (within ~5 km of coast) and tapers off-shore.

**Wind-history resuspension.** Based on Green & Coco (2014):
turbidity in shallow water scales with W²/H (wind speed squared over
water depth). Critical threshold around 19 knots over typical bottoms;
72 h wind history matters because suspended sediment persists for
24–72 h after wind subsides.

**Tide-stage modifier.** Spot-type aware. In *enclosed* waters
(bays, sounds, inlets), falling tide flushes sediment-laden water out
(viz penalty) and rising tide pulls cleaner offshore water in (viz
boost). In *open coast*, tide stage matters mostly only at slack. In
*semi-open*, the effect is muted.

**HAB bulletin override.** When NOAA publishes an active harmful-algal-
bloom warning within 50 km, severity-weighted penalty applies and a
user-visible warning is surfaced regardless of the rest of the score.

**River-plume penalty (US).** For every USGS river within 50 km, the
plume contribution is a function of discharge magnitude, distance
(Gaussian decay, ~10 km characteristic length), and recent flood
signal (72 h discharge change).

**Algal-bloom climatology.** Where chlorophyll satellite data is
unavailable, a seasonal climatology applies — temperate spring blooms,
California upwelling-season (Feb–Jul) penalties.

### Composition

```
estimate_m  = Σ(value_i × weight_i) / Σ(weight_i)
disagreement = stdev(value_i) over components with weight > 0.05
n_signals    = count of components with weight > 0.05
coverage     = 1 + 2/n_signals       # fewer signals → wider band

p50 = estimate_m
p10 = estimate_m × 0.7  − disagreement × coverage
p90 = estimate_m × 1.3  + disagreement × coverage
```

The width of the uncertainty band reflects (a) how much the components
disagree among themselves and (b) how many components contributed at
all. A confident estimate (satellite fresh, reports nearby, weather
inputs available) has a tight band. A guess (satellite stale, no
reports, sparse weather) has a wide one — and the app shows both.

### Multi-satellite fusion

Three independent satellite ocean-colour sources contribute to the
Kd490 value used by the Lee equation:

| Source | Resolution | Cadence | Latency |
|---|---|---|---|
| VIIRS-SNPP (NOAA CoastWatch L3) | 750 m / 4 km | Daily | 1–3 days |
| VIIRS+OLCI multi-mission DINEOF | 2.32 km | Daily, gap-filled | 10–12 days |
| GOES-16 ABI L1b → ACOLITE | 2 km | Hourly daylight | <30 s from publish |

Fusion uses **inverse-variance weighting in log-Kd space** — the
maximum-likelihood combination of independent Gaussian observations
with known variances. The math is associative, so we cache the
slow-changing ERDDAP fusion separately from the fast-moving GOES
sample and re-fuse at query time.

The geostationary GOES contribution is the rare ingredient: most
single-sensor visibility products quote a number that's at minimum a
day stale. GOES gives sub-daily freshness during cloud-free daytime
hours, when ocean conditions can change fastest.

### Bathymetric cap

The model caps `p50 ≤ bottom_depth × 1.5` when the seafloor is
shallower than the 122 m diver-relevance horizon. Beyond that depth,
the cap is removed — pelagic queries (Bahamas deep, open Atlantic) can
return the genuine 60–100 ft Gulf Stream visibility.

### Adjacency / shore-bias correction

Satellite ocean-colour retrievals within ~3 km of land suffer
adjacency effects (land reflectance bleeds into the water-pixel signal,
inflating apparent clarity) and bottom contamination (in shallow
water, bottom reflectance distorts the colour). Both biases push the
signal toward "clearer than reality".

We downweight the satellite contribution at shore-adjacent points
**only when Kd suggests clear water**. A satellite reading of high Kd
near shore is honest about the local turbidity (turbidity dominates
the bias). A reading of low Kd near shore is suspect (bias plus
clear-water signal together), so we trust it less and lean more on the
weather-driven and report-driven components.

## Layer B — crowd-sourced reports

Every dive trip in Submarius can end with a one-tap viz rating.
Reports are H3-quantised before they leave the device — Submarius
itself never receives precise dive-spot coordinates.

Reports feed the model two ways:

1. **Direct pinning.** A report less than 24 h old within ~5 km of a
   query weights into the local estimate. The blend asymptotes at
   ~85 % weight on reports when 5+ recent nearby reports exist; with
   fewer, the physical model dominates.
2. **Long-running calibration.** Accumulated reports recalibrate
   regional coefficients over weeks-to-months horizons.

## Layer C — statistical refinement

In regions with sufficient report density, a quantile-regression
gradient-boosted model trained on `(feature_vector, observed_viz)`
pairs replaces or augments Layer A. The Lee equation and the physical
penalties remain present — the statistical layer just refines the
weighting.

## Forecast horizon

The same model runs forward in time, fed by forecasted feature vectors
(Open-Meteo Marine 7-day forecast for wind/wave/precipitation,
deterministic tides and lunar, persistence with mean-reversion for
satellite Kd, USGS short-term forecasts where available).

The uncertainty band widens with horizon because the underlying
weather forecasts widen. **We don't hide this.** A Saturday-9am p50 of
11 m means very different things if p10/p90 is 9–13 m versus 4–18 m,
and the app shows the band, not just the median.

## Best-window detection

For each forecast horizon hour, the model computes p50 / p10 / p90.
A "best window" is identified as the contiguous run of hours within
the next 7 days where p50 is highest *and* the band is tight enough to
be plannable. Surfaced as: *"Best dive window this week: Saturday
9 AM – noon, ~12 m visibility."*

This is the single most-used Pro feature.

## Sentinel validation

The model's output is captured every six hours at a small set of
sentinel locations with known clarity ranges. Snapshots persist with
full per-source breakdown. The system auto-flags:

- p50 outside expected range for the location
- p50 drift beyond a threshold versus prior snapshot, without an
  upstream change to explain it
- low confidence (broad uncertainty band) at a location that should be
  well-constrained

When a calibration regression slips through code review, sentinels
catch it on the next 6 h cycle — before users complain.

## Things we deliberately don't do (yet)

- **Full Ensemble Kalman Filter (EnKF) state estimation.** Today the
  model uses point-estimate fusion. EnKF would couple time history
  more rigorously and improve performance during multi-day cloud
  cover. Planned.
- **Sub-cell resolution via Sentinel-2 (10 m).** Today our finest
  native resolution is GOES at 2 km. Sentinel-2 opportunistic
  processing for sub-kilometre features (jetty vs reef differences) is
  planned.
- **Per-region calibration of Kd→Secchi.** Today we use the published
  Lee 2015 coefficient with the Case-1/Case-2 adjustment. Per-region
  regression against in-situ data (where available) would tighten
  uncertainty further.

These aren't hidden — the next material change will land here when it
ships. Until then, the model does what's described above.

## See also

- [features/water-clarity.md](../features/water-clarity.md) — what
  users see
- [methodology/kd490-explained.md](kd490-explained.md) — the optical
  theory behind Kd → Secchi
- [methodology/data-sources.md](data-sources.md) — full provider list
  with citations
- [methodology/data-honesty.md](data-honesty.md) — the principles that
  shape the model's outputs
