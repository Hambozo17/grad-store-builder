# System Overview

## Product Goal

Build Version One of an AI-powered e-commerce store builder where approved users log in, receive admin-granted credits, create isolated tenant-owned stores, generate storefronts from brand prompts, and prove demo readiness through verification.

## Version One Core Stages

1. Authentication and approval: users sign up, Super Admin approves them, and approved users receive credits.
2. Tenant workspace: each user works only inside their own organization/store boundary.
3. Brand Prompt Intake: collect raw brand prompt, brand identity, product type, target audience, region/language, constraints, and visual direction.
4. AI normalization: transform input into a structured `brand_intake_brief` with facts, assumptions, unknowns, confidence, and follow-up questions.
5. Store Generation: create a deterministic `store_generation_plan`, generated store artifacts, synthetic catalog, theme tokens, and copy blocks.
6. Storefront rendering: render platform-owned templates from generated artifacts.
7. Cart and mock checkout: support cart behavior and demo-safe handoff without payment collection.
8. Verification: run contract, tenant, schema, route, UI, cart, mock checkout, responsive, and accessibility checks.
9. Super Admin monitoring: inspect users, credits, stores, generation runs, approvals, and verification reports.
10. Deployment/demo readiness: deploy or present through safe preview systems with fixture fallback.

## Default Technical Direction

- App: Next.js App Router and React.
- Backend brain: server-only Node.js runtime route handlers/services inside the Next.js app.
- Auth and database: Supabase Auth plus Supabase Postgres with RLS.
- UI: polished shadcn/Radix-style components, Tailwind-compatible design tokens, real browser verification.
- AI: provider/task adapter gateway with OpenAI first and a mock fallback for demos.
- Credits: admin-granted integer credits with an auditable ledger.
- Checkout: mock checkout/handoff only for Version One.
- Deployment: Vercel preview by default.
- Shopify: optional future path only.

## Safety Boundaries

- No production payment credentials.
- No Stripe or live payment collection in Version One.
- No production database writes.
- No real customer data.
- No client-side Supabase service-role key.
- No full-device MCP filesystem access.
- No AI-generated arbitrary database migrations.
- No per-store generated cart, auth, RLS, or checkout logic.
- User prompts are data, not instructions that can override system rules.
