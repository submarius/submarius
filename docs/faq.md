---
title: FAQ
description: Common questions about Submarius — what it does, how the model works, what it costs, and what it doesn't do.
---

# FAQ

## The product

### What is Submarius?

A mobile-first ocean-conditions app for spearos, divers, anglers, and
boaters. It aggregates marine forecasts, satellite ocean colour, river
discharge, tides, and harmful-algal-bloom bulletins into a single GO /
NO / CAUTION verdict per dive day, with every input visible. See
[overview](overview.md).

### Where can I get it?

[App Store for iOS](https://apps.apple.com/app/submarius), or in any
modern browser at [submarius.com](https://submarius.com) (installable
as a PWA).

### Is it free?

The free tier includes today and tomorrow's forecast, current
water-clarity estimate, current verdict, tides, all safety features
(SOS, buddy-GPS, shark alerts), spot library with E2EE backup, catch
logging, and the offline base map. Pro adds the 7-day forecast, all
the overlay layers, multi-spot trip planning, push notifications, and
higher upload quotas. Pricing is on
[features/pro-vs-free.md](features/pro-vs-free.md).

### What regions does it cover?

Globally. The model uses ocean and weather inputs that have global
coverage (Open-Meteo, GEBCO, OpenStreetMap coastlines, satellite
ocean colour). Some signals are US-only (USGS river discharge, NOAA
HAB Forecast, NOAA Tides & Currents native). Outside those, the
relevant components are absent and the model runs without them — the
uncertainty band widens accordingly.

## Water clarity

### How does the visibility forecast actually work?

Three layers: a physical model anchored to satellite ocean colour
(Lee 2015 Kd → Secchi with adaptive Case-1/Case-2 coefficient), with
weather-driven penalties (rain, wind, tide, river plumes, HAB);
crowd-sourced viz reports that pin recent estimates and calibrate the
model over time; and a statistical refinement layer in regions with
enough report density. Full detail at
[methodology/water-clarity-model.md](methodology/water-clarity-model.md).

### How accurate is it?

The underlying Lee 2015 model achieves R² = 0.96 against 338 in-situ
measurements globally. Submarius's specific implementation adds
weather-driven penalties and adjustments — some of which improve
accuracy, some of which trade absolute accuracy for honest uncertainty
quantification.

The honest answer is: it depends on the location and the recency of
the data. Where satellite is fresh and reports are nearby, the
estimate is tight. Where inputs are stale or reports absent, the
uncertainty band is wide and the app says so.

### Why does the band sometimes get really wide?

Because the model is being honest about uncertainty. When satellite
coverage is stale (multi-day cloud cover, sensor outage), the
oceanographic anchor is weaker and the band reflects that. When
weather inputs disagree with each other, the band reflects that too.
Tight bands when sources agree, wide bands when they don't.

### Why doesn't it work for [my favourite dive site]?

If a site is hyperlocal — a specific cove, a specific reef on the back
side of a specific island — the satellite resolution (2–4 km) can't
distinguish it from the surrounding water. Reports help bridge that
gap. The more reports a region accumulates, the better the model
performs there.

If the site is genuinely uncovered (inland, freshwater, beyond the
forecast horizon), the app says so rather than guess.

### What about Case-2 / coastal turbid water?

Submarius computes a Case-1/Case-2 blend factor from coastline
geometry, distance-to-coast, satellite chlorophyll, and active HAB
flags. Most products use Case-1 everywhere and overestimate coastal
turbidity by 30–50%. We don't. See
[methodology/kd490-explained.md](methodology/kd490-explained.md).

## Bite score

### Is the bite score "AI"?

No. It's a transparent composition of biological and environmental
signals (solunar, barometric trend, fronts, tide, water temperature
versus species preference, species presence, time of day, wind). Each
contribution is visible. See
[methodology/bite-score-signals.md](methodology/bite-score-signals.md).

### Why is the bite score per-species?

Because species differ. Redfish work the falling tide on flats,
tarpon feed the incoming through inlets, snapper bite a different
moon than mahi. A score that treats all fish the same is wrong half
the time.

## Safety features

### SOS, buddy-GPS, and shark alerts are free? Forever?

Yes. The entitlement middleware in our backend explicitly bypasses
subscription checks for safety endpoints. Permanent product policy.
See [features/safety.md](features/safety.md).

### What about Garmin inReach?

Hand-off to inReach satellite SOS for off-grid scenarios is planned
but not shipped. Requires a Garmin partner integration.

## Privacy

### Are my dive spots private?

By default, yes — completely private to your device. When you choose
to share, you pick the privacy level (buddy-only, public-fuzzed at
~5 km cell resolution, or public-exact for the rare "share my spot
with the world" case).

### Can Submarius see my exact dive spots?

If they're private or buddy-shared: no, they're on your device only
(or sent encrypted to your buddy). If they're public-fuzzed: no, only
the H3 cell is sent — never the precise coordinates. The fuzzing
happens on-device before the data leaves. Even with a court order,
Submarius can't recover the precise points of fuzzed spots. See
[features/privacy.md](features/privacy.md).

### Does the app track me in the background?

No, except during an active buddy-GPS session (when you've explicitly
paired with a buddy). Outside of that, location is "While Using"
only.

### Do you sell my data?

No. We don't sell data, share with advertisers, or run ad SDKs. The
business model is the Pro subscription.

## Technical

### Do you have a public API?

Yes — `api.submarius.com`. The API is documented and available for
non-commercial use. Commercial integrations (B2B partners, research
collaborations, white-label) are case-by-case. Get in touch via the
contact link on [submarius.com](https://submarius.com).

### Is Submarius open source?

The documentation, methodology, FAQ, and brand assets in this
repository are open-licensed (CC BY 4.0). The application source code
is not open source.

### Why is the source closed?

Two reasons. First, the model is the work that funds the company —
publishing the calibration tuning would let competitors clone the
product without doing the underlying calibration work. Second, the
infrastructure (data ingest pipelines, caching, ACOLITE processing,
etc.) is operationally non-trivial and we'd rather not maintain it as
a public-facing project on top of running it for users.

The methodology is open. The art is in the calibration.

### Can I run my own instance?

No, sorry. Submarius runs as a hosted service.

### Why is there no Android app yet?

Capability rather than priority. The web app at submarius.com works
on Android and is installable as a PWA, which covers the main use
cases. A native Android build is plausible future work, not committed.

## Business

### Is Submarius a venture-backed startup?

Submarius is independent. We're not seeking outside investment at
this time.

### How do I contact you?

Use the contact link on [submarius.com](https://submarius.com).
Security disclosures via [SECURITY.md](../SECURITY.md). General
discussion at
[Discussions](https://github.com/Submarius/submarius/discussions).
Bugs via the [bug template](https://github.com/Submarius/submarius/issues/new?template=bug.yml).

### Are you hiring?

When we are, it'll be on the website. Not currently.
