---
name: project-super-admin-auth-gotcha
description: Recurring bug — auth gates that check only sp_get_current_team_member() silently 403/reject SUPER ADMINS; use sp_is_authed() (core 2.5.11)
metadata:
  type: project
  originSessionId: bce3fcb0-21e9-482d-b4c5-fd9cda46bc99
---

**The platform has TWO independent auth identities, each its own cookie:**
- **Team member** → `sp_team_auth` cookie (`id|hmac`, HMAC of the id with `wp_salt('auth')`), looked up in `{prefix}sp_team WHERE status='active'`. Read by `sp_get_current_team_member()`. **Set with `expires = 0` → it's a SESSION cookie, so it vanishes when the browser restarts.**
- **Super admin (the vendor, Richard)** → `sp_vendor_auth` cookie. Read by `sp_is_super_admin()`. Completely separate; a super admin is **NOT** a team-member row, so `sp_get_current_team_member()` returns **null** for them.

Both cookies are set with path `/`, so they DO reach `/wp-json/` and `admin-ajax.php` (no WP nonce needed — this is NOT WordPress user auth; `current_user_can()` is irrelevant here).

**THE RECURRING BUG:** any gate written as `if ( ! sp_get_current_team_member() ) { reject }` locks out super admins. `sp_route()` and `sp_is_admin_member()` both accept super admins, so the person can browse the app fine — but every AJAX/REST call silently 403s. It looks like "the feature broke" and is maddening to diagnose. It reliably APPEARS after a browser restart drops the session-scoped `sp_team_auth` cookie, leaving only `sp_vendor_auth`.

**Fixed occurrences (all the same defect):**
1. `start-performance-ai` — `sp_ai_pipeline_insights` etc. → "Error: Not authorized" on the dashboard. Fixed with `sp_ai_authed()` (ai 1.3.1).
2. `start-performance-smti-sales` — `sp_smti_sales_rest_authed()` → **403 on EVERY quote-builder REST endpoint** (`search-contacts`, `get-contact-details`, **`create-deal`** i.e. saving a quote, `get-deal-quote`, revisions, `delete-local-quote`). Symptom Richard reported: "my search is not working in SMTI quote." Also `sp_smti_sales_auth_check()`. Fixed in smti-sales 1.3.9/1.3.10.
3. `start-performance` core — `sp_handle_export()` (bounced super admins to sp-login) and `sp_ajax_toggle_task()` (super admin couldn't tick workflow checklist items). Fixed in core 2.5.11.

**THE FIX / RULE:** core 2.5.11 added a canonical helper — **use `sp_is_authed()`** for "is this person allowed to use the app at all":
```php
function sp_is_authed() {
    return sp_get_current_team_member() !== null || sp_is_super_admin();
}
```
Never gate on `sp_get_current_team_member()` alone. Use `sp_is_admin_member()` only when you genuinely need admin-level rights (it already includes super admins). Addons should guard with `function_exists('sp_is_authed')` since core may be older.

**Debugging recipe:** in the browser, `fetch(url,{credentials:'same-origin'})` and read the **status + body**. A `403` whose body is the endpoint's own payload (e.g. `{"results":[]}`) means OUR callback rejected it, not WordPress. Then check whether the gate accepts super admins. `sp_team_auth` / `sp_vendor_auth` are HttpOnly, so `document.cookie` won't show them.
