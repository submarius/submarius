---
title: Offline mode
description: Submarius is built to keep working when the boat wifi drops. Offline base map, cached conditions, queued mutations.
---

# Offline mode

Boats lose signal. Submarius is designed for that — every endpoint
degrades gracefully and the most-used features keep working with no
connectivity.

## Offline base map

Submarius downloads a bathymetry-only vector tile pack for any region
you mark as "offline area" before you leave the dock. The pack
includes:

- Bathymetric contours and depth-shaded raster
- Coastline and shoreline detail
- Channel markers and navigational aids (where in OSM)
- Reefs, wrecks, and structure
- Marine protected areas

Deliberately *not* included: street-level OSM POI render. The marine
chartplotter aesthetic is intentional — when you're a mile offshore,
seeing every restaurant in the nearest harbour is noise.

Tiles are stored in Dexie (IndexedDB) on the client and persist between
app launches. There's no separate "download for offline" flow per
trip — once a region is marked offline, it stays available.

## Cached conditions

The last successful fetch of conditions (forecast, tide, clarity,
verdict) for any location you've viewed is cached on-device with a
freshness timestamp. When offline, the app shows the cached value with
a clear *"last updated 4 hours ago"* indicator.

Critical: the verdict is *recomputed* on-device from the cached inputs,
not just shown statically. So if the cached forecast says wind has
dropped from 25 to 8 knots over the next six hours, and you scroll
through the day, the verdict updates as if you were online.

## Mutations queued for sync

Anything you do that would change server state is queued locally and
replayed when connectivity returns:

- Catch logs
- Post-dive viz reports
- Shark sightings
- Spot edits
- Buddy-GPS position broadcasts

The queue lives in Dexie (`syncQueue`) and is processed FIFO on the
next successful network round-trip. Conflicts (e.g. you edited a spot
offline that your buddy also edited) are resolved last-write-wins for
simple fields and merged for collections.

## What requires connectivity

Plain about what doesn't work offline:

- **SOS**. Cellular or wifi is required to deliver the emergency
  message. Garmin inReach hand-off (planned) is the mitigation.
- **Live buddy-GPS**. Both ends need network for the WebSocket.
- **Live shark sightings**. Crowd-sourced data needs the round-trip.
- **First-time view of a new location.** If you've never viewed
  conditions for a spot, there's nothing cached to show.
- **Live tile updates** (radar, SST, GHRSST overlay). The base map
  works; the live overlays don't.

## Why this matters

Boat connectivity is unreliable in the geographies Submarius is most
useful in. A nearshore-only app that fails the moment you cross the
inlet is a bad app. The offline-first architecture is one of the
project's foundational design principles, not a polish item.
