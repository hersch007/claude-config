---
name: project-fruth-meeting-fis5100rd
description: FIS-5100RD resin cost still unpriced as of the Kris Hall workbook (2026-07-18) — still deferred, don't guess a value
metadata: 
  node_type: memory
  type: project
  originSessionId: 461ba5b1-0f51-4405-b9c4-585f70bc770f
---

`FIS-5100RD (Red PP)` has no resin cost. Originally flagged as a gap in the source Excel workbook (`Resin Prices` sheet, blank cell), deferred to a Fruth meeting the week of 2026-07-06. As of 2026-07-18, the newer "Kris Hall" workbook (adopted into the app in Core 8.9.15) **still has this formula unpriced** — so either the meeting didn't resolve it, or Kris's sheet hasn't caught up yet. Two sibling formulas (`FIS-3000BK (Black Cond)`, `FIS-1010 (Zipper)`) disappeared from Kris's workbook entirely with no replacement and were removed from the app per the user's direction — `FIS-5100RD` was deliberately left in place (still selectable, still $0 cost) rather than removed, since its status is genuinely unresolved rather than confirmed-discontinued. See `docs/TODO.md` and `docs/CHANGELOG.md` (Core 8.9.15) in the Fruth project.

**Why:** This is a real missing business number that only Fruth can supply — not something to guess at, hack around, or silently remove.

**How to apply:** Don't propose a workaround or a guessed value for `FIS-5100RD`. If the user reports Fruth has finally supplied a real cost, update `DATA.formulaCosts['FIS-5100RD (Red PP)']` in `sales-quote-system.php` (base64-encoded `SQS_DEFAULT_DATA_B64` constant) *and* the matching row in each live site's Data Editor (pricing tables are database-backed per site — see [[project-standalone-fruth-grandfathered-pricing]]), then re-validate per the existing `FORMULA-VALIDATION.md` process. See [[fruth-sales-system-overview]].
