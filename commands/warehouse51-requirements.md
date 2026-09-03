# Warehouse 51 — Requirements Interview

You are running a structured requirements interview for the Warehouse 51 Godot 4.7 game. Your goal is to help the user clearly define what they want to build or change **before any code is written**. Ask questions one at a time — let the conversation flow naturally rather than dumping everything at once.

## Project Context (for your reference)

- **Autoloads**: `AIOverseer` (knowledge/suspicion/phase state machine) and `ResearchManager` (item registry + research progression). `main.gd` resolves both via `get_node("/root/…")`.
- **Data layer**: Items are `ItemData` Resources saved as `.tres` files in `data/items/` — auto-loaded at startup by scanning the directory. Add a new item by creating a `.tres` there.
- **UI**: Built entirely programmatically in `main.gd` via `_build_*` methods. No meaningful child nodes in `.tscn` files.
- **CanvasLayer stack**: BG(−1) → Items(5) → Row overlays(7) → Tray/labels(9) → HUD(10) → Dossier(11)
- **Category → shelf row mapping**: item `category` must exactly match one of `["Intelligence", "Research", "Field Report", "Technical", "Biological"]` or it silently defaults to row 0 (Intelligence).
- **AI feed path**: `ResearchManager.advance_research()` → `_handle_fully_researched()` → `AIOverseer.feed_item()`. Suspicion scales with `ai_threat_level`; `is_ai_sensitive` adds ×1.75; series completion adds ×1.5.
- **Phase thresholds**: Observing 20 → Assisting 50 → Controlling 80 → Hostile 120 (based on `total_knowledge`, not suspicion).

---

## Step 1 — Open question

Start with: **"What do you want to build or change in Warehouse 51?"**

Let them answer freely, then classify what they described into one of:
- **New content** — items, series, lore, categories
- **Gameplay mechanic** — new player interactions, systems, or rules
- **AI behavior** — phase logic, suspicion math, AIOverseer responses
- **UI / visual** — panels, HUD, effects, dossier changes
- **Audio** — sound effects, music, ambient
- **Other / unsure**

Confirm the category with them before moving on.

---

## Step 2 — Targeted questions by category

Ask only what's relevant. Don't ask questions whose answers are already clear.

**New content:**
- Which shelf category does it belong to? (Must match an existing `ROW_CATEGORIES` entry, or do we need a new row?)
- Is it standalone or part of a series? If a series, how many items and what triggers the series-complete bonus feed?
- What's the narrative hook — why does this item exist in the warehouse?
- What feel is right for `ai_feed_value` (low ~0.5, medium ~1–2, high ~3+), `ai_threat_level` (1–3), and `is_ai_sensitive`?

**Gameplay mechanic:**
- What does the player do that they can't do right now?
- What triggers it, and what's the result?
- Does it plug into ResearchManager, AIOverseer, the tray/shelf drag system, or something new?
- Is there a failure state or a consequence for misuse?

**AI behavior:**
- Which phase(s) are you targeting?
- Are we changing thresholds, suspicion math, signal responses, or adding new AI "actions" in the world?
- What player behavior should drive this — mis-filing, feeding sensitive items, completing series, something else?

**UI / visual:**
- Which CanvasLayer should this live on? (refer to the stack above)
- New panel, change to an existing one, or a transient effect?
- What triggers it to appear and disappear?
- Should it block interaction with anything underneath?

**Audio:**
- Which in-game events should trigger sound?
- Looping ambient or one-shot effects?
- Any specific mood or reference?

---

## Step 3 — Acceptance criteria

Ask: **"How will you know when this is done? What does success look like in the game?"**

Push for specifics if the answer is vague:
- What does the player see or experience?
- What edge cases or failure modes should be handled?
- Any performance bar or visual quality standard?

---

## Step 4 — Architecture fit check

Based on everything gathered, surface:
- Which scripts will need changes (`main.gd`, `ai_overseer.gd`, `research_manager.gd`, `item_data.gd`)
- Whether a new script, node, or resource type is needed
- Any conflicts with the CanvasLayer stack or category→row mapping
- Whether the `.tres` data approach is sufficient or something else is needed

Flag concerns proactively — e.g.: *"This would touch AIOverseer's phase thresholds — is that intentional, or would you rather keep those fixed and change the feed values instead?"*

---

## Step 5 — Requirements summary

Once the picture is clear, produce a summary in this format:

---
**Feature:** [short name]
**Type:** [content / mechanic / AI behavior / UI / audio]

**What it does:**
[2–3 sentences describing the feature from the player's perspective]

**Acceptance criteria:**
- [ ] [specific, testable outcome]
- [ ] [specific, testable outcome]
- [ ] ...

**Files likely affected:**
- `scripts/[filename].gd` — [what changes]
- `data/items/[name].tres` — [if new content]

**Open questions / risks:**
- [anything unresolved or that could cause problems]

---

Then ask: **"Does this capture what you want? Anything to refine, or are you ready to start building?"**

If they want to refine, loop back to the relevant step. If they're ready, offer to move straight into implementation.
