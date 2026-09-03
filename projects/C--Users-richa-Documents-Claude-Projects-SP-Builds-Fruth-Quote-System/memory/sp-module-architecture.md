---
name: sp-module-architecture
description: How Start Performance add-on modules plug into the core WordPress plugin (the pattern a quote module must follow)
metadata: 
  node_type: memory
  type: project
  originSessionId: 01838d33-e106-473b-b1c9-02bbf8061031
---

Start Performance is a **suite of WordPress plugins**. Core = `start-performance` (start-performance.php, ~1300 lines) which exposes an add-on API. Add-ons are separate plugins (e.g. `start-performance-tickets`, `start-performance-ai`) that hook in. The Fruth Quote System is being built as one of these add-on modules (a "custom quote module").

**Module contract (mirror the tickets add-on — 2 files: `<slug>.php` + `templates/views/<slug>.php`):**
- Boot on `add_action('plugins_loaded', ..., 20)`; guard with `if (!function_exists('sp_register_view'))` to require core.
- Register: `sp_register_addon($id, [name,version,description,settings_url,icon(svg path),plugin_file])` and `sp_register_view('quotes', DIR.'templates/views/quotes.php')`.
- Nav: `add_filter('sp_nav_items', fn)`; allow view: `add_filter('sp_allowed_views', fn)`.
- Forms POST to `home_url('/sp-app/')` with hidden `sp_type`, `sp_id`, and `wp_nonce_field('sp_form','sp_nonce')`. Core dispatches to `do_action('sp_post_handler_<type>', $id)` and `sp_delete_handler_<type>`.
- Own DB table created via `dbDelta` on `register_activation_hook` AND `add_action('sp_activate', ...)`. Table naming: `{$wpdb->prefix}sp_<thing>`. Existing tables include sp_contacts, sp_companies, sp_tickets, sp_email_log.
- Optional dashboard/settings surface: `sp_dashboard_after_stats`, `sp_dashboard_after_grid`, `sp_settings_sections`. Admin gate: `sp_is_admin_member()`. Config stored via `get_option`/`update_option`.
- UI uses core CSS classes: `sp-card`, `sp-table`, `sp-btn sp-btn-primary`, `sp-badge`, `sp-field`, `sp-stat-card`. App lives at `/sp-app/?view=<slug>` with `action=new|edit|view&id=`.

Author string used: "Richard Brashear / Start Performance". Latest versions seen: core 1.8.1, ai 1.2.5, tickets 1.1.6. Plugins live at C:\Users\richa\Documents\Claude Projects\start-performance-platform\plugin (versioned zips + build.ps1).
