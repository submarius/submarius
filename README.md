# Submarius

> Water-clarity forecasts, honest bite scores, and a single GO/NO verdict per dive day — for spearos, divers, anglers, and boaters.

[![Get it on the App Store](https://img.shields.io/badge/App_Store-Download-0a84ff?style=for-the-badge&logo=apple&logoColor=white)](https://apps.apple.com/app/submarius)
[![Web App](https://img.shields.io/badge/Web_App-submarius.com-1f6feb?style=for-the-badge&logo=safari&logoColor=white)](https://submarius.com)
[![Docs](https://img.shields.io/badge/Docs-docs.submarius.com-2e8b57?style=for-the-badge&logo=readthedocs&logoColor=white)](https://docs.submarius.com)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey?style=for-the-badge)](LICENSE)

This repository is the **public documentation** for Submarius — methodology,
data sources, feature explainers, FAQ, and changelog. **It does not contain
source code or compiled apps.** The product ships through the App Store and
the web; this repo exists so the model behind it is open to scrutiny.

---

## What Submarius is

A mobile-first ocean-conditions app built around one question: **should I go,
or not?**

Most marine-weather apps hand you ten numbers and let you decide. Submarius
runs the integration step itself: tide stage, swell, wind history,
satellite-derived water clarity, river plumes, harmful-algal-bloom bulletins,
moon phase, and bathymetry — combined into a single GO / NO / CAUTION
verdict per dive day, with every input tappable for its full reasoning.

Built for:

- **Spearos and freedivers** — water clarity is the question that decides
  the trip. Submarius is the only app that forecasts it from physically
  grounded oceanography (NOAA CoastWatch satellite Kd490 → Secchi via the
  Lee 2015 model), not vibes.
- **Anglers** — bite-score from solunar timing, barometric pressure,
  recent fronts, and tide stage, with the inputs visible.
- **Divers and boaters** — tides, currents, marine forecast, reef and
  wreck overlays, OCEARCH tagged-shark alerts, free SOS with Plus Code
  rescue location, buddy-GPS sharing.

---

## What's in this repo

- **[docs/overview.md](docs/overview.md)** — the product map: every
  feature, every data source, who it's for.
- **[docs/features/](docs/features/)** — one page per feature
  (water clarity, bite score, the verdict, tides, shark alerts, safety,
  offline mode, privacy, Pro vs free).
- **[docs/methodology/](docs/methodology/)** — how the models actually
  work. Cited oceanography, no hand-waving.
- **[docs/glossary.md](docs/glossary.md)** — every acronym defined plainly
  (Kd490, Secchi, H3, Case-1/Case-2, CDOM, FNU…).
- **[docs/faq.md](docs/faq.md)** — real questions from real users.
- **[docs/press/](docs/press/)** — press kit and coverage log.
- **[CHANGELOG.md](CHANGELOG.md)** — every shipped release, dated.

Mirrored at [docs.submarius.com](https://docs.submarius.com) via GitHub
Pages.

## What's *not* in this repo

- No source code. The Go backend, React user app, admin SPA, and Astro
  marketing site are private.
- No APKs, no IPAs, no binaries. The app distributes through the App
  Store; the web app runs at [submarius.com](https://submarius.com).
- No internal calibration coefficients, model weights, training data,
  infrastructure topology, or commercial details.
- No roadmap. Shipped is shipped; everything else is just intent.

If you're looking to build on Submarius data, the public conditions API is
documented at [submarius.com](https://submarius.com) — get in touch via the
contact link there.

---

## How the clarity model works (one paragraph)

Submarius fuses three satellite ocean-color sources (VIIRS-SNPP daily,
the NOAA VIIRS+OLCI multi-mission DINEOF gap-filled product, and GOES-16
ABI processed hourly via ACOLITE) using **inverse-variance weighting in
log-Kd space** — the maximum-likelihood combination for independent
Gaussian observations. The fused Kd is converted to Secchi depth via the
Lee 2015 mechanistic model (R² = 0.96 across 338 globally distributed
in-situ measurements), with an adaptive coefficient that switches between
Case-1 (open ocean, ~2.38) and Case-2 (coastal, sediment- and
CDOM-dominated, ~1.5) based on coastline-geometry enclosure, distance to
shore, satellite chlorophyll, and active HAB flags. The result is then
penalized for recent precipitation (river-plume + nearshore runoff,
~2-day lag), wind-history resuspension over shallow bottoms (W²/H
scaling, Green & Coco 2014), tide-stage current effects, and HAB
bulletins. Bathymetry caps the estimate at `bottom_depth × 1.5` when the
seafloor is shallower than the 122 m diver-relevance horizon —
a four-metre bottom can't yield twenty-five-metre visibility. Every
component contributes a `(value, weight, reason)` triple, surfaced in the
API response so the user can see exactly why the number is what it is.

Full methodology: [docs/methodology/water-clarity-model.md](docs/methodology/water-clarity-model.md).

---

## Data sources

Submarius pulls from public oceanographic and meteorological providers.
None are proprietary; we just integrate them honestly.

- **NOAA CoastWatch ERDDAP** — satellite Kd490, chlorophyll-a (VIIRS,
  multi-mission DINEOF)
- **NOAA NODD on AWS** — GOES-16 ABI L1b for sub-daily ocean color
  (processed via [ACOLITE](https://github.com/acolite/acolite))
- **NOAA NDFD / HRRR** — short-range surface wind, marine forecast
- **Open-Meteo Marine + Forecast** — wind, swell, wave, sea-surface
  temperature, precipitation history
- **NOAA Tides & Currents** — tide predictions, water level, currents
- **NOAA HAB Forecast** — Gulf Coast harmful-algal-bloom bulletins
- **USGS Water Services** — river discharge and turbidity (US)
- **OCEARCH** — tagged-shark tracking
- **GEBCO** — global bathymetry
- **OpenStreetMap** — coastlines, reef and wreck features
- **Marine Regions** — maritime boundaries, MPAs

Full provider list with links and license terms:
[docs/methodology/data-sources.md](docs/methodology/data-sources.md).

---

## Honesty principles

Submarius operates under three rules that show up in every part of the
product:

1. **Never claim what we don't measure.** No "great viz today!" copy
   unless the satellite or recent reports actually support it. Where the
   model is uncertain, the app says so.
2. **No hardcoded jurisdiction-specific data.** No "good in Florida,
   bad in Maine" lookup tables. Classifications come from coastline
   geometry and oceanographic inputs, not place names.
3. **Safety is permanently free.** OCEARCH shark alerts, SOS with Plus
   Code rescue location, dive-buddy GPS sharing, and emergency contact
   are bypassed by the entitlement middleware regardless of subscription
   tier.

More: [docs/methodology/data-honesty.md](docs/methodology/data-honesty.md).

---

## Get the app

- **iOS** — [Download on the App Store](https://apps.apple.com/app/submarius)
- **Web** — [submarius.com](https://submarius.com) (works on any modern
  browser; install as a PWA)
- **Pricing** — Free tier with all safety features. Pro is $12.99/mo,
  $99/yr, or $149 lifetime.

---

## Press and contact

- Press kit, screenshots, founder bio: [docs/press/press-kit.md](docs/press/press-kit.md)
- Coverage log: [docs/press/coverage.md](docs/press/coverage.md)
- General inquiries: via the contact link on [submarius.com](https://submarius.com)
- Security disclosures: see [SECURITY.md](SECURITY.md)

---

## License

The contents of this repository — prose, diagrams, and brand imagery
explicitly placed in `assets/brand/` — are licensed under
[Creative Commons Attribution 4.0 International](LICENSE). Quote, link,
adapt, translate. Attribution back to Submarius is appreciated.

The Submarius name, logo, and product trade dress are not covered by the
CC BY licence.
