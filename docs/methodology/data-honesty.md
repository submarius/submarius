---
title: Data-honesty principles
description: The three rules that govern what Submarius will and won't claim, hardcode, or assume.
---

# Data-honesty principles

Submarius operates under three principles that show up in the source
code, the model, the API responses, and the user-facing copy. They are
load-bearing — they shape what the product is willing to say and what
it refuses to fake.

## 1. Never claim what we don't measure

If the model isn't actually computing something from real data, the app
doesn't display a number for it. There is no fake confidence, no
placeholder text that reads as a forecast, no "looks good!" copy that
isn't backed by signal.

Concretely:

- Where satellite coverage is too stale to anchor the clarity model,
  the app surfaces *"insufficient recent data"* — not a guess.
- Where the verdict's required inputs aren't all present, the verdict
  is suppressed entirely rather than computed on a partial set.
- The water-clarity uncertainty band reflects the actual disagreement
  among the sub-models. When the band is wide, the app *shows it
  wide*. We do not artificially narrow the band to look more
  confident.
- The bite score collapses toward zero when the target species is
  unlikely to be present at the season and location. We don't quote a
  high score for a species that isn't there.

The discipline is unglamorous — every product team is tempted to fill
empty space with confident-sounding output — but trust is the
product. A wrong "GO" verdict that turns out to be 1 m visibility is a
worse user experience than no verdict at all.

## 2. No hardcoded jurisdiction-specific data

Submarius does not ship lookup tables of "Florida = clear, New Jersey
= murky". It does not bake state-by-state lists of species, regions,
or thresholds into the code or the copy.

Why:

- Hardcoded jurisdiction data is a maintenance time bomb. It rots the
  moment a regulation changes, a coastline shifts, or a season moves.
- It implies coverage we don't have. A code-baked list of "supported
  states" tells the user we know something authoritative about each;
  in practice we know whatever the public oceanographic data tells
  us, which generalises across coastlines.
- It encodes assumptions that don't generalise. Coastline geometry,
  bathymetry, and oceanographic inputs *do* generalise — the model
  works the same way in Florida and the Adriatic.

When the model needs to classify a location (e.g. "is this an
enclosed bay or an open coast?"), the classification comes from
**coastline geometry analysis** (offshore distance, shelter angle,
adjacent water cover), not from a list of named places. Same for
seasonal climatology — driven by latitude band and date, not a list of
"warm states".

The single exception is **regulations data** — fishing size and bag
limits per jurisdiction — which is genuinely per-jurisdiction and is
shipped as a structured YAML file with a clearly-dated last-update
field. When that file is stale, the app surfaces the date and tells
the user to verify with local authority.

## 3. Safety features are permanently free

Three features bypass the entitlement middleware regardless of
subscription tier and remain available to every user:

- **SOS** (with Plus Code rescue location)
- **Dive-buddy GPS sharing**
- **OCEARCH tagged-shark proximity** and crowd-sourced shark sightings

This is permanent product policy, not a free-trial. The reasoning is
covered on [features/safety.md](../features/safety.md) and
[features/pro-vs-free.md](../features/pro-vs-free.md).

## How these show up in the API

The Submarius public conditions API returns, for every clarity query:

- The estimate (`p50`)
- The uncertainty band (`p10`, `p90`)
- The list of contributing factors with each one's value, weight, and
  a plain-language reason
- The age and source of each satellite contribution
- A `data_status` field that flags `insufficient_satellite_coverage`,
  `insufficient_baseline`, `bathymetry_unavailable`, etc., when any
  required input is missing

Anyone building on the API can verify the principles above by
inspecting the response. There is no hidden state; the model's
reasoning is on the wire.

## Things you might expect that we don't do

Some product practices we've explicitly declined:

- **No "personalised" forecasts based on past behaviour.** The model
  is the model; it doesn't quietly nudge results based on what you've
  liked.
- **No A/B testing of safety messaging.** Safety copy is deterministic
  across all users.
- **No engagement-loop dark patterns.** No "streak", no "you haven't
  logged in for 3 days!", no manufactured urgency. The app should be
  useful when you need it and invisible when you don't.
- **No selling user data.** The privacy posture is on
  [features/privacy.md](../features/privacy.md).
- **No location tracking outside an active dive trip.** Background
  location is not used for product analytics.

## Where this came from

These principles aren't aspirational — they're enforced by code review
and live in the product's architecture decisions. The water-clarity
model spec encodes the *"never claim what we don't measure"* rule in
the data-status path; the entitlement middleware encodes the
*safety-permanently-free* rule in the access-control path; the
classification subsystem encodes the *no-hardcoded-jurisdiction-data*
rule by computing classifications from coastline geometry rather than
from a list of named places.

The three rules survive feature work because they have to. Without
them Submarius would be just another marine app guessing at what
might happen.
