# Project Documentation

This folder is the working brain of the graduation project. The active target is now Version One: an authenticated, multi-tenant AI e-commerce store builder with admin-granted credits, AI store generation, polished UI/UX, and demo-safe verification.

## Active Version One Definition

Version One must include:

- Supabase Auth login/signup and protected builder routes.
- Custom user profiles, roles, approval status, and tenant ownership.
- One isolated workspace/store area per approved user.
- Admin-granted credits and an auditable credit ledger.
- A lazy Super Admin dashboard for user approval, credit grants, store approval, generation run inspection, and verification reports.
- Brand Prompt Intake and AI normalization.
- Flexible AI provider adapters so OpenAI can be swapped or supplemented later.
- Store Generation through stable contracts and generated artifacts.
- Generated storefront rendering, cart behavior, mock checkout/handoff, confirmation, and verification.
- Impeccable UI/UX verified with browser checks, responsive checks, and accessibility basics.

Version One must not include production payments, live Shopify, live ad campaign launch, arbitrary drag-and-drop editing, production customer data, marketplace/add-ons, or vendor payouts.

## Folder Map

- `architecture/`: system shape, data flow, tenancy boundaries, MCP/skills setup, generated-store boundaries.
- `plans/`: implementation plans, sequencing, backlog, runbooks, and plan templates.
- `prompts/`: reusable prompts for source confirmation, planning, and implementation kickoff.
- `specs/`: product and technical specifications.
- `research/`: source notes and competitor/technical analysis. Keep historical research factual; do not rewrite it to match new scope.
- `references/`: durable guides and imported reference material.
- `notes/`: temporary working notes.
- `logs/`: dated summaries of completed work. Old logs remain historical.
- `decisions/`: accepted decisions and tradeoffs.
- `archive/`: duplicates and superseded material.

## Naming Rules

- Use lowercase kebab-case filenames.
- Prefix durable plans and decisions with numbers when ordering matters.
- Prefix reusable prompts with numbers, such as `00-master-planning-prompt.md`.
- Prefix dated logs with `YYYY-MM-DD`, such as `2026-05-05-version-one-doc-refresh.md`.
- Keep raw imported research in `research/` or `references/`; summarize it into plans/specs before implementation.

## Current Readiness

- Workspace is documentation-first; no application scaffold exists yet.
- Active docs now target authenticated Version One rather than the old anonymous/Stripe-test MVP.
- Core specs should be treated as implementation input: scope, auth/tenancy, credits/admin, AI gateway, UI/UX, contracts, Brand Prompt Intake, Store Generation, commerce data, and cart/mock checkout.
- Before coding, run a source confirmation pass with Context7/OpenAI/Supabase/Next.js/shadcn/Playwright docs for syntax-sensitive decisions.
