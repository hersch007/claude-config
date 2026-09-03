---
name: project-warehouse51
description: "Core concept, design decisions, and current build state for Warehouse 51 (Godot 4.7 game)"
metadata: 
  node_type: memory
  type: project
  originSessionId: 8ce03153-6c6e-4f4f-8887-c189706875aa
  modified: 2026-08-22T13:13:59.297Z
---

Warehouse 51 is a Godot 4.7 desktop game — an Arcane Library-style catalog/research loop reskinned into a dark conspiracy / Area 51 setting.

**Core differentiator:** The AI Oversight system IS the library. Every fully-researched item feeds the AI (knowledge + suspicion). Completing a series triggers a major AI feed event and phase shift. The player is always deciding: keep researching (personal goal) vs. starving the AI (survival).

**AI phases:** Dormant → Observing (20 knowledge) → Assisting (50) → Controlling (80) → Hostile (120).

**Series = conspiracy threads.** Completing a series grants series_feed_multiplier bonus to AI feed and triggers `series_completed` signal with major consequences.

**Platform:** Desktop only (Godot 4.7).

**Current build state (2026-08-22): FULLY 2D — pure backdrop + item cards.**
- main.tscn root is Node2D; main.gd extends Node2D
- ARCHITECTURE PIVOT: abandoned procedural 3D entirely. The Midjourney image (assets/Warehouse51 Background.png) is the scene — rendered as a full-screen TextureRect in CanvasLayer(-1).
- Items are PanelContainer cards (ColorRect-style) positioned at pixel coordinates calibrated to shelf cells in the backdrop image
- LEFT_SHELF_X=115, RIGHT_SHELF_X=1165, SHELF_ROW_Y=[172,282,388,488,578] for 1280×720
- Items alternate left/right shelves. Scroll offset slides all items horizontally (arrow keys, wheel, right-drag)
- Click via gui_input on PanelContainer; hover glow via mouse_entered/exited
- No 3D, no Camera3D, no Area3D — pure 2D Control nodes
- AIOverseer and ResearchManager autoloads unchanged
- Dossier panel (layer 11) and HUD (layer 10) unchanged from 3D version

**File structure:**
```
res://
├── project.godot              (autoloads: AIOverseer, ResearchManager; viewport 1280x720)
├── scenes/main.tscn           (single Node3D root + main.gd script)
├── scripts/
│   ├── item_data.gd           (Resource class — no class_name conflict)
│   ├── ai_overseer.gd         (extends Node, no class_name)
│   ├── research_manager.gd    (extends Node, no class_name)
│   └── main.gd               (extends Node3D — builds entire 3D world + 2D UI in code)
└── data/items/
    ├── blue_book_vol1/2/3.tres
    ├── neural_interface_proto.tres / _mk2.tres  (AI-sensitive, series_feed_multiplier=2.0)
    └── roswell_fragment.tres
```

**Why:** Locked in from Grok design chat — the "AI is the true library" hook chosen as primary differentiator, series/volumes as supporting system.

**How to apply:** When adding features, keep the research→feed→AI-phase-shift loop central. 3D scene is built entirely in code — no .tscn children, no external 3D assets. New items go in `data/items/` as `.tres` files. New room geometry uses `_room_box()` helper. New clickable objects need Area3D added to `case_area_to_item` dict.
