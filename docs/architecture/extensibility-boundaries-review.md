# Extensibility Boundaries Review

## Verdict

The previous anonymous/Stripe-test MVP boundary is superseded. Version One is now an authenticated multi-tenant builder with admin-granted credits and a lazy Super Admin dashboard. This is bigger, but still feasible if future-heavy features remain behind explicit boundaries.

The strongest architecture choice remains contract-first generation: AI produces structured briefs, store plans, content, tokens, catalog seed data, and reports. Platform code owns auth, tenancy, credits, templates, cart, mock checkout, RLS, and verification.

## Active Version One Boundary

Build now:

- Supabase Auth login/signup.
- User profiles and approval status.
- Organization/tenant ownership.
- Admin-granted credits and ledger.
- Lazy Super Admin dashboard.
- Brand Prompt Intake.
- AI adapter gateway.
- Store Generation.
- Generated artifacts.
- Storefront renderer.
- Cart and mock checkout.
- Verification runner.
- Demo deployment/runbook.

Do not build now:

- Stripe/live payments.
- Live Shopify.
- Campaign launch.
- Marketplace/add-ons.
- Arbitrary drag-and-drop.
- Vendor payouts.
- Full production analytics dashboard.

## Platform vs Generated Boundary

Platform-owned code:

- Auth/session handling.
- Tenant resolution and authorization.
- RLS policy design.
- Credit ledger logic.
- AI provider gateway and validators.
- Intake normalization orchestration.
- Store generation orchestration.
- Template and section registry.
- Catalog generator.
- Storefront renderer.
- Cart engine.
- Mock checkout.
- Verification runner.
- Super Admin actions.

Generated artifacts:

- Store configuration.
- Theme tokens.
- Catalog seed data.
- Copy blocks.
- Generation report.

Generated artifacts must not contain secrets, RLS policies, migrations, arbitrary React app logic, payment logic, or tenant authorization logic.

## Modules That Should Exist From Day One

| Module | Purpose |
| --- | --- |
| `auth-tenancy` | Supabase session, profiles, organization membership, approval checks. |
| `admin` | Super Admin approvals, grants, action audit. |
| `credits` | Credit balances, ledger, reserve/deduct/refund. |
| `contracts` | Shared schemas and validators. |
| `ai-gateway` | Provider/task adapters. |
| `intake` | Prompt capture, normalization, follow-up questions. |
| `generation` | Run lifecycle and artifact creation. |
| `templates` | Base template and category presets. |
| `catalog` | Synthetic products and variants. |
| `cart` | Stable cart state and calculations. |
| `mock-checkout` | Demo checkout handoff. |
| `persistence` | JSON fixture and Supabase dev adapters. |
| `verification` | Checks and reports. |
| `ui-system` | Product-grade UI primitives and patterns. |

## Future Interfaces Only

| Future Area | Version One Position |
| --- | --- |
| Stripe/payments | Disabled. Credits are admin-granted only. |
| Shopify | Documentation/future adapter only. |
| Visual builder | Config foundation only; no arbitrary editing. |
| Drag-and-drop | Future controlled section ordering. |
| Campaigns | Stored extension notes only. |
| Marketplace/add-ons | Future registry only. |
| Vendor payouts | Future business logic only. |

## Lifecycle States

### User

- `user.pending_approval`
- `user.approved`
- `user.suspended`

### Credits

- `credit.granted`
- `credit.reserved`
- `credit.deducted`
- `credit.refunded`
- `credit.adjusted`

### Intake

- `intake.draft`
- `intake.needs_followup`
- `intake.ready_for_generation`
- `intake.rejected`

### Generation

- `generation.queued`
- `generation.validating`
- `generation.planned`
- `generation.artifacts_created`
- `generation.verification_running`
- `generation.pending_admin_approval`
- `generation.demo_ready`
- `generation.failed`

## Recommended Folder Shape

```text
src/
  app/
    (auth)/
    (builder)/
    (admin)/
    (store)/
      [storeSlug]/
    api/
  core/
    contracts/
    lifecycle/
    validation/
    auth-tenancy/
  features/
    admin/
    credits/
    intake/
    ai-gateway/
    generation/
    templates/
    catalog/
    storefront/
    cart/
    mock-checkout/
    verification/
    ui-system/
  adapters/
    persistence/
      json-fixture/
      supabase-dev/
    ai/
      openai/
      mock/
    future/
      stripe/
      shopify/
      campaigns/
      visual-builder/
  generated/
    stores/
      [storeId]/
```

## Risks To Prevent

- Cross-tenant data leaks.
- Super Admin checks only in client UI.
- Credit balance updates without ledger events.
- AI-generated schema or RLS.
- AI-generated checkout/payment logic.
- Treating generated UI as demo-ready without verification.
- Turning lazy Super Admin into a full analytics product.
- Activating Stripe or Shopify by accident.
