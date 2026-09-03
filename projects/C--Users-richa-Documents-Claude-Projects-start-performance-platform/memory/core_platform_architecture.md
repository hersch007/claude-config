---
name: core-platform-architecture
description: "SP addon/view registration system, auth pattern, settings extensibility hooks"
metadata: 
  node_type: memory
  type: project
  originSessionId: f256f0f7-3dd7-41b0-92c9-aaf30a545038
---

## Registration hooks
- `sp_register_addon( $id, $config )` — registers an addon module
- `sp_register_view( $view_slug, $file_path )` — registers a routed view
- `sp_nav_items` filter — addons append nav entries
- `sp_allowed_views` filter — addons whitelist their view slugs
- `sp_settings_sections` action — addons output their settings HTML
- `sp_settings_anchor_tabs` filter — addons add tabs to the settings tab bar
- `sp_dashboard_before_stats` / `sp_dashboard_after_stats` / `sp_dashboard_after_grid` actions — addons render their own dashboard stat cards/widgets. `sp_dashboard_before_stats` (added 2026-07-04, fires right after Quick Actions, before the hardcoded baseline stat row in `templates/views/dashboard.php`) is for **client-specific/custom modules** (`smti-sales`, `smti-service`) so their real numbers appear above the generic Contacts/Companies/Leads/Tasks row — which is often all zeros for a client using an external system like HubSpot as source of truth. `sp_dashboard_after_stats` (used by core `tickets`, `sales`) fires after the baseline row. **`sp_dashboard_after_grid` no longer has the AI Pipeline Insights card** — as of 2026-07-10 AI was pulled off the core dashboard entirely (see [[project_ai_not_a_destination]]); the hook still exists but that consumer moved to `sp_intel_after_ai_summary`. See [[project_kpi_abstraction_needed]] for why this mattered (SMTI's dashboard was showing 4 rows of zeros above their actual $1.86M pipeline).
- `sp_intel_summary_lines` filter (added 2026-07-04, `apply_filters('sp_intel_summary_lines', $lines, $days, $since, $today)` in `start-performance-intelligence`) — addons append their own KPI bullet(s) to the AI Business Summary prompt instead of Intelligence Core hardcoding `SHOW TABLES` checks per module. See [[project_kpi_abstraction_needed]] for the SMTI implementation (`sp_smti_sales_intel_summary_lines`, `sp_smti_intel_summary_lines`) as reference templates.
- `sp_intel_after_ai_summary` action (added 2026-07-10, `do_action('sp_intel_after_ai_summary')` in `start-performance-intelligence/templates/views/intelligence.php`, right after the AI Smart Summary card) — where AI-analysis surfaces render inside Intelligence Core. The AI plugin's **Pipeline Insights** card hooks here now (was `sp_dashboard_after_grid` on the core dashboard until 2026-07-10). See [[project_ai_not_a_destination]].
- `sp_is_view_hidden( $view_slug )` helper (added 2026-07-04, in core `start-performance.php`) — returns true if a view slug is in the `sp_hidden_nav_items` option. Nav rendering (`app.php`) already respected this; as of 2026-07-04, dashboard-widget callbacks registered on `sp_dashboard_after_stats`/`sp_dashboard_after_grid` now call it too (`sp_tickets_dashboard_stats`/`_grid`, `sp_sales_dashboard_stats`, `sp_smti_dashboard_stats`, `sp_ai_pipeline_insights_ui`, `sp_smti_sales_dashboard_stats`) so a module hidden via Settings → Navigation doesn't leave an orphaned stat card on the dashboard. **Extended further 2026-07-04**: the core dashboard's own hardcoded baseline stat cards (`templates/views/dashboard.php` — Contacts, Companies, Leads, Overdue Tasks, Pipeline Value) now also call `sp_is_view_hidden()` for their corresponding view slug (`contacts`, `companies`, `leads`, `tasks`, `sales-estimates`), not just addon-contributed cards. New addon dashboard widgets should call this too.
- `sp_get_inactive_addon_plugins()` (added 2026-07-04, core `start-performance.php`) — scans `get_plugins()` for installed-but-inactive plugins whose folder starts with `start-performance-` (excludes core itself). Used by the Add-ons page (`templates/views/addons.php`) to list inactive SP modules with an in-app **Activate** button (super-admin only), reusing the existing `sp_ajax_addon_toggle` AJAX handler which already supported `toggle=activate` — it just had no UI wired to it before. This closes the gap where deactivating a plugin removed all its own in-app settings/toggle UI, leaving WP admin → Plugins as the only way back in.
- `sp_ai_card_start( $label, $action_html = '' )` / `sp_ai_card_end()` (added 2026-07-04, core `start-performance.php`) — shared dark-gradient chrome (icon badge + uppercase label + optional right-aligned action) for every AI-generated content card, so they all look like Intelligence Core's "AI Smart Summary" card. `sp_ai_content_well_style()` returns a light-background style string for nesting markdown/HTML output inside the dark card (since that output assumes dark-text-on-light). Applied to the AI plugin's "Pipeline Insights", "AI Contact Summary", "AI Ticket Triage" cards (`start-performance-ai.php`). **NOTE (2026-07-10): the Dashboard "AI Business Summary" card that used these helpers was removed** — AI no longer renders on the core dashboard (see [[project_ai_not_a_destination]]). `sp_ai_card_start`/`_end` remain in core as generic card chrome; the AI addon is now their only consumer.

- `wp_ajax_sp_toggle_task` (added 2026-07-05, core `start-performance.php`, function `sp_ajax_toggle_task`) — flips a task's status (open/done) and returns JSON, without the page redirect the older `sp_type=task` POST handler always does (back to `?view={record_type}s&action=view&id={record_id}&tab=tasks`). Use this AJAX endpoint instead of the POST-redirect pattern whenever a task checkbox is embedded somewhere that isn't the task's own contact/company page — e.g. Operations Core's inline workflow checklist (`operations.php`).

- `sp_is_addon_active( $addon_id )` (added 2026-07-06, core `start-performance.php`) — checks `sp_get_addons()` for whether an addon's plugin actually ran `sp_register_addon()` this request. **Use this, not `SHOW TABLES LIKE` checks**, to gate any dashboard/KPI element on "is this module active" — a deactivated plugin's tables stick around, so a table-existence check can't tell active from merely-installed-once. `sp_hidden_nav_items` (`sp_is_view_hidden()`) is a separate, independent toggle (manually hide a nav item while the plugin stays active) — don't conflate the two. Addon IDs: `sp-sales`, `sp-tickets`, `sp-operations`, `sp-knowledge`, `sp-smti-sales`, `sp-smti` (service).

## Member core access (per-team-member permissions)
- `sp_team.core_access` column = JSON array of allowed core section-ids. **Empty = unrestricted (ALL cores).** Super admins always pass. `core-system` and `dashboard` are ALWAYS accessible (hardcoded in `sp_member_has_core_access()`), so only the six core slots (`intelligence-core`, `sales-core`, `service-core`, `operations-core`, `knowledge-core`, `chat-core`) are restrictable.
- Set per member in **Admin → Team** (`team.php`): role dropdown is **`agent`** ("can create & edit records") vs **`admin`** ("full access"); `sp_is_admin_member()` is true only for admin (+ super admin). Core-access checkboxes let you limit an agent to specific cores.
- **As of core 2.5.19**, a member restricted from a core does NOT lose it from the nav — it renders as a dimmed **locked upsell teaser** (`view=sp-core-upsell&core=<id>`), same visual as an inactive-addon slot. Real access is still blocked by the view gate in `app.php`, which redirects a restricted member to that core's upsell screen. This "see-but-can't-open" behavior is intentional (upsell hook) and platform-wide. The nav-gating + upsell-injection are one combined pass in `templates/app.php`.
- **Fruth quote-system role split** (`sales-quote-system`): `fruth-quotes` (Fruth Quotes) is agent-accessible; `fruth-pricing` (Fruth Pricing Data / the "edit tables") is **admin-only** — gated both in `sqs_pricing_calculator_sp_nav_items()` (nav) AND inside `sp-views/fruth-pricing.php` (blocks direct-URL access). So an **Agent** = quotes yes, pricing-edit no; an **Admin** = both + user management. This is the plugin's designed "Admin gets Sales + Data Editor + manage users; Agent gets Sales only" split.

## Auth
- `sp_get_current_team_member()` — returns current logged-in team member (SP session, not WP login)
- `sp_is_admin_member()` — true if current member has admin role
- Session stored in `sp_team_auth` cookie
- REST routes use `permission_callback => '__return_true'` then check SP session manually inside the handler

## Branding (as of v1.3.8)
- `sp_accent_color` option — hex color, drives `--sp-accent` CSS variable injected inline in app.php and login.php
- `sp_brand_icon_url` option — image URL for sidebar/login icon (priority 1)
- `sp_brand_initials` option — 1–3 letter fallback (priority 2)
- Default SVG logo mark used if neither is set
- Helper functions in start-performance.php: `sp_hex_to_rgb()`, `sp_darken_hex()`

## Settings save handler
In `start-performance.php`, `$type === 'settings'` block saves:
- `sp_platform_name`, `sp_logo_url`, `sp_brand_icon_url`, `sp_accent_color`, `sp_brand_initials`
- `sp_hidden_nav_items` (array of hidden view slugs)

## PHP constraints
- PHP 5.6+ compatible — no typed properties, union types, match(), arrow functions, `??` null coalescing
- MySQL 5.5 compatible SQL
- No autoloader, no namespaces, plain functions with `sp_` prefix

## Zip builds
Use PowerShell `.NET ZipArchive` with forward-slash entry paths. Pattern:
```powershell
$zip = [System.IO.Compression.ZipFile]::Open($out, 'Create')
Get-ChildItem -LiteralPath $src -Recurse -File | ForEach-Object {
    $rel = $_.FullName.Substring($src.Length + 1).Replace('\','/')
    [System.IO.Compression.ZipFileExtensions]::CreateEntryFromFile($zip, $_.FullName, 'plugin-folder/' + $rel, 'Optimal') | Out-Null
}
$zip.Dispose()
```
