---
name: project-riverside-demo
description: "Riverside = fictional-city DEMO instance of Start Performance + Government Service Core, built Sep 8 2026 for screenshots/sales; own cPanel account on the startperformance VPS with shell + WP-CLI"
metadata: 
  node_type: memory
  type: project
  originSessionId: 5c1cb463-823b-4341-b621-fd45ce6d6181
  modified: 2026-09-08T14:32:01.330Z
---

"Riverside" / "City of Riverside" is a DEMO instance (no real client) built 2026-09-08 for website/LinkedIn screenshots and as a sales demo for other cities. URL https://riverside.startperformance.com (app at /sp-app/, public form at /service-request/, vendor login at /sp-login/?vendor=1). It runs on the startperformance VPS 64.247.178.244 (same box as Clinton) in its OWN cPanel account `riverside`, docroot `/home/riverside/public_html`, DB `riverside_wp`. Unlike Clinton, this account HAS shell access (WHM Manage Shell Access, package detached) and WP-CLI at /usr/local/bin/wp; SSH with key `C:/Users/richa/.ssh/id_ed25519` as riverside@64.247.178.244 works fully.

Seeded by `scripts/riverside-seed.php` (wp eval-file; wipes and rebuilds city tables + team): 56 tickets over 60 days, 10 staff. Demo logins (email + PIN): dana.whitfield@riverside-demo.example / 2468 (City Admin), marcus.bell@ / 1234 (Supervisor), tyler.nguyen@ / 1234 (Employee), kim.alvarez@ / 1234 (Call Center Admin). Vendor PIN and WP admin password are in wp-config.php (SP_SUPER_ADMIN_PIN) and were printed by `scripts/riverside-setup.sh` on 2026-09-08; do not store them here. Weekly report email is disabled on Riverside (recipients are fake .example addresses).

**Why:** Clinton is a live client site with one real ticket, so its screenshots were empty and seeding fake tickets there would have emailed Randall Tuttle.

**How to apply:** Use Riverside, never Clinton, for marketing captures and feature demos. Screenshot scripts live in `scripts/capture-riverside*.js` (puppeteer-core, pin DNS to the VPS IP, one Dana login per run; team session cookies do not survive browser restarts). Public form header now uses sp_city_name (GSC 1.2.36); AI addon 1.4.2 raised the analysis token cap to 2500 and added a 500-word limit. Re-seed any time with `wp eval-file /tmp/riverside/riverside-seed.php --path=/home/riverside/public_html` (re-upload the seed first). Deploy plugins there with scp + unzip like the other shell sites. Local DNS on Richard's laptop may lag behind the wildcard record; pin to 64.247.178.244 when testing. See [[project-clinton-city-instance]].
