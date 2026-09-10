# Memory Index

- [CPFlex Client (C-P Flexible Packaging)](project-cpflex.md) — "Garlock SEO" = this client; new HubSpot site is PRE-LAUNCH on hs-sites, gcpflexpack.com parked, all 77 site pages live on staging, blog 24/31 done + link pass pending
- [Garlock Sealing Client (garlock.com)](project-garlock-sealing.md) — Separate client, SPA site, score 54, shares folder name with CPFlex
- [CPFlex billing model](feedback-dont-ask-billing.md) — hourly by day under "Garlock" in Start Billables, ~0.75–1 h per implemented page; never ask
- [CPFlex docx editing rules](feedback-cpflex-docx-editing.md) — Word logs were corrupted Sep 5 by in-place zip edit; write new zip with Content_Types, Word-test after
- [SRB PPTX version numbers](feedback-srb-version-numbers.md) — Bump version on every rebuild; current is v3
- [City of Clinton instance](project-clinton-city-instance.md) — GSC-based SP install; AI Queue Analysis on dashboard + weekly email to admins/supervisors (AI 1.4.0 + GSC 1.2.35, Sep 8 2026), triage rejected; own cPanel host, SFTP-only, deploy-clinton.ps1
- [Riverside demo instance](project-riverside-demo.md) — fictional-city demo on the startperformance VPS (own cPanel acct, shell + WP-CLI); use for screenshots/sales demos, never Clinton
- [Local PHP CLI](reference-local-php.md) — PHP 8.4 via winget (Sep 9 2026); absolute php.exe path for php -l before building plugin zips
- [Client-facing branding](feedback-client-facing-branding.md) — client deliverables use Start Performance + RBStart@StartPerformance.com; never GroupRB / richard@grouprb.com
- [AnagraSign e-signature app](project-anagrasign.md) — Adobe Sign-style contract signing app in Claude Projects/AnagraSign; Node/Express/sqlite/pdf-lib MVP, smoke test passing Sep 9 2026, not deployed
- [Lawn Ace SEO client](project-lawnace-seo-client.md) — lawnace.com onboarded Sep 9 2026, crawler score 81/100; GroupRB-branded (exception to Start Performance rule) since GroupRB built the site
- [SEO audit report format](feedback-seo-audit-report-format.md) — deliverable is the seo-tool crawler HTML/PDF dashboard (Fruth-style), not a hand-written Word doc; run audit.js, watch for 429s, encode %20 in file:// URLs for Chrome PDF export
