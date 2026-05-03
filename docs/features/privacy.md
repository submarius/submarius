---
title: Privacy and spot protection
description: How Submarius protects dive-spot locations, supports end-to-end encrypted backup, and stays out of your business.
---

# Privacy and spot protection

Spearos and divers do not casually share spot coordinates. Submarius
treats that with appropriate paranoia.

## Spot privacy modes

Every spot in the app has a visibility setting:

| Mode | Who sees the precise coordinates |
|---|---|
| **Private** (default) | Only you, on your device |
| **Buddy** | You and divers you've explicitly paired with |
| **Public, fuzzed** | Visible on the discover map at H3 cell resolution (≈ 1 km) — never the precise point |
| **Public, exact** | Anyone who can see the spot (rare; opt-in) |

The default is private. There is no opt-out from privacy by accident —
making a spot public is a deliberate action.

## H3-fuzzed coordinates everywhere

When a spot or a dive event is shared (with a buddy, with the public
discover map, with a viz report), the coordinates are quantised to an
H3 cell on the device, before the data is sent to Submarius.

The implication: **even with full database access (or a court order),
Submarius itself cannot recover the precise coordinates of a fuzzed
spot.** This is enforceable engineering, not a policy promise.

H3 resolution levels used:

- **Discover map**: H3 resolution 7 (~5 km cell)
- **Viz reports for the clarity model**: H3 resolution 9 (~150 m cell)
- **Buddy-GPS sharing during an active trip**: precise coordinates,
  buddy-only, ephemeral (not persisted server-side)

## End-to-end encrypted backup

Optional. When enabled, your spot library, catch log, and dive history
are encrypted on the device with a key derived from your account
passphrase, then uploaded to Submarius for backup. Submarius servers
hold ciphertext; the key never leaves your device.

Recovery: your passphrase decrypts the backup on a new device. Lose the
passphrase, lose the backup — Submarius cannot recover it for you. (We
also can't be compelled to.)

Implementation summary:
- Symmetric key derived via Argon2id from the passphrase plus a
  per-account salt
- AES-GCM-256 encryption of the spot/catch payload
- Per-record nonces; nonce reuse impossible
- Key rotation supported via re-wrap on passphrase change

The crypto is locked — no plans to change algorithms or parameters
without a hard-coded version bump and explicit user consent.

## What we collect

The minimum viable set:

- **Account**: email (for login and recovery), display name (optional),
  passkey or passphrase
- **Subscription state**: tier, billing identifier from Apple
- **Usage**: anonymised analytics on which features are used, no
  per-user tracking
- **Spots and catch logs**: only what you've created, with the privacy
  level you chose
- **Crash reports**: stack traces, no PII

We do not sell data. We do not share with advertisers. We do not have
ad SDKs in the app.

## What we don't collect

- No precise location history beyond what's needed for an active
  buddy-GPS session (which is ephemeral)
- No contact list scraping
- No social-graph correlation across users
- No background location when the app isn't actively in use

## GDPR / CCPA

Right of access, right of deletion, right of portability — supported.
Email the privacy team via the contact link on
[submarius.com](https://submarius.com).

## Source

The privacy posture above is implemented in the app and backend; it
isn't a wish list. The user-facing copy at
[submarius.com/privacy](https://submarius.com/privacy) is the legal
canonical version. The blog post
[*your spots, your keys*](https://submarius.com/blog/your-spots-your-keys)
walks through the design rationale.
