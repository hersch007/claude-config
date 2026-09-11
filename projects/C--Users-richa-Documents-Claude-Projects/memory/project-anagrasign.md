---
name: project-anagrasign
description: "AnagraSign is the user's self-hosted Adobe Sign-style e-signature app (Node/Express/node:sqlite/pdf-lib) in Claude Projects/AnagraSign, scaffolded Sep 9 2026"
metadata: 
  node_type: memory
  type: project
  originSessionId: efb65d6b-cb46-4761-aa48-e5aa93c17a0a
  modified: 2026-09-11T12:35:18.219Z
---

AnagraSign lives at `C:\Users\richa\Documents\Claude Projects\AnagraSign`. Built Sep 9 2026 as an
Adobe Sign / DocuSign alternative: upload PDF, drag fields, send signing links, consent + draw/type
signature, completed PDF with stamped fields and a signature-certificate page, SHA-256 audit trail.

Stack: Node 24, Express 5, `node:sqlite` (no native build), pdf-lib, pdf.js + signature_pad served
from node_modules, plain ES-module frontend, no build step. Single admin password login.
`npm run smoke` is the end-to-end test (43 checks passing as of Sep 9 2026). Signer verification
(email one-time code or sender-set access code) was added the same day after the user asked
whether signatures are legally binding.

