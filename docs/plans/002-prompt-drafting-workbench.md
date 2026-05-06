# Prompt Drafting Workbench Plan

## Summary

Maintain a reusable prompt library that helps the project move from rough idea into high-quality Version One plans and implementation prompts.

## Current State

- Root `AGENTS.md` defines updated Version One rules.
- Imported proposal is stored in `docs/references/imported-project-proposal.txt`.
- Research files are organized under `docs/research/`.
- Active specs now cover auth, tenancy, credits, AI gateway, UI/UX, intake, generation, commerce, cart/mock checkout, and confirmation.

## Implementation Steps

1. Keep `docs/prompts/` as the home for reusable prompts.
2. Keep the master planning prompt aligned with authenticated Version One.
3. Keep feature-specific prompts aligned with tenant, credit, and admin requirements.
4. Keep research prompts focused on risks and mitigations.
5. Keep source-confirmation prompts tied to Context7 and official docs.
6. Update prompt README whenever a prompt is added or superseded.

## Test Plan

- Confirm prompts reference real workspace files.
- Confirm prompts preserve Version One scope.
- Confirm prompts keep future phases as extension points.
- Confirm docs index mentions the prompt folder.

## Assumptions

- Prompt drafting supports implementation planning.
- Prompts must not reintroduce old anonymous/Stripe-first scope.
