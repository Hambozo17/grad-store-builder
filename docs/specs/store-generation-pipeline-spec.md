# Store Generation Pipeline Spec

## 1. Store Generation Goal

Store Generation converts a validated tenant-owned `brand_intake_brief` into a generated store foundation: store plan, artifacts, synthetic catalog, storefront pages, cart behavior, mock checkout/handoff, verification report, and admin approval status.

This stage is an orchestration pipeline, not one-shot code generation. Platform-owned modules handle auth checks, credits, templates, cart, mock checkout, persistence, verification, and tenant safety. AI generates structured content and artifacts only.

## 2. End-To-End Flow

1. Confirm authenticated user, approval status, organization membership, and credit balance.
2. Receive final `brand_intake_brief`.
3. Validate contract and tenant context.
4. Reserve required credits.
5. Create `generation_run`.
6. Build deterministic `store_generation_plan`.
7. Select template and category preset.
8. Generate store config, theme tokens, catalog seed, copy blocks, and generation report.
9. Persist artifacts under the tenant/store.
10. Render storefront pages from artifacts.
11. Attach platform-owned cart engine.
12. Attach mock checkout/handoff.
13. Run verification checks.
14. Deduct or refund credits based on run result.
15. Send successful store to Super Admin approval queue.
16. Mark store `demo_ready` only after verification and admin approval.

## 3. Platform-Owned Modules

| Module | Responsibility |
| --- | --- |
| `auth-tenancy` | User/session/organization checks. |
| `credit-ledger` | Grants, reservations, deductions, refunds. |
| `contracts` | Shared schemas and validators. |
| `ai-gateway` | Provider/task adapters and structured output validation. |
| `generation-orchestrator` | Run lifecycle, errors, retries, artifact references. |
| `template-registry` | Base templates, category presets, section slots. |
| `catalog-generator` | Synthetic products and variants. |
| `artifact-store` | Generated config, tokens, catalog, copy, reports. |
| `storefront-renderer` | Platform templates consuming artifacts. |
| `cart-engine` | Stable cart behavior. |
| `mock-checkout` | Demo handoff without payment collection. |
| `verification-runner` | Contract, tenant, UI, cart, checkout, responsive, accessibility checks. |
| `admin-review` | Super Admin approval/rejection. |

## 4. Generated Artifacts

| Artifact | Contents |
| --- | --- |
| `store.config.json` | Store identity, routes, navigation, sections, locale, currency, trust elements. |
| `theme.tokens.json` | Colors, typography, spacing, radius, imagery direction, style keywords. |
| `catalog.seed.json` | Synthetic products, variants, slugs, prices, media placeholders, inventory. |
| `copy.blocks.json` | Hero, collection, product, trust, FAQ, cart, checkout, confirmation copy. |
| `generation.report.json` | Assumptions, warnings, checks, credit usage, admin review status, demo readiness. |

## 5. Template Strategy

- Use one strong base storefront template first.
- Add category presets for fashion, beauty/skincare, electronics, home, fitness, gifts, and generic.
- Presets may change sections, copy emphasis, and product fields.
- Presets must not change database schema, cart logic, auth logic, or mock checkout logic.
- Generated visual direction must flow through tokens and config, not generated React files.

## 6. Data/Persistence Strategy

- Start with JSON fixtures for deterministic demo fallback.
- Mirror the same model in Supabase development tables.
- Use RLS on tenant-owned tables.
- Use idempotent seed logic keyed by store/generation run.
- Store product prices as minor units plus currency.
- Never let AI generate arbitrary migrations per store.

## 7. Cart And Mock Checkout Strategy

- Cart is platform-owned and stable across generated stores.
- Product/variant IDs are the identity source.
- Mock checkout is the only Version One checkout mode.
- No card fields, no Stripe, no live payment collection.
- Confirmation is a demo/test state, not a real order operation.

## 8. Verification Requirements

Required checks:

- Auth and tenant ownership.
- Contract/schema validation.
- Credit ledger event integrity.
- Artifact presence and schema.
- Product/variant identity.
- Required routes render.
- Cart add/remove/quantity/subtotal.
- Mock checkout success/cancel.
- Responsive layout.
- Accessibility basics.
- Prompt injection and secret redaction.
- Admin approval status.

## 9. Acceptance Criteria

- Store Generation rejects invalid brief, unapproved user, missing tenant context, or insufficient credits.
- Run status is visible and persisted.
- Artifacts are inspectable.
- Storefront renders from artifacts.
- Cart and mock checkout work.
- Credits are deducted/refunded correctly.
- Verification report passes.
- Super Admin can approve the store.
- Store is not `demo_ready` until verification and admin approval pass.

## 10. Out Of Scope

- Stripe.
- Live Shopify checkout.
- Production orders.
- Generated migrations.
- Generated auth/RLS/cart/checkout code.
- Arbitrary visual builder editing.
