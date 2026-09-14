---
name: project-fruth
description: "Fruth Custom Packaging SEO client (fruth.com, HubSpot CMS) — current implementation status, key files, and the stale-tracker lesson from 2026-09-14"
metadata: 
  node_type: memory
  type: project
  originSessionId: 9ce4a646-9100-4804-9ba3-e161977b9cca
  modified: 2026-09-14T12:32:56.830Z
---

Fruth Custom Packaging (fruth.com) is a Start Advertising / GroupRB SEO client, separate from the CPFlex/[[project-cpflex]] and Garlock Sealing clients despite naming overlap. Site is on HubSpot CMS; no HubSpot portal access has been confirmed for this client (checked 2026-09-11 via connected browser — only Garlock Flexibles, Sport Medical Technology, and Tri-CoGo portals were reachable).

**Key files (all in `SEO-Clients-Hub/clients/Fruth/`):**
- `WORK-LOG.md` — internal hours log, billed in 15-min increments (see [[feedback-fruth-billing-increments]])
- `FCP-SEO-AUDIT-2026-08-31.md` — hand-written audit (63/100). Its central claim ("meta descriptions missing on all 6 main pages") was wrong — built via WebFetch, which doesn't reliably see `<head>` meta tags.
- `Fruth-Custom-Packaging-SEO-Audit-2026-08-31.html` — automated crawler dashboard (91/100), source of truth for title/meta/schema/alt presence.
- `FRUTH-IMPLEMENTATION-PACKET.md` — copy-paste packet of drafted page content, now annotated with "ALREADY LIVE — DO NOT PASTE" / "BLOCKED — 404" warnings after the 2026-09-14 correction.
- `pages-data.js` + `gen-running-log.js` — regenerate `FCP-On-Page-SEO-Audit-Running-Log.docx` (the client-facing backup doc, mirrors CFB's pattern). Edit the data file, then `node gen-running-log.js` from the Fruth folder.
- `content-creation/CE_Fruth_*.docx` — the original 38 drafted page-copy files (body expansion + FAQ schema), built in an earlier session (~2026-09-03).

**Critical lesson (2026-09-14):** `content-creation/build-client-docs.js` has a hardcoded `implemented` dict claiming only 14 of 38 drafted pages were live. A direct live-site check (no HubSpot login needed — public pages) found **9 more pages were already live** that the tracker never recorded, plus 1 page (`/products/bags/esd-packaging`) that 404s — the draft targets a URL that doesn't exist. **Never trust that tracker without a live re-check.** Verified status as of 2026-09-14: 25 pages live, 14 genuinely pending, 1 blocked.

**How to apply:** Before handing over any more "pending" page content from the packet, confirm its current status in `pages-data.js` (or re-check live if it's been a while) — don't assume the packet's per-page status is current.
