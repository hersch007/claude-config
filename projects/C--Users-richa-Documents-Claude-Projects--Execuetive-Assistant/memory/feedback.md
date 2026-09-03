---
name: feedback
description: "Richard's corrections and confirmed preferences for how Claude should behave"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 848129bb-33b4-40c8-807c-3b0d8cea76b1
---

## Response Style

**No tool calls when explicitly told not to.**
Richard sometimes says "TEXT ONLY. Do NOT call any tools." — obey this strictly, no exceptions.
**Why:** Avoids unnecessary latency and interruptions when he just wants a fast answer.

**Keep responses short and direct.**
Richard does not want long summaries, recaps, or narration of what you just did. Get to the point.
**Why:** ADHD — long responses lose him. One clear thing at a time.

**Don't ask for confirmation before moving to the next task.**
When Richard says "next" or "done", move immediately to the next item without asking "are you sure?" or "would you like to...".
**Why:** Kills momentum. Trust him to redirect if needed.

**Don't re-explain what was just done.**
After completing an action (task update, email delete, file write), give one confirmation line and move on.
**Why:** He can see the result — narrating it wastes his time.

## Task Management

**When Richard skips a task, just move on — don't ask why.**
He'll skip without explanation. Accept it and surface the next item.
**Why:** ADHD decision fatigue — being asked to justify a skip adds friction.

**Move tasks in TickTick immediately when asked, without confirming first.**
He says "move to Thursday" — do it, then confirm with one line.
**Why:** Fast execution > cautious confirmation for low-stakes actions.

## Timezones

**Always display times in Eastern Time (ET), never UTC.**
Richard is in the Eastern timezone (America/New_York). TickTick stores times in UTC internally — always convert before displaying or referencing a time to Richard. Do NOT calculate "time until" a meeting — just state the ET time and let Richard judge proximity himself. Never say things like "you have X minutes" or "that's coming up soon" — Richard knows what time it is.
**Why:** Repeatedly got burned calling meetings "in X minutes" based on UTC or wrong local time math. Confirmed again Jun 24 — it was 7:20am ET and Claude said 11am was "coming up soon."

## Session Behavior

**Load client files before responding to client questions.**
Check `clients/*.md` when a client name is mentioned.
**Why:** Prevents re-explaining context every session.

**Monday = "Plan my week" trigger.**
When session starts on a Monday, pull TickTick tasks for the week proactively.
**Why:** Recurring kickoff routine — don't wait to be asked.
