---
title: iOS app
description: System requirements, install instructions, and iOS-specific behaviour.
---

# iOS app

## Install

[Download Submarius on the App Store.](https://apps.apple.com/app/submarius)

The web app at [submarius.com](https://submarius.com) works in Mobile
Safari and supports "Add to Home Screen" as a progressive web app, but
the native iOS build offers tighter integration: better background
behaviour, native widgets (planned), and lower battery cost during
buddy-GPS sessions.

## Requirements

- iOS 16.0 or later
- iPhone (iPad supported but not yet optimised; iPad-specific layouts
  are on the roadmap)
- Cellular or wifi for live data; the app degrades gracefully offline
  (see [features/offline-mode.md](../features/offline-mode.md))
- Location permissions for "While Using" — the app does not request
  background location except during an active buddy-GPS session

## What's iOS-specific

- **Push notifications** — verdict-change alerts at saved spots,
  shark-proximity alerts, buddy-GPS pairing requests
- **Native share sheet** — share a spot, a verdict, or a viz report
  via iMessage, Mail, or any installed share target
- **HealthKit dive logging** (planned) — write dive entries from the
  catch-log to Apple Health so they appear alongside Activity data
- **App Clip** for the public viz-report submission flow (no install
  required)
- **Lockscreen widgets** (planned) — today's verdict at a saved spot

## Privacy on iOS

Submarius's privacy posture is platform-agnostic, but iOS surfaces it
via the standard mechanisms:

- App Privacy label discloses the categories of data collected
  (account info, optional location during dive trips, anonymised usage)
- Location is requested as "While Using" only; background location is
  used solely for active buddy-GPS sessions and stops when the trip
  ends
- App Tracking Transparency: we don't track you across other apps and
  websites; we don't display the ATT prompt because we don't need it.

## Subscription management

Subscriptions (Pro monthly, yearly, lifetime) are managed through the
Apple App Store.

- Subscribe in-app via the standard StoreKit flow
- Restore purchases from Settings → Account → Restore
- Cancel anytime in **iOS Settings → Apple ID → Subscriptions**

## Bug reports from iOS

If you hit a bug, please file using the
[bug template](https://github.com/Submarius/submarius/issues/new?template=bug.yml).
Include the app version (Settings → About in-app) and the iOS
version. Attach screenshots if you can — please redact any precise
location data.
