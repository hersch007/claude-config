---
name: reference-parts-of-practice-brand
description: "Confirmed Parts of Practice brand colors and contact email, for SEO-Clients-Hub client deliverables issued under that entity."
metadata: 
  node_type: memory
  type: reference
  originSessionId: 0e6f26ce-cedb-49f1-b2a0-6d12cf84d2c4
  modified: 2026-09-14T12:55:55.741Z
---

**Parts of Practice** (one of Richard's four SEO provider companies in [SEO-Clients-Hub](C:\Users\richa\Documents\Claude Projects\SEO-Clients-Hub) — see [[feedback-seo-provider-always-ask]]):

- **Primary color:** navy `#003366`
- **Accent color:** amber `#f59e0b`
- **Contact email:** `Richard@PartsofPractice.com` (confirmed 2026-09-14 — an earlier guess of `PartsofPractice@gmail.com` seen in an older client file, `WanderingFreeCounseling/WFC-SEO-Pricing-2026-09-01.html`, is NOT the one to use going forward)

Confirmed from three sources: the live page at partsofpractice.com/therapy-seo-services-local-seo-therapists/, `clients/WanderingFreeCounseling/WFC-SEO-Pricing-2026-09-01.html`'s CSS (`--navy: #003366; --amber: #f59e0b`), and `clients/Karen Hubbars Therapy/SEO and Web Pricing.pdf`.

**Why this memory exists:** On 2026-09-14, before finding these sources, Claude invented placeholder colors (red `#D7263D` / teal-aqua `#0E6E76`) for Parts of Practice in `seo-tool/audit.js`'s `PROVIDERS` map and baked them into Joe Welch Photography's Master Report, Prospect Report, and crawler HTML audit — all wrong. Corrected after the user caught it and said "Use Richard@PartsofPractice.com, fix all 4 documents."

**How to apply:** Use navy `#003366` / amber `#f59e0b` and `Richard@PartsofPractice.com` for any new Parts of Practice-branded deliverable (audit reports, docx reports, pricing flyers) rather than guessing from a color-name description alone — check `seo-tool/audit.js`'s `PROVIDERS['3']` first, since it should stay in sync with this.
