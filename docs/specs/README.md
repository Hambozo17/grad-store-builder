# Specs

Use this folder for product and technical specifications. Specs describe what Version One must do, the contracts it exposes, and the acceptance criteria implementation must satisfy.

## Active Scope Specs

- `project-scope-v1-and-roadmap.md`: Version One scope and future roadmap.
- `graduation-proposal-refined-scope.md`: proposal-ready project framing.
- `prompt-output-contracts.md`: stable cross-stage contracts and generated artifact shapes.

## Feature And Platform Specs

- `auth-users-tenancy-spec.md`: login, profiles, roles, tenant isolation, and RLS principles.
- `credits-admin-approval-spec.md`: admin-granted credits, approvals, ledger, and Super Admin controls.
- `ai-adapter-gateway-spec.md`: flexible AI provider/task gateway.
- `ui-ux-quality-spec.md`: impeccable UI/UX quality bar and verification rules.
- `brand-prompt-intake-spec.md`: prompt intake and normalization.
- `store-generation-pipeline-spec.md`: store generation orchestration and generated artifacts.
- `commerce-data-model-spec.md`: products, variants, stores, carts, mock checkout, and verification data.
- `cart-checkout-spec.md`: cart behavior and mock checkout/handoff.
- `search-confirmation-evidence-spec.md`: current-doc/source confirmation requirements.

## Version One Rules

- Version One includes auth, tenant-owned stores, admin-granted credits, store generation, cart/mock checkout, verification, and a lazy Super Admin dashboard.
- Version One excludes Stripe/live payments, live Shopify, ad campaigns, arbitrary drag-and-drop, production customer data, and vendor payout operations.
- Contract changes should be additive where possible and documented in this folder before implementation.
