---
name: annies-spa-site-setup
description: "Annie's Spa WordPress site — staging URL, subdirectory install, page slugs, and the phone-first CTA convention"
metadata: 
  node_type: memory
  type: project
  originSessionId: 9f7fc22d-07f3-498d-b5fd-db4859706835
  modified: 2026-07-21T19:48:05.092Z
---

Annie's Spa (massage/body wellness, 2246 Celanese Road, Rock Hill, SC 29732, 803.417.8549) is a WordPress/GeneratePress site staged at `https://startwebservicesbackup.com/annie/`. Pages are edited by pasting hand-written HTML into Custom HTML blocks; the local project folder holds no source files.

**Why:** The install lives in the `/annie/` subdirectory, so root-relative links like `/services/` resolve to the domain root and 404. Internal links must be `/annie/service/`, `/annie/contact-annies-spa-rock-hill/`, `/annie/`. These all need `/annie` stripped when the site moves to its production domain.

**How to apply:**
- Page slugs are `/annie/service/` (singular) and `/annie/contact-annies-spa-rock-hill/`, not the names implied by the nav labels.
- No online booking exists. Every CTA is `<a href="tel:+18034178549">Call Now For Appointment</a>`. Any `/appointments/` link is dead.
- Hours are Monday–Sunday 8:30 AM – 8:00 PM. Gift certificates and coupons are offered; 5% card fee, Visa/Mastercard, no checks, gratuity not included.
- Custom markup is namespaced `ans-`. The custom footer is `.ans-custom-footer` inside page content; the GeneratePress theme footer `.site-info` renders below it and is hidden via CSS.
- As of 2026-07-21 the Services page intentionally keeps its older single-column footer and two `/appointments/` links — the user reviewed both and chose to leave them.

See [[annies-spa-image-conventions]].
