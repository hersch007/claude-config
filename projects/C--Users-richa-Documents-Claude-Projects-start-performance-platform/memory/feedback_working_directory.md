---
name: feedback-working-directory
description: Master source of truth is C:\Users\richa\Documents\Claude Projects\start-performance-platform
metadata: 
  node_type: memory
  type: feedback
  originSessionId: f256f0f7-3dd7-41b0-92c9-aaf30a545038
---

Always read and write plugin source files from `C:\Users\richa\Documents\Claude Projects\start-performance-platform\plugin\...` — this is the master source of truth. G: drive is no longer in use.

**Why:** G: drive was an old path that is no longer accessible or used.

**How to apply:** Every file read, edit, write, and zip build must use the `C:\Users\richa\Documents\Claude Projects\start-performance-platform\` base path.
