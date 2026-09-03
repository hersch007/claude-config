---
name: project-standalone-fruth-grandfathered-pricing
description: Standalone Fruth site intentionally kept on pre-Kris Formula Costs — do not sync it to sp-fruth's updated pricing
metadata:
  type: project
  originSessionId: 461ba5b1-0f51-4405-b9c4-585f70bc770f
---

The standalone Fruth Custom Packaging site (`startwebservicesbackup.com/fruth/`) is **intentionally** left on the original, pre-Kris-update Formula Costs. The user explicitly declined to apply the same pricing update there, calling it "grandfathering."

**Context**: `sp-fruth` (the Start Performance instance) had its Formula Costs table updated by hand in the live "Fruth Pricing Data" Data Editor to match the new "Kris Hall" workbook (11 changed values, 2 discontinued formulas removed — see `docs/CHANGELOG.md` Core 8.9.15 and `docs/TODO.md`). Pricing tables are database-backed per site once activated (see `docs/decisions.md`, "Important operational fact" section) — the two sites' databases are independent, so this was a deliberate per-site choice, not an oversight or a step that got skipped.

**Why:** User's explicit call — the standalone site keeps its existing (older) pricing rather than adopting Kris's updated numbers.

**How to apply:** Do not update the standalone Fruth site's Data Editor with the Kris pricing values unless the user explicitly asks for it in a future session. If asked to "sync" or "bring pricing up to date" on the standalone site, confirm this decision still stands before touching it — it may have been superseded by then. See [[fruth-sales-system-overview]].
