---
name: stp-code-authority
description: STP client code = Start Performance regardless of what the task note references
metadata: 
  node_type: memory
  type: feedback
  originSessionId: bcf93e5d-c164-4015-9c58-2940b764669c
---

For the [[daily-billables-workflow]] and Start Performance audit log: the 3-char **client CODE column is authoritative**, NOT the task note text. STP means Start Performance even when the note mentions another client's name (e.g. 6/1 "LawnAce Chat Prep for Cust Visit" is coded STP → Start Performance / unbilled, NOT LawnAce/Lawn Ace).

**Why:** STP work is often prep/internal work *about* other clients, so notes reference names like LawnAce, Accelecom, City of Rock Hill — but if the code is STP it belongs to Start Performance.

**How to apply:** classify every row by its client code, never by keywords in the note. Only STP rows go in the Start Performance audit log.

**STA is NOT STP** (Richard stressed this): STA is a separate non-billable code (admin / general meetings) that lands in the matrix STA column, never Start Performance. Exclude STA rows from the STP audit log even when they sit among STP rows. (June matrix sometimes bucketed a STA block into Start Performance, e.g. 6/15 — the audit log follows the tracker code, not the matrix.)
