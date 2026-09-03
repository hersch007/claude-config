---
name: daily-billables-workflow
description: "How to produce Richard's daily billable summary xlsx for the Start group"
metadata: 
  node_type: memory
  type: project
  originSessionId: bcf93e5d-c164-4015-9c58-2940b764669c
  modified: 2026-08-15T22:10:27.159Z
---

Richard submits billing to the Start group. `Start Billables.xlsx` (monthly tabs: June, May, …) is a per-day × per-client HOURS MATRIX (cols F–N = billable clients like Garlock/Fruth, rows = days). Separately, he wants a **daily** detail sheet.

**Daily deliverable:** each day Richard pastes his 15-min time-tracker table (Del?, time, Client code e.g. CFB/GAR, Note, activity checkbox, Job Number, Billable?, .25 per block). I turn it into a one-row-per-job-number summary sheet.

**Confirmed choices:** input = pasted table · grouping = one row per job number (notes combined) · output = Excel .xlsx saved in `daily/`.

**How to run (two deliverables):**
1. Write parsed rows to `daily/YYYY-MM-DD.json` — shape `{ date, employee:"Richard Brashear", minutesPerBlock:15, rows:[{client,note,job,billable}] }`. INCLUDE non-billable rows too (billable:false) — they feed the matrix. Empty slots (no client) are ignored.
2. Daily summary sheet: `node tools/make-daily.js daily/YYYY-MM-DD.json` → `daily/Daily YYYY-MM-DD.xlsx`. Billable only; **sorted by client code then job number**; one row per job number; per-client subtotals + grand total.
3. Append into monthly matrix (both billable + non-billable):
   - `node tools/build-append.js daily/YYYY-MM-DD.json` → `daily/append-YYYY-MM-DD.json`, shape `{date,employee,billable:{name:hrs},nonBillable:{name:hrs}}` keyed by CLIENT NAME (matching column headers). Uses `tools/client-map.json`; ERRORS on unknown code or null name (nothing silently mis-posted).
   - `powershell -NoProfile -ExecutionPolicy Bypass -File tools/append-month.ps1 -Workbook "Start Billables.xlsx" -AppendJson daily/append-YYYY-MM-DD.json` (`-KeepBackups` default 5)
     Backs up + prunes to 5, auto-creates missing month tab by cloning newest sheet, finds the date row, clears its client cells, writes by header name. Re-runnable/idempotent. Workbook must be CLOSED (COM).

**ARCHITECTURE (header-name based, refactored 2026-07-09):** tools resolve columns by HEADER NAME, NOT fixed letters, using the "NON BILLABLE" column as the divider — client columns left of it = billable, right = non-billable. This lets billable columns be inserted/grown without breaking anything. `client-map.json` maps code→{billableName,nonBillableName} (names must exactly match sheet headers, e.g. "Start PErf"). Do NOT reintroduce column letters.

**Matrix layout:** A=date(newest top), B=Total(E+O), C=Unpaid(B-E), D=Ties to Intime, E=Billable total=SUM(F:last-billable), then billable client cols, then "NON BILLABLE" total col=SUM(nonbill range), then non-billable client cols. Billable headers: Start PErf, Sport Medical, Wireless TS, Garlock, Fruth, LAWN, Advernology, TriCoGo, SPA Web, Automotive Service Products. NonBill headers: Start Performance, Sport Medical, Wireless TS, Bolsterist, Fruth, Lawn Ace, Advernology, Power Sports, Catoe Nalley, STA.

**Adding a brand-new client that needs a NEW matrix column** (e.g. ASP=Automotive Service Products, added 2026-07-09): `powershell ... tools/add-billable-column.ps1 -Workbook "Start Billables.xlsx" -Sheet "July" -Header "<client>"` — inserts a billable column just before the "NON BILLABLE" divider (billables stay contiguous), matches formatting, extends Billable=SUM(F:new). Future month clones inherit it. Add to July (current) only; past months already submitted. Then add the code→name to client-map.json.

