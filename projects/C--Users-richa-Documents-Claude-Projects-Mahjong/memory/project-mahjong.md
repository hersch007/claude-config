---
name: project-mahjong
description: "American Mahjong game built in vanilla HTML/CSS/JS — architecture, key bugs fixed, and current status"
metadata: 
  node_type: memory
  type: project
  originSessionId: bddec945-0b92-4ff5-8e7b-7d0151af6ef4
---

Vanilla JS American Mahjong game in `C:\Users\richa\Documents\Claude Projects\Mahjong`.

**Why:** User requested a full single-player / hot-seat American Mahjong game with NMJL rules, AI opponents, and rules/FAQ pages.

**How to apply:** Any follow-up requests about this project should reference this architecture.

## File layout
- `index.html` — game table (4-player grid layout)
- `rules.html` — searchable rules reference (10 sections)
- `faq.html` — accordion FAQ (jokers, card, Charleston, AI, comparisons)
- `css/main.css` — global styles, nav, content pages
- `css/tiles.css` — tile visual design (CSS-only, no images)
- `css/game.css` — game table grid, call bar, modals
- `js/tiles.js` — 152-tile deck builder, `MJ` global namespace (`var MJ`)
- `js/american-rules.js` — 29 NMJL-style hand patterns + joker/charleston rules
- `js/game-logic.js` — `GameState` class, deal/charleston/play/call/steal-joker
- `js/ai.js` — heuristic AI (discard scoring, call evaluation)
- `js/drag-drop.js` — pointer-event drag-drop for tile rack
- `js/ui.js` — DOM rendering (rack, discard pile, modals, call bar)
- `js/main.js` — event wiring, AI timing, button handlers

## Key bugs fixed during build
1. **Global namespace**: all files declared `const MJ = ...` — second declaration threw SyntaxError. Fixed by using `var MJ` in tiles.js only; all other files removed the declaration.
2. **DOMContentLoaded too late**: scripts at bottom of body means event already fired. Fixed by calling `MJ.App.init()` directly.
3. **Cyrillic characters** in event names (`charlestонDone` etc.) — corrected to ASCII.
4. **Charleston modal double-close**: `hideCharlestonModal()` called after `executeCharleston()` which already re-showed modal. Removed the extra hide call.
5. **Discard zone ID mismatch**: HTML had `discard-zone`, JS expected `discard-pile`. Unified to `discard-pile`.

## Dev server
`.claude/launch.json` configured: `npx serve -p 3131 .`
Server ID (may change): `c526cc5f-83f9-4eae-ab93-7782b6b46939`
