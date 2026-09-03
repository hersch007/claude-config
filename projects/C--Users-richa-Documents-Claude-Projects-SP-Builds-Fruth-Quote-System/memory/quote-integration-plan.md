---
name: quote-integration-plan
description: "Decisions + status for porting the Fruth Quote Builder into Start Performance as the \"Quotes\" module"
metadata: 
  node_type: memory
  type: project
  originSessionId: 01838d33-e106-473b-b1c9-02bbf8061031
---

Building the standalone Quote Builder ([[quote-plugin-architecture]]) into a Start Performance add-on module ([[sp-module-architecture]]). Build workspace = the project working dir `...\SP Builds\Fruth Quote System\`. Module folder `start-performance-quotes/` (packaged as `start-performance-quotes-1.0.0.zip` via the platform's build.ps1 convention).

**Decisions (from user, 2026-06-30):**
- Customer/company stay **free-text** (NOT linked to sp_contacts/sp_companies) — intentional, because the quote data **feeds HubSpot for Fruth**. Don't wire quotes to the SP CRM.
- **Faithful port first**, enhance later — pricing math/print logic carried over verbatim.
- Rate-table editing = **admins only** (`sp_is_admin_member()`).

**Module structure built:**
- `start-performance-quotes.php` — SP wrapper: register_addon, register_view('quotes' + 'quote_settings'), nav item spliced into `sales-core` section, allowed_views, tables on sp_activate (delegates to engine's `sqs_pricing_calculator_activate`), Recent Quotes dashboard grid.
- `includes/quote-engine.php` — ported sales-quote-system.php v8.5. Neutralized: its own shortcodes, wp-admin menus, `wp_head` styles hook, `init` PHP-session, `register_activation_hook`. Rewired: `sqs_pricing_calculator_user_can_quote_ajax()` → SP auth; portal "home" → `/sp-app/?view=dashboard`. KEPT: all calc JS, rate tables, and the `wp_ajax[_nopriv]_sqs_save_quote|load_quotes|get_quote` handlers.
- `includes/quote-print.php` — ported print module. Neutralized: `wp_head` + `admin_menu` hooks. KEPT: the 3 `sqs_*` extension-point hooks. Print assets now emitted by the quotes view.
- `templates/views/quotes.php` — calls `sqs_app_bar_styles()`, `sqsp_head_assets()`, then `sqs_pricing_calculator_render()`.
- `templates/views/quote_settings.php` — admin-gated; handles the `save_all` rate-table POST (nonce `sqs_frontend_save`), renders `sqs_pricing_calculator_render_frontend_editor()`.

**Status:** first cut complete + packaged. Verified: brace/paren balance, no BOM, hooks intact. NOT yet tested on a live WP+SP install (no PHP CLI locally).

**Known follow-ups / caveats to verify on install:**
- The engine's own app-bar (Home/Sign Out) renders *inside* the SP shell → visual double-chrome; may want to hide `.sqs-app-bar`/`.sqs-quote-toolbar` Home/Sign-Out in the SP context later.
- Branding still reads `sqs_*` options (sqs_company_name, sqs_logo_url, sqs_company_address, sqs_quote_terms) — no wp-admin page now exposes them; may fold into SP settings or the quote_settings view.
- Confirm SP's `/sp-app/` renders views on POST (needed for the rate-table save) and that admin-ajax `nopriv` reaches SP cookie auth.
- HubSpot sync itself is not built yet — quotes store free-text so the data is HubSpot-ready, but the actual push/sync is a future module concern.
