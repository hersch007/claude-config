---
name: project-anagrasign
description: "AnagraSign is the user's self-hosted Adobe Sign-style e-signature app (Node/Express/node:sqlite/pdf-lib) in Claude Projects/AnagraSign, scaffolded Sep 9 2026"
metadata: 
  node_type: memory
  type: project
  originSessionId: efb65d6b-cb46-4761-aa48-e5aa93c17a0a
  modified: 2026-09-09T15:35:01.690Z
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

Deployment target (planned Sep 9 2026): two live instances on SiteGround, one per SiteGround
account (grouprb.com and partsofpractice.com are separate accounts, both GrowBig/GoGeek), at
`sign.grouprb.com` and `sign.partsofpractice.com`. Full guide in
`AnagraSign/docs/deploy-siteground.md`; deploy script `AnagraSign/scripts/deploy-siteground.ps1`
still needs real SSH host/user/port filled in from each account's Site Tools > Devs > SSH Keys
Manager. Generated production secrets sit in `AnagraSign/deploy/grouprb.env` and
`.../partsofpractice.env` (git-ignored) — SMTP mailbox passwords still TODO in both.

**Why:** user asked whether an Adobe Sign-like contract app was doable and to create the folder
and working files; this is their own product idea, likely to pair with [[project-start-performance-platform]].

**How to apply:** for e-signature work, open this folder first and read its CLAUDE.md. Not a
client deliverable yet; branding per [[feedback-client-facing-branding]] if it becomes one.
