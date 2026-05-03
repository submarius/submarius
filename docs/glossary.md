---
title: Glossary
description: Every acronym and oceanographic term used in the Submarius docs, defined plainly.
---

# Glossary

Plain definitions for the terms that show up in the Submarius docs and
API responses. If something here is wrong or unclear, please open a
[methodology question](https://github.com/Submarius/submarius/issues/new?template=methodology-question.yml).

## A

**ABI** — Advanced Baseline Imager. The instrument on NOAA's GOES-16
satellite that takes the visible/infrared imagery Submarius processes
into hourly water-colour data.

**ACOLITE** — An open-source atmospheric-correction processor for
satellite imagery, originally developed for Sentinel-2 and Landsat,
extended to GOES. Submarius uses it to extract ocean-colour signal
from raw GOES-16 ABI L1b radiance. https://github.com/acolite/acolite

**Adjacency effect** — In satellite ocean colour, the bias caused by
land or shallow-bottom reflectance bleeding into the signal of an
adjacent water pixel. Pushes apparent clarity *higher* than reality.
Submarius downweights affected pixels.

**AOT** — Aerosol Optical Thickness. Measured by satellite to
distinguish atmospheric haze from water-leaving radiance.

## B

**Bathymetry** — The underwater equivalent of topography: depth of the
seafloor as a function of position. Submarius uses GEBCO bathymetry to
cap visibility estimates (you can't see past the bottom).

**Bite score** — Submarius's 0–10 fish-activity index for a target
species at a location and time. See
[features/bite-score.md](features/bite-score.md).

## C

**Case-1 / Case-2** — An oceanographic classification of water type.
Case-1 is open ocean dominated by phytoplankton; Case-2 is coastal
water dominated by suspended sediment, CDOM, and river-borne material.
The Lee 2015 Kd→Secchi coefficient differs between them. See
[methodology/kd490-explained.md](methodology/kd490-explained.md).

**CDOM** — Coloured Dissolved Organic Matter. Tannin-like dissolved
organics that absorb blue light and tint water yellow-brown. Common in
river-influenced coasts, mangrove systems, and tannic regions like
the Carolinas and northern Australia.

**Chlorophyll-a** — Pigment in phytoplankton; satellite-measurable
proxy for phytoplankton biomass. Used by Submarius for water-type
classification (high chlorophyll → bloom-influenced → leans Case-2).

**CoastWatch** — NOAA's satellite ocean-colour and SST data programme.
Distributes the VIIRS-SNPP and multi-mission DINEOF products
Submarius consumes.

## D

**DINEOF** — Data Interpolating Empirical Orthogonal Functions. A
gap-filling technique used by NOAA to produce continuous Kd490 fields
from cloud-affected daily satellite data.

**Diver-relevance horizon** — Submarius caps clarity output at 122 m
(400 ft) because most divers don't go past that depth. Beyond it,
visibility numbers are operationally meaningless.

## E

**Edge Network** — Reverse-proxy CDN edge. Submarius doesn't currently
use one; mentioned occasionally in the docs context.

**EnKF** — Ensemble Kalman Filter. A state-estimation technique for
combining noisy time-series observations with a forward physical
model. Submarius's clarity model uses point-estimate fusion today;
EnKF integration is planned.

**ERDDAP** — Environmental Research Division Data Access Protocol. A
NOAA data-server framework that exposes scientific data via uniform
HTTP endpoints. Submarius pulls satellite Kd490 from CoastWatch's
ERDDAP instance.

## F

**FNU** — Formazin Nephelometric Unit. A standard turbidity unit. USGS
publishes river turbidity in FNU; we don't currently use it directly
in the clarity model but it's available as an input signal.

**Front (cold front)** — A weather-system boundary; passage typically
suppresses fish activity for 6–24 h, then often produces a 24–36 h
rebound.

## G

**GEBCO** — General Bathymetric Chart of the Oceans. Free global
bathymetric grid used by Submarius for the depth cap and the
bathymetry overlay.

**GHRSST** — Group for High Resolution Sea Surface Temperature. A
NASA/NOAA-coordinated SST data product used by Submarius for the SST
overlay.

**GIBS** — Global Imagery Browse Services. NASA's tile-server for
public satellite imagery; Submarius consumes the GHRSST tiles via
GIBS.

**GOES-16** — Geostationary Operational Environmental Satellite. A
NOAA weather satellite parked over the Americas. Its ABI instrument
provides the hourly imagery Submarius processes for sub-daily ocean
colour.

## H

**H3** — Uber's open-source hexagonal hierarchical spatial indexing
system. Submarius uses H3 cells to (a) cache spatial data efficiently
and (b) fuzz user spot locations for privacy. https://h3geo.org/

**HAB** — Harmful Algal Bloom. Submarius integrates NOAA's Gulf Coast
HAB Forecast for active-bloom severity, which both penalises the
clarity estimate and surfaces a user-visible warning.

**HRRR** — High-Resolution Rapid Refresh. A NOAA 3 km surface weather
model used by Submarius for the wind-particle overlay (Pro feature).

## I

**Inverse-variance weighting** — The maximum-likelihood combination of
independent observations with known variances. The mathematical
backbone of Submarius's multi-satellite Kd490 fusion.

**IndexedDB** — Browser-native client-side database used by Submarius
for offline tile storage, conditions caching, and the mutation queue.

## K

**Kd490** — Diffuse attenuation coefficient at 490 nanometres. Measured
by satellites; converts to Secchi depth via the Lee 2015 model. See
[methodology/kd490-explained.md](methodology/kd490-explained.md).

## L

**L1b / L2 / L3** — Standard NASA/NOAA satellite-data processing
levels. L1b = calibrated radiance; L2 = derived geophysical products
at the original sensor resolution; L3 = gridded, time-averaged
products.

**Lee 2015** — The published mechanistic model for Kd → Secchi that
underlies Submarius's clarity computation. Lee, Z. P. et al. (2015),
*Remote Sensing of Environment* 169, 139–149. R² = 0.96 against 338
in-situ measurements.

## M

**MapLibre** — Open-source vector-map renderer used by Submarius. Fork
of Mapbox GL JS.

**MPA** — Marine Protected Area. Submarius surfaces MPA boundaries
from Protected Planet and the NOAA MPA Inventory.

## N

**NDFD** — National Digital Forecast Database. NOAA's authoritative
gridded weather forecast.

**NDBC** — National Data Buoy Center. Real-time observational data
from offshore buoys; Submarius consumes select NDBC stations for
ground-truthing.

**NODD** — NOAA Open Data Dissemination programme. Distributes raw
satellite L1b on AWS S3 (e.g. the GOES-16 ABI bucket Submarius
processes).

## O

**OBIS** — Ocean Biodiversity Information System. Authoritative species
occurrence records used by Submarius for seasonal range modelling.

**OCEARCH** — Long-running shark-tracking research organisation;
publishes the tagged-shark position feed Submarius surfaces in the
shark-alerts feature.

**OLCI** — Ocean and Land Colour Instrument on the Sentinel-3
satellites. Component of the multi-mission DINEOF Kd490 product.

## P

**Plus Code** — Open Location Code. A short alphanumeric code that
uniquely identifies any 14×14 m square on Earth. Submarius surfaces
Plus Codes alongside lat/lon for SOS and shared spots.
https://plus.codes/

**PWA** — Progressive Web App. A web app installable like a native
app, with offline support and push notifications.

## R

**Resuspension** — Wind-driven re-mobilisation of bottom sediment
into the water column. Major contributor to nearshore turbidity.
Modelled in Submarius via the W²/H scaling from Green & Coco (2014).

## S

**Secchi disk / Secchi depth (Z_SD)** — A 30 cm white disk lowered
into water until it disappears from view. The depth at which it
disappears is the Secchi depth. The classical standard for water
clarity, and the unit Submarius reports.

**SNPP** — Suomi National Polar-orbiting Partnership. The satellite
carrying VIIRS, the primary daily ocean-colour sensor for Submarius.

**Solunar** — Theory that fish activity peaks during specific lunar
position windows (moon overhead, moon underfoot, moonrise, moonset).
A component of the bite score; not load-bearing on its own.

**SST** — Sea Surface Temperature. Used by Submarius across the
clarity model, the bite-score, and as a map overlay.

## V

**Verdict** — Submarius's per-day GO / NO / CAUTION output for the
chosen activity at the chosen location. See
[features/verdict.md](features/verdict.md).

**VIIRS** — Visible Infrared Imaging Radiometer Suite. The instrument
on Suomi NPP and JPSS satellites that produces the daily ocean-colour
data Submarius's clarity model is anchored to.

**Viz / Visibility** — Underwater visibility, in metres or feet.
Approximated by Secchi depth.

## W

**WoRMS** — World Register of Marine Species. Authoritative marine
taxonomy and biology database; cached in Submarius for species data.

**W²/H** — Wind speed squared over water depth. The empirical scaling
parameter for wave-driven sediment resuspension in shallow water.

## Z

**Z_SD** — Secchi disk depth. See *Secchi*.
