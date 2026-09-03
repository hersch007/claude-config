---
name: feedback-smti-accent-on-black
description: "Design gotcha — SMTI's brand accent (#111827) ≈ its black sidebar, so any raw --sp-accent nav highlight is invisible there; composite translucent white over the accent"
metadata:
  type: feedback
  originSessionId: bce3fcb0-21e9-482d-b4c5-fd9cda46bc99
---

**SMTI's brand accent is `#111827` (near-black) and its sidebar is pure black `rgb(0,0,0)`.** So any sidebar element coloured with raw `var(--sp-accent)` or `var(--sp-accent-bg)` (= `rgba(17,24,39,.18)`) is effectively invisible on SMTI — channel delta ~17, imperceptible. SP and Fruth don't show this because their accent is red `#CC1F1F` on a dark-navy sidebar `rgb(15,23,41)`.

**The fix (use every time you add an accent-coloured nav highlight):** composite a translucent-white layer over the accent so there's always contrast against the sidebar, while the accent still carries brand colour where it's vivid:
```css
background: linear-gradient(rgba(255,255,255,.06),rgba(255,255,255,.06)), var(--sp-accent-bg);
```
A solid `border-left`/`border` can't be composited, so render the bar as a `::before` pseudo-element whose background is `linear-gradient(rgba(255,255,255,.3),rgba(255,255,255,.3)), var(--sp-accent)`. On SMTI that resolves to `rgb(88,93,104)` (visible grey on black); on SP it's a lightened red (still clearly accent).

**Why:** brand-driven accent theming can't assume the accent contrasts with the sidebar — a client may pick an accent nearly equal to their sidebar colour. Highlights must stay visible regardless.

**How to apply:** never colour a nav highlight with bare `--sp-accent`/`--sp-accent-bg`. Layer translucent white. Bitten twice: the active-nav pill (core 2.5.10) and the Intelligence-Core "hero" highlight (core 2.5.17). See [[core_platform_architecture]] (branding vars) and [[plugin_versions]].
