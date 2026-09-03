---
name: project-wts
description: "WTS jurisdiction onboarding platform — deployment, architecture, and status notes"
metadata: 
  node_type: memory
  type: project
  originSessionId: 4b25fea1-3b48-4f94-82ed-7fc14e04252d
  modified: 2026-07-21T10:38:52.024Z
---

Single WTS instance only — user confirmed there are no other WTS sites to deploy to.

**Why:** Originally expected 3 additional instances; user clarified there is only one.
**How to apply:** Do not prompt about deploying to other WTS instances.

## Current deployed versions
- Core plugin: **v2.5.35** — fixes admin bypass of onboard screen + cookie banner suppression + invite auto-login
- WTS plugin: **v1.0.4** — adds Delete button to jurisdiction list

## Key architecture
- Zip build method: copy previous zip, update entries with `ZipFile.Open` Update mode (forward-slash prefix required)
- File system on hosting is read-only for plugin dirs — plugin editor saves silently fail; changes must go via zip upload
- All team members share the first WP admin's auth session; `sp_team_auth` cookie identifies current member

## Pending cleanup
- Admin's own `sp_wts_member_status_{id}` DB option is still set to 'pending' from testing — routing bypasses it now but should be deleted via phpMyAdmin SQL
