# Plans

Use this folder for implementation plans, sequencing, runbooks, and plan drafts. Plans should be decision-complete enough that implementation can start without guessing.

Use `_plan-template.md` for new plans.

## Current Plans

- `000-planning-backlog.md`: active Version One planning backlog.
- `001-environment-readiness-plan.md`: workspace/environment readiness plan.
- `002-prompt-drafting-workbench.md`: prompt workbench setup plan.
- `003-store-generation-implementation-plan.md`: Store Generation implementation plan updated for authenticated Version One.
- `004-search-confirmation-readiness-plan.md`: source-confirmation and research verification readiness plan.
- `005-version-one-complete-implementation-plan.md`: end-to-end Version One plan.
- `demo-readiness-runbook.md`: demo verification, fallback, and recovery plan.

## Planning Priority

1. Source-confirm current framework/library facts with Context7 and official docs.
2. Scaffold the Next.js full-stack app with Supabase Auth and RLS-ready data boundaries.
3. Build login, profiles, tenant ownership, admin approvals, and credit ledger.
4. Build Brand Prompt Intake and the AI adapter gateway.
5. Build Store Generation, generated artifacts, storefront rendering, cart, mock checkout, and verification.
6. Build the lazy Super Admin dashboard and graduation demo flow.

## Out Of Scope For Plans Unless Reopened By Decision

- Stripe/live payments.
- Live Shopify.
- Production customer data.
- Campaign launch and ad spend.
- Marketplace/add-ons.
- Arbitrary drag-and-drop builder.
- Vendor payouts and commission settlement.
