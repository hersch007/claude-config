---
name: feedback-version-bump
description: "Always increment plugin version for every build, even minor fixes — same version number silently fails on upload"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 4b25fea1-3b48-4f94-82ed-7fc14e04252d
  modified: 2026-07-21T13:15:08.974Z
---

Always bump the version number on every single build, no exceptions — including small one-line CSS fixes, wording changes, etc.

**Why:** WordPress plugin upload silently fails or does nothing when the version number hasn't changed. The user has been burned by this multiple times.

**How to apply:** Before running build.ps1, always edit both the `* Version:` header AND the `define('SP_VERSION', ...)` / `define('SP_WTS_VERSION', ...)` constant to the next number. Never re-build a zip at the same version even as a "quick fix."
