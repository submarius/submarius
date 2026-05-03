---
title: Bite score
description: A 0–10 fish-activity index from solunar timing, barometric pressure, fronts, tide stage, and species temperature preferences.
---

# Bite score

A 0–10 number indicating how active fish are likely to be at a given
location and time. Tappable to see exactly which signals contributed and
by how much. No black-box "AI" claims — every component is published.

## What's wrong with most bite scores

Most fishing apps surface a "bite quality" number whose source is
opaque. It correlates loosely with solunar tables and doesn't react to
weather fronts, recent rain, or species presence. The user has no way to
know whether the number reflects genuine biological activity or a
random-feeling marketing veneer.

Submarius makes the inputs visible. If the score is high because the
moon is overhead and the tide is rising and a high-pressure system is
parked over the area for 36 h, you can see all three. If it's low
because a cold front rolled through 18 h ago and the species you targeted
prefers warmer water than is currently present, you see that too.

## Inputs

| Signal | What it measures | Weight role |
|---|---|---|
| **Solunar phase** | Major and minor periods (moon overhead/underfoot, moonrise/set) | Primary timing signal |
| **Barometric trend** | 24-hour pressure change | Falling pressure ahead of a front spikes activity; stable high-pressure suppresses it |
| **Recent fronts** | Cold-front passage in last 48 h | Post-front lull, then 6–24 h rebound |
| **Tide stage** | Rising / falling / slack, and rate of change | Most species feed actively on moving water |
| **Water temperature** | Sea-surface temperature versus species preference range | Warm-water fish dial down in cold water and vice versa |
| **Species presence** | Seasonal range overlap from OBIS / iNaturalist | If the target species isn't likely to be present, the score reflects that |
| **Time of day** | Dawn/dusk windows | Most predatory species are crepuscular |
| **Wind and wave** | From marine forecast | Heavy chop suppresses activity for many species |

## Per-species, not generic

The score is computed against a target species (or species category) —
"redfish in the flats", "yellowtail snapper offshore", "tuna pelagic" —
rather than a generic "fish". Temperature preference, tide preference,
and time-of-day pattern all differ. A score that ignores species is a
score that's wrong half the time.

## What it's *not*

- **Not a guarantee.** Fish are wild animals; a 9/10 score doesn't mean
  you'll catch one.
- **Not a substitute for local knowledge.** A captain who knows the spot
  beats any model. The score is an input to your decision, not a
  replacement for it.
- **Not "AI predictions" in the buzzword sense.** It's a transparent
  composition of well-understood biological and environmental signals.

## Explainability

Tap the score to expand the breakdown. Every contributing signal shows:

- Its current value (e.g. *"barometric pressure: 1018 hPa, falling
  4 hPa in last 24 h"*)
- Its pull on the score (e.g. *"+0.8 — falling pressure ahead of a
  weather change"*)
- A short reason in plain English

If you disagree with a weighting, the underlying logic is documented in
[methodology/bite-score-signals.md](../methodology/bite-score-signals.md)
and we welcome discussion via
[methodology questions](https://github.com/Submarius/submarius/issues/new?template=methodology-question.yml).
