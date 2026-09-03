---
name: annies-spa-image-conventions
description: "Image sizing, naming, and optimization workflow agreed for the Annie's Spa site"
metadata: 
  node_type: memory
  type: project
  originSessionId: 9f7fc22d-07f3-498d-b5fd-db4859706835
  modified: 2026-07-21T19:48:15.431Z
---

Annie's Spa images are optimized before upload: content photos resized to 1200px wide, hero banners to 1920px, JPEG quality 82. Filenames are lowercase-hyphenated and end in `-rock-hill-sc` (e.g. `foot-scrub-rock-hill-sc.jpg`). Optimized files uploaded 2026-07-21 live in `/wp-content/uploads/2026/07/`; anything still in `/2026/06/` is an unoptimized original.

**Why:** Originals were uploaded straight from stock sources at 2560px (WordPress appends `-scaled` above that threshold), so a 508px slot was being fed a 2560px file. The batch cut 3.35 MB to 910 KB across 12 images.

**How to apply:**
- No ImageMagick or Pillow on this machine. Resize/re-encode with PowerShell + `System.Drawing` (`HighQualityBicubic`, JPEG encoder quality param). Works fine, including PNG alpha.
- Uploading a duplicate filename makes WordPress append `-1` — check for that before writing `src` paths.
- The storefront photo `Annie-Spa-Building.jpg` is only 470x416 but displays at 622px, so it is upscaled and soft. Re-compressing it is not worth the generational loss; it needs a genuinely higher-resolution replacement.
- Adobe Stock is a source for spa photography here — commercial license should be confirmed for client sites.

See [[annies-spa-site-setup]].
