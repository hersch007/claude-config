---
name: project-cpflex
description: "CPFlex (C-P Flexible Packaging) SEO client — folder, domains, pre-launch status, what's next"
metadata:
  type: project
---

C-P Flexible Packaging (CPFlex) SEO work lives in `SEO-Clients-Hub/clients/Garlock/`. Richard calls this engagement "Garlock SEO" — the folder is named for the Garlock/C-P merged brand (logo file is garlock-cp-logo.png). Source of truth for status is the two Word running logs in that folder, not the markdown work log.

**Domains (verified 2026-09-07):**
- New HubSpot build (portal 24024882): https://gcpflexpack-24024882.hs-sites.com — all SEO work is implemented here. NOT LIVE yet.
- Launch domain: gcpflexpack.com — registered May 12 2026, Cloudflare NS, currently a Directnic parking page. All schema/canonical targets in the audits assume this domain.
- Current live sites: https://www.cpflexpack.com (old WordPress C-P site) and https://www.garlockflexibles.com (old HubSpot Garlock site). Both need 301 maps into the new site at launch.

**Progress (verified on staging 2026-09-07):** all 77 site pages are implemented on staging. Word log fully reconciled 2026-09-07 (all 77 site pages implemented, incl. Pouches Glossary published by Richard that day). About Us and Pouches Glossary are marked "ready to bill". Homepage open fixes: hero alt + global logo alt. Blog (verified 2026-09-07): 24 of 31 articles have title/meta/BlogPosting schema live; 7 remain (#10, #17, #31 full; #5, #7, #30 title only; #15 meta) plus an internal-link pass. Blog Word log has a status line per article since 2026-09-07. Never rebuild the Word log from build-running-audit.js — the builder is stale (no billing lines); edit the docx XML in place.

**Reports:** Progress reports are prepared under the "Start Advertising" name, period Jun 24–Jul 28 2026. The Jul 28 report already queued a launch-day checklist (301s, sitemap, robots, GSC).

**Why:** Prior sessions assumed gcpflexpack.com was live production; it is not. Any "check the live site" task must use the hs-sites staging URL.
**How to apply:** For CPFlex tasks read the Word running logs first, verify against the staging site, and treat launch cutover as a separate open workstream. See [[feedback-dont-ask-billing]].
