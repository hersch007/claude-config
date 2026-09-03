---
name: project-seo-audit-template
description: "Master HTML template for SEO audit reports — location, design specs, agency switcher details"
metadata: 
  node_type: memory
  type: project
  originSessionId: 2daf382b-9c9f-4441-9fd7-89088dd4afe7
  modified: 2026-08-31T14:25:30.136Z
---

# SEO Audit Report Master Template

**File:** `C:\Users\richa\Documents\Claude Projects\SEO-Clients-Hub\_templates\SEO-AUDIT-TEMPLATE.html`

**Origin:** Reverse-engineered from `clients\Fruth\Fruth-Custom-Packaging-SEO-Audit-2026-08-14.html`, saved as master on 2026-08-31.

**Why:** User confirmed this is the definitive visual format for all client audit reports. Previous attempts used a custom design and were rejected.

**How to apply:** For every new or updated client HTML audit report, copy this template and replace placeholder values. Do NOT create a new visual design from scratch.

## Design Specs (do not change)
- Font: 'Segoe UI', Arial, sans-serif (system fonts — no Google Fonts)
- Background: #f5f6fa
- Cover: #003366 (dark navy)
- Accent / borders: #003366
- Body text: #1e293b
- Good: #16a34a | Warn: #d97706 | Bad: #dc2626 | Neutral: #003366
- Cards: white, border-radius 10px, box-shadow 0 1px 4px rgba(0,0,0,.07)
- Section titles: uppercase, border-bottom 2px solid #003366
- Quick win cards: border-top 3px solid #003366
- Performance cards: border-bottom 3px solid #003366

## Agency Switcher
All reports include a dropdown in the cover (top-left) to switch the "Prepared by" attribution. Persists via localStorage.

Three agencies:
- Start Advertising (startadvertising.com)
- GroupRB Marketing (grouprb.com)
- Parts of Practice (partsofpractice.com)

localStorage key pattern: `[client_initials]_audit_agency`
- Fruth: `fruth_audit_agency` (default: Start Advertising)
- G&G: `ggc_audit_agency` (default: GroupRB Marketing)

## Score Labels
- 85-100 → Good
- 70-84 → Needs Attention
- 50-69 → Needs Work
- 0-49 → Critical

## Score Trend SVG
X positions by number of data points (viewBox 0 0 520 110, x range 36-500):
- 5 pts: 36, 152, 268, 384, 500
- 6 pts: 36, 129, 222, 314, 407, 500

Y formula: `y = 82 - ((score - minScore) / scoreRange) * 72`
Current data point: r=5, fill="#fff" (improving) or fill="#fbbf24" (declining)

## Published Artifacts
- Fruth Aug 31: https://claude.ai/code/artifact/751d532b-0050-4265-ad88-e069e321b49d
- G&G Aug 31: https://claude.ai/code/artifact/e3c48f07-6998-467d-9609-480b7018beba
- TecHouse Aug 25: https://claude.ai/code/artifact/9dcd7da0-1474-4fba-843d-5fb6e5249ed8

## localStorage keys
- fruth_audit_agency (default: Start Advertising)
- ggc_audit_agency (default: GroupRB Marketing)
- techouse_audit_agency (default: Start Advertising)

## SVG x positions for 4 data points
36, 188, 352, 500
