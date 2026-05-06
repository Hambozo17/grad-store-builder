# Store Generation Implementation Plan

## Summary

Build Store Generation as a tenant-aware, credit-aware orchestration pipeline. It converts a validated `brand_intake_brief` into generated artifacts, synthetic catalog data, storefront rendering, platform-owned cart behavior, mock checkout/handoff, verification report, and Super Admin approval state.

## Current State

- Workspace is documentation-first; no app scaffold exists yet.
- Version One includes auth, tenants, credits, admin approval, AI gateway, intake, generation, cart/mock checkout, and verification.
- Payments are skipped. Stripe is future-only.
- Shopify is future-only unless a real development store is intentionally provided.

## Decisions

- Use Next.js full-stack app with server-only Node.js route handlers/services.
- Use Supabase Auth and Supabase Postgres/RLS.
- Use admin-granted credits, not paid billing.
- Keep Store Generation platform-owned and artifact-driven.
- Use mock checkout only.
- Use JSON fixture persistence first, then Supabase development tables.
- Store Generation requires approved user, organization context, and available credits.

## Implementation Steps

1. Define schemas for tenant context, `brand_intake_brief`, `store_generation_plan`, artifacts, credit events, admin review, and verification report.
2. Build auth/tenant guards used by all generation routes.
3. Build credit reserve/deduct/refund helpers.
4. Build AI task adapter gateway.
5. Build Store Generation orchestrator lifecycle.
6. Build template registry and category presets.
7. Build catalog generator.
8. Build artifact store.
9. Build storefront renderer from artifacts.
10. Build platform cart engine.
11. Build mock checkout/handoff.
12. Build verification runner.
13. Add Super Admin approval queue for generated stores.
14. Add demo fixtures and fallback run.
15. Record decisions/logs after implementation.

## Interfaces And Data

- Input: `brand_intake_brief`.
- Intermediate: `store_generation_plan`.
- Generated artifacts: `store.config.json`, `theme.tokens.json`, `catalog.seed.json`, `copy.blocks.json`, `generation.report.json`.
- Tenant data: `organization_id`, `created_by_user_id`, `generation_run_id`.
- Credits: ledger events and balance snapshots.
- Checkout: `mock_checkout` only.

## Test Plan

- Unit-test validators, credit helpers, plan builder, catalog generator, cart calculations, and mock checkout state.
- Integration-test generation with JSON fixtures.
- RLS/authorization negative tests for cross-user store access.
- Browser-test intake, generation status, store preview, cart, mock checkout, confirmation, Super Admin approval.
- Responsive and accessibility checks with Playwright/Chrome DevTools.
- Verify no production payment/Stripe path exists.

## Assumptions

- Supabase Auth is the login system.
- Credits are manually granted by Super Admin.
- Super Admin dashboard is intentionally simple.
- Mock checkout is enough for Version One buyer-flow proof.
- Generated stores are artifact-driven, not separate generated apps.
