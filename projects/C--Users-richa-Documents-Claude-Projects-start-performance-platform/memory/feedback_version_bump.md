---
name: feedback_version_bump
description: "Always bump the plugin version number on every build, no exceptions"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: f256f0f7-3dd7-41b0-92c9-aaf30a545038
---

Always increment the version number on every build — either the minor segment (1.x.0) or the patch segment (1.1.x). Never rebuild without bumping first.

**Why:** WordPress uses the version string to detect new files. If the version doesn't change, the server serves cached/old files and changes don't take effect. This burned time early in the project when old templates persisted after upload.

**How to apply:** Before running build.ps1, bump the Version header AND the VERSION constant in EVERY plugin that was touched — core (start-performance.php), sp-tickets, sp-ai, and any future addons. If a build touches multiple plugins, all of them get a version bump. Don't bump only the one that changed.

**Zip naming:** Always include the version number in the zip filename — e.g. `start-performance-smti-sales-1.0.2.zip`. Never produce a versionless zip like `start-performance-smti-sales.zip`. This lets the user know exactly which build they are uploading.
