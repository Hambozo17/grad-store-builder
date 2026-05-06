# 2026-05-05 Version One Doc Refresh

Updated the active documentation set to reflect the new Version One scope.

Changed direction:

- Replaced the old anonymous/Stripe-test MVP framing with authenticated multi-tenant Version One.
- Added Supabase Auth, custom user profiles, tenant isolation, admin approvals, admin-granted credits, AI adapter gateway, lazy Super Admin dashboard, mock checkout, and UI/UX quality as active requirements.
- Moved Stripe/live payments, live Shopify, campaigns, marketplace/add-ons, arbitrary drag-and-drop, production customer data, and vendor payouts out of Version One.

Updated:

- `AGENTS.md`
- `mcp-profiles.json`
- `docs/README.md`
- `docs/architecture/`
- `docs/specs/`
- `docs/plans/`
- `docs/prompts/`
- `docs/decisions/0002-version-one-authenticated-scope.md`

Added specs and plans:

- `auth-users-tenancy-spec.md`
- `credits-admin-approval-spec.md`
- `ai-adapter-gateway-spec.md`
- `ui-ux-quality-spec.md`
- `commerce-data-model-spec.md`
- `cart-checkout-spec.md`
- `search-confirmation-evidence-spec.md`
- `005-version-one-complete-implementation-plan.md`
- `demo-readiness-runbook.md`

Historical research and logs were left intact as source history.
