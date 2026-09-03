---
name: project-government-service-core
description: Government Service Core — new SP product vertical for city/municipal governments. Plugin government-service-core, built Aug 2026, at v1.2.17.
metadata:
  type: project
  originSessionId: auto-memory-2026-08-28
  modified: 2026-08-28T12:21:10.995Z
---

**Government Service Core** is a new SP product vertical targeting city and municipal governments. Plugin key: `government-service-core`, claims the `government-service` core slot. Strips all standard core slots (sales, service, operations, intelligence, knowledge, chat) — this is a standalone product, not an addon on top of the regular SP stack.

**Why:** Entirely different customer type (city/gov) than the business clients (SMTI, Fruth, WTS). Stand-alone vertical with its own role model, views, and data.

**How to apply:** Do not treat this like the business-client plugins. It replaces most of the SP nav and has its own tables/roles. Deploy only to government/city instances. Current version: 1.2.17 (as of 2026-08-28 — actively being built).

## What's built (1.2.17)

**Core concept:** multi-department public service ticket system. Citizens submit requests (water main break, pothole, etc.); staff triage and resolve them. On-call scheduling routes new tickets to the right person automatically.

**DB tables (all `{prefix}sp_city_*`):**
- `sp_city_departments` — Electric/Water/Sewer/Maintenance/Garbage (seeded on activate); custom colors per dept
- `sp_city_tickets` — ticket_number (CSR-YYYYMMDD-NNNNN), source (public/staff), reporter fields, address, description, priority (emergency/high/normal/low), status (open/in_progress/resolved/closed), acknowledged_at/by, assigned_to, photo_ids
- `sp_city_ticket_depts` — many-to-many: a ticket can span multiple departments, each with its own acknowledged_by/at
- `sp_city_oncall` — shift schedule: team_member_id + dept_id + start/end_datetime + contact override phone/email
- `sp_city_ticket_notes` — staff notes with is_public flag
- `sp_city_notification_log` — audit log of every email/SMS sent

**5-tier role system (stored in `sp_city_member_roles` option):**
- `city_admin` — full access (settings, branding, delete, assign anyone)
- `city_supervisor` — run queue (assign anyone, delete), no settings
- `city_employee` — process/close tickets, assign to supervisors only, no delete
- `call_center_admin` — create + edit tickets + notes + acknowledge; no assign/delete/settings; cannot resolve/close
- `call_center` — create new tickets only

**3 SP views registered:**
- `city-tickets` — full ticket list + create/edit detail view
- `city-oncall` — on-call schedule management (add/edit/delete shifts, add/edit departments)
- `city-onduty` — live on-duty board (who's on shift right now, per dept)

**Dashboard hooks:**
- `sp_dashboard_before_stats` → "On Duty Right Now" card (dept-colored tiles showing current on-call person)
- `sp_dashboard_after_stats` → 3 stat cards: Open Tickets / Emergency count / On Duty Now count
- `sp_dashboard_after_grid` → Open tickets table (sorted emergency→high→normal, top 6)

**Notification system:**
- New ticket → email + SMS to current on-call for each department
- No one on call → fallback email to supervisor
- Assignment change → email + SMS to newly assigned member
- Reporter gets confirmation email (if email provided)
- Escalation: WP-Cron `sp_city_escalation_check` fires after `sp_city_escalate_minutes` (default 30) if ticket still unacknowledged → supervisor email + SMS
- Twilio SMS (E.164 normalization, US default). SMS inactive for Clinton (first client) — fields present but unused.
- Full notification audit log

**Public-facing features:**
- `[city_service_form]` shortcode — styled public submission form with dept checkboxes, priority, address, description, optional reporter contact; photo upload (up to 3, images only, 8MB each, stored as WP attachments)
- `/service-request/` standalone template (bypasses SP auth gate entirely)
- `wp-json/sp-city/v1/ticket-status?number=CSR-xxx` — public REST endpoint for ticket status lookup (no auth, returns non-PII fields only)

**Branding overrides:**
- `pre_option_sp_platform_name/sp_brand_icon_url/sp_brand_initials` → reads `sp_city_name/logo_url/initials` options
- Primary color (`#1e3a5f` default) + secondary (`#c8a84b`) set via `sp_app_footer` CSS injection
- Hides standard CRM dashboard cards (contacts/companies/leads/tasks/sales-estimates)
- Strips Core System nav section
- `sp_public_frontend=true` — entire app is public (no SP login gate)

**Settings panel** (under SP Settings): branding, colors, logo, escalation timer, supervisor email/phone, Twilio config, department list (add/edit/sort/color), team role assignments.

**Sample data seeder:** REST endpoint `sp-city/v1/seed-sample-data` (admin-only) seeds 5 sample tickets + 7 team members + 10 on-call shifts.

## First client
City of Clinton (South Carolina). SMS not active there. The settings UI has a hardcoded "SMS is not active for Clinton" notice — may need to be made generic for future clients.

## Build notes
- ZIP: use `build.ps1` (auto-discovers the folder). Key is `government-service-core` in deploy.ps1 (not yet confirmed — check before deploying).
- The old `city-service-core` 1.1.0 (built Aug 6–11) is the prototype; `government-service-core` is the current renamed version.
- `city-core-theme.zip` and `city-core-cleanup.zip` also exist from Aug 11 — likely one-off install/theme helpers for the initial City of Clinton deploy.

## What's NOT yet built
- The `city-tickets.php` and `city-onduty.php` view templates (in `templates/views/`) — need to read those to know current state
- resolved_at field exists in save_note but check if it's in the DB schema (it's not in db.php as of 1.2.17 — potential bug)
- Public ticket status page (the REST endpoint exists but there's no frontend page using it)
