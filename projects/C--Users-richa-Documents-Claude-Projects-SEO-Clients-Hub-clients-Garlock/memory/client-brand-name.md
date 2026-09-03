---
name: client-brand-name
description: "The Garlock client's correct brand name for deliverables is \"C-P Flexible Packaging\", not Garlock"
metadata: 
  node_type: memory
  type: project
  originSessionId: e0b2c77d-0249-409d-b366-00b43286a689
---

The client folder is named "Garlock" and the system framing calls it "Garlock Flexibles", but the **correct company/brand name to use in all deliverables (page titles, meta descriptions, image alt text, schema provider, document branding) is "C-P Flexible Packaging"** — confirmed by the user on 2026-06-24.

The live site (gcpflexpack-24024882.hs-sites.com) brands itself "C-P Flexible Packaging". The logo asset filename is `garlock-cp-logo.png` (Garlock + C-P), which is why the relationship is easy to confuse — but for customer-facing SEO copy, write "C-P Flexible Packaging".

**Why:** Early audits (Aerospace, Autoclave, Bags, Laser Die-Cut, Child-Resistant) were written with "Garlock"/"Garlock Flexibles" and had to be corrected across the Word running log, the Excel tracker, and the per-page .md files.

**How to apply:** Never put "Garlock" in deliverable copy. "C-P Flexible Packaging" is 22 chars, so it does NOT fit in a ≤70-char SEO title alongside keywords — keep titles brandless. **Production domain confirmed (2026-06-24): `gcpflexpack.com`** — use it in all schema URLs (staging is `gcpflexpack-24024882.hs-sites.com`, which is noindex so schema there is unseen). Confirm each page's path is unchanged at launch before relying on the schema `url`. See [[garlock-seo-deliverable-workflow]].
