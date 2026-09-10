---
name: project-lawnace-chatbot
description: "LawnAce WordPress chatbot plugin — current version, file location, key contacts, and outstanding work items"
metadata: 
  node_type: memory
  type: project
  originSessionId: 155d6655-8ec7-43fc-a395-19479e1bd806
  modified: 2026-09-09T11:30:00.000Z
---

Current version is **v3.89.0**, saved as `lawnace-chatbot-3.89.0.zip` (built 2026-09-09, upload pending). v3.88.0 is LIVE. v3.89.0 reverts the WP sidebar menu icon to the original small SVG glyph — a 200px PNG as add_menu_page icon renders huge in the WP sidebar (Richard: "remove the logo on the sidebar"). Do not use lawnie-icon.png as a menu icon again; page-title logos are fine. v3.87.0 is LIVE (confirmed: AI-assistant greeting + disclaimer on site). v3.88.0: WP admin menu "Ace Analytics" renamed to "Lawnie" (page title "Lawnie Dashboard", menu icon = lawnie-icon.png), Settings page titled "Lawnie Settings", Lawnie icon in both admin h1s. No "Ace" as a product/bot name remains anywhere. v3.87.0: widget greeting now "Lawn Ace's AI assistant" (was "virtual assistant"); legal line `#la-disclaimer` under the widget footer ("Lawnie is an AI assistant. Answers are general information, not a quote or professional advice, and you act on them at your own risk. Chats are saved so our team can follow up." + Privacy link if WP privacy page is set). Richard chose this wording over a softer option; not lawyer-reviewed. v3.86.0: every "Ace"/"CLOVER" reference to the BOT itself renamed to Lawnie (lead email transcript label, email From name + footer, prompt sample-dialogue labels, admin "Lawnie Intelligence" panel); Lawnie icon added to lead + digest email headers via `lawnace_email_logo_html()` in lead.php. "Ace Analytics" (wp-admin menu) intentionally kept — that is the dashboard name, not the bot. v3.84.0 built same day (digest footer trim) — check whether Richard uploaded it; v3.83.0 confirmed live with digest working.

**v3.83.0 adds:** (1) lead tag now carries phone + address: `[LEAD_CAPTURED:name=,email=,phone=,address=]`, parsed in lead.php (`lawnace_chatbot_parse_lead`, `lawnace_lead_row_text`, `lawnace_parse_lead_row`, `lawnace_merge_lead`); a second tag in the same session merges into the existing lead row and sends an "Updated Lead Info" email instead of duplicating. Lead row text format: `Name: X | Email: Y | Phone: Z | Address: A`. Recent Leads table and task rows show tel:/mailto:/maps links. (2) Morning digest (`includes/digest.php`): WP-Cron hook `lawnace_morning_digest` daily at 8:00 site time; settings `lawnace_digest_enabled` (default on) and `lawnace_digest_emails` (comma list, falls back to lead notification email); "Send Test Digest Now" button on Settings. Shared task queries live in `lawnace_client_task_sources($start,$end)` in client-dashboard.php. Depends on WP-Cron, so a quiet site can delay the send until first visitor after 8am.

