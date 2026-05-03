---
title: Safety features
description: SOS, dive-buddy GPS sharing, Plus-Code rescue location, OCEARCH alerts. Permanently free, always.
---

# Safety features

Submarius treats safety features as ethically off-limits to the
subscription business model. They are permanently free — not "free
trial", not "free-tier with limits", not "free for one feature, paid
for the rest". Free.

## The features

### SOS

Press-and-hold the SOS button to fire an emergency signal. Submarius:

1. Captures your current GPS position (with horizontal accuracy)
2. Computes a [Plus Code](https://plus.codes/) for the position — an
   open-standard, short, human-readable rescue location that works
   without street addresses
3. Sends a message to your designated emergency contacts via push
   notification (if they have the app), SMS (fallback), and email
4. Optionally fans out to the local Coast Guard / lifesaving service
   (where a published intake exists)

The rescue message includes your name, position, Plus Code, time of
SOS, and (if known) your dive activity, last buddy position, and
duration submerged.

### Dive-buddy GPS sharing

Two divers can pair their phones for a single trip. While paired, each
phone broadcasts its position to the other every 30 seconds via a
real-time WebSocket. The pairing ends when either diver ends the trip
or 12 hours pass, whichever comes first.

If a dive overruns expected duration, the surface buddy is alerted with
the last-known position. If a phone goes offline, the other side sees
the gap and the time of last contact.

### Plus Code rescue location

Plus Codes (Open Location Code) are a Google-developed, open-source,
short alphanumeric code that uniquely identifies any 14×14 m square on
Earth. They're more compact than lat/lon, easier to transcribe than
DMS coordinates, and don't require a street address.

Every Submarius position output (SOS, shared spots, catch logs) shows
the Plus Code alongside the conventional coordinates. Coast Guard
operators can decode them with publicly available tools.

### Tagged-shark and crowd-sourced shark proximity

Covered separately in [shark-alerts.md](shark-alerts.md). Free, free,
free.

### Garmin inReach satellite SOS (planned)

For users with a Garmin inReach satellite communicator, Submarius will
hand off the SOS to inReach for satellite delivery when cellular
coverage is unavailable. This requires a Garmin partner integration and
is on the roadmap, not yet shipped.

## Why these are permanently free

Three reasons:

1. **They might save someone's life.** That's not a feature to gate
   behind a $99/year subscription.
2. **Network effects.** Buddy-GPS and crowd-sourced shark sightings get
   better the more users participate. A paywall on participation
   defeats the value.
3. **Trust.** The brand promise of Submarius is that the app is honest
   about what it knows and what it doesn't. Charging for an SOS button
   would be inconsistent with that promise.

The relevant entitlement middleware in the Submarius backend
explicitly bypasses subscription checks for safety endpoints. This is
permanent product policy, not a marketing free-trial period.

## What safety features *can't* do

Plain about the limits:

- Submarius is not a replacement for professional dive training, an
  experienced surface buddy, or proper equipment.
- Cellular/wifi coverage is required for SOS and buddy-GPS; satellite
  hand-off via Garmin is the planned mitigation but isn't shipped.
- The OCEARCH tagged-shark feed reflects only tagged animals; absence
  isn't presence-of-nothing.
- Plus Codes don't replace the human Coast Guard infrastructure — they
  make it faster.
