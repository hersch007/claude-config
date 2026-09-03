---
name: quote-plugin-architecture
description: "How the standalone Fruth \"Quote Builder\" plugin works (the thing being ported into Start Performance as a module)"
metadata: 
  node_type: memory
  type: project
  originSessionId: 01838d33-e106-473b-b1c9-02bbf8061031
---

The quote system to port = TWO standalone WordPress plugins by "Start Advertising | RH Brashear":

**1. Quote Builder Core** (`sales-quote-system.php`, v8.5, ~2740 lines, prefix `sqs_`)
- A **pricing calculator for Fruth Custom Packaging** (plastic film/bags). 3 product types: **tubing, inline (BSB), zipper**.
- The **pricing math is client-side JavaScript** inside `sqs_pricing_calculator_render()` (lines ~1353–2740). It emits a DOM `sqs:update` event with `{result, state, quoteNumber}`. Rate tables come from DB.
- **DB tables** (created via dbDelta on `register_activation_hook`): `wp_sqs_pricing_data` (key/value rate tables, seeded from a big base64 `SQS_DEFAULT_DATA_B64` blob — packaging, resinDensity, formulaCosts, rate tables, setup tables, defaults, productCodes ~120 Fruth codes); `wp_sqs_quotes` (quote_number `SQS-YYYY-#####`, revision_number/label Rev A/B/C, status Draft|Final|Archived, product_type, customer_name, company_name, total_sales, quote_json blob); `wp_sqs_quote_items` (line items).
- **Own auth**, separate from WP users: PHP session-based, two roles — sales (`sqs_calc_sales_*` opts) and data-editor (`sqs_calc_editor_*`). `current_user_can('manage_options')` also passes.
- **UI = 3 shortcodes** on 3 WP pages: `[sqs_portal]` (2-door landing), `[sqs_pricing_calculator]` (sales calculator), `[sqs_pricing_data_admin]` (front-end rate-table editor). Plus wp-admin menu: Pricing Tables editor, Login & Branding, Reset Logins.
- **Persistence via admin-ajax**: actions `sqs_save_quote`, `sqs_load_quotes`, `sqs_get_quote`, `sqs_signout`; nonce `sqs_quote_actions`; gate `sqs_pricing_calculator_user_can_quote_ajax()`. Fires `do_action('sqs_quote_saved', $id, $payload)`.
- **Extension hooks** (for the print addon): `sqs_toolbar_extra_buttons`, `sqs_quote_info_extra_fields`, `sqs_bottom_stack_extra`.
- Branding options: `sqs_company_name`, `sqs_logo_url`, `sqs_company_address`, `sqs_quote_terms`.

**2. Quote Builder — Print Module** (`quote-builder-print.php`, v1.13, prefix `sqsp_`)
- Addon to the core (boots only if `SQS_VERSION` defined). Hooks the 3 extension points + listens for `sqs:update` to render a print-only "Quotation" card (Veritiv-style: dims → material/product code → packaging; price-break rows w/ MOQ; lead time, FOB, terms). Print via `@media print` + JS moving the card to `<body>`. Adds extra quote-info fields (customerEmail, preparedByEmail, leadTime, FOB) and a Print Quote button.

**Integration target:** collapse both into ONE Start Performance add-on module (see [[sp-module-architecture]]) at `/sp-app/?view=quotes`, replacing the bespoke portal/session auth with SP team-member auth, keeping the calc JS + rate tables + quote persistence. Open decision: link quotes to SP `sp_contacts`/`sp_companies` vs keep free-text customer/company. See [[quote-integration-plan]].

Source zips: sales-quote-system.zip + quote-builder-print.zip (were in C:\Users\richa\Downloads).
