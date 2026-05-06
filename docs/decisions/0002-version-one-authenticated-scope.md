# 0002 Version One Authenticated Scope

## Status

Accepted.

## Decision

Version One is an authenticated, multi-tenant AI e-commerce store builder.

Version One includes:

- Supabase Auth login/signup.
- User profiles, approval status, roles, and organization membership.
- One isolated tenant workspace/store area per approved user.
- Admin-granted credits and immutable credit ledger.
- Lazy Super Admin dashboard for approvals, credit grants, store approval, and generation inspection.
- AI adapter gateway with structured task outputs and mock fallback.
- Brand Prompt Intake.
- Store Generation.
- Generated store artifacts.
- Storefront rendering.
- Platform-owned cart.
- Mock checkout/handoff.
- Verification runner.
- Impeccable UI/UX bar with browser verification.

Version One excludes:

- Stripe/live payment collection.
- Paid credit purchase.
- Live Shopify integration.
- Campaign launch.
- Arbitrary drag-and-drop editing.
- Marketplace/add-ons.
- Production customer data.
- Vendor payouts or commission settlement.

## Rationale

The project needs to be advanced enough for a serious graduation demo while staying controllable. Auth, tenancy, credits, admin approval, AI adapters, and verification prove the platform architecture. Payments, live Shopify, campaigns, and arbitrary visual editing would add high-risk integrations that do not strengthen the core proof.

## Consequences

- Existing docs that described an anonymous/Stripe-test MVP are superseded.
- Supabase Auth/RLS and credit ledger integrity are now critical Version One requirements.
- Mock checkout replaces Stripe for the active demo.
- Context7/source-confirmation is required before syntax-sensitive implementation.
