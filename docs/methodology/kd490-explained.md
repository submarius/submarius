---
title: Kd490 → Secchi — the optical theory
description: How satellite-derived attenuation coefficients translate to underwater visibility, and why the Lee 2015 model is the canonical relationship.
---

# Kd490 → Secchi: the optical theory

This page explains the single relationship at the heart of the
Submarius water-clarity model: the conversion from satellite-derived
diffuse attenuation coefficient (Kd490) to Secchi disk depth (a proxy
for "vertical visibility").

## What's Kd490?

The **diffuse attenuation coefficient at 490 nanometres** measures how
much downwelling blue-green light is absorbed and scattered per metre
of water column. Higher Kd = more attenuation = less light getting
through = lower visibility.

Units: m⁻¹ (per metre).

Typical values:

| Water type | Kd490 (m⁻¹) | Visual character |
|---|---|---|
| Open ocean (clear) | 0.02 – 0.05 | Deep blue, 30 m+ visibility |
| Coastal blue water | 0.05 – 0.15 | Blue-green, 10–25 m visibility |
| Coastal turbid | 0.15 – 0.5 | Green-grey, 3–10 m visibility |
| Estuarine / river-plume | 0.5 – 5+ | Brown-green, < 3 m visibility |

Satellites measure Kd490 by inverting the water-leaving radiance
spectrum — essentially, by reading the colour of the water from above
and modelling what attenuation would produce that colour.

Submarius pulls Kd490 from three independent sources (VIIRS-SNPP,
NOAA's VIIRS+OLCI multi-mission gap-filled product, GOES-16 ABI
processed via ACOLITE) and fuses them via inverse-variance weighting
in log-Kd space.

## What's Secchi depth?

The **Secchi disk** is a 30 cm white disk lowered into water until it
disappears from view. The depth at which it disappears is the **Secchi
depth (Z_SD)**. It's the oldest standardised measurement of water
clarity and the one divers most intuitively recognise.

Roughly: Secchi depth in metres ≈ vertical visibility a diver
experiences. Horizontal visibility is typically 1.5–2× Secchi depth in
clear water.

## The classical "1.7/Kd" rule

For decades the standard relationship was:

> Z_SD ≈ 1.7 / Kd(490)

This worked tolerably well for moderate-clarity coastal water but
broke at the extremes — too pessimistic for very clear blue water,
too optimistic for very turbid coastal/estuarine water.

## The Lee 2015 mechanistic model

[**Lee, Z. P. et al. (2015)** *Secchi disk depth: A new theory and
mechanistic model for underwater visibility*, Remote Sensing of
Environment 169, 139–149.](https://www.sciencedirect.com/science/article/pii/S0034425715300043)

Lee derived a new equation based on contrast detection by the human
eye at the **most transparent wavelength of the water type**, rather
than fixed 490 nm. The simplified form:

> Z_SD ≈ 1 / Kd_tr × ln(|r_t − r_w_pc| × R_d_pc / N_c_pc)

where Kd_tr is Kd at the transparent-window wavelength (which shifts
with water type — ~480 nm for clear blue ocean, ~550–580 nm for
turbid coastal water).

The logarithmic ratio is empirically nearly constant across diverse
water types: **2.38 ± 0.03**.

So the practical simplification:

> **Z_SD ≈ 2.38 / Kd_tr**

Validated against N = 338 globally distributed in-situ measurements
spanning 1 m to 30 m+ visibility:

> **R² = 0.96, ~18% mean absolute error.**

This is the strongest published physical relationship between
satellite-observable optical properties and human-perceived underwater
visibility. It's the foundation Submarius builds on.

## Wavelength shifts by water type

The "transparent window" — the wavelength at which water absorbs and
scatters light least — shifts with water composition. From Lee 2015
Figure 4:

| Z_SD | Transparent wavelength | Perceived water colour |
|---|---|---|
| 5 m | 540 nm | Green-yellow |
| 10 m | 530 nm | Green |
| 20 m | 510 nm | Blue-green |
| 40 m | 480 nm | Blue |

Practical implication: Kd at 490 nm (what the satellite measures most
directly) is most accurate for clear water. For coastal or turbid
water, Kd(490) underestimates the relevant attenuation, and a naive
2.38/Kd(490) calculation will over-estimate visibility.

This is why **Submarius uses an adaptive coefficient** — Case-1 vs
Case-2 — rather than a fixed value.

## Case-1 vs Case-2 water

Oceanographers classify water into two broad optical types:

- **Case-1** (open ocean, chlorophyll-dominated): the optical
  properties are determined primarily by phytoplankton. Lee's
  coefficient ≈ 2.38.
- **Case-2** (coastal, dominated by suspended sediment, CDOM, and
  river-borne material): the optical properties are dominated by
  inorganic particles and dissolved organics, not phytoplankton. The
  effective coefficient drops to ~1.5.

Most published consumer-facing visibility products use 2.38 everywhere
and silently overestimate coastal turbidity by 30–50%.

Submarius computes a **case-2 factor** from:

- Coastline-geometry enclosure class (open / semi-open / enclosed)
- Distance to coast
- Satellite chlorophyll-a (elevated → bloom-influenced → leans Case-2)
- Active HAB flag
- Freshwater detection from coastline data

…then blends coefficients:

```
coeff = (1 − case2_factor) × 2.38 + case2_factor × 1.5
Z_SD  = coeff / Kd
```

A Florida east-coast surf zone with Kd = 0.18 maps to Secchi ~8 m
(realistic) instead of the ~12 m a Case-1-everywhere model would
produce. The Bahamas deep with Kd = 0.04 maps correctly to ~60 m.

## Where Kd490 falls short

The relationship has known limits. We don't claim performance outside
them:

- **Optically shallow water.** When the seafloor is bright (sand, coral
  rubble) and the water is shallow, satellite-observed colour is
  contaminated by bottom reflectance. Kd retrievals there are biased.
  Submarius corrects this with the bathymetric cap — see the model
  page.
- **Adjacency effects within ~3 km of land.** Land reflectance bleeds
  into nearby water pixels, inflating apparent clarity. Submarius
  downweights satellite Kd in shore-adjacent cells when Kd suggests
  clear water (high-Kd readings are honest; low-Kd readings are
  suspect).
- **Severely turbid water (Kd > 1).** Both the linear approximation and
  the Case-2 calibration weaken. Submarius reflects this in the
  uncertainty band.
- **Subsurface features.** Thermoclines, haloclines, and lensed plumes
  are invisible to surface satellites. The model can't see them.

## Citations and further reading

- Lee, Z. P. et al. (2015). *Secchi disk depth: A new theory and
  mechanistic model for underwater visibility.* Remote Sensing of
  Environment 169, 139–149. [DOI](https://www.sciencedirect.com/science/article/pii/S0034425715300043)
- IOCCG (2000). *Remote sensing of ocean colour in coastal, and other
  optically-complex, waters.* Reports of the International
  Ocean-Colour Coordinating Group, No. 3.
- Mobley, C. (1994). *Light and Water: Radiative Transfer in Natural
  Waters.* Academic Press. The textbook on the underlying physics.

The Submarius blog post
[*how Kd490 satellite data predicts water clarity*](https://submarius.com/blog/how-kd490-satellite-data-predicts-water-clarity)
covers this material at lower technical density for a non-oceanographer
audience.