Status: working MVP. Local `.env` has ADMIN_PASSWORD=changeme (must change before hosting). SMTP
not configured locally, so emails go to `storage/outbox/`. Dev server config is in the parent
`.claude/launch.json` (name `anagrasign`, port 3900). Renamed from StartSign to AnagraSign on
Sep 9 2026 (user's choice); folder, package.json, db filename, and all UI/doc text were updated.
A checkmark-with-flourish SVG logo mark (navy badge, two-tone "Anagra"/"Sign" wordmark) was added
to all four pages' top bars the same day.

Deployment (Sep 10 2026): grouprb.com is LIVE at https://sign.grouprb.com (deployed, SSL active,
login verified). Correction to earlier assumption — grouprb.com and partsofpractice.com are on
the SAME SiteGround account (one login, both GoGeek), not separate accounts. Real deploy mechanism
turned out different from the original SSH-based guide: SiteGround's Node.js hosting is a
completely separate "Node.js Projects" resource (Site Tools > Node.js, or account-level Websites >
Node.js Projects tab), deployed by uploading a .zip/.tar.gz through their wizard (or connecting
GitHub — not used here since the app isn't in its own repo). No SSH/git deploy was used.
`AnagraSign/docs/deploy-siteground.md` and `scripts/deploy-siteground.ps1` describe the OLD
(wrong) SSH approach and need rewriting to match the actual manual-upload flow before reuse.

Actual steps taken for grouprb.com: created Node.js Project (Node 24, Express preset auto-detected,
build command changed from default `npm run build` to `npm install` since there's no build step),
uploaded a tar.gz of the app (excludes node_modules/storage/.env/deploy), added env vars one at a
time via Site Tools UI (BASE_URL, ADMIN_PASSWORD, SESSION_SECRET, MAIL_FROM, NOTIFY_EMAIL,
TRUST_PROXY, SECURE_COOKIES — SMTP_* skipped, mailbox not created yet), clicked Save and Deploy
(requires re-uploading the same archive to actually trigger the deploy), created a `sign` subdomain
under grouprb.com's own Subdomains tool then deleted it again (wrong approach — caused a
"belongs to an existing web app" error), then correctly parked `sign.grouprb.com` via the Node
project's own Parked Domains tool, then issued a Let's Encrypt cert for it via Security > SSL
Manager since a new parked domain has no cert by default. Node project's SiteGround-assigned
hostname is `richardb918.sg-host.com` (siteId `Smd6eFlYc0ZJZz09`); grouprb.com's own siteId is
`S0F6M1lYOEZJQT09`.

Two-role login added (Sep 11 2026): one login screen (`public/login.html`) with an Admin/User
toggle. Shared passwords (not per-person accounts) stored scrypt-hashed in a new `app_auth` table,
changeable from a Settings modal in the app — no more redeploying to change ADMIN_PASSWORD.
`ADMIN_PASSWORD` env var only seeds the admin row on first run (or force-resets it if
`RESET_ADMIN_PASSWORD=1`, a lost-password escape hatch); there's no env var for the user password —
it starts disabled until an admin sets one from Settings. Permission split (my judgment call,
flagged to the user, not yet corrected): User can do everyday document work (upload/prepare/send/
import) but not void/delete or touch either password except their own; Admin can do everything.
Built, tested locally (53 smoke checks incl. new role tests) and in-browser (both roles' dashboard
views and Settings modal confirmed visually). Not yet deployed to sign.grouprb.com as of writing
this note — same deploy dance as everything else (repackage, Save and Deploy, re-upload archive).

CRITICAL platform bug found & fixed (Sep 11 2026): SiteGround's Node.js Projects extract every
"Save and Deploy" into a brand-new timestamped `app_source` folder and discard the old one —
confirmed by diagnostic write/redeploy/read testing. This means the default `storage/` folder
(database + every uploaded/signed PDF) was silently wiped on EVERY redeploy done this session.
No real user data was lost (grouprb.com had zero real production use yet, only my own test
envelopes, always cleaned up) but this would have destroyed real contracts going forward. Fixed:
`server/config.js` now honors `STORAGE_DIR` env var as an absolute-path override; set on
grouprb.com's live Node.js Project to `/home/customer/www/richardb918.sg-host.com/anagrasign-storage`
(one level above public_html, outside the versioned folder, confirmed to survive a redeploy).
`docs/deploy-siteground.md` was fully rewritten (the original was based on wrong SSH/git
assumptions, never actually verified) and `scripts/deploy-siteground.ps1` was rewritten to just
package the archive (no SSH — SiteGround Node.js Projects only accept upload-wizard deploys).
**Before ever repeating this for partsofpractice.com, set STORAGE_DIR there too from the start.**
Verified fixed and deployed: full sign flow + import both survived an actual redeploy afterward
(checked by redeploying once more with no other changes and confirming both test envelopes and
their PDFs were still there). Also noted: SiteGround's edge caches responses (even DELETEs)
aggressively by URL — use a cache-busting query string when manually verifying live behavior.

Import feature (Sep 10 2026): can now import already-signed PDFs from other e-sign tools for
record-keeping (`POST /api/envelopes/import`, `envelopes.status = 'imported'`). Stores the
original untouched plus an honestly-labeled "Import Record" page (source/date/note the importer
typed) — deliberately NOT styled like the real signature certificate, since AnagraSign never
witnessed that signing. Built and tested locally (43 smoke checks + manual API/browser checks);
not yet deployed to sign.grouprb.com as of this note — deploy it the same way as prior updates
(repackage tar.gz, Save and Deploy, re-upload archive) next time that site needs a push.

SMTP for grouprb.com (Sep 10 2026): grouprb.com's mail runs on Google Workspace, so a SiteGround
mailbox was never created — instead sends through smtp.gmail.com using an app password on
richard@grouprb.com (2-Step Verification + app password, not the account's real login password).
MAIL_FROM is "GroupRB <richard@grouprb.com>" (must match the authenticated Gmail account or
Gmail rejects/rewrites it — can't send as a different address without a configured Workspace
alias). Verified end-to-end with a real send via the API (created envelope, sent, got
`sent: true`, deleted the test envelope). Adding env vars in SiteGround's Node.js UI does NOT
apply until the app is redeployed — Save and Deploy still demands re-uploading the same archive,
same as the first deploy. When adding vars via that UI, click BACK and re-screenshot before each
one — reusing stale element refs across additions can silently concatenate the previous key/value
into the new one (hit this twice while adding SMTP_SECURE).

partsofpractice.com instance: not yet done, same process should apply once repeated.
Generated production secrets sit in `AnagraSign/deploy/grouprb.env` and `.../partsofpractice.env`
(git-ignored) — grouprb.env is now fully live-accurate; partsofpractice.env still has placeholder
SiteGround-mailbox SMTP settings that will need the same Google-Workspace-relay treatment if that
domain also uses Workspace mail.

**Why:** user asked whether an Adobe Sign-like contract app was doable and to create the folder
and working files; this is their own product idea, likely to pair with [[project-start-performance-platform]].

**How to apply:** for e-signature work, open this folder first and read its CLAUDE.md. Not a
client deliverable yet; branding per [[feedback-client-facing-branding]] if it becomes one.
