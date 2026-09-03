---
name: project-sp-app-no-cache
description: "Root cause + fix for the recurring \"screens crapping out\" — the SP app was being browser-cached by Newfold endurance-page-cache"
metadata: 
  node_type: memory
  type: project
  originSessionId: bce3fcb0-21e9-482d-b4c5-fd9cda46bc99
---

The recurring "screens crapping out" (blank gray page with a broken-file icon, on SMTI settings/dashboard, intermittently all through the 2026-07-06 session) was NOT a PHP fatal. Root cause: the hosting stack's **Newfold/Bluehost `endurance-page-cache`** (a must-use plugin; `X-Newfold-Cache-Level: 2`) was sending `Cache-Control: max-age=7200` on the SP app routes, telling **browsers to cache the app for 2 hours**. During the day's many deploys, a browser cached a broken/partial mid-deploy response and kept serving it — a hard refresh (Ctrl+Shift+R, bypasses browser cache) temporarily "fixed" it, which is why we kept band-aiding with `speedycache purge` + hard refreshes without ever solving it.

There are THREE cache layers on these sites: (1) speedycache (its `advanced-cache.php` dropin — clear via `wp_content/cache/speedycache/<domain>/` dir delete; the `wp speedycache purge cache` CLI is broken), (2) Newfold endurance-page-cache (header-based browser/edge caching, `X-Newfold-Cache-Level`; `wp newfold` has NO purge subcommand — it works by setting Cache-Control headers), (3) the browser.

**Fix (core `start-performance` 2.5.3, 2026-07-06):** in `sp_route()` (the `init:1` handler in `start-performance.php`), for any SP route (`sp-app`, `sp-login`, `sp-setup`) we now force no-store before rendering/exiting: `define('DONOTCACHEPAGE', true)`, `nocache_headers()`, and explicit `Cache-Control: no-cache, no-store, must-revalidate, max-age=0` + `Pragma: no-cache` + `Expires: 0`. The SP app is a dynamic, session-authed application and must NEVER be page-cached anywhere. Verified live: the sp-app response now returns `no-store` (overriding Newfold's `max-age=7200`); `X-Newfold-Cache-Level: 2` still appears but our explicit header wins.

**Why:** an authed SPA served from cache shows stale/broken pages to users. This self-protecting header approach beats configuring each cache layer's exclusions (Newfold, speedycache, future CDN) — the app declares its own non-cacheability.

**Note:** after deploying the fix, a browser that already cached the old page still needs ONE hard refresh to drop the stale copy; thereafter no-cache prevents recurrence. Ships to all sites via the shared core zip.
