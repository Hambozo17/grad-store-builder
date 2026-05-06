# Version One Scope And Future Roadmap

## Product Vision

Build an authenticated AI e-commerce store builder where a user signs up, is approved by a Super Admin, receives credits, describes a brand/product/audience, and gets an isolated generated store foundation that can be previewed, tested, and demonstrated.

## Version One Scope

Version One is the complete graduation release, not a throwaway prototype. It includes:

1. Supabase Auth login/signup and protected routes.
2. User profiles with custom details, role, approval status, and tenant ownership.
3. Multi-tenancy so each approved user has their own isolated organization/store data.
4. Lazy Super Admin dashboard for approving users, granting credits, approving stores, and inspecting generation runs.
5. Admin-granted credits with an auditable ledger.
6. Brand Prompt Intake.
7. Flexible AI adapter gateway for prompt normalization and generation tasks.
8. Store Generation from stable contracts and generated artifacts.
9. Product/catalog data model with variants, prices, inventory, slugs, and media placeholders.
10. Storefront rendering for home, product listing, product detail, cart, mock checkout/handoff, and confirmation.
11. Platform-owned cart behavior.
12. Mock checkout/handoff only.
13. Verification runner and demo readiness report.
14. Impeccable UI/UX quality bar with browser verification.
15. Vercel preview/demo readiness.

## Explicitly Out Of Scope For Version One

- Stripe and live payment collection.
- Production billing, subscriptions, or paid credit purchase.
- Production customer data.
- Production Shopify app release or live Shopify checkout.
- Live ad campaign launch and live ad spend.
- Arbitrary drag-and-drop visual builder.
- Marketplace/add-on system.
- Vendor payouts, commissions, disputes, or settlement.
- Production fulfillment, tax, shipping, refunds, returns, and real order operations.

## Future-Ready Extension Points

- Stripe/payment adapter after a payment decision.
- Shopify Storefront/UCP/Cart MCP/Checkout MCP path after a real dev store is available.
- Visual builder with controlled inspector/selector.
- Controlled drag-and-drop section ordering.
- Creative assets service.
- Campaign orchestration service.
- Human/client review workflow.
- Analytics and observability dashboards.
- Billing and paid credits.
- Marketplace/add-ons.
- Vendor operations and commission settlement.

## Architecture Principle

Version One should expose stable contracts, tenant boundaries, admin action records, credit ledger events, and generated artifacts. AI should produce structured outputs and store artifacts, not arbitrary per-store application logic, RLS policies, migrations, cart code, or checkout code.

## Demo Principle

The graduation demo must be safe, repeatable, and believable. Use development databases, approved test users, admin-granted credits, synthetic products, mock checkout, stored generation reports, and fallback fixtures.
