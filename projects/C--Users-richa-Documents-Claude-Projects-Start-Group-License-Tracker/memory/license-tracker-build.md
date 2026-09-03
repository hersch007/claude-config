---
name: license-tracker-build
description: Status and architecture of the License Tracker multi-tenant SaaS build
metadata: 
  node_type: memory
  type: project
  originSessionId: dcfcd7ed-f8f7-4e7c-92fc-f2650d6e349f
---

Secure multi-tenant License & Subscription Management SaaS scaffolded 2026-06-25 in this repo (Next.js 15 App Router, TS, Prisma/Postgres, better-auth, Stripe, Tailwind+shadcn, Docker).

Core security model (fully implemented + verified): **envelope encryption** — random per-account DEK wrapped by a KEK derived from the account master passphrase via Argon2id (+ server `ENCRYPTION_PEPPER`); DEK held only in an in-memory per-session store (`src/lib/crypto/key-store.ts`, 30min sliding TTL). Passphrase change re-wraps the DEK (O(1), no data re-encryption) and bumps `keyVersion`. Tenant isolation chokepoint is `src/lib/tenant.ts` (`getTenantContext`/`tenantDb`/`requireDek`). License keys/sensitive notes/attachments are AES-256-GCM. Reveal endpoint is rate-limited + audited + no-store.

**Done:** crypto core, auth+signup(creates tenant), vault unlock/onboarding, license CRUD UI, dashboard, settings (members/billing/passphrase), Stripe webhook+checkout+portal, CSV/JSON/encrypted-backup export, .ics, REST `/api/licenses`, Docker+compose+README.

**Not yet built (scaffolded/TODO):** invite acceptance page `/invite/[token]`, attachment upload UI, bulk CSV import w/ column mapping, real Resend emails (dev logs to console), scheduled reminder/GDPR-purge jobs, Redis-backed rate-limit/key-store for horizontal scaling, Team-plan API-key guard.

VERIFIED 2026-06-25: `npm install` done, `npx tsc --noEmit` clean (exit 0), `next build` passes all 14 routes. better-auth integration compiles fine. Fixes applied during verification: (1) crypto module returns `Uint8Array<ArrayBuffer>` via `toStorable()`/`StoredBytes` type to satisfy Prisma `Bytes` (Node Buffer is now generic over ArrayBufferLike); (2) Stripe apiVersion pinned to `2025-02-24.acacia`, `sub.current_period_end` (not item-level); (3) Prisma JSON fields cast to `Prisma.InputJsonValue`; (4) login page wrapped `useSearchParams` in Suspense; (5) bumped Next 15.1.3→15.5.19 (critical CVE-2025-66478). Remaining `npm audit`: 2 moderate transitive postcss in Next internals — do NOT `audit fix --force` (downgrades Next to v9).

Baseline migration generated 2026-06-25 via `prisma migrate diff --from-empty` → `prisma/migrations/0_init/migration.sql` (+ migration_lock.toml), so `migrate deploy` works on a fresh server with no extra steps. Not runtime-tested against a live Postgres (no Docker on dev machine).

DEPLOY TARGET: User's WordPress is on cPanel SHARED hosting (cannot run this — PHP/MySQL only). Decision: run app on a cheap VPS (Option B, Hetzner/DO), keep WordPress on cPanel, point subdomain `licenses.startwebservicesbackup.com` (A record in cPanel Zone Editor) at the VPS. HTTPS via Caddy reverse-proxy to localhost:3000. NOTE: prod email verification needs RESEND_API_KEY or user must read the verify link from `docker compose logs app`. User: richard@grouprb.com (Start Group), domain startwebservicesbackup.com.
