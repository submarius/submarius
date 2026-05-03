---
title: Shark alerts
description: Tagged-shark proximity from OCEARCH plus crowd-sourced sightings. Permanently free.
---

# Shark alerts

A safety feature, permanently free. Two complementary signals:

1. **Tagged-shark proximity** from [OCEARCH](https://www.ocearch.org/),
   the long-running shark-tracking research organisation.
2. **Crowd-sourced sightings** posted by users in the app, time-bounded
   and verified.

## OCEARCH tracking

OCEARCH publishes positions for hundreds of acoustically- and
satellite-tagged sharks (white sharks, tiger sharks, bull sharks, mako,
hammerheads). Each tagged animal has a name, a tagging date, a length,
and a near-real-time position trail.

Submarius pulls the OCEARCH feed and shows:

- Tagged sharks within a configurable radius of the user
- Each shark's species, name, length, and last-ping time
- The full historical track for any selected animal
- Push notifications when a tagged shark transmits a position within
  range (configurable)

Caveats we surface to the user, because honesty about what the data
*is*:

- Tagged sharks are a tiny fraction of total population. *Absence* of a
  tagged shark in the area means nothing about the absence of untagged
  ones.
- Acoustic tags only ping when the animal surfaces; gaps of hours to
  days are normal. A "last seen 6 hours ago" position is the *known*
  position, not the *current* one.
- The species mix in OCEARCH's tagged population skews toward the species
  they study — large pelagic sharks. Smaller coastal species are
  under-represented.

## Crowd-sourced sightings

Users can post a sighting from the app: species (or "unknown shark"),
estimated length, behaviour, photo. Sightings carry a time-decay weight
and are deduplicated by H3 cell — multiple users seeing the same animal
in the same cell within minutes are aggregated.

Like Waze for sharks: the more users in an area, the more accurate the
data.

## Permanently free

Shark alerts (both OCEARCH and crowd-sourced) are part of the free
safety tier and are not gated by Pro subscription. Per the
[data-honesty principles](../methodology/data-honesty.md), safety
features must be available to every user regardless of payment.
