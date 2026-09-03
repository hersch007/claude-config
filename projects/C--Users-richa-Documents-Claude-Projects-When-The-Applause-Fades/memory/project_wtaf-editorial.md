---
name: project-wtaf-editorial
description: Editorial pass completion status and rules for When The Applause Fades
metadata: 
  node_type: memory
  type: project
  originSessionId: 723ce6a1-fc96-42fb-bf0f-6ca8a9427f99
  modified: 2026-08-23T00:25:59.373Z
---

Full chapter-by-chapter editorial pass completed on all chapters (35–55/epilogue) on 2026-08-22.

**Editorial rules applied:**
- Remove adverbs from speech tags ("he said quietly" → "he said")
- Remove adverbs from significant narrative actions ("she exhaled slowly" → "she exhaled")
- No contractions in narration/interior monologue; contractions allowed freely in dialogue and text messages
- Strict single POV per chapter (multi-POV chapters labeled WORLD POV or HOME POV are omniscient)

**How:** Each chapter: unzip .docx → edit word/document.xml → rezip → copy to _draft-manuscript folder. Node.js scripts handle both curly (U+2019) and straight apostrophes. Run-fragmented text (proofErr XML elements splitting words) handled with XML-level search.

**Status:** All chapters edited and saved to `_draft-manuscript\`. Manuscript is complete.

**Why:** Author-directed editorial polish pass before manuscript is considered final.
**How to apply:** If further chapters are added, apply same three rules.
