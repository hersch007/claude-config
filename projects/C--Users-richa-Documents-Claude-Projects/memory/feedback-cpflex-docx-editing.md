---
name: feedback-cpflex-docx-editing
description: How to safely edit the CPFlex Word running logs — they were corrupted on Sep 5 2026 by an in-place zip edit; repair recipe and rules
metadata:
  type: feedback
---

The two CPFlex Word logs (`Garlock-On-Page-SEO-Audit-Running-Log.docx`, `Garlock-Blog-SEO-Audit-Running-Log.docx`) were unreadable in Word from 2026-09-05 until repaired on 2026-09-07. Cause: an in-place zip edit dropped `[Content_Types].xml` and wrote backslash entry names. Word reports "The file appears to be corrupted."

**Rules:**
- Never rebuild from `Page Audits/build-running-audit.js` — the builder is stale (no billing lines, wrong statuses). Edit the docx XML in place instead.
- When editing docx XML in PowerShell: read parts with `ZipFile::OpenRead`, then write a NEW zip in `Create` mode with forward-slash names and `[Content_Types].xml` first. Never use `ZipFile::Open(..., 'Update')` + entry Delete/Create.
- Word-test every docx after writing (COM: `Documents.Open(path,$false,$true)` with a backslash path — forward-slash paths with spaces fail).
- The Bash tool collapses `\` to `\` inside heredocs — write PowerShell scripts with the Write tool, or use `[char]92`.
- A valid reference copy with `[Content_Types].xml` exists at git commit 0243ad4 (Sep 3 2026); repair script pattern is in the 2026-09-07 "Garlock SEO" session (repair2.ps1).

**Why:** Richard relies on these logs as the source of truth for billing; a corrupt file silently blocks his work.
**How to apply:** Any session that touches these docx files must run the Word open test before finishing. See [[project-cpflex]].
