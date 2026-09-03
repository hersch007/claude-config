---
name: feedback_ckeditor_wix_pattern
description: Working pattern for editing text in Wix Studio via CKEditor JS API in the claude-in-chrome extension
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 8148ba77-474b-44eb-b5e4-3957de4c623e
  modified: 2026-08-25T16:43:32.957Z
---

The only reliable method to change text in Wix Studio canvas using claude-in-chrome:

1. Double-click on the canvas element to reach Cell level
2. Double-click again to reach Text level (breadcrumb shows "Section Grid > Cell > Text")
3. Use `find` tool to locate "Edit Text" button, get its ref
4. Click the ref (e.g., ref_595) to enter CKEditor mode
5. **CRITICAL**: Call `editor2.focus()` FIRST via JavaScript before setData
6. Then call `editor2.setData(html, { callback: () => editor.fire('change') })`
7. Press Escape key — this triggers Wix to sync CKEditor content to its internal model

**Why:** Without calling `.focus()` first, CKEditor's focusManager.hasFocus remains false, and pressing Escape doesn't trigger Wix's save handler. The data gets set in the JS object but never synced to the canvas.

**Also:** Clicking the canvas after setData (without Escape first) causes Wix to reload the editor with its stale internal state, overwriting your changes. Always Escape before clicking elsewhere.

**How to apply:** Use this exact sequence for every text edit in Wix Studio. The editor2 instance handles the active text element. editor1 is a secondary instance — editor2 is the one wired to the Escape handler.

JavaScript snippets:
```javascript
// Step 1: Focus editor2
const CKE = window.CKEDITOR;
CKE.instances['editor2'].focus();

// Step 2: Set data
const editor = CKE.instances['editor2'];
editor.setData('<h2 class="font_2">Your text here</h2>', {
  callback: function() { editor.fire('change'); }
});

// Then press Escape key via computer tool
```

Classes for common HTML:
- H1: `<h1 class="font_0">text</h1>`
- H2: `<h2 class="font_2">text</h2>`  
- Body: `<p class="font_7"><span>text</span></p>`
