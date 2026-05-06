# Graduation E-Commerce AI Builder

This workspace is for planning and building Version One of an authenticated, multi-tenant AI e-commerce store builder. Version One must let approved users generate their own isolated stores from brand prompts, spend admin-granted credits, preview generated storefronts, and verify demo readiness without production payments or production customer data.

## Working Style

- Be direct, concise, and factual. If an approach is risky, weak, duplicated, or not production-suitable, say so clearly.
- Verify before editing. Read or search the relevant files before changing imports, functions, folders, prompts, schemas, or configs.
- Search before adding new abstractions. Do not create a new component, helper, schema, MCP config, or document pattern until checking whether an equivalent already exists.
- Plan before coding. Identify the target files, reason for each change, and verification path before implementation.
- Keep significant work recorded. After meaningful changes, add or update a short factual note in `docs/logs/`, `docs/plans/`, or `docs/decisions/`.

## Documentation Structure

- `docs/architecture/`: system maps, folder maps, data flow, MCP/skills architecture.
- `docs/plans/`: implementation plans, plan drafts, backlog, and plan templates.
- `docs/prompts/`: reusable deep prompts for planning, research synthesis, scope refinement, and plan-phase kickoff.
- `docs/specs/`: product specs, feature specs, schemas, interfaces, and acceptance criteria.
- `docs/research/`: competitor reviews, technical analysis, market notes, and source material.
- `docs/references/`: stable reference docs, imported instructions, official-doc notes, and tool guides.
- `docs/notes/`: rough working notes that are not yet plans or decisions.
- `docs/logs/`: dated factual change summaries.
- `docs/decisions/`: durable architecture/product decisions.
- `docs/archive/`: duplicates, superseded drafts, and retained originals.

## Tooling Rules

- Use the local project MCP setup before broad global tools.
- The active MCP profile is `core-dev`: OpenAI Docs, Context7, Shopify Dev MCP, shadcn, Playwright, Chrome DevTools, Cloudflare Docs, X Docs, and project-scoped filesystem access.
- For account-specific systems, consult `mcp-profiles.json` and activate only the profile needed for the current task.
- Prefer read-only or development/test access first, especially for GitHub, Supabase, Prisma/Neon, Cloudflare, Sentry, Shopify, and automation tools.
- Never use full-device filesystem MCP access for this project.
- Never commit or paste live secrets, production payment keys, production database credentials, real customer data, private store tokens, or Supabase service-role keys into docs or configs.

## Official Docs Rules

- Use the OpenAI developer documentation MCP server when working with OpenAI APIs, Codex, ChatGPT Apps SDK, model choices, tool schemas, or agent workflows.
- Use Context7 for current framework and library docs before implementing syntax-sensitive code. This is mandatory for Next.js, React, Supabase, Auth, database/RLS, shadcn/Radix, state management, validation, testing, and AI SDK/provider work.
- Use Shopify official docs at `https://shopify.dev/docs` or `shopify-dev-mcp` for Shopify work.
- Route Shopify work by area: Apps for admin/extensions, Storefronts for themes/Hydrogen/Storefront API, and Agents for UCP, Catalog, Cart MCP, and Checkout MCP.
- Check the Shopify developer changelog before relying on Shopify API behavior.
- Use `docs/references/shopify-docs-guide.md` for local Shopify guidance.
- Use `docs/references/imported-project-proposal.txt` as the retained original proposal source. Active scope is defined by `docs/specs/project-scope-v1-and-roadmap.md` and `docs/decisions/0002-version-one-authenticated-scope.md`.

## Project Defaults

- Default stack: Next.js/React full-stack app, Node.js runtime route handlers/server modules, Supabase Auth, Supabase Postgres with RLS, shadcn/Radix-style UI primitives, AI provider adapters, Playwright/Chrome DevTools verification, and Vercel preview deployment.
- Payment processing is intentionally skipped for Version One. Use admin-granted credits and mock checkout/handoff only. Do not add Stripe or live payments unless a later decision explicitly reopens payments.
- Shopify support is an optional commerce path unless a real Shopify store domain and credentials are intentionally provided.
- Current Version One focus is a complete authenticated builder: login/users, multi-tenancy, admin approvals, credits, AI intake, store generation, generated storefront, cart/mock checkout handoff, verification, and a lazy Super Admin dashboard.
- Keep visual builder editing, drag-and-drop, real Shopify, live campaigns, marketplace/add-ons, vendor payouts, and production payments as future architecture paths unless explicitly approved.
- Keep the graduation demo on safe test/dev systems.
- Treat MCP and skills as project infrastructure: evaluate additions with `mcp-evaluator` before expanding the tool surface.

## Project Skills

Prefer these project skills when relevant: `brand-prompt-intake`, `store-generator`, `ecommerce-schema`, `cart-checkout-testing`, `ui-quality-review`, `mcp-evaluator`, `deployment-runbook`, and `grad-demo-script`.
