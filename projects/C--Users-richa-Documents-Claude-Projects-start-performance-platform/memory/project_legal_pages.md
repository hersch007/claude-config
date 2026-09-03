---
name: project-legal-pages
description: "Terms/Privacy/DPA pages and login acceptance feature — built but hidden, ready to re-enable"
metadata: 
  node_type: memory
  type: project
  originSessionId: 98802c1b-d71e-4d7e-8f2b-5d61ab6f7ea4
---

Legal pages and terms acceptance are fully built but currently disabled on the login screen.

**Why:** Pages were live but the footer links redirected to /sp-app/ due to the stealth layer. Feature was pulled while polishing for public demo.

**What exists:**
- WordPress pages `/terms/`, `/privacy/`, `/dpa/` are live on all three installs (smti, sp, fruth) with full content
- `terms_accepted` tinyint column exists on `sp_team` table (added via sp_maybe_migrate)
- `sp_pages` whitelist in `template_redirect` already includes `'terms', 'privacy', 'dpa'` so pages load without redirect
- Legal document source files: `legal/terms-of-service.md`, `legal/privacy-policy.md`, `legal/data-processing-agreement.md`
- Script to recreate pages on any site: `scripts/create_legal_pages.php` + `scripts/create_legal_pages.ps1`

**How to re-enable (one session):**
1. In `templates/login.php` — restore the `.terms-wrap` checkbox block (before the Sign In button) and `.footer-links` div (after `.footer`)
2. In `start-performance.php` — uncomment the terms acceptance block in the login POST handler (search "Terms acceptance — disabled")
3. Restore CSS classes `.terms-wrap` and `.footer-links` (they're already in the `<style>` block in login.php)
4. Bump version, build zip, deploy core

**Checkbox copy:** "By signing in, I agree to the Terms of Service and Privacy Policy." — links use `home_url('/terms/')` and `home_url('/privacy/')`
**Footer copy:** Terms · Privacy · DPA — links use `home_url('/terms/')`, `home_url('/privacy/')`, `home_url('/dpa/')`
