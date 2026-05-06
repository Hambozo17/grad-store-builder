# Plan Phase Kickoff Prompt

```text
Create a decision-complete implementation plan for one Version One subsystem.

Before planning, read the relevant specs and architecture docs. Verify current framework/library facts with Context7 or official docs when syntax-sensitive.

Every plan must include:
- Goal.
- Inputs and prerequisites.
- Target files/modules.
- Data contracts.
- Database/RLS implications.
- UI/UX implications.
- Tests and verification.
- Failure modes.
- Out-of-scope boundaries.

Version One includes auth, tenancy, credits, Super Admin, AI gateway, intake, generation, storefront, cart, mock checkout, verification, and polished UI.

Do not include Stripe/live payments, live Shopify, campaigns, marketplace, arbitrary drag-and-drop, or vendor payouts unless a later decision explicitly changes scope.
```
