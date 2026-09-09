---
name: feedback-srb-version-numbers
description: Increment PPTX version number each rebuild for Steph Brashear media kit
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 772e0b5b-91f9-4f33-a68f-53995a6818d3
  modified: 2026-09-08T18:33:42.844Z
---

Always bump the version number when rebuilding the SRB media kit PPTX. Current version is v12.

**Why:** User expects versioned files so they can track which copy is current.

**How to apply:** Before rebuilding, rename the build script (e.g. `build-srb-v4.js`) and update the output filename inside it. Output file goes to the project folder as `srb-media-kit-v4.pptx`. Same applies if the HTML 2-pager ever gets versioned.
