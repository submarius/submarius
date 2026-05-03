---
title: Water clarity forecast
description: How Submarius forecasts underwater visibility for any coastal location, using fused satellite ocean colour and physical penalties.
---

# Water clarity forecast

The single most important question for a spearo or freediver is: *"will
the water be clear?"* Submarius answers it directly.

This page describes the **feature** — what you see and what it does. The
underlying mathematics is in
[methodology/water-clarity-model.md](../methodology/water-clarity-model.md).

## What you get

For any coastal point on Earth and any hour of the next seven days,
Submarius returns:

- A **median visibility estimate** in metres (or feet, by preference)
- An **uncertainty band** (p10 / p50 / p90 quantiles)
- The **best window** in the coming week — the hour or hour-range with
  the highest expected visibility
- A list of **contributing factors** with each one's pull on the number:
  satellite, baseline, recent rain, wind history, tide stage, HAB
  bulletin, river plume, post-dive reports

If the model can't speak confidently about a location (e.g. a fully
inland pond, no satellite coverage in the last 14 days, or a region with
no oceanographic baseline), the app says so explicitly. We don't
fabricate.

## What makes it different

Three things, none of which any other consumer app does:

1. **Multi-sensor fusion.** Submarius blends three independent satellite
   ocean-colour sources (VIIRS-SNPP, NOAA's VIIRS+OLCI multi-mission
   gap-filled product, and GOES-16 ABI processed hourly via ACOLITE)
   using inverse-variance weighting in log-Kd space. When the sources
   agree, the uncertainty band tightens. When one is cloud-locked, the
   others carry the estimate.
2. **Honest uncertainty bands.** The width of the band reflects how
   confidently the model can speak — narrow when satellite is fresh and
   reports are nearby, wide when the inputs are stale. A 10 m estimate
   with a 9–11 m band means something different from a 10 m estimate
   with a 4–18 m band, and the app shows both.
3. **Physical penalties layered on the satellite signal.** Recent rain,
   wind history over shallow bottoms, river-discharge plumes, tide
   stage, and active HAB warnings all pull the estimate down with their
   own confidence levels. Each is visible in the breakdown.

## Sources of input

| Signal | Provider | Notes |
|---|---|---|
| Diffuse attenuation Kd490 | NOAA CoastWatch (VIIRS-SNPP, multi-mission DINEOF) | Daily L3 product, 1–3 day latency |
| Geostationary ocean colour | NOAA NODD (GOES-16 ABI L1b) → ACOLITE | Hourly daylight, sub-30 s from publish |
| Chlorophyll-a | NOAA CoastWatch | Used for water-type classification |
| Wind history | Open-Meteo, NOAA HRRR | 24/72/168 h aggregates |
| Precipitation history | Open-Meteo Historical | 24/48/72 h cumulative |
| River discharge | USGS Water Services (US) | 15-minute readings, 50 km radius |
| Bathymetry | GEBCO | For the depth cap |
| HAB bulletins | NOAA Gulf Coast HAB Forecast | Severity-weighted penalty |
| Post-dive reports | Submarius (crowd-sourced) | H3-fuzzed for privacy |

## Diver-relevance horizon

Submarius caps the visibility output at **122 m (400 ft)** even when the
satellite suggests it could physically be higher. Most divers don't go
past that depth, so any number above it is operationally meaningless.
Pelagic readings up to that horizon remain uncapped — the Gulf Stream
genuinely does deliver 30 m+ visibility and we report it.

## Bathymetric cap

Visibility cannot exceed roughly `bottom_depth × 1.5` when the seafloor
is shallower than the diver-relevance horizon. A four-metre coastal
bottom with crystal-clear satellite Kd will still cap at six metres of
useful visibility because the bottom reflects light back through the
water column and you can't see *through* the seafloor.

## Reports flywheel

Every dive trip in Submarius can end with a one-tap viz rating. Reports
are H3-quantised before they leave the device (we never store precise
coordinates) and feed back into the model two ways:

- **Pinning recent estimates** — a report less than 24 h old within
  ~5 km of a query directly weights the local estimate.
- **Calibrating the baseline** — accumulated reports refine the regional
  coefficients. The more reports a region has, the tighter the model
  becomes there.

The model ships good on day one (it's grounded in published oceanography
before any user has ever opened the app), then gets sharper with use.
