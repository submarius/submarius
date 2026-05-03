---
title: Overview — what Submarius does
description: Product feature map and data sources for Submarius, the ocean-conditions app for spearos, divers, anglers, and boaters.
---

# Overview

Submarius is a mobile-first ocean-conditions app for spearos, divers,
anglers, and boaters. It aggregates marine forecasts, satellite ocean
colour, river discharge, tide predictions, and harmful-algal-bloom
bulletins, then layers a handful of custom intelligence products
(water-clarity forecast, bite score, GO/NO verdict) on top.

The product runs as a native iOS app and as a progressive web app at
[submarius.com](https://submarius.com).

This page is the index of *what* Submarius does. The
[methodology section](methodology/) explains *how*.

---

## Ocean and weather conditions

| Feature | Source |
|---|---|
| Wind, swell, wave forecast | Open-Meteo Marine, NOAA NDFD |
| Short-range surface wind raster | NOAA HRRR |
| Tides and currents | NOAA Tides & Currents |
| Sea-surface temperature | NOAA CoastWatch, NASA GHRSST, Open-Meteo Marine |
| Water clarity / visibility | NOAA CoastWatch (VIIRS, OLCI), GOES-16 ABI via [ACOLITE](https://github.com/acolite/acolite), NOAA HAB bulletins, USGS, post-dive reports |
| Moon phase, sun times | Astronomical computation |
| Activity-rated verdict | Submarius (see [The verdict](features/verdict.md)) |

The clarity layer is the differentiating one. Most marine apps stop at
the marine forecast and let you guess the visibility from wind history.
Submarius forecasts visibility directly via fused satellite ocean colour
combined with weather-driven penalties — the
[methodology page](methodology/water-clarity-model.md) walks through
every component.

---

## Maps, charts, and overlays

| Feature | Source |
|---|---|
| Base vector tiles | Self-hosted MapLibre (OpenStreetMap + GEBCO + NOAA ENC) |
| Bathymetry overlay | ESRI World Ocean Base |
| Sea-surface temperature overlay | NASA GIBS GHRSST |
| Animated precipitation radar | RainViewer |
| Wind particles | NOAA HRRR |
| Reefs and wrecks | OpenStreetMap + state datasets |
| Nautical charts (US) | NOAA ENC |
| MPAs and no-take zones | Protected Planet, NOAA MPA Inventory |
| Maritime boundaries | Marine Regions |
| Reverse geocoding | Public Nominatim |

All third-party tiles are fetched server-side and cached, so the client
never talks to upstream tile servers directly. This keeps the client
fast on flaky boat wifi and keeps Submarius inside upstream rate limits.

The base map is deliberately minimal — bathymetry, contours, channel
markers — rather than a full street/POI render. Marine-chartplotter
aesthetic, not Google Maps.

---

## Safety

Permanently free, regardless of subscription tier. See
[features/safety.md](features/safety.md) for the full breakdown and the
rationale.

| Feature | Source |
|---|---|
| Tagged shark tracking | OCEARCH |
| Live shark sightings | Crowd-sourced |
| Dive-buddy GPS sharing | Submarius (real-time WebSocket) |
| Emergency SOS | Client-side, fan-out via push + SMS |
| Precise rescue location | [Plus Codes](https://plus.codes/) |
| Garmin inReach satellite SOS | Planned, requires partner integration |

---

## Fish intelligence

| Feature | Source |
|---|---|
| Fish-ID by location | iNaturalist + OBIS + WoRMS |
| Species biology | WoRMS |
| Bite score | Submarius (see [features/bite-score.md](features/bite-score.md)) |
| Size and limit checker | Per-jurisdiction regulations data |
| Catch logging | Submarius |

The bite score is computed from solunar timing, barometric pressure
trend, recent fronts, tide stage, and water temperature versus species
preference. Inputs are tappable for full reasoning; no black-box AI
claims.

---

## Spot intelligence

| Feature | Source |
|---|---|
| Crowd-sourced dive spots | Submarius, with privacy controls and GPS fuzzing |
| Personal spot library | On-device, optional E2EE backup |
| Spot privacy modes | Submarius |
| Reefs, wrecks, structure | OSM and state datasets |

Spots are private by default. When shared, locations are H3-quantised
before they leave the device — Submarius itself can't recover precise
coordinates. Optional end-to-end encrypted backup keeps spot data
recoverable without exposing it server-side. See
[features/privacy.md](features/privacy.md).

---

## Offline-first

Every endpoint is designed to degrade gracefully when the network drops,
because boat connectivity is what it is.

- Offline base map (bathymetry-only) usable with no signal
- Conditions cached on last successful fetch and shown with a freshness
  indicator
- Mutations (catch logs, viz reports, buddy positions) queued locally
  and replayed on reconnect

See [features/offline-mode.md](features/offline-mode.md).
