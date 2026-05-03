---
title: The GO/NO/CAUTION verdict
description: Submarius' single per-day decision output, computed from the activity-relevant subset of the marine forecast.
---

# The verdict

Most marine apps hand you ten dashboards. Submarius runs the integration
step itself and gives you one of three outputs per dive day:

- **GO** — conditions favourable for the activity at the chosen
  location and time
- **CAUTION** — conditions workable but with notable risks or
  uncertainty; the breakdown explains what to watch
- **NO** — conditions unsuitable; the breakdown explains why

This is the verdict-first product philosophy:
*[a verdict, not a dashboard](https://submarius.com/blog/a-verdict-not-a-dashboard)*.

## How it's computed

The verdict is **activity-aware**. The same conditions can be GO for
fishing and NO for spearfishing, because the activities care about
different signals:

| Activity | Dominant signals |
|---|---|
| Spearfishing / freediving | Water clarity, current, swell direction, surface chop, water temperature |
| Scuba diving | Surface conditions for entry/exit, current, water temperature, swell penetration vs site depth |
| Fishing (boat) | Wind, wave height, fronts, tide stage, bite-score window |
| Fishing (shore / wading) | Wind, surf, tide stage |
| Boating | Wind, wave height, fog, fronts, lightning risk |

For each activity, Submarius computes a per-signal traffic-light state
(green / yellow / red) and combines them into the overall verdict using
hand-tuned activity-specific rules — *not* a single weighted-sum score
that treats a 30-knot wind the same as a missing tide table.

## Why one verdict instead of ten numbers

Two reasons:

1. **Decision quality.** When the user has to integrate ten numbers
   themselves, they integrate inconsistently and slowly, and they pick
   up on whichever number happens to be most salient that morning. A
   pre-integrated verdict is what an experienced captain would tell you
   if you called them.
2. **Accountability.** A model that just shows ten numbers is never
   wrong. A model that says "GO" and then delivers two-foot visibility
   is wrong, and we can measure how often it's wrong, calibrate against
   reports, and improve. Saying nothing is the easy way out.

## When the verdict is suppressed

- **Inland points.** Submarius is a coastal product; we don't compute
  verdicts for points far from any coastline.
- **Insufficient data.** If the satellite coverage is too stale, the
  forecast horizon too far out, or the location lacks a regional
  baseline, the app surfaces *"insufficient data"* rather than guess.
  Per the data-honesty principle: never claim what we don't measure.
- **Active hazards.** Lightning risk above threshold, hurricane warning
  in effect, harmful-algal-bloom severity above 2 — these short-circuit
  to NO regardless of the rest of the forecast.

## Drilling in

Tap the verdict to see the per-signal breakdown. Tap any signal to see
the source data and the model's reasoning. Nothing is hidden; every
factor is reachable in three taps.
