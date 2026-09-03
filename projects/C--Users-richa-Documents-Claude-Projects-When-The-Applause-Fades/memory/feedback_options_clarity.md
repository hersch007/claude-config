---
name: feedback-options-clarity
description: "When presenting options, always lead with a clear recommendation and state the final answer plainly — don't make the user dig for it"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 723ce6a1-fc96-42fb-bf0f-6ca8a9427f99
  modified: 2026-07-28T11:45:46.387Z
---

When giving ranked options, the recommended option must be fully self-explanatory — the user should never have to ask a follow-up to understand what it means in practice. If the recommendation requires explanation about WHERE or HOW it applies, include that explanation inline with the option, not in a separate response.

**Why:** User twice said "geez" when options were unclear or when the recommended answer required additional back-and-forth to land. The fix should be obvious from the recommendation alone.

**How to apply:** After presenting options, if the recommended one involves replacing specific text, show the exact before/after inline. Never make the user ask "where does this go?" or "what is the final answer?"
