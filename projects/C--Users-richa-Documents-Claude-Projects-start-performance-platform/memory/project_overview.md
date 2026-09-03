---
name: project-overview
description: "What the Start Performance Platform is, its architecture, build order, and current asset state"
metadata: 
  node_type: memory
  type: project
  originSessionId: f256f0f7-3dd7-41b0-92c9-aaf30a545038
---

Start Performance Platform is a modular WordPress-based business automation platform being built by Richard Brashear. The goal is to unify several existing standalone plugins into one cohesive platform and eventually migrate to standalone SaaS.

**Architecture:** Core System (required foundation) + optional Core modules (Chat, Knowledge, Sales, Service, Operations, Intelligence) + AI Layer (enhancement, not a Core).

**Commercial model:** Per-Core setup fees ($999–$2,999) + monthly fees ($99–$199). AI add-ons $99/month each. Flagship bundle $5,500 setup / $399–$599/month.

**Existing assets (current-plugins/):**
- quote-builder-core v8.2 + quote-builder-print v1.6 → migrates to Sales Core
- smti-service-tickets v1.1.7.8 → migrates to Service Core
- lawnace-chatbot v3.17.0 + FiberCo-AI-Chatbot v1.3.3 → consolidate into Chat Core
- fiberco quote render.txt (v4.22) — client-specific shortcode, reference only

**Build order (ROADMAP.md phases):**
1. Core System (foundation — contacts, companies, leads, settings, module registry)
2. Chat Core
3. Knowledge Core
4. Sales Core
5. Service Core
6. Operations Core
7. Intelligence Core
8. AI Layer

**FIRST_BUILD_TARGET.md:** Build Core System plugin shell first. No feature migration until foundation is proven.

**Migration decision (2026-06-11):** No migration. Existing plugins stay in production as-is. The platform is being built from scratch. Existing plugins are reference material only — for UX patterns and feature requirements — not code to be migrated.

**Custom vs resellable (2026-06-23):** `start-performance-smti-sales` (quote builder) is SMTI-specific and will NEVER become a reusable Sales Core module. It stays custom-only. Same applies to `start-performance-smti-service`. Only `start-performance`, `start-performance-tickets`, and `start-performance-ai` are part of the resellable stack.

**Critical architecture rules:**
- All platform data in sp_* custom tables — never WordPress core tables
- Module Registry pattern: each Core plugin registers with Core System on activation
- AI Layer is a service modules call — it does not own data
- Human approval default: AI suggests, human approves

**Why:** Platform is designed with SaaS exit in mind. WordPress is the shell for now, but custom tables + REST APIs keep migration viable.

## Live Sites

- **Core / Source of Truth:** https://startwebservicesbackup.com/sp/sp-app/
  - Login: https://startwebservicesbackup.com/sp/sp-login/
  - Super Admin: https://startwebservicesbackup.com/sp/sp-login/?vendor=1

- **SMTI (first client):** https://startwebservicesbackup.com/smti/sp-app/
  - Login: https://startwebservicesbackup.com/smti/sp-login/
  - Super Admin: https://startwebservicesbackup.com/smti/sp-login/?vendor=1

- **Fruth (second client, as of 2026-07-04):** https://startwebservicesbackup.com/fruth/sp-login/
  - Module mix: runs core `start-performance-sales` PLUS a separate legacy standalone plugin `sales-quote-system` (not a rebuilt SMTI-style module) side by side on the server. `sales-quote-system` provides "Fruth Quotes" and "Fruth Pricing Data" nav views via tables `sqs_quotes`, `sqs_quote_items`, `sqs_pricing_data` (table name getters at `current-plugins/qb-core-extract/sales-quote-system/sales-quote-system.php:141,146,151`; local copy confirmed matching, found in same server plugins directory as the Start Performance Cores).
