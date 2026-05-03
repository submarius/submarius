---
title: Web app
description: Submarius runs in any modern browser as a progressive web app.
---

# Web app

[submarius.com](https://submarius.com) is the web version of Submarius.
It's a full-featured progressive web app — every feature from the iOS
build is available, with the browser-specific caveats below.

## Browser support

- **Chrome / Edge / Brave** (Chromium-based) — full support, current
  version
- **Safari** (macOS and iOS) — full support, current version
- **Firefox** — full support, current version
- **Older browsers** — degrades. Map rendering may fall back to a
  raster tile path; some Pro overlays may be disabled.

The minimum target is "browsers shipped in the last 18 months". We
don't support IE, Edge Legacy, or pre-2023 Safari.

## Install as a PWA

The web app supports the standard PWA install prompt. On Mobile Safari
use **Share → Add to Home Screen**; on Chrome use the install icon in
the address bar. Once installed, the app launches full-screen with no
browser chrome and behaves close to a native app — the offline base
map, cached conditions, and queued mutations all work.

## Web-specific differences from iOS

- **Push notifications** — supported on Chrome and the modern Web Push
  flow; supported on iOS Safari 16.4+ as part of the standard. Older
  versions don't support web push.
- **Background location** — limited; web apps lose location access
  when not in the foreground tab. Buddy-GPS sessions on web work
  while the tab is active and slow when the tab backgrounds.
- **App Store payments** — the web app uses a non-Apple payment
  processor for Pro subscriptions; the iOS app uses StoreKit. Pro
  subscriptions are interchangeable — purchase on web and the
  benefits unlock on iOS via the same account, and vice versa.
- **HealthKit / Garmin native integrations** — not available on web.

## Offline on web

The PWA supports an installable offline pack via the same mechanism as
iOS — Dexie (IndexedDB) for tile and conditions storage, with a
Service Worker handling cache-first delivery. See
[features/offline-mode.md](../features/offline-mode.md) for what works
and what doesn't.

## Privacy on web

Same posture as iOS — see [features/privacy.md](../features/privacy.md).
Browser specifics:

- We don't use third-party analytics (no Google Analytics, no Segment,
  no Mixpanel). Anonymous first-party usage logging only.
- We don't run ad SDKs.
- We use one-time, single-purpose cookies for session and CSRF; no
  cross-site tracking cookies.

## Bug reports from web

[Bug template](https://github.com/Submarius/submarius/issues/new?template=bug.yml).
Include browser, browser version, OS, and a screenshot if possible —
again, please redact location markers.
