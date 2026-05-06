# Search Confirmation Readiness Plan

## Summary

Prepare the project for a source-backed confirmation pass before Version One implementation. The confirmation pass should verify auth, tenancy, credits, AI gateway, UI stack, Store Generation, mock checkout, deployment, MCP/tool setup, and missing decisions.

## Current State

- Version One scope has been updated to authenticated multi-tenant builder.
- Specs exist for auth/tenancy, credits/admin approval, AI gateway, UI/UX, Brand Prompt Intake, Store Generation, commerce model, cart/mock checkout, and contracts.
- Research synthesis exists and should be used as risk input, not current implementation truth.
- Prompt workbench has been updated for Version One.

## Implementation Steps

1. Use `docs/prompts/07-search-and-confirmation-deep-prompt.md`.
2. Verify current official docs for Next.js, React, Supabase Auth/RLS, OpenAI structured outputs, shadcn/Radix, Playwright, and Vercel.
3. Keep Shopify confirmation future-only unless a real Shopify task is opened.
4. Do not include Stripe confirmation for Version One except to confirm it is intentionally out of scope.
5. Record verified facts, project assumptions, and remaining risks.

## Test Plan

- Confirm the search prompt references real workspace files.
- Confirm the prompt keeps Version One on auth, tenants, credits, AI gateway, intake, generation, cart/mock checkout, verification, and UI quality.
- Confirm the prompt excludes live payments, live Shopify, campaigns, marketplace, arbitrary drag-and-drop, and vendor payouts.

## Assumptions

- The next implementation work should source-confirm docs before coding.
- Context7 is mandatory for syntax-sensitive library/framework choices.
