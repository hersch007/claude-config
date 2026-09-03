---
name: project-wts-lms
description: "New WTS instance (wts-app) + wts-lms addon — a file-based AMP LMS ported native under Knowledge Core. Built 2026-07-13."
metadata:
  type: project
  originSessionId: bce3fcb0-21e9-482d-b4c5-fd9cda46bc99
---

**New 4th instance: "Wireless Tower Solutions" at `/wts-app`** (`/home1/start6zs/public_html/wts-app`, URL startwebservicesbackup.com/wts-app). Created by Richard as a **clone of an sp site** (WP 7.0.1, prefix `wphu_`). Setup done 2026-07-13:
- Deactivated every core except **core + knowledge** (knowledge-only instance).
- Branding: `sp_platform_name` = "Wireless Tower Solutions", `sp_brand_initials` = WTS, `sp_accent_color` = **#29A8E0** (WTS sky blue; navy #1A4F8A used for LMS headings).
- Wiped all cloned demo data — TRUNCATE'd 37 `sp_*` data tables, kept `sp_team` (2 admin logins). Cleared cloned `sp_chat_*` demo options.
- deploy.ps1: added `wts` site (`/wts-app`); `wts` appended to **core + knowledge only** (NOT the other cores, so a deploy can't reactivate them); new `wts-lms` plugin key (sites=@("wts")).

**`wts-lms` addon (v0.1.0, plugin folder `plugin/wts-lms/`, deliberately NOT `start-performance-` prefixed per Richard):** wraps WTS's existing standalone AMP LMS as a native SP addon under the `knowledge-core` slot (nav item "AMP Training"). The original was a custom PHP+MySQL+session app at `/wts_documentation/training/` (own `users`/`courses`/`lessons`/`progress`/`quiz_results` tables, own login). Ported to native:
- **Content = file-based**, shipped INSIDE the plugin (`content/course1|course2/*.md` lessons + `*-quiz.json`). Course/lesson structure is a PHP manifest in `wts_lms_courses()` (no DB courses/lessons tables). Quiz JSON format: `{questions:[{question,options:{A..D},answer,explanation}]}`. Parsedown (safe mode) bundled in `includes/Parsedown.php`.
- **Auth = SP** (learners = SP team members; `wts_lms_actor_id()` = member id, super-admin previews as id 0). No second login. WTS staff manage learners via the native **SP Team page**.
- **Progress in SP tables** `{prefix}wts_lms_progress` + `wts_lms_quiz_results` keyed by (member_id, lesson_slug). **80% pass threshold** (`score >= ceil(total*0.8)`); passing auto-marks the lesson complete.
- Views (registered): `wts-training` (course grid + progress), `wts-course` (lesson list), `wts-lesson` (markdown content + checkpoint quiz with correct/wrong highlight + explanations + Try Again). Forms POST to `/sp-app/` with `sp_type=wts_lms` → `sp_post_handler_wts_lms` (actions: quiz_submit / mark_complete / mark_incomplete; last answers stashed in a transient for the results view).
- 2 courses: **AMP Fundamentals** (7 lessons incl. final exam), **AMP for Jurisdiction Reviewers** (7 lessons). Verified server-side end-to-end (render, grade, pass→auto-complete, progress).

**Security fix 2026-07-13:** deleted `/wts_documentation/training/install.php` — it was publicly reachable and printed the admin login in plaintext (`richard@grouprb.com / WTS-Admin-2026!`). If that password was ever live, it should be rotated.

**v0.3.0 (2026-07-14) — AMP Training REPLACES Knowledge Core's built-in Training.** Knowledge Core (`sp_kb_nav_items`, priority 10) adds three items under `knowledge-core`: `knowledge` (Knowledge Base), `kb-resources` (Resources), `kb-training` (Training). On WTS that duplicated training. Fix: `wts_lms_nav_items` now hooks `sp_nav_items` at **priority 20** (after Knowledge Core) and **swaps the `kb-training` item out in place** for AMP Training + Training Report — so the menu reads KB, Resources, AMP Training, Training Report, no duplicate. This is the pattern for a **custom module replacing ONE core sub-feature** (vs. [[feedback_custom_module_deactivation]]'s "deactivate the whole core plugin" — used when the client keeps the rest of the core, here the KB). Note: the built-in `kb-training` VIEW is still URL-reachable (Knowledge Core keeps it in `sp_allowed_views`), just unlinked + empty — benign. Verified the full filter chain server-side.

**v0.2.x–0.3.2 lesson polish:** left/right padding on lesson body, capped reading width (70ch on p/li/headings, tables full), heading top spacing (H2 2.4rem / H3 2rem), `---` HRs styled as invisible spacers (`.wts-content hr{border:none;height:0}`). **`includes/Parsedown.php` is a custom 160-line mini-parser (NOT real Parsedown) and had two table bugs fixed in 0.3.1/0.3.2:** (1) body rows concatenated the `tableRow()` array → printed "ArrayArrayArray" (fix: `implode`); (2) the separator regex `[-:| ]+` also matched `---`, so every horizontal rule was misread as a table separator — silently breaking real tables AND the spacers (fix: require the header line and separator to contain a `|`). Watch this parser for other markdown edge cases (it's minimal).

**NOT built yet / TODO:** admin **progress dashboard** (original had `admin/progress.php` + `users.php` — a "who completed what" view for WTS staff; user mgmt itself is the native SP Team page); **certificates** (original `course.php` linked a `certificate.php` that never existed); provisioning real learner accounts as SP team members. Visual/browser check still pending — needs a login to `/wts-app` (separate install, so the sp session cookie doesn't carry over). See [[core_platform_architecture]] (member core-access), [[plugin_versions]].
