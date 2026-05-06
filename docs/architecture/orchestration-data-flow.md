# Orchestration Data Flow

## Version One Flow

1. User signs up through Supabase Auth.
2. `profiles` row is created with `pending_approval`.
3. Super Admin approves user and grants credits.
4. Approved user opens builder workspace.
5. User submits Brand Prompt Intake.
6. AI gateway normalizes prompt into `brand_intake_brief`.
7. System estimates credits and validates readiness.
8. Store Generation reserves credits and creates `generation_run`.
9. AI gateway creates `store_generation_plan`, catalog, copy, and reports through task adapters.
10. Artifact store persists config, tokens, seed data, copy blocks, and report.
11. Storefront renderer reads artifacts and platform templates.
12. Verification runner checks tenant, contracts, UI, cart, mock checkout, and accessibility.
13. Credit ledger deducts or refunds.
14. Store enters Super Admin approval queue.
15. Approved store becomes demo-ready.

## Data Ownership

Every generated store record must include:

- `organization_id`
- `created_by_user_id`
- `generation_run_id`
- status
- timestamps

Every generated artifact must be reachable from the store and run, not from chat history alone.

## Failure Handling

- Validation failure: no credit deduction.
- AI/provider failure: refund reserved credits.
- Verification failure: store remains not demo-ready.
- Admin rejection: store remains visible to owner with rejection notes.
- Tenant authorization failure: return user-safe error and log security event.