**Codes confirmed:** GAR→Garlock; CFB→Fruth(bill+nonbill); SMT→Sport Medical(bill+nonbill); STA→STA(nonbill only, "ADMIN"/emails/meetings); STP→bill "Start PErf"/nonbill "Start Performance"; TCG→TriCoGo(bill only); ADV→Advernology(bill+nonbill); WTS→Wireless TS(bill+nonbill); ASP→Automotive Service Products(bill only, own column added July); ANS→ANS(bill only; repurposed the empty SPA Web column, renamed header to "ANS"); MSC→MISC(bill only, "miscellaneous billing"; header "MISC"); CLB→"Click Broadband"(bill only, own column added Aug). Unmapped headers still available: LAWN, Bolsterist, Lawn Ace, Power Sports, Catoe Nalley. When a NEW code appears, build-append errors — confirm with Richard, add, re-run.

**Richard sometimes edits the workbook directly in Excel between sessions** (e.g. he manually added the "MISC" billable column to the August tab). Before add-billable-column, CHECK if the target column already exists (build-append/header dump) to avoid duplicates. Because tools resolve columns by header name, manual inserts don't corrupt data — but they can create dupes if you also add one.

**Reconciliation check:** paste header shows "Today's Billable Total"/"Today's Non-Bill Total" — ALWAYS cross-check before posting; long single-client blocks miscount easily (caught on 7/4). Note: some tracker rows have an activity ✓ but no Billable? check and no time value → they count as ZERO (tracker excludes them); OMIT them from the json (saw this 7/8: "City of Rock Hill Meeting" rows). A billable row with no job number posts to the matrix fine but shows under a "(no job)" line on the daily sheet (make-daily handles this).

July 2026 COMPLETE (all 31 days) as of 2026-08-04: Billable 152.5 / Non-Bill 69.0 / Total 221.5. Billable by client: Fruth 52.75, Garlock 41, Wireless TS 20, Sport Medical 12.5, TriCoGo 12, ASP 6, Advernology 4.75, ANS 3.5. Non-bill: Start Performance 55, STA 13, Sport Medical 1. New in July: ASP (Automotive Service Products, own column) and ANS (repurposed the empty SPA Web column → header renamed "ANS"). STP audit log = June 65.75 + July 55 = 120.75h, all unbilled, 76 sessions.

**START PERFORMANCE AUDIT LOG (IRS, added 2026-07-10):** Richard needs a contemporaneous timestamped record of ALL Start Performance (STP code) time — billed + unbilled — for tax/audit. Ledger `logs/stp-sessions.json` = merged task sessions `{date,start,end,hours,task,job,billed}` (contiguous same-task blocks merged; times from the tracker's time-of-day column). `node tools/build-stp-log.js` regenerates `Start Performance Time Log.xlsx` (Date|Start|End|Hours|Task|Job#|Billed?, monthly subtotals, billed/unbilled/grand totals). Each STP hour ties to the matrix (Start PErf billable / Start Performance nonbillable) — reconcile before adding. INTEGRITY: never put reconstructed times into the audit log unless verified against the day's matrix total; if a day can't be reconciled exactly, ask Richard to re-paste. Backfill status: JUNE COMPLETE (2026-07-10) — all 18 STP days, 65.75h unbilled / 0 billed, 34 sessions. (Matrix said 67.5; the −1.75 is because June matrix bucketed some STA blocks into Start Performance — 6/15 −0.25, 6/23 −1.75, 6/29 +0.25. Audit log is code-accurate.) July done: 7/3,7/5,7/8 (7.5h unbilled + 0.25 billed); 7/4 (7.25h unbilled) RECONSTRUCTION STILL PENDING Richard's confirm. Log total 73.5h. Going forward capture time-of-day for STP days. Key lesson: reconcile STP to the tracker CODE (not matrix P, which absorbed STA); STA≠STP [[stp-code-authority]].

The generator ([tools/make-daily.js](tools/make-daily.js)) is zero-dependency (builds the xlsx zip directly) because **Python is NOT installed on this machine** — Node v24 is. Each 15-min block = .25 hr. Output: Job Number | Client | Notes | Billable Hrs, sorted by job number, plus per-client subtotals + grand total (grand total should match the tracker's "Today's Billable Total"). Verify by opening via Excel COM in PowerShell if needed.
