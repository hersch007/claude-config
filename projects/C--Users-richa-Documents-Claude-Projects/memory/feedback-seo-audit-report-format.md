---
name: feedback-seo-audit-report-format
description: "For SEO-Clients-Hub audits, the expected deliverable is the seo-tool crawler HTML/PDF dashboard, not a hand-written Word doc"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 2422bc59-e89c-41c8-8930-abbaff9b38f7
  modified: 2026-09-10T01:06:45.735Z
---

When asked to "run an SEO audit" on a client site within `SEO-Clients-Hub`, the deliverable the user actually wants is the automated dashboard report produced by `SEO-Clients-Hub/seo-tool/audit.js` — a live crawl rendered into the styled HTML/PDF format seen in `clients/Fruth/Fruth-Custom-Packaging-SEO-Audit-2026-08-31.pdf` (purple/navy cover with score box + trend chart, snapshot cards, quick-win cards, findings-by-page cards, all-pages table).

**Why:** On the first Lawn Ace audit (2026-09-09), a manually-researched Word doc (via the `new-seo-client` skill's master/prospect .docx templates) was delivered first and the user corrected it — "this is not the right report type" — pointing to the Fruth PDF as the reference. The manual skill-based audit is not wrong to produce, but it is not the primary deliverable to send.

**How to apply:**
1. Check whether `SEO-Clients-Hub/seo-tool/clients/<client>.json` exists or create one (fields: `name`, `url`, `brand_color`, `output_dir` pointing at the client folder, `max_pages`, `ignore_paths`).
2. Run `node audit.js <client>` from `SEO-Clients-Hub/seo-tool/` — pipe `"N"` to stdin (`echo "N" | node audit.js <client>`) to skip the interactive GSC performance-metrics prompt when no live data is being entered.
3. Watch for HTTP 429s if the tool was just run against the same domain minutes earlier (its own rate limiting or the target site's WAF) — wait ~60-90s and re-run rather than trusting a run with error rows, since errored pages tank the score inaccurately.
4. The generator hardcodes "Start Advertising" as the agency name/footer text — if the client needs different branding (e.g. GroupRB), `sed` the generated HTML rather than re-running the crawl, and check for other hardcoded `#003366` navy accents (qw-card/perf-card borders, print-mode link color) that should also be swapped to match `brand_color` if the client has its own brand color.
5. To get a PDF, headless-print the HTML with Chrome (`chrome.exe --headless --disable-gpu --no-pdf-header-footer --print-to-pdf=out.pdf <url>`) — the file:// URL MUST have spaces percent-encoded (`%20`) or Chrome silently produces a broken single-page ~20KB PDF instead of erroring. Also write the output PDF to the scratchpad directory first (writing directly into the Claude Projects client folder via a spawned Chrome subprocess hits an "Access is denied" sandbox error, even though Claude's own Bash/Write tools can write there fine) then copy it over.
6. See [[project-lawnace-seo-client]] for the concrete example.
