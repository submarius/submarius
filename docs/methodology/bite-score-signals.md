---
title: Bite-score signals
description: The biological and environmental signals that compose the Submarius bite score, and the literature behind each.
---

# Bite-score signals

The user-facing
[features/bite-score.md](../features/bite-score.md) describes what the
bite score is. This page documents the signals that compose it and the
literature behind each.

We don't publish the exact numerical weights — those get retuned as
data accumulates — but the structure and the per-signal logic is open.

## Signal: solunar timing

**What it captures.** Major and minor feeding periods derived from the
moon's position: moon overhead, moon underfoot, moonrise, moonset.

**Why it's in the model.** The solunar theory has a mixed academic
record but a strong practitioner record. Pelagic predator activity
spikes during major periods are well-documented across reef fish,
billfish, and inshore species. The signal is not load-bearing on its
own — it's one input among many — but ignoring it would be wrong.

**How it's used.** Time-distance from the nearest major or minor
period maps to a positive contribution that decays with hours from the
period centre. Major periods contribute roughly twice the magnitude of
minors.

## Signal: barometric pressure trend

**What it captures.** The 24-hour change in surface barometric
pressure, in hectopascals.

**Why it's in the model.** Falling pressure ahead of a weather change
is the most-cited "feeding spike" condition in fishing literature.
Stable high pressure suppresses activity in many species; rapidly
rising pressure after a front is associated with a 12–24 h lull.

**How it's used.** Pressure trend (24 h delta) maps to a contribution
that's positive for falling pressure (ahead of a system), neutral for
stable, and slightly negative for rapidly rising. The magnitude is
species-modulated — gamefish respond more strongly than bottom species.

## Signal: recent fronts

**What it captures.** Cold-front passage in the last 48 hours.

**Why it's in the model.** Cold fronts depress activity for ~6–24 h
post-passage, then often produce a strong "rebound" 24–36 h later as
fish resume feeding under the new stable pattern. The signal is partly
redundant with the barometric trend but captures the specific
post-front rebound timing better.

**How it's used.** A binary "front passed in last X hours" combined
with a continuous "hours-since" curve produces a contribution that's
negative for the first 24 h post-front and positive in the 24–48 h
window.

## Signal: tide stage

**What it captures.** Rising / falling / slack state, plus the rate of
water-level change.

**Why it's in the model.** Most species feed actively on moving
water — bait gets displaced, predators ambush, currents concentrate
forage. Slack tends to suppress activity. The relationship varies by
species and habitat: redfish work the falling tide on flats; tarpon
feed the incoming through inlets; bottom species have weaker tide
preferences.

**How it's used.** Per-species tide preference (encoded as a curve
over the tide cycle) is multiplied by the rate of water-level change.
Spring tides amplify the signal; neap tides damp it.

## Signal: water temperature versus species preference

**What it captures.** Current sea-surface temperature versus the
target species' published preferred range.

**Why it's in the model.** Warm-water species become lethargic in
cold water and vice versa. Activity drops sharply outside the
species-specific comfort band, and very sharply outside the survival
band.

**How it's used.** Distance between observed SST and the centre of the
species preference range maps to a smooth penalty. Within the
preference range, no penalty. Beyond it, an exponential drop-off to
zero contribution.

The species temperature ranges come from WoRMS (World Register of
Marine Species) and FishBase, with hand-curation for species where
those sources disagree.

## Signal: species presence (seasonal range)

**What it captures.** Whether the target species is plausibly present
in the area at the current season, based on iNaturalist / OBIS
observation density.

**Why it's in the model.** A spectacular bite-score for a species that
isn't in the area is a useless number. We treat species presence as a
gating signal: if the seasonal range overlap is below a threshold, the
score is suppressed and the app surfaces a "this species is
unlikely-to-be-present here / now" note.

**How it's used.** Presence is a multiplier on the rest of the score,
not a separate additive contribution. If presence is near zero, the
score collapses to near zero regardless of other signals.

## Signal: time of day

**What it captures.** Civil dawn, civil dusk, solar noon, midnight —
plus the sun-altitude curve through the day.

**Why it's in the model.** Most predatory species are crepuscular —
peak feeding within an hour or two of sunrise and sunset. Some species
(swordfish, certain pelagics) are night-feeders; others (mahi, dolphin)
are mid-day surface predators.

**How it's used.** A per-species diel-activity curve maps the current
time-of-day to a contribution. The bite-score for sailfish at solar
noon is structurally different from the bite-score for bonefish at
solar noon.

## Signal: wind and surface conditions

**What it captures.** Wind speed, gust, wave height.

**Why it's in the model.** Heavy chop suppresses surface and
near-surface feeding. Some species (snook, redfish in the surf zone)
actually feed *better* in moderate chop because bait is disoriented.

**How it's used.** Per-species "preferred wind" curve, mostly
penalising high wind, with some species-specific bumps for moderate
chop.

## What the model doesn't include (yet)

Honest list of signals we know matter but haven't integrated:

- **Forage abundance** — direct knowledge of where the bait is. We
  approximate with seasonal climatology; we don't have real-time
  data.
- **Recent fishing pressure** — heavy pressure on a spot suppresses
  activity for hours. Not modelled.
- **Sub-surface temperature gradient** — many species hold at the
  thermocline; surface SST is a poor proxy.
- **Lunar perigee / apogee** — peripheral solunar signal; not
  currently included.

These gaps are honest, not hidden. As we get data, the model gets
sharper.

## The composition

Once each signal computes its `(value, weight, reason)` triple, the
final score is:

```
score = (Σ value_i × weight_i) / (Σ weight_i)   × presence_multiplier
score = clamp(0, 10, score × 10)                 # scale to 0–10
```

Each contribution is visible in the breakdown. If you disagree with a
weighting, the
[methodology question template](https://github.com/Submarius/submarius/issues/new?template=methodology-question.yml)
exists for that.