**LIVE SITE is `https://startwebservicesbackup.com/lawnace/`** (WordPress install where the plugin runs; Richard calls this the live site). Not lawnace.com.
- Customer/client dashboard (Kyle's team, the ONLY external dashboard): `https://startwebservicesbackup.com/lawnace/la-team/`
- WP admin dashboard: `https://startwebservicesbackup.com/lawnace/wp-admin/admin.php?page=lawnace-chat-logs` (sidebar: Ace Analytics)
- Plugin upload form: `.../wp-admin/admin.php?page=lawnace-settings` — always update there, never Plugins → Add New.
- WP admin menu is now "Lawnie" (was "Ace Analytics" until v3.88.0); same slugs (lawnace-chat-logs, lawnace-settings).
- The public `/ace-dashboard/` PIN dashboard was RETIRED in v3.82.0 at Richard's request (file moved to `Chatbot/retired/public-dashboard-3.81.0.php`; Admin Dashboard PIN setting removed). Do not bring it back.

Plugin directory: `C:\Users\richa\Documents\Claude Projects\Chatbot\lawnace-chatbot\`

**Key contacts:** Kyle Flanagan (kyle@lawnace.com, President) and Tammi Atkins (tammi@lawnace.com). Dashboard walkthrough meeting with both held 2026-09-09 11:30am ET.

**What's been built:** Full versioned WordPress plugin with dashboard, analytics, AI insights, photo upload, lead capture, sales-focused system prompt, visual polish (animations, online dot, gradient header), and complaint handling.

**Zip build:** use Windows bsdtar so entry paths use forward slashes (PowerShell Compress-Archive writes backslashes, which breaks WP uploads):
`/c/Windows/System32/tar.exe -a -cf lawnace-chatbot-X.Y.Z.zip lawnace-chatbot` run from the Chatbot folder. Lint first with local PHP 8.4 (see [[reference-local-php]] in the Claude Projects memory for the php.exe path).

**Recent major changes:**
- v3.81.0 (2026-09-09): Soil testing content — Lawnie was wrongly saying we don't do soil tests. Added Soil Testing to add-on services list, a SOIL TESTING Q&A script (before FALL WEBWORMS), and changed the "wrong" yellow-grass example so it no longer discourages soil tests. Also made the client dashboard mobile-responsive (820px and 560px breakpoints in client-dashboard.php; conversation-detail page stacks transcript rows as cards).
- v3.80.0: Fall webworm Q&A script (harmless to lawns, we do NOT treat large trees, refer to arborist)
- Lawnie mascot icon used as launcher and avatar — v3.78.0–3.79.0 (`assets/images/lawnie-icon.png`)
- Renamed bot from "Clover" to "Lawnie" throughout (trademark issue) — v3.69.0
- Tasks tab, Sales/Service cards, Service Area Inquiries by ZIP — v3.62.0–3.68.0
- Bot name in all files: system-prompt.php, widget.js, client-dashboard.php

**Standing rule:** At the end of every work session, bump the version number, rebuild the zip, and remind the user to upload it.

**v3.85.0 (built 2026-09-09) adds:** lead status pipeline (new/contacted/quoted/won/lost; Won/Lost auto-complete the task), assignee dropdown (names from setting `lawnace_team_members`, default "Kyle, Tammi, Tim"), per-task notes (max 2000 chars, saves on blur), all stored in option `lawnace_task_meta[sid]`; AJAX action `lawnace_task_meta`. Lead Pipeline panel + Status/Assigned columns on Sales tab. CSV export at `/la-team/?export=leads&range=…` (BOM, formula-injection guard). Lead Notification Email now accepts a comma list (`lawnace_sanitize_email_list`, `lawnace_notify_recipients()`).

**Declined by Richard (2026-09-09), do not re-suggest:** pushing chatbot leads to Kyle's Zapier hook / CRM.

**Pending:** nothing approved. Ideas not yet raised: real server cron for the digest if WP-Cron proves late.

**Why:** Active live client project; Kyle sends content corrections by email as customers hit gaps.

**How to apply:** When user says "update lawnace plugin" without details, ask what changed; check Gmail from kyle@lawnace.com for the request. Content fixes go in system-prompt.php as a Q&A SCRIPT block plus a line in the relevant services list.

**Team manual (2026-09-09):** `Chatbot/Lawnie-Dashboard-Team-Guide.docx` (+ .pdf), 16 pages, sign-in URL printed as https://lawnace.com/la-team/ at Richard's request (the plugin will move to lawnace.com), built with docx-js from `scratchpad/manual/build-manual.js` (session 29804b27). Dashboard screenshots are MOCKS rendered from the plugin's real CSS with sample customers (no login needed); chat screenshots are live from the site. To regenerate after UI changes: re-run render-tabs.php + puppeteer capture, then build-manual.js, convert with Word COM (no LibreOffice), pdftoppm is at the winget Poppler path. Richard wants the manual SIMPLE — plain language, no jargon.
