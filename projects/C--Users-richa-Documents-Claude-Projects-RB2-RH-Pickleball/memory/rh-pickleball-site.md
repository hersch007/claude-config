---
name: rh-pickleball-site
description: "How the Rock Hill Pickleball resource website is built, hosted, and edited"
metadata: 
  node_type: memory
  type: project
  originSessionId: 4b7d932b-5625-4e40-a171-fba207cb9add
  modified: 2026-07-28T18:55:00.273Z
---

Live site: **https://rockhillpickleballclub.com** (note: the domain is rockhillpickleball**club**.com, NOT rockhillpickleball.com — that's a different site).

It is a **static HTML site**, not managed through the WordPress dashboard. WordPress + GeneratePress is installed underneath in `public_html`, but the static `index.html` serves the front page via a `DirectoryIndex index.html index.php` line added to `public_html/.htaccess`. Trying to build it inside WordPress/GeneratePress was too fiddly for the volunteer, so we went static.

**Files on the host (cPanel/DirectAdmin File Manager), all in `public_html/` root:**
- Pages: `index.html`, `courts.html`, `find-players.html`, `getting-started.html`, `about.html`, `scheduler.html`
- `assets/` folder: `site.css` (content styles), `header-footer.css`, `scheduler.css`, `site.js` (injects the shared header/footer/nav + logo + favicon + scroll-reveal), `scheduler.js` (the tournament tool), the logo PNG, and the hero background PNG.

**Source of truth on the user's PC:** `C:\Users\richa\Documents\Claude Projects\RB2\RH Pickleball\rockhill-site\` — edit here, then the user uploads to overwrite via File Manager. Always tell them to hard-refresh (Ctrl+F5) after CSS/JS changes.

**Editing workflow:** user is non-technical. Either they paste the change request and I hand them the updated file to upload, or they use the File Manager editor for small text tweaks. Which file = which page is in [[rh-pickleball-editing-map]] if written.

**Already wired:** CourtReserve portal link (org 16310), 6 court Group.Me links on Find Players, footer Facebook/Instagram show "coming soon". Tournament scheduler is MIT-licensed and also exists as a standalone/shareable widget.

**Still TODO:** the pages contain `[FILL IN: ...]` placeholders needing real data — court cost/hours/addresses, club email, gear recommendations, DUPR link. Also a real contact form (About page has only a visual placeholder; needs a form service since it's static, not WordPress).
