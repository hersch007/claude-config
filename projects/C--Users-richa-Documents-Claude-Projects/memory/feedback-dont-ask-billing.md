---
name: feedback-dont-ask-billing
description: CPFlex/Garlock billing model — hourly by day under the "Garlock" column in Start Billables; do not ask Richard how to bill
metadata:
  type: feedback
---

Do not ask Richard how he bills CPFlex ("Garlock") SEO work. The model (confirmed from the billables data on 2026-09-07):

- Richard bills **Start Advertising** by **hours per client per day**, not per page. Hours go in `RB2/GroupRB Billables for Start/Start Billables.xlsx` under the billable column **"Garlock"** (tracker code `GAR`), fed by `daily/append-YYYY-MM-DD.json` files and the scripts in `tools/`.
- Historical rate for implementing an audited page in HubSpot (title, meta, alts, schema paste): **about 0.75–1 hour per page** (e.g. Jul 8: 2 pages/1.5 h; Jul 10: 1/1 h; Jul 13: 2/2 h; Jul 27: 6/5.75 h). Audit-writing days in Aug ran 3–6.5 h.
- The "Billing: ✓ Billed / — Pending" line in the Word running log only tracks whether a page's work has been included in a logged day.

**Why:** Richard has had to re-explain this repeatedly; it is a known frustration.
**How to apply:** When a page is implemented, suggest logging ~1 h (light page) to ~1.5 h (heavy alt-text page) under Garlock for that day, and flag if the work may already be covered by hours logged on the day it was done. See [[project-cpflex]].

**Where the per-item detail lives (found 2026-09-15):** `RB2/GroupRB Billables for Start/daily/YYYY-MM-DD.json` has a `rows[]` array of 15-minute blocks with `client` (GAR), `job` code and a free-text `note` naming the page/article and phase ("Blog Shrink Band Schema"). The `append-*.json` files only hold day totals. To answer "was X billed?", grep the `note` fields across `daily/2026-*.json` — do not rely on the ☐/✓ Billing flags in the Word running logs, which were stale until reconciled on 2026-09-15.

Job codes seen: 0790165 SEO Research · 0790147 AI SEO · 0790149 Schema · 0790166/0790167/0790146 Implementation/Web Updates · 0790148 Additional Content · 0790168 SEO Tracking/Audit.
