---
name: project-clinton-city-instance
description: "City of Clinton = Government Service Core instance of Start Performance; AI added Sep 7 2026 (AI 1.3.5 + GSC 1.2.35); own cPanel host 64.247.178.244 user clinton, SFTP-only (no shell), deploy with plugin/deploy-clinton.ps1"
metadata: 
  node_type: memory
  type: project
  originSessionId: 5c1cb463-823b-4341-b621-fd45ce6d6181
  modified: 2026-09-08T12:17:02.407Z
---

"City of Clinton instance" = a Start Performance install running `plugin/government-service-core` (GSC, tickets in `sp_city_tickets`, roles city_admin/supervisor/employee/call_center) plus `city-core-theme` and `city-core-cleanup`. The instance lives on its OWN cPanel server: 64.247.178.244, account `clinton`, plugins at `/home/clinton/public_html/wp-content/plugins`. Shell access is DISABLED there, so ssh commands and WP-CLI fail; SFTP works with key `C:/Users/richa/.ssh/id_ed25519` (authorized in cPanel SSH Access, Sep 8 2026). Deploy with `plugin/deploy-clinton.ps1` (sftp put -r of plugin folders). New plugins must be activated by Richard in WP Admin. The auto-mode classifier blocks uploading mu-plugins there.

AI for city instances (added 2026-09-07, reshaped 2026-09-08): `start-performance-ai` >= 1.3.8 loads `includes/city.php` when `SP_CITY_VERSION` is defined and renders ONE feature, the AI Queue Analysis card, on the DASHBOARD via `sp_dashboard_after_stats` priority 20, for city_admin/city_supervisor/super admin only; auto-refreshes once a day on load. Richard REJECTED per-ticket AI Triage as overkill for a city desk (removed in 1.3.7); do not re-add it. GSC 1.2.35 still fires `sp_city_ticket_edit_after` and `sp_city_tickets_after_list` but nothing uses them. Reporter phone/email are never sent to the model. Provider is chosen from the key prefix (sk-ant- = Anthropic) since 1.3.6. Live on Clinton with an Anthropic key. Weekly emailed report BUILT 2026-09-08 (AI 1.4.0, `includes/city-weekly.php`): WP-Cron hook `sp_ai_city_weekly_report`, default Monday 7am site time, recipients = active sp_team members with city role city_admin or city_supervisor (+ extra emails option), settings card "Weekly Report" (vendor or city admin) with Send-now button. Relies on WP-Cron, so a cPanel cron hitting wp-cron.php makes it punctual.

**Why:** Clinton uses a separate ticket schema, so the stock AI addon (which hooks `sp_tickets`) did nothing there.

**How to apply:** Any AI feature for Clinton goes in `includes/city.php`, not the core ticket handlers. Deploying to Clinton needs GSC >= 1.2.35 and AI >= 1.3.5 together. startwebservicesbackup.com (the other SP sites) is still password-only from Claude; Richard could authorize the same public key there. See [[project-cpflex]] for the unrelated Garlock work.
