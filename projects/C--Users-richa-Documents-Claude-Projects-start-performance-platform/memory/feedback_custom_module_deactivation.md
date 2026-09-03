---
name: feedback-custom-module-deactivation
description: "When a client has a custom module replacing a Core, deactivate the corresponding core-stack plugin entirely on that site — don't just hide its nav item"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: bce3fcb0-21e9-482d-b4c5-fd9cda46bc99
---

Rule (stated by Richard, 2026-07-04): when a client has its own custom module for a Core (like SMTI's `smti-sales`/`smti-service` replacing core Sales/Tickets), the corresponding resellable core-stack plugin is supposed to be **deactivated entirely** on that client's site — not just have its nav item hidden via Settings, and not left active-but-redundant.

**Why:** Hiding via `sp_hidden_nav_items` only suppresses the nav item; the plugin's dashboard widgets, AJAX handlers, and any hardcoded queries against its tables keep running. In SMTI's case this was actively harmful — `start-performance-tickets`'s dashboard widget was silently erroring every page load because `wp_sp_tickets` didn't even exist there. Full deactivation is the correct fix, not a settings toggle. See [[plugin_versions]] for the SMTI before/after state (deactivated `start-performance-tickets` and `start-performance-sales` there on 2026-07-04, after confirming their tables were empty/unused).

**How to apply:** Before deactivating a core plugin on a client site, verify:
1. The client's custom module genuinely replaces *all* of what the core plugin provides — not just part of it. (`start-performance-sales` also provides the "Leads" view; only deactivate it if the client's external system — e.g. HubSpot — is truly the source of truth for leads too, not just quotes/estimates. Confirmed true for SMTI 2026-07-04.)
2. The core plugin's tables are empty or genuinely unused on that site (`wp db query "SELECT COUNT(*) FROM ..."` over SSH) before deactivating, so no live data becomes inaccessible.
3. Deactivating removes the plugin's own in-app Settings/toggle UI too, since those hooks live inside the plugin itself — re-activating from then on requires real WP admin → Plugins, not the SP app's Settings page. (Update 2026-07-04: the Add-ons page now has an in-app Activate button for this, super-admin only — see [[core_platform_architecture]].)
4. **Deploying an updated zip to a site later will silently reactivate a plugin you deliberately deactivated there**, since `deploy.ps1` always runs `wp plugin install --force --activate`. Learned this the hard way when redeploying `start-performance-tickets` reactivated it on SMTI. Always re-check `wp plugin list` after any deploy that touches a plugin with a deliberately-inactive site.

This does **not** automatically apply to every client with any custom module — e.g. Fruth's `sales-quote-system` only replaces quotes/pricing (not Leads), and it's unconfirmed whether Fruth treats an external system as source of truth for Leads the way SMTI does with HubSpot. Check case-by-case per [[project_overview]]'s site list before deactivating anything on Fruth or future clients.
