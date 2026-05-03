---
title: Press kit
description: Logos, screenshots, founder bio, boilerplate copy. Free to use under CC BY 4.0 with attribution.
---

# Press kit

Everything in this page is free to use under
[CC BY 4.0](../../LICENSE) with attribution to Submarius.

For interview requests, fact-checking, or anything not covered here,
use the contact link on [submarius.com](https://submarius.com).

## Boilerplate (one paragraph)

> Submarius is a mobile-first ocean-conditions app that gives spearos,
> divers, anglers, and boaters a single GO / NO / CAUTION verdict per
> day instead of forcing them to integrate ten dashboards themselves.
> The product is built around a physically-grounded water-clarity
> forecast — the only consumer app that fuses three independent
> satellite ocean-colour sources (VIIRS, GOES-16 ABI, multi-mission
> DINEOF) and converts them to underwater visibility via the published
> Lee 2015 mechanistic model. Submarius runs as a native iOS app and
> as a progressive web app at submarius.com. Safety features (SOS with
> Plus Code rescue location, dive-buddy GPS sharing, OCEARCH
> tagged-shark alerts) are permanently free regardless of subscription
> tier.

## Boilerplate (one sentence)

> Submarius is the marine-conditions app that tells you when to go,
> with a physically-grounded water-clarity forecast and permanently
> free safety features.

## Boilerplate (tagline)

> A verdict, not a dashboard.

## Logos and brand assets

Located in [`assets/brand/`](../../assets/brand/) at the repository
root:

- `icon-512.png` — App icon, 512×512 PNG
- `icon-192.png` — App icon, 192×192 PNG
- `apple-touch-icon.png` — iOS-style rounded icon
- `feature-graphic.png` — Banner graphic
- `og-default.png` — Open Graph card (1200×630)
- `submarius.avif` — Brand image
- `favicon-96.png` — Favicon, 96×96 PNG

Use of the Submarius name and logo for editorial purposes (news
articles, reviews, podcast cover art, etc.) is permitted without
prior approval. Use of the name or logo to imply endorsement,
partnership, or affiliation requires written permission.

## Screenshots

Product screenshots will be added to [`assets/screenshots/`](../../assets/screenshots/)
as the screenshot review settles.

For up-to-date App Store imagery, see the
[App Store listing](https://apps.apple.com/app/submarius).

## Quick facts

| | |
|---|---|
| **Product** | Submarius |
| **Category** | Marine conditions, water-clarity forecast, dive planning, fishing intelligence |
| **Platforms** | iOS (native), Web (PWA) |
| **Coverage** | Global (with US-specific signals — USGS rivers, NOAA HAB, NOAA Tides — adding precision in the US) |
| **Pricing** | Free tier with all safety features. Pro: $12.99/mo, $99/yr, $149 lifetime |
| **Launched** | 2026 |
| **Founded by** | Independent founder; not venture-backed |
| **Website** | https://submarius.com |
| **Docs / methodology** | https://github.com/Submarius/submarius |

## What's distinctive

If you're writing about Submarius, the things that genuinely
differentiate the product:

1. **Multi-satellite ocean-colour fusion** with the inverse-variance
   maths to back the uncertainty bands. No other consumer marine app
   fuses three independent sensors.
2. **Hourly geostationary clarity updates** via GOES-16 ABI processed
   through ACOLITE. Most clarity products are minimum 1-day stale.
3. **Adaptive Case-1 / Case-2 coefficient** in the Kd → Secchi
   conversion. Most products use 2.38 everywhere and silently
   overestimate coastal turbidity.
4. **Honest uncertainty bands.** The width of the band reflects actual
   model confidence; we don't artificially narrow it to look more
   confident.
5. **Bathymetric cap.** A 4 m bottom doesn't get a 25 m visibility
   estimate, regardless of what the satellite says about the surface.
6. **Permanently free safety features.** SOS, buddy-GPS, OCEARCH
   tagged-shark proximity, crowd-sourced sightings — never gated.
7. **Privacy-by-engineering.** User dive spots are H3-quantised on
   device before leaving; even Submarius can't recover precise
   coordinates of fuzzed spots.
8. **No black-box AI.** Every number in the app is composed from
   visible signals. Tap the verdict, see the breakdown; tap a signal,
   see its source.

## Suggested questions for an interview

- *Why build verdict-first instead of dashboard-first?*
- *How did you settle on Lee 2015 as the foundation?*
- *Why are safety features permanently free, even when free users
  cost you money?*
- *What's the data-honesty principle and where did it come from?*
- *What's the biggest single misconception about marine forecasting?*
- *What does the model do when it doesn't know?*

## What we won't do for press

- Custom screenshots that depict false data
- Endorsements or "Submarius approves of [other product]"
- Speaking on behalf of users we don't represent
- Comments on competitor products beyond the published comparison
  pages on submarius.com

## Contact

Use the contact link on [submarius.com](https://submarius.com).
Response time: 24–48 hours for press inquiries.
