# Security policy

## Reporting a vulnerability

If you've found a security issue in the Submarius app, web app, or API,
**please don't file a public GitHub issue.** Report it privately so we can
fix it before disclosure.

Send the report to **security@submarius.com** with:

- A description of the issue and its impact
- Steps to reproduce, ideally including a proof-of-concept
- Your assessment of severity (low / medium / high / critical)
- Whether you'd like public credit when the fix ships

You can also use GitHub's private vulnerability reporting on this repo:
**Security → Report a vulnerability** in the top navigation.

## What to expect

- Acknowledgement within **3 business days**.
- An initial assessment and severity classification within **7 days**.
- A target patch window communicated within 14 days. Critical issues
  ship as fast as a build can be cut and reviewed (App Store review
  is the long pole — typically 1–3 days).
- Public credit in the changelog after the fix is live, if you want it.

## Scope

In scope:

- The Submarius web app at `submarius.com`, `*.submarius.com`
- The iOS app distributed via the App Store
- The public API endpoints under `api.submarius.com` and `submarius.com/api`

Out of scope:

- Denial-of-service attacks. Please don't run them.
- Issues in upstream services we depend on (NOAA, USGS, Open-Meteo,
  OCEARCH, etc.). Report those to the respective providers.
- Reports generated solely by automated scanners with no demonstrated
  exploitability.
- Social-engineering attacks against Submarius staff.

## Out-of-band assurances

- Submarius does not run a paid bug-bounty program at this time.
  Coordinated disclosure earns public credit, our gratitude, and as much
  back-and-forth as you'd like during the fix.
- We will not pursue legal action against good-faith security researchers
  who follow this policy.
