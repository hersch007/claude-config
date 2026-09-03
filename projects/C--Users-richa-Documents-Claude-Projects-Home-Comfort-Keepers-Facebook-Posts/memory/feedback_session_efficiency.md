---
name: feedback-session-efficiency
description: "How to run HCK post sessions efficiently — default behaviors, request formats, and what to avoid"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 0baaf57e-c392-41ad-8f90-978f4c42e023
---

Default caption length is 4 lines max — short and punchy. Never write long captions that need to be cut down after. Only expand if user explicitly asks.

**Why:** Almost every caption in the first session was too long and required a separate "shorten" request, wasting tokens and round-trips.

**How to apply:** Write tight by default. 3–5 lines. One idea per line. Stop.

---

Always ask for or wait for image style direction before writing the image concept. Do not guess. If not provided, ask in one line: "Image style — lifestyle, product shot, or industry graphic?"

**Why:** Multiple posts required 2–4 rounds of image concept revisions because the direction was assumed rather than confirmed.

**How to apply:** Before writing image concept, confirm: lifestyle (back-to-camera, warm light), product (clean background, premium), or industry (stat-based, brand graphic).

---

Never position Home Comfort Keepers as an HVAC company. They are an insulation, attic sealing, air quality, and remodeling company. They install products that work WITH existing HVAC — they do not service or install HVAC systems.

**Why:** A caption described them as an HVAC company, which is inaccurate and off-brand.

**How to apply:** Review any caption that mentions HVAC — framing should be "works with your existing HVAC" not "our HVAC services."

---

Proactively offer to save every post immediately after the user approves it. Do not wait to be asked.

**Why:** User had to ask "do you store it?" multiple times, indicating the save step wasn't automatic enough.

**How to apply:** After any user approval signal (yes, I used this, good to go, etc.) — immediately save to the correct folder without being asked.

---

Skip multi-choice clarifying questions when possible. User often answered with something not on the list anyway.

**Why:** Questions added round-trips without improving output quality.

**How to apply:** If the topic and day are clear, just write the post. Only ask if genuinely blocked.

---

Ideal request format from user (remind them if session starts scattered):
> Day + Service/Product + Vibe + Image style
> Example: "Thursday. Attic ventilation. Summer heat angle. Industry product shot."

Ideal weekly brief format:
> "This week: Mon lifestyle, Tue [product], Wed [product], Thu [service], Fri Homey. Short captions. [image style] unless noted."

**Why:** A single brief message at session start can replace most back-and-forth and cut session length in half.
