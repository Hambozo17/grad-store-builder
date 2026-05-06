# Master Planning Prompt

Use this prompt when an agent needs to deeply understand the workspace before drafting plans or editing docs.

```text
You are working inside:
C:\Users\PC\Downloads\Grad project dPlan

First verify the workspace. Read:
- AGENTS.md
- mcp-profiles.json
- docs/README.md
- docs/specs/project-scope-v1-and-roadmap.md
- docs/specs/graduation-proposal-refined-scope.md
- docs/specs/prompt-output-contracts.md
- docs/specs/auth-users-tenancy-spec.md
- docs/specs/credits-admin-approval-spec.md
- docs/specs/ai-adapter-gateway-spec.md
- docs/specs/ui-ux-quality-spec.md
- docs/specs/brand-prompt-intake-spec.md
- docs/specs/store-generation-pipeline-spec.md
- docs/specs/commerce-data-model-spec.md
- docs/specs/cart-checkout-spec.md
- docs/architecture/system-overview.md
- docs/architecture/extensibility-boundaries-review.md
- docs/architecture/orchestration-data-flow.md
- docs/plans/005-version-one-complete-implementation-plan.md

Current active scope:
Build Version One of an authenticated, multi-tenant AI e-commerce store builder with Supabase Auth, user profiles, tenant isolation, admin-granted credits, lazy Super Admin dashboard, AI adapter gateway, Brand Prompt Intake, Store Generation, generated storefront, cart, mock checkout, verification, and polished UI/UX.

Do not reintroduce Stripe/live payments, live Shopify, live campaigns, arbitrary drag-and-drop, marketplace/add-ons, production customer data, or vendor payouts into Version One.

Use Context7 and official docs before syntax-sensitive implementation decisions.

Return:
1. Confirmed workspace facts.
2. Current Version One scope.
3. Missing decisions.
4. Target files/modules.
5. Verification path.
```
