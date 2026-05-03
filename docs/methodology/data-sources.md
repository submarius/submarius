---
title: Data sources and licences
description: Every public oceanographic and meteorological provider Submarius uses, with citations and licence terms.
---

# Data sources and licences

Submarius integrates public oceanographic and meteorological data. None
of these providers are proprietary to us; we just consume them
honestly, cache responsibly, and credit them visibly.

If you're an upstream provider and you'd prefer different attribution
or have rate-limit concerns, please contact us via the link on
[submarius.com](https://submarius.com).

---

## Satellite ocean colour

### NOAA CoastWatch ERDDAP

- **What we use**: Diffuse attenuation coefficient at 490 nm (Kd490)
  from VIIRS-SNPP daily L3 products, plus the multi-mission VIIRS+OLCI
  DINEOF gap-filled product. Chlorophyll-a from the same providers.
- **Portal**: https://coastwatch.noaa.gov/
- **ERDDAP**: https://coastwatch.pfeg.noaa.gov/erddap/
- **Resolution**: 750 m (regional sectors), 4 km (global)
- **Latency**: 1–3 day lag for daily L3 products; 10–12 days for the
  DINEOF gap-fill
- **Licence**: NOAA data is in the public domain in the US. Attribution
  appreciated.
- **Notes**: We cache responses with TTLs aligned to the upstream
  publication cadence. We don't hammer the API.

### NOAA NODD on AWS — GOES-16 ABI L1b

- **What we use**: Geostationary L1b radiance, processed via
  [ACOLITE](https://github.com/acolite/acolite) for atmospheric
  correction and ocean-colour extraction.
- **Source**: NOAA Open Data Dissemination programme, S3 bucket
  `noaa-goes16`.
- **Resolution**: 2 km nadir (CONUS)
- **Cadence**: hourly daylight, sub-30 second from publish
- **Licence**: Public domain. Attribution to NOAA.
- **Citation**: ACOLITE: Vanhellemont, Q. (2019). *Daily metre-scale
  mapping of water turbidity using CubeSat imagery.* Optics Express
  27(20), A1372–A1399.

---

## Atmosphere and marine forecast

### Open-Meteo

- **What we use**: Marine forecast (wind, swell, wave, SST), forecast
  weather (precipitation, temperature, pressure), and historical
  weather (precipitation history for the clarity model).
- **Portal**: https://open-meteo.com/
- **Licence**: CC BY 4.0 — attribution required.
- **Attribution**: "Weather data by Open-Meteo.com" surfaced on
  conditions detail screens.

### NOAA NDFD (National Digital Forecast Database)

- **What we use**: Short-range surface wind and marine forecast for
  the verdict computation.
- **Portal**: https://www.weather.gov/mdl/ndfd_home
- **Licence**: Public domain.

### NOAA HRRR (High-Resolution Rapid Refresh)

- **What we use**: 3 km surface wind raster for the wind-particle
  overlay (Pro feature).
- **Portal**: https://rapidrefresh.noaa.gov/hrrr/
- **Licence**: Public domain.

---

## Tides, currents, sea level

### NOAA Tides & Currents

- **What we use**: Tide predictions, current predictions, observed
  water level, harmonic constants.
- **Portal**: https://tidesandcurrents.noaa.gov/
- **API**: https://api.tidesandcurrents.noaa.gov/api/prod/
- **Licence**: Public domain.
- **Coverage**: US coastal (CONUS, Alaska, Hawaii, territories).

For non-US locations we fall back to global tidal modelling via
Open-Meteo Marine.

---

## Sea-surface temperature overlay

### NASA GIBS — GHRSST L4

- **What we use**: Daily SST raster for the map overlay (Pro feature).
- **Portal**: https://gibs.earthdata.nasa.gov/
- **Licence**: Public domain. Attribution to NASA EOSDIS GIBS.

---

## Harmful algal blooms

### NOAA Gulf Coast HAB Forecast

- **What we use**: Active HAB flags, severity (0–3), and bloom centre
  positions for the clarity-model penalty and the user-visible
  warning.
- **Portal**: https://coastalscience.noaa.gov/science-areas/habs/hab-forecasts/gulf-coast/
- **Licence**: Public domain.
- **Coverage**: Gulf of Mexico, Florida east coast.

---

## Hydrology

### USGS Water Services

- **What we use**: Real-time river discharge (parameter 00060), gauge
  height (00065), and turbidity (63680) for the river-plume penalty
  in the clarity model.
- **API**: https://waterservices.usgs.gov/docs/instantaneous-values/
- **Licence**: Public domain.
- **Rate limit**: 50 req/h without API key, 1000 req/h with key.
- **Coverage**: US-only.

---

## Bathymetry

### GEBCO (General Bathymetric Chart of the Oceans)

- **What we use**: Global bathymetric grid, used for the depth cap on
  the clarity estimate and for the bathymetry overlay.
- **Portal**: https://www.gebco.net/
- **Licence**: GEBCO 2024 grid is freely available; users are asked
  to credit GEBCO.

### ESRI World Ocean Base

- **What we use**: Bathymetry tile overlay for the map.
- **Portal**: https://www.arcgis.com/home/item.html?id=1e126e7520f9466c9ca28b8f28b5e500
- **Licence**: Used per ESRI's terms of use (free for non-commercial
  embedded use; we proxy and cache to stay under their fair-use
  guidance).

---

## Cartography

### OpenStreetMap

- **What we use**: Coastlines, marine features (reefs, wrecks,
  harbours, channel markers), nautical-relevant POIs.
- **Portal**: https://www.openstreetmap.org/
- **Licence**: ODbL — attribution required and database-share-alike.
- **Attribution**: "© OpenStreetMap contributors" surfaced on the
  base map.

### NOAA ENC (Electronic Navigational Charts)

- **What we use**: US nautical chart features (channels,
  obstructions, depth contours).
- **Portal**: https://nauticalcharts.noaa.gov/charts/noaa-enc.html
- **Licence**: Public domain.

### Marine Regions

- **What we use**: Maritime boundaries (EEZ, territorial waters).
- **Portal**: https://www.marineregions.org/
- **Licence**: CC BY 4.0.

### Protected Planet

- **What we use**: Marine Protected Area boundaries.
- **Portal**: https://www.protectedplanet.net/
- **Licence**: Open data; attribution to UNEP-WCMC and IUCN required.

---

## Marine biology

### iNaturalist

- **What we use**: Crowd-sourced species observations for the fish-ID
  tile grid (location-filtered species presence).
- **Portal**: https://www.inaturalist.org/
- **Licence**: Per-observation; we use only CC0 / CC BY / CC BY-NC
  observations.

### OBIS (Ocean Biodiversity Information System)

- **What we use**: Authoritative species occurrence records for
  seasonal range modelling.
- **Portal**: https://obis.org/
- **Licence**: CC BY 4.0.

### WoRMS (World Register of Marine Species)

- **What we use**: Species taxonomy, biology, habitat.
- **Portal**: https://www.marinespecies.org/
- **Licence**: CC BY 4.0.
- **Cache TTL**: 30 days (taxonomy doesn't change often).

---

## Shark tracking

### OCEARCH

- **What we use**: Tagged-shark position pings.
- **Portal**: https://www.ocearch.org/
- **Licence**: Per OCEARCH's data sharing terms; we cache and surface
  positions with attribution.

---

## Reverse geocoding

### Public Nominatim (OSM)

- **What we use**: Reverse geocoding of lat/lon to nearby place names
  (display only; never used as a model input — see the
  [data-honesty page](data-honesty.md) for why).
- **Portal**: https://nominatim.org/
- **Licence**: ODbL via OpenStreetMap.

---

## Submarius-internal

The following are computed in-house, not pulled from a third party:

- **Astronomical** — sun, moon, solunar, twilight times. Computed via
  standard astronomical formulae (Meeus); no external API.
- **Reefs and wrecks structure overlay** — built from OSM features
  plus per-state datasets where they exist.
- **Activity-rated verdicts** (clarity, bite, GO/NO) — composed in our
  own backend from the inputs above.

## Attribution policy

Submarius surfaces the active data providers in two places:

1. **Per-feature, in the conditions detail view.** When you tap into
   the water-clarity breakdown, every contributing source is listed.
2. **In the app's About / credits page.** All providers above are
   listed with their licence terms.

If you're a provider and want different wording or different placement,
get in touch.
