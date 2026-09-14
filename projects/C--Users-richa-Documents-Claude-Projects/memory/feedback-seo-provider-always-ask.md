---
name: feedback-seo-provider-always-ask
description: "Never auto-infer the SEO audit \"provider company\" (billing/letterhead) from CLIENT-BRIEF.md or any other doc — always ask the user in chat, even when a source looks unambiguous."
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 0e6f26ce-cedb-49f1-b2a0-6d12cf84d2c4
  modified: 2026-09-14T11:41:23.896Z
---

When running an SEO audit via [run-seo-audit skill](C:\Users\richa\Documents\Claude Projects\SEO-Clients-Hub\.claude\skills\run-seo-audit\skill.md), always ask the user in chat which of the four provider companies (Start Advertising, Start Performance, Parts of Practice, GroupRB) the report should be prepared/billed under — even if `CLIENT-BRIEF.md`'s "Prepared by" field appears to name one unambiguously.

**Why:** On 2026-09-14, when rerunning the Joe Welch Photography audit, Claude auto-selected "Parts of Practice" because `CLIENT-BRIEF.md` said "Prepared by: Parts of Practice" — without asking first. The user stopped the tool call and corrected this ("SHOULDNT YOU ASK ME?"). This is a real business/billing decision (it determines the agency name, contact email, and branding shown on a client-facing report) — exactly the kind of consequential, easily-wrong-if-guessed choice that should always be confirmed with the user, not inferred from a doc field that could be stale.

**How to apply:** This applies broadly, not just to the provider prompt — for [SEO-Clients-Hub](C:\Users\richa\Documents\Claude Projects\SEO-Clients-Hub) work generally, treat "which of Richard's several business entities/brands does this apply to" as a question to ask, not infer, unless the user has explicitly pinned it (e.g. a `"provider"` field already set in a client's `seo-tool/clients/<slug>.json` config from a prior run — that's an explicit prior answer, safe to reuse without re-asking). See [client-facing-branding](feedback-client-facing-branding.md) for the related rule on which brand identity to use in client deliverables generally.
