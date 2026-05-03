---
title: Tides and currents
description: Tide predictions and current data from NOAA Tides & Currents, with day-planning UI built around them.
---

# Tides and currents

Tide and current data drives a lot of what Submarius does — the verdict,
the bite-score, the water-clarity penalties — but it's also surfaced
directly as a feature so you can plan around it.

## What's shown

For any coastal location:

- **High and low tide times** for the next seven days
- **Tide range** (height between consecutive high and low)
- **Current rising / falling / slack** state at any selected hour
- **Spring vs neap** indicator (within ±3 days of new/full moon)
- **Hourly water-level graph** with the times of all the day's slack
  windows highlighted

Currents are shown where NOAA publishes prediction data (US coastal,
including Alaska and Hawaii). Outside that coverage we display tides
only.

## Source

- **Tide predictions** — [NOAA Tides & Currents](https://tidesandcurrents.noaa.gov/),
  via the predictions API. NOAA's harmonic-analysis model is the gold
  standard and we use it directly.
- **Currents predictions** — same provider, where available.
- **Sea-level observations** (where stations exist) for the live
  comparison.

For non-US locations, we fall back to global tidal modelling from
Open-Meteo Marine.

## Why this matters for the verdict

Tide stage feeds three other features:

1. **Water-clarity model.** Falling tide in an enclosed bay flushes
   sediment-laden water out; rising tide brings cleaner offshore water
   in. The clarity penalty for tide stage depends on the coastline
   geometry classification (open / semi-open / enclosed) of the
   location.
2. **Bite score.** Most species feed actively on moving water; activity
   drops near slack.
3. **Diving access.** At many sites, slack is the only safe time to
   enter the water — strong currents during peak flow can be dangerous
   for shore divers.

The verdict integrates these without forcing you to read the tide chart
yourself, but the chart is one tap away.

## Spring tides

Spring tides (within a few days of new and full moon) produce the
largest tidal range and the strongest currents. For some activities
that's good (more bait movement, hungry predators); for others it's bad
(too much current, too much sediment churn). The app indicates spring
state and the verdict adjusts accordingly per activity.
