---
name: version-one-advanced-plan
overview: "Comprehensive review and advanced-grade rewrite of the Version One graduation plan: locks the 2026 stack (Next.js 16, AI SDK v6, Drizzle, Supabase RLS, Vercel Workflows), restructures the 28 flat phases into 7 enforceable gates, defines all schemas, designs the AI gateway, finalizes the MCP/skills inventory, and lists every doc to update."
todos:
  - id: g0-source
    content: "Gate 0: Source-confirm Next.js 16, AI SDK v6, Drizzle, Supabase SSR, Vercel Workflows, shadcn/cli v4, Sentry via Context7 and write source-confirmation log"
    status: pending
  - id: g0-decisions
    content: Write ADR 0003 (stack 2026 lockin) and ADR 0004 (AI task routing matrix) using adr-skill
    status: pending
  - id: g0-docs-restructure
    content: Update PLAN.md, docs/plans/005, system-overview.md, mcp-and-skills-architecture.md, ai-adapter-gateway-spec.md, AGENTS.md and add monorepo-layout.md, observability-spec.md, image-generation-spec.md, workflows-orchestration-spec.md
    status: pending
  - id: g0-mcp-skills
    content: Update mcp-profiles.json with active sentry/vercel/supabase, add ai-runtime and data-layer profiles; declare 9 new project-custom skills under ~/.codex/skills/
    status: pending
  - id: ga-foundation
    content: "Gate A: Turborepo + pnpm scaffold, apps/web Next.js 16, all empty packages, Supabase Auth, Drizzle migrations, Sentry+OTel, deploy preview, RLS on profiles passing negative test"
    status: pending
  - id: gb-tenancy-credits
    content: "Gate B: Drizzle schemas + pgPolicy for tenancy, credits, admin; immutable ledger; lazy Super Admin queue; ledger invariant test; cross-tenant negative tests"
    status: pending
  - id: gc-ai-intake
    content: "Gate C: packages/ai-gateway with AI SDK v6 task adapters, per-task model routing, mock provider, safety/redaction, Brand Intake UI with follow-ups, AI telemetry into Sentry"
    status: pending
  - id: gd-generation
    content: "Gate D: Vercel Workflow generation.run with 9 steps, template registry + 6 category presets, artifact persistence, idempotent retries"
    status: pending
  - id: ge-storefront-verify
    content: "Gate E: storefront renderer, cart engine, mock checkout, verification runner with schema/tenant/route/cart/checkout/responsive/a11y/injection checks"
    status: pending
  - id: gf-demo-ready
    content: "Gate F: Super Admin store-approval queue, fixture-based demo fallback, Vercel preview env lock, Sentry release+alerts, demo runbook + graduation script"
    status: pending
  - id: g0-git-skill
    content: Author project-custom skill `git-flow-phase-aware` (agent-never-commits, staging awareness, diff-size thresholds, gate-scoped Conventional Commits, branch + tag conventions, PR template) and ADR 0005 referencing it
    status: pending
  - id: g0-build-vs-rent
    content: Add ADR 0006 (build-vs-rent) refusing WebContainers and locking the SaaS infra list (Supabase + Vercel + OpenAI/Anthropic/Google + Replicate + Resend + Upstash + Sentry + Cloudflare R2 + Tavily/Firecrawl)
    status: pending
  - id: g0-cost-keys
    content: Author docs/references/api-keys-and-costs.md from §14 cost matrix; add .env.example with all variables; provision dev-tier accounts and store keys per §14
    status: pending
isProject: false
---

# Version One Advanced Implementation Plan (2026)

> Supersedes the flat 28-phase outline in `[PLAN.md](c:\Users\PC\Downloads\PLAN.md)` and `[docs/plans/005-version-one-complete-implementation-plan.md](c:\Users\PC\Downloads\Grad project dPlan\docs\plans\005-version-one-complete-implementation-plan.md)`. Everything below replaces the old direction with a 2026-current stack, gated phasing, complete schemas, and the exact MCP and skill inventory.

---

## 1. Critical Review Of Current State

**Strengths in your existing workspace.** Your `docs/specs/`, `docs/architecture/`, and `docs/decisions/` are production-grade for a graduation project. The contract-first approach (`brand_intake_brief`, `store_generation_plan`, `credit_ledger_event`, `ai_task_result`) and the platform-vs-generated boundary in `[docs/architecture/extensibility-boundaries-review.md](c:\Users\PC\Downloads\Grad project dPlan\docs\architecture\extensibility-boundaries-review.md)` are exactly right and stay.

**What is weak or outdated.**

- **Flat 28-phase list with no gates.** No binary pass/fail signal. Replaced below with 7 named gates.
- **AI layer underspecified.** Your `ai-adapter-gateway-spec.md` lists tasks but no concrete implementation. The 2026 default is **Vercel AI SDK v6** with `Output.object()` + `ToolLoopAgent`, multi-provider routing, and OpenTelemetry tracing into Sentry. The old `generateObject` API is deprecated as of Feb 2026.
- **No background job runtime.** Generation is multi-step, multi-minute, multi-failure. Server actions alone are wrong. **Vercel Workflows** went GA April 2026 and integrates natively with AI SDK v7's `WorkflowAgent`. This is the right runtime.
- **No ORM committed.** Plan says "Supabase Postgres + RLS" but does not pick an ORM. **Drizzle** is the 2026 default for serverless + RLS (declarative `pgPolicy`, 90% smaller than Prisma, edge-friendly). Prisma 7 is fine but heavier; pick Drizzle.
- **Folder shape assumes single Next.js app.** A Turborepo + pnpm monorepo with `apps/web` + `packages/{db,contracts,ui,ai-gateway,verification}` is the cleaner advanced shape, even before adding a second app.
- **No image-generation strategy.** Your spec says "media placeholders" but a graduation demo needs real synthetic product photos for the Wow factor. Add **GPT-Image-1** (text/composition) + **Flux Ultra 1.1 via Replicate** (photoreal) behind a single image adapter.
- **No observability story.** Sentry is in `mcp-profiles.json` but the plan does not say where it plugs in. Add `@sentry/nextjs` + `@sentry/opentelemetry` wired to AI SDK telemetry on day one.
- **No model strategy.** "OpenAI first" is too vague. Below is a per-task model matrix using GPT-5.4 / Claude Sonnet 4.6 / Haiku 4.5 / Gemini 2.5 Flash with fallbacks.
- **MCP profile is good but missing key 2026 servers.** Add Vercel Workflows MCP, Inngest MCP (backup), Drizzle/Linear/Notion as project-knowledge.
- **Skills inventory is partial.** Eight project skills listed in `[AGENTS.md](c:\Users\PC\Downloads\Grad project dPlan\AGENTS.md)`. Your global `~/.codex/skills` already includes many more relevant ones (`use-ai-sdk`, `react-best-practices`, `frontend-design`, `web-design-guidelines`, `web-perf`, `webapp-testing`, `playwright`, `sentry-nextjs-sdk`, `sentry-setup-ai-monitoring`, `deploy-to-vercel`, `adr-skill`, `composition-patterns`, `security-best-practices`). Adopt them explicitly. Add 9 new project-custom skills below.

---

## 2. Locked Architecture (2026 Stack)

| Layer | Decision | Why |
| --- | --- | --- |
| **Repo** | Turborepo + pnpm workspaces | First-party Next.js; remote caching; clean future split |
| **App** | Next.js 16 App Router, React 19, Cache Components, PPR | `"use cache"` opt-in caching; static shell + streamed dynamic |
| **Auth** | Supabase Auth (`@supabase/ssr`) | Cookies-based SSR session, no client tokens |
| **DB** | Supabase Postgres + RLS | Tenant isolation enforced at DB layer |
| **ORM** | Drizzle ORM with `pgPolicy` | Declarative RLS in TS, 7.4 KB bundle, edge-safe |
| **Validation** | Zod v4 contracts in `packages/contracts` | Single source of truth for AI I/O + DB types |
| **AI gateway** | Vercel AI SDK v6 | `generateText({ output: Output.object() })`, `ToolLoopAgent`, multi-provider |
| **Models** | OpenAI GPT-5.4 + Claude Sonnet 4.6 + Haiku 4.5 + Gemini 2.5 Flash | Per-task routing matrix below |
| **Image gen** | GPT-Image-1 + Flux Ultra 1.1 (Replicate) behind one adapter | Composition + photoreal |
| **Background jobs** | **Vercel Workflows** (GA April 2026) | Durable AI orchestration, native AI SDK integration |
| **UI system** | shadcn/cli v4 + Tailwind v4 + Radix base | Registry-driven, token-themed, presets-based |
| **Observability** | Sentry + `@sentry/opentelemetry` + AI SDK `experimental_telemetry` | Token cost, span tracing, AI Agents Insights dashboard |
| **Browser verification** | Playwright + Chrome DevTools MCP | Responsive, a11y, keyboard, overflow |
| **Deploy** | Vercel preview + Fluid Compute | `waitUntil`, Workflows on same plane |
| **Optional later** | Cloudflare Workers/Workflows path stays viable through Hono adapter | `apps/worker` extension point only |

---

## 3. Monorepo Folder Structure

```text
grad-store-builder/
├── apps/
│   └── web/                        Next.js 16 App Router
│       ├── app/
│       │   ├── (marketing)/        Optional landing
│       │   ├── (auth)/             Supabase signup/login/pending
│       │   ├── (builder)/          Approved-user workspace
│       │   │   ├── intake/
│       │   │   ├── runs/[runId]/
│       │   │   └── stores/[storeId]/
│       │   ├── (admin)/            Super Admin (lazy)
│       │   ├── (storefront)/[storeSlug]/   Generated store preview
│       │   ├── api/
│       │   │   ├── ai/             AI gateway routes
│       │   │   ├── workflows/      Vercel Workflows triggers
│       │   │   └── webhooks/
│       │   └── workflows/          Workflow definitions (server-only)
│       ├── lib/
│       │   ├── supabase/           Server + client + service-role helpers
│       │   ├── auth/               Session + tenant guards
│       │   └── telemetry/          Sentry + OTel
│       └── instrumentation.ts      Sentry + AI SDK tracing
├── packages/
│   ├── db/                         Drizzle schema + RLS + migrations
│   │   ├── schema/
│   │   │   ├── auth.ts             profiles, memberships
│   │   │   ├── tenancy.ts          organizations, organization_members
│   │   │   ├── admin.ts            admin_actions
│   │   │   ├── credits.ts          credit_balances, credit_ledger
│   │   │   ├── stores.ts           generated_stores, generation_runs, store_artifacts
│   │   │   ├── catalog.ts          products, product_variants
│   │   │   ├── commerce.ts         carts, cart_items, mock_checkout_sessions
│   │   │   └── verification.ts     verification_reports
│   │   ├── policies/               RLS pgPolicy definitions per table
│   │   ├── seed/                   Synthetic seed scripts
│   │   └── client.ts               postgres-js + drizzle factory
│   ├── contracts/                  Zod schemas (= AI I/O = DB inferred types)
│   │   ├── context.ts              ContextEnvelope schema
│   │   ├── intake.ts               BrandIntakeBrief schema (v1)
│   │   ├── generation.ts           StoreGenerationPlan schema (v1)
│   │   ├── credits.ts              CreditLedgerEvent schema (v1)
│   │   ├── ai.ts                   AiTaskResult, errors, usage
│   │   └── verification.ts         VerificationReport schema (v1)
│   ├── ai-gateway/                 Provider + task adapters using AI SDK v6
│   │   ├── providers/              openai, anthropic, google, mock
│   │   ├── tasks/                  normalize-brand, plan-store, gen-catalog, gen-copy, review-safety, summarize-verification
│   │   ├── routing.ts              Per-task model matrix
│   │   ├── safety.ts               Prompt-as-data, secret redaction
│   │   └── telemetry.ts            experimental_telemetry config
│   ├── generation/                 Orchestrator (called from Workflows)
│   ├── storefront/                 Pure renderer that consumes artifacts
│   ├── cart/                       Platform-owned cart engine
│   ├── verification/               Contract, RLS, UI, cart, a11y checks
│   ├── ui/                         shadcn registry + tokens + blocks
│   └── config/                     tsconfig, eslint, biome, tailwind presets
├── tooling/
│   ├── playwright/                 e2e test fixtures + helpers
│   └── fixtures/                   Demo fallback artifacts (offline mode)
├── docs/                           (existing)
├── mcp-profiles.json               (existing, extended in §10)
├── turbo.json
├── pnpm-workspace.yaml
└── package.json
```

This replaces the flat `src/` shape suggested in `extensibility-boundaries-review.md`. The `packages/` split is what makes the project look advanced and what enables future `apps/worker`, `apps/admin-portal`, or `apps/mobile` without rewrites.

---

## 4. System Architecture Diagram

```mermaid
flowchart TB
    subgraph client[Browser]
        BuilderUI[Builder UI]
        AdminUI[Super Admin UI]
        StoreUI[Generated Storefront]
    end

    subgraph web[apps/web - Next.js 16 on Vercel Fluid]
        AppRouter[App Router + Server Actions]
        Guards[Auth + Tenant Guards]
        ApiAi[/api/ai routes]
        WfTrigger[/api/workflows trigger]
    end

    subgraph workflows[Vercel Workflows - Durable]
        GenWf["generation.run workflow<br/>steps: validate, reserve credit,<br/>plan, catalog, copy, persist,<br/>verify, deduct or refund"]
    end

    subgraph packages[packages]
        Contracts[contracts - Zod]
        AiGw[ai-gateway - AI SDK v6]
        Gen[generation orchestrator]
        Cart[cart engine]
        Storefront[storefront renderer]
        Verify[verification runner]
        Db[db - Drizzle]
    end

    subgraph providers[AI Providers]
        OpenAI[OpenAI GPT-5.4 / Image-1]
        Anthropic[Claude Sonnet 4.6 / Haiku 4.5]
        Google[Gemini 2.5 Flash]
        Replicate[Flux Ultra 1.1]
        Mock[Mock fixture provider]
    end

    subgraph supabase[Supabase Cloud - dev project]
        Auth[Supabase Auth]
        Pg[("Postgres + RLS")]
    end

    subgraph obs[Observability]
        Sentry[Sentry + OTel]
    end

    BuilderUI --> AppRouter
    AdminUI --> AppRouter
    StoreUI --> AppRouter
    AppRouter --> Guards --> ApiAi --> AiGw
    AppRouter --> WfTrigger --> GenWf
    GenWf --> Gen
    Gen --> AiGw
    Gen --> Db
    AiGw --> OpenAI
    AiGw --> Anthropic
    AiGw --> Google
    AiGw --> Replicate
    AiGw -.fallback.-> Mock
    Storefront --> Db
    Cart --> Db
    Verify --> Db
    Verify --> Storefront
    Guards --> Auth
    Guards --> Pg
    Db --> Pg
    AppRouter -.traces.-> Sentry
    AiGw -.traces.-> Sentry
    GenWf -.traces.-> Sentry
```

---

## 5. Phased Plan With 7 Gates

The 28 flat phases become 7 named gates with binary done-criteria. Each gate must pass before the next starts.

### Gate 0 — Source Confirmation (1 day)
- Use `context7-mcp` skill against: `next.js@16`, `@supabase/ssr`, `drizzle-orm`, `ai@6`, `@ai-sdk/openai`, `@ai-sdk/anthropic`, `@ai-sdk/google`, `@vercel/workflows`, `shadcn-ui`, `@sentry/nextjs`, `playwright`.
- Use `openai-developer-docs` MCP for structured outputs and image-1.
- Output: `[docs/logs/2026-05-XX-source-confirmation.md](c:\Users\PC\Downloads\Grad project dPlan\docs\logs\source-confirmation.md)` with confirmed versions.
- **Done when**: pinned versions recorded in `package.json` plan and a confirmation log exists.

### Gate A — Foundation (3–4 days)
- Scaffold Turborepo + pnpm workspaces.
- Bootstrap `apps/web` with Next.js 16, Tailwind v4, shadcn/cli v4 (`--base radix`), Biome, Sentry instrumentation hooks.
- Create empty `packages/{db,contracts,ai-gateway,generation,storefront,cart,verification,ui,config}`.
- Drizzle wired to a fresh Supabase **development** project. `drizzle-kit generate` + `migrate`.
- Supabase Auth on signup/login/logout/protected layout. Pending-approval state.
- Sentry + OTel + Vercel AI SDK telemetry hooks installed but no AI calls yet.
- **Done when**: empty app deploys to Vercel preview, login works, Sentry receives a test error, RLS is enabled on `profiles` with negative cross-user test passing.

### Gate B — Tenancy + Credits + Admin (4–5 days)
- Drizzle schema for `profiles`, `organizations`, `organization_members`, `credit_balances`, `credit_ledger`, `admin_actions`.
- All RLS via co-located `pgPolicy` blocks; performance pattern `(SELECT auth.uid())`.
- Server-only credit helpers `reserveCredits`, `deductCredits`, `refundCredits`, `grantCredits` writing immutable ledger.
- Lazy Super Admin pages: pending-user queue, credit grant form, admin action audit list.
- Server-side super-admin guard checked on every privileged action; never trust client claims.
- **Done when**: pending user blocked, approved user lands in builder, super-admin can grant credits, ledger sums equal `credit_balances.balance` for every org (invariant test passes), cross-tenant access tests pass.

### Gate C — AI Gateway + Brand Intake (5–6 days)
- `packages/ai-gateway` task adapters with AI SDK v6 `Output.object()` and Zod schemas from `packages/contracts`.
- Per-task routing matrix:
  - `normalizeBrandBrief` → Claude Sonnet 4.6 (best at structured reasoning) → fallback GPT-5.4
  - `createStorePlan` → GPT-5.4 (best instruction following on complex JSON) → fallback Claude Sonnet 4.6
  - `generateCatalog` → GPT-5.4 → fallback Gemini 2.5 Flash
  - `generateCopyBlocks` → Claude Sonnet 4.6 → fallback GPT-5.4
  - `reviewSafety` → Haiku 4.5 (cheap, fast) → fallback GPT-5-nano
  - `summarizeVerification` → Haiku 4.5
  - `generateProductImage` → GPT-Image-1 (text on label, hero) + Flux Ultra (photoreal) → fallback static placeholder
  - All have `mock` provider fallback for demo safety
- Safety: `packages/ai-gateway/safety.ts` strips secrets, treats prompt as data, redacts before persistence.
- Brand Intake UI: prompt-first form + advanced fields (per `[brand-prompt-intake-spec.md](c:\Users\PC\Downloads\Grad project dPlan\docs\specs\brand-prompt-intake-spec.md)`), follow-up questions, accept-assumptions flow, credit estimate visible.
- AI SDK telemetry on with stable `functionId` per task, traces flowing to Sentry AI Agents Insights.
- **Done when**: a sample brief produces `ready_for_generation` brief in <30 s, prompt injection attempts produce inert data, mock provider produces full demo flow with zero network.

### Gate D — Store Generation Pipeline (6–7 days)
- Vercel Workflow `generation.run`:
  ```
  step 1: load brief + tenant context
  step 2: reserve credits
  step 3: ai-gateway.createStorePlan
  step 4: ai-gateway.generateCatalog
  step 5: ai-gateway.generateCopyBlocks
  step 6: ai-gateway.generateProductImages (per product, parallel, with mock fallback)
  step 7: persist artifacts to Supabase
  step 8: ai-gateway.summarizeVerification (after Gate E pass)
  step 9: deduct or refund credits based on terminal status
  ```
- Use Workflows step retries (3 with exponential backoff) and `step.waitForEvent` for human-in-the-loop admin approval.
- Template registry seeded with one strong base template + 6 category presets (fashion, beauty, electronics, home, fitness, gifts).
- Artifact store writes `store.config.json`, `theme.tokens.json`, `catalog.seed.json`, `copy.blocks.json`, `generation.report.json` into `store_artifacts` and signed-URL R2/Supabase Storage.
- **Done when**: a brief reaches `generation.artifacts_created` reliably, credits deducted only on success, mock-provider end-to-end in <10 s, every artifact validates against its schema.

### Gate E — Storefront + Cart + Mock Checkout + Verification (5–6 days)
- `packages/storefront` renders home, product listing, product detail, cart, mock checkout, confirmation purely from artifacts.
- `packages/cart` enforces variant selection, qty caps, subtotal, persistence, empty state.
- Mock checkout creates `mock_checkout_sessions`, never collects card data, success/cancel routes.
- `packages/verification` runs:
  - schema-presence checks
  - tenant-ownership probe (impersonate other org)
  - credit-ledger invariant
  - all required routes 200
  - cart add/remove/qty/subtotal Playwright suite
  - mock-checkout success + cancel
  - responsive at 360 / 768 / 1280 / 1920
  - Playwright a11y axe scan
  - prompt injection / secret leak grep on persisted artifacts
- **Done when**: 100% of verification checks pass; failed verification keeps store out of `demo_ready`.

### Gate F — Demo Readiness + Super Admin Polish (3–4 days)
- Super Admin gains store-approval queue + verification report viewer + run inspector.
- Demo fallback fixtures shipped under `tooling/fixtures/` so a network-less demo still completes.
- Vercel preview env vars locked: dev Supabase, dev OpenAI/Anthropic/Google keys, no prod payment keys.
- Sentry release tagged; alert created for AI errors > threshold.
- Demo runbook in `[docs/plans/demo-readiness-runbook.md](c:\Users\PC\Downloads\Grad project dPlan\docs\plans\demo-readiness-runbook.md)` updated with new flow.
- Graduation demo script + slide deck per `grad-demo-script` skill.
- **Done when**: a stranger can sign up, you approve them, they generate a store, and the whole thing works on a fresh device with only the preview URL.

---

## 6. Database Schema (Drizzle + RLS)

Tables (all UUID PK, timestamps, RLS enabled):

```
profiles (id ref auth.users, full_name, display_name, role, status, avatar_url, language, region)
organizations (id, name, slug, owner_user_id, created_at)
organization_members (organization_id, user_id, role, status)
admin_actions (id, actor_user_id, action_type, target_*, payload jsonb, created_at)
credit_balances (organization_id, balance, updated_at)  -- materialized from ledger
credit_ledger (id, organization_id, user_id, admin_actor_id, generation_run_id, type, amount, balance_after, reason, created_at)  -- immutable, append-only
generated_stores (id, organization_id, created_by_user_id, slug, status, template_key, preset_key, locale, currency)
generation_runs (id, store_id, organization_id, brief_id, status, credits_required, credits_reserved, credits_deducted, error jsonb, started_at, finished_at)
store_artifacts (id, store_id, run_id, artifact_type, schema_version, body jsonb, created_at)
products (id, store_id, slug, name, description, category, price_minor, currency, inventory_state)
product_variants (id, product_id, sku_placeholder, option_values jsonb, price_minor_override, inventory_qty)
carts (id, store_id, organization_id, session_key)
cart_items (id, cart_id, product_id, variant_id, qty, unit_price_minor_snapshot)
mock_checkout_sessions (id, cart_id, store_id, status, created_at, completed_at)
verification_reports (id, run_id, status, checks jsonb, demo_ready bool, created_at)
brand_intake_briefs (id, store_id, organization_id, schema_version, body jsonb, status)
```

**RLS rules (excerpt, declared via Drizzle `pgPolicy`):**

```ts
// packages/db/policies/tenancy.ts (sketch)
pgPolicy("members can read own org rows", {
  for: "select",
  to: authenticatedRole,
  using: sql`organization_id IN (
    SELECT organization_id FROM organization_members
    WHERE user_id = (SELECT auth.uid())
  )`,
})
```

**Invariants (must be tested):**

- `sum(credit_ledger.amount where org=X) === credit_balances.balance(X)` for every org
- No row in `generated_stores`, `generation_runs`, `store_artifacts`, `products`, `carts`, `cart_items`, `mock_checkout_sessions`, `verification_reports` is reachable across orgs
- `demo_ready = true` requires `verification_reports.status = passed` AND `admin_actions` row of type `store_approved`

---

## 7. AI Gateway Contracts (Zod, condensed)

```ts
// packages/contracts/context.ts
export const ContextEnvelope = z.object({
  schemaVersion: z.literal("context-v1"),
  userId: z.string().uuid(),
  organizationId: z.string().uuid(),
  storeId: z.string().uuid().nullable(),
  generationRunId: z.string().uuid().nullable(),
  actorRole: z.enum(["user", "super_admin"]),
  source: z.enum(["builder_ui", "super_admin", "fixture"]),
  createdAt: z.string().datetime(),
});

// packages/contracts/ai.ts
export const AiTaskResult = z.object({
  schemaVersion: z.literal("ai-task-result-v1"),
  task: z.enum(["normalize_brand","create_store_plan","generate_catalog","generate_copy","review_safety","summarize_verification","generate_product_image"]),
  provider: z.enum(["openai","anthropic","google","replicate","mock"]),
  model: z.string(),
  status: z.enum(["succeeded","failed","refused","fallback_used"]),
  output: z.unknown(),
  errors: z.array(z.string()).default([]),
  warnings: z.array(z.string()).default([]),
  usage: z.object({
    estimatedCredits: z.number().int().nonnegative(),
    inputTokens: z.number().int().nonnegative().optional(),
    outputTokens: z.number().int().nonnegative().optional(),
  }),
});
```

The `BrandIntakeBrief` and `StoreGenerationPlan` schemas already exist as JSON in `[docs/specs/prompt-output-contracts.md](c:\Users\PC\Downloads\Grad project dPlan\docs\specs\prompt-output-contracts.md)`. Convert each verbatim into Zod in `packages/contracts/intake.ts` and `packages/contracts/generation.ts`. Use `@ai-sdk/core`'s `Output.object({ schema })` so OpenAI/Anthropic structured-output validation is enforced provider-side and `safeParse` runs before persistence.

---

## 8. Skills Inventory (Existing + New)

**Project skills already declared in `[AGENTS.md](c:\Users\PC\Downloads\Grad project dPlan\AGENTS.md)` (keep, all eight):** `brand-prompt-intake`, `store-generator`, `ecommerce-schema`, `cart-checkout-testing`, `ui-quality-review`, `mcp-evaluator`, `deployment-runbook`, `grad-demo-script`.

**Adopt from your global skill library (already on your machine, just declare them as project-preferred):** `use-ai-sdk`, `react-best-practices`, `frontend-design`, `web-design-guidelines`, `web-perf`, `webapp-testing`, `playwright`, `sentry-nextjs-sdk`, `sentry-setup-ai-monitoring`, `deploy-to-vercel`, `vercel-cli-with-tokens`, `adr-skill`, `composition-patterns`, `security-best-practices`, `context7-mcp`.

**New project-custom skills to author (scaffold each in `~/.codex/skills/<slug>/SKILL.md`):**

1. `rls-policy-review` — checklist + tests every RLS migration must pass before merge
2. `credit-ledger-invariants` — the ledger-vs-balance invariant suite + reconciliation runbook
3. `ai-gateway-task-author` — how to add a new AI task: contract, provider routing, mock, telemetry
4. `generation-run-fixture-author` — produce a deterministic fallback fixture for demo offline mode
5. `multi-tenant-test-suite` — generates negative cross-tenant Playwright + SQL probes
6. `schema-zod-drizzle-sync` — keeps `packages/contracts` and `packages/db` schemas in lockstep
7. `storefront-template-author` — adds a new base template or category preset without breaking the platform boundary
8. `prompt-injection-redteam` — adversarial prompt pack for Brand Intake
9. `demo-fallback-mode` — verifies `tooling/fixtures/` boots a full demo without network
10. `git-flow-phase-aware` — agent-never-commits contract, staging/diff-size awareness, gate-scoped Conventional Commits, branch + tag + PR conventions (full spec in §11)

---

## 9. MCP Servers (May 2026, Active vs Reserved)

Update `[mcp-profiles.json](c:\Users\PC\Downloads\Grad project dPlan\mcp-profiles.json)`:

**Keep `core-dev` active (current):** Context7, OpenAI Docs, Shopify Dev MCP, shadcn, Playwright, Chrome DevTools, Cloudflare Docs, X Docs, project-scoped filesystem.

**Add to `core-dev`:**

- `vercel` (already as template, promote to active)
- `sentry` (already as template, promote to active — needed from Gate A)
- `supabase-readonly` (promote to active — read schema at all times)

**New profile `ai-runtime`:**

- `vercel-workflows` (Vercel MCP exposes workflow tools — confirm endpoint)
- `inngest` (backup runtime, only if Workflows has issues)
- `openai-developer-docs`
- `anthropic-docs` (add via context7 fallback)

**New profile `data-layer`:**

- `supabase-readonly`
- `drizzle` (community MCP if available, else context7)
- `prisma-remote` (kept as template only — not used)
- `neon` (kept as template only — not used)

**Stay disabled / reserved:** Stripe (`payments-future`), Shopify Storefront (`commerce-dev` — Shopify part), Pipedream/Zapier/Make/n8n (`automation`), Figma (`frontend-design` — only when designing).

**Drop or warn:** any "filesystem-global" MCP that would expose your full device — your `[mcp-profiles.json](c:\Users\PC\Downloads\Grad project dPlan\mcp-profiles.json)` already enforces this, keep that rule.

---

## 10. Documentation Updates

These docs must change to match the new plan. List of edits, in order:

- **Replace** `[PLAN.md](c:\Users\PC\Downloads\PLAN.md)` 28-row table with the 7-gate structure from §5.
- **Replace** `[docs/plans/005-version-one-complete-implementation-plan.md](c:\Users\PC\Downloads\Grad project dPlan\docs\plans\005-version-one-complete-implementation-plan.md)` with this gated plan.
- **Add** `[docs/decisions/0003-stack-2026-lockin.md](c:\Users\PC\Downloads\Grad project dPlan\docs\decisions\0003-stack-2026-lockin.md)` — record Drizzle, Vercel Workflows, AI SDK v6, monorepo as accepted.
- **Add** `[docs/decisions/0004-ai-task-routing-matrix.md](c:\Users\PC\Downloads\Grad project dPlan\docs\decisions\0004-ai-task-routing-matrix.md)` — per-task model + fallback table.
- **Add** `[docs/architecture/monorepo-layout.md](c:\Users\PC\Downloads\Grad project dPlan\docs\architecture\monorepo-layout.md)` — the `apps/`+`packages/` map.
- **Update** `[docs/architecture/system-overview.md](c:\Users\PC\Downloads\Grad project dPlan\docs\architecture\system-overview.md)` — embed the §4 mermaid diagram.
- **Update** `[docs/architecture/mcp-and-skills-architecture.md](c:\Users\PC\Downloads\Grad project dPlan\docs\architecture\mcp-and-skills-architecture.md)` — add §8 skills and §9 MCP additions.
- **Update** `[docs/specs/ai-adapter-gateway-spec.md](c:\Users\PC\Downloads\Grad project dPlan\docs\specs\ai-adapter-gateway-spec.md)` — replace generic adapter language with AI SDK v6 / `Output.object` / per-task routing.
- **Update** `[docs/specs/commerce-data-model-spec.md](c:\Users\PC\Downloads\Grad project dPlan\docs\specs\commerce-data-model-spec.md)` — point to Drizzle implementation in `packages/db`.
- **Add** `[docs/specs/observability-spec.md](c:\Users\PC\Downloads\Grad project dPlan\docs\specs\observability-spec.md)` — Sentry + OTel + AI telemetry contract.
- **Add** `[docs/specs/image-generation-spec.md](c:\Users\PC\Downloads\Grad project dPlan\docs\specs\image-generation-spec.md)` — GPT-Image-1 + Flux Ultra adapter behavior + placeholders fallback.
- **Add** `[docs/specs/workflows-orchestration-spec.md](c:\Users\PC\Downloads\Grad project dPlan\docs\specs\workflows-orchestration-spec.md)` — Vercel Workflows step graph + retry policy + idempotency.
- **Add** `[docs/decisions/0005-git-flow-phase-aware.md](c:\Users\PC\Downloads\Grad project dPlan\docs\decisions\0005-git-flow-phase-aware.md)` — accept the §11 git contract.
- **Add** `[docs/references/git-flow-phase-aware.md](c:\Users\PC\Downloads\Grad project dPlan\docs\references\git-flow-phase-aware.md)` — quick-lookup card mirroring the skill (commit prefixes, branch names, tags, PR template).
- **Add** `.gitmessage` template at repo root + `.github/pull_request_template.md` enforcing the gate-scoped format.
- **Update** `[mcp-profiles.json](c:\Users\PC\Downloads\Grad project dPlan\mcp-profiles.json)` per §9.
- **Update** `[AGENTS.md](c:\Users\PC\Downloads\Grad project dPlan\AGENTS.md)` "Project Skills" line with the §8 list and add a "Git Flow" section pointing at §11.
- **Add** a log per gate completion in `docs/logs/` named `YYYY-MM-DD-gate-X-pass.md` matching the tag.

---

## 11. Git Flow And Change Awareness

This section defines the project-custom skill `git-flow-phase-aware`. It is operational, not optional, and supersedes any default agent commit behavior.

### 11.1 Hard Rules For The Agent

- **The agent never runs `git commit`, `git push`, `git tag`, `git merge`, `git rebase`, `git reset --hard`, `git restore --staged`, or `git stash drop` unless the user explicitly says "commit", "push", "tag", "merge", "rebase", "reset", or "drop stash" in the same message.**
- The agent may freely read state with `git status`, `git diff`, `git diff --cached`, `git diff --stat`, `git log`, `git log --oneline -n 20`, `git show`, `git branch -vv`, `git remote -v`, `git reflog`. These are read-only signals.
- The agent may run `git add -p` or `git add <path>` only when the user says "stage" or "stage <path>". Otherwise it proposes the staging plan and waits.
- Before suggesting any edit, the agent reports a current-state block:

  ```
  branch: <name>  upstream: <ahead/behind>
  staged:    <files+lines>
  unstaged:  <files+lines>
  untracked: <files>
  last commit: <sha7> <subject>
  ```

  This block is the entry condition for any non-trivial edit suggestion.

### 11.2 Diff-Size Thresholds (Awareness Contract)

| Class | Bound | Agent behavior |
| --- | --- | --- |
| **Tiny** | ≤ 20 lines, ≤ 2 files | Suggest single commit, single message |
| **Small** | ≤ 50 lines, ≤ 3 files | Suggest single commit |
| **Medium** | ≤ 200 lines, ≤ 10 files | Suggest 1–2 commits with a split plan |
| **Large** | > 200 lines or > 10 files | **Stop.** Refuse single commit. Propose a numbered split (commit-by-commit table) before any further edit |
| **Huge** | > 500 lines or > 25 files | **Stop.** Treat as a structural change; propose a branch split (multiple PRs) |

A "line" counts both additions and deletions in `git diff --shortstat`. Generated/lockfile diffs (`pnpm-lock.yaml`, `*.snap`, build outputs) are reported separately and excluded from the threshold count.

### 11.3 Linear History Conventions

- `pull.rebase = true`, `merge.ff = only`, `rebase.autoStash = true`. Recorded in repo `.gitconfig`-equivalent docs; not auto-applied to user global config.
- No merge commits on feature branches. Use rebase + fast-forward.
- Squash a feature branch only if it is small and noisy; otherwise keep its commits as the linear story of the gate.
- Force-push only allowed on personal feature branches with `--force-with-lease`. Never on `main`.

### 11.4 Branch Naming (Tied To Gates)

```
gate-0/source-confirmation
gate-a/foundation-scaffold
gate-a/sentry-otel-bootstrap
gate-b/credits-ledger
gate-b/admin-approval-queue
gate-b/rls-policies
gate-c/ai-gateway-tasks
gate-c/brand-intake-ui
gate-d/workflows-generation-run
gate-d/template-registry
gate-e/storefront-renderer
gate-e/cart-mock-checkout
gate-e/verification-runner
gate-f/super-admin-store-approval
gate-f/demo-fallback-fixtures
chore/<area>      # build/tooling, no gate scope
docs/<area>       # docs-only changes
fix/<gate>/<area> # bugfixes inside a gate's scope
```

`main` is always green. Each branch targets a single gate area.

### 11.5 Commit Message Convention (Conventional Commits + Gate Scope)

```
<type>(<gate>/<area>): <imperative subject ≤72 chars>

<body — explain WHY, not WHAT. wrap at 72 chars.
reference contracts/spec sections that drove the change.>

Refs: docs/specs/<file>.md#<anchor>
Gate: <0|A|B|C|D|E|F>
```

Allowed `<type>`: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `perf`, `build`, `ci`, `style`, `revert`.
Allowed `<gate>`: `gate-0`, `gate-a`, `gate-b`, `gate-c`, `gate-d`, `gate-e`, `gate-f`, or `meta` for cross-gate housekeeping.

Examples:

```
feat(gate-b/credits): add credit_ledger schema with pgPolicy
test(gate-b/credits): assert ledger sum equals balance per org
docs(gate-c/ai-gateway): record per-task model routing matrix
fix(gate-d/workflows): make catalog step idempotent on retry
chore(meta/repo): pin AI SDK to 6.0.x and add renovate config
refactor(gate-e/cart): extract subtotal calculator into pure module
```

### 11.6 Tags (One Per Gate Pass)

Tag is created only after a gate's done-when block is fully green and the corresponding `docs/logs/` entry is merged.

```
v0.0-gate-0-source-confirmed
v0.1-gate-a-foundation-pass
v0.2-gate-b-tenancy-credits-pass
v0.3-gate-c-ai-intake-pass
v0.4-gate-d-generation-pass
v0.5-gate-e-storefront-verify-pass
v1.0-gate-f-graduation-ready
```

Tag message body must list the gate's done-when checklist with each item ticked.

### 11.7 What MUST NOT Be Mixed In One Commit

- Schema migration + business logic that depends on it. Split: migration commit first, then logic.
- Refactor + behavior change. Split: pure refactor first (tests still green), then behavior.
- New dependency + feature using it. Split: build/install commit first, then feature.
- Two different gates' work. **Never.** Open a separate branch.
- Generated artifacts (lockfile, `node_modules/`, build output) bundled with logic. Split lockfile into its own `chore(meta/deps)` commit.
- Formatting/lint sweeps mixed with logic. Split: `style(meta): biome format sweep`.

### 11.8 Pre-Commit Hygiene (Suggested, Not Auto-Run)

The agent surfaces this checklist before recommending a commit; it does not run them automatically.

- `pnpm typecheck` passes for changed packages.
- `pnpm lint` (Biome) passes.
- `pnpm test --filter=...changed` passes.
- For schema changes: `pnpm db:generate` clean, `pnpm db:migrate:dry-run` succeeds, RLS invariant test green.
- For credit changes: ledger-vs-balance invariant test green.
- For AI gateway changes: mock-provider end-to-end test green.

A `lefthook.yml` or `husky` pre-commit hook may enforce these; the agent proposes the hook config but does not install hooks autonomously.

### 11.9 Pull Request Template

`.github/pull_request_template.md`:

```md
## Gate
Gate: <0|A|B|C|D|E|F>
Branch: <branch-name>

## Summary
- <bullet 1>
- <bullet 2>

## Done-when items advanced
- [ ] <copy from §5 of the plan for this gate>

## Diff size
- staged: <files / +lines / -lines>
- generated/lockfile excluded: <files>

## Verification
- [ ] typecheck
- [ ] lint
- [ ] tests (changed packages)
- [ ] RLS / ledger invariants (if applicable)
- [ ] Playwright smoke (if UI)

## Risks / rollback
<one paragraph>

## Refs
- docs/specs/<...>
- docs/decisions/<...>
```

PR title format: `[Gate <X>/<area>] <imperative subject>`.

### 11.10 Recovery Patterns The Agent May Suggest (Never Auto-Run)

- "You probably want `git restore --staged <path>` to unstage."
- "Use `git switch -c gate-b/credits-ledger` from `main` to start a fresh branch."
- "`git reflog` will show the lost SHA from <action>."
- "`git stash push -k -m 'gate-c WIP'` to keep your index but park the worktree."
- "`git rebase -i origin/main` to clean up the gate's commit story before opening the PR."

### 11.11 Mapping To Gates

Every gate in §5 ends with the same operational closing step:

1. Run the gate's done-when verification.
2. Merge any open PRs into `main` via fast-forward only.
3. Write `[docs/logs/YYYY-MM-DD-gate-<X>-pass.md](c:\Users\PC\Downloads\Grad project dPlan\docs\logs)` with the green checklist.
4. Tag the merge commit per §11.6.
5. Open the next gate's tracking branch.

This is the canonical "linear flow organized into the phases" the user requested.

### 11.12 Skill File Outline

When the skill is authored at `~/.codex/skills/git-flow-phase-aware/SKILL.md`, it must include:

- One-paragraph description and trigger conditions ("activates whenever the user says commit/branch/tag, or before any non-trivial edit").
- The hard rules from §11.1.
- The diff-size table from §11.2.
- Branch/commit/tag/PR conventions from §11.4–§11.9.
- A short "what the agent does on session start" recipe: read `git status -sb`, `git diff --stat`, `git log --oneline -n 5`, and surface the current-state block.
- Cross-references to ADR `0005-git-flow-phase-aware.md` and the references card.

---

## 12. Updated Risks And Mitigations

| Risk | Mitigation |
| --- | --- |
| Vercel Workflows beta-style instability | Step-level retries; idempotency keys per step; mock provider fallback |
| AI SDK v6 still has breaking changes vs v5 | Pin exact version; lock `@ai-sdk/*` to same minor; track migration guide |
| Multi-tenancy leak | RLS at DB layer + Drizzle `pgPolicy` + `multi-tenant-test-suite` skill running in CI |
| Credit ledger drift | `credit-ledger-invariants` skill enforces sum-equals-balance check on every PR |
| Prompt injection | `prompt-injection-redteam` skill, `safety.ts` redaction, structured outputs validated server-side |
| Image gen cost blowout | Per-org image quota + cached placeholders + mock provider for non-demo runs |
| Demo network failure | `tooling/fixtures/` + `demo-fallback-mode` skill + `mock` provider on every task |
| Sentry quota waste | Sample non-error traces at 0.1; full sampling on errors and AI tasks |
| Monorepo overhead for one app | Justified by `packages/contracts` reuse; same shape supports later `apps/worker` without rewrite |
| AI hallucination of schemas | All AI outputs go through Zod `safeParse` before persistence; failure refunds credits |
| Agent makes silent / mixed commits | §11 hard rule: agent never commits; staging plan + diff-size class reported before any edit |
| Large untracked diff drift | §11.2 thresholds force a split plan once a change crosses Medium |
| Out-of-order gate work landing on `main` | §11.4 branch + §11.6 tag conventions tie every commit to a single gate |
| Copying Bolt/Stunning's WebContainer engine | §13 explicit refusal; artifact-driven architecture in `extensibility-boundaries-review.md` already correct |
| Underestimating per-demo AI cost | §14 cost matrix with worst-case-run estimate; per-org image quota; mock provider for non-graded runs |
| Missing email service for Supabase Auth | §13 + §14 add Resend with dev-tier free quota |
| Cart session loss across tabs | §13 + §14 add Upstash Redis (or Supabase row) for session persistence |

---

## 13. Infrastructure Strategy: Build vs Rent

This section is the direct answer to "why reinvent the wheel — does Stunning/Bolt sell their sandbox as SaaS?"

### 13.1 What Stunning.so And Bolt.new Actually Run On

From your own research in `[docs/research/stunning-so-technical-review-transcript-breakdown.txt](c:\Users\PC\Downloads\Grad project dPlan\docs\research\stunning-so-technical-review-transcript-breakdown.txt)` and `[docs/research/bolt-new-scaling-limitations-discussion.txt](c:\Users\PC\Downloads\Grad project dPlan\docs\research\bolt-new-scaling-limitations-discussion.txt)`:

- **Bolt.new** and **Stunning.so** both run on **StackBlitz WebContainers** — a browser-side Node.js runtime built on WebAssembly. Stunning's network calls were traced to `stackblitz.com`; Bolt is StackBlitz's own product.
- **WebContainers is not sold as a public SaaS tier.** It is enterprise-only with custom-negotiated agreements based on traffic volume. Confirmed in your transcript: "you cannot just buy a standard subscription; companies must contact StackBlitz to negotiate custom enterprise agreements".
- Stunning also embeds **Convex** (database) inside an iframe so the user sees a "live DB" inside the builder.
- The Bolt post-mortem in your research catalogues the failure modes that come with this architecture: context rot at 15 components, $200+ token burn from failure loops, OOM browser crashes, Supabase CORS deployment hell, no CI/CD, 800-line monolithic files, schema divergence.

### 13.2 Verdict: Refuse The WebContainer Path For Version One

**We do not run a per-tenant Node sandbox. We do not generate per-tenant React apps.** This is already the rule in `[docs/architecture/extensibility-boundaries-review.md](c:\Users\PC\Downloads\Grad project dPlan\docs\architecture\extensibility-boundaries-review.md)`:

> "Generated stores are data-driven. The platform renders them from approved templates and generated artifacts. Store Generation must not generate independent applications per user."

That decision is the right one for ecommerce specifically:

- Ecommerce stores share 95% of their structure (product list / detail / cart / checkout). The differences are **data, copy, theme tokens, layout choices** — exactly your artifact set.
- A platform-rendered storefront avoids every Bolt failure mode in your research. No context rot (no LLM in the render loop), no token burn per visit, no per-tenant CORS, no per-tenant deployment, no monolithic files to maintain.
- It also lets you scale to many stores on a single Vercel deployment, which Bolt cannot.
- WebContainers makes sense if the product **is** a code editor. Your product is a **store generator**, not an IDE. Different problem.

If a future graduation-bonus feature wants live in-browser code preview (e.g., theme tweaks before generating), a sandbox-as-a-service like **e2b.dev** or **Cloudflare Sandbox SDK** is dramatically cheaper and saner than WebContainers, and is documented as an explicit future extension only — not Version One.

### 13.3 What We DO Rent (Same Layer, Without The Engine)

We rent the **same surrounding SaaS** that Stunning and Bolt rent — auth, DB, AI, image, hosting, observability, email, search — and own only the orchestrator and renderer. This is the "don't reinvent the wheel" answer that actually applies.

| Need | Verdict | Service | Notes |
| --- | --- | --- | --- |
| Auth + DB + Storage + RLS | Rent | **Supabase** (free tier, dev project) | Already locked. Same provider Bolt deploys into. |
| Hosting + serverless + workflows | Rent | **Vercel** (Pro for demo) | Fluid Compute + Workflows GA April 2026. |
| Background AI orchestration | Rent | **Vercel Workflows** | Native AI SDK integration. Inngest as backup. |
| LLM | Rent | **OpenAI + Anthropic + Google** through AI SDK v6 | Per-task routing matrix in §5/Gate C. |
| Product image generation | Rent | **OpenAI gpt-image-1** + **Replicate Flux Ultra 1.1** | Two-track behind one adapter; placeholder fallback for cost. |
| Transactional email | Rent | **Resend** (free 3K/mo dev tier) | NEW. Required for Supabase Auth confirmation/recovery emails. |
| Cart session + rate limiting | Rent | **Upstash Redis** (free 10K cmd/day) | NEW. Cheap and avoids hammering Postgres for guest carts. |
| Artifact blob storage | Rent | **Cloudflare R2** OR **Supabase Storage** | R2 has free egress; Supabase keeps everything on one provider. Pick Supabase Storage for simplicity in Version One. |
| Error + AI observability | Rent | **Sentry** + `@sentry/opentelemetry` | AI Agents Insights dashboard for token cost per run. |
| Brand research (optional) | Rent | **Tavily** or **Firecrawl** | Used during intake assumptions; mock for demo. |
| Browser sandbox (per-tenant) | **Refuse** | n/a | See §13.2. |
| Code execution sandbox | Refuse for V1 | (future: e2b / Cloudflare Sandbox SDK) | Not Version One. |
| WebContainers | **Refuse** | n/a | Not publicly sold; not the right architecture for ecommerce. |

### 13.4 Two New Components Added By This Section

These were missing from earlier sections and are now part of Gate A and B:

- **Resend** for transactional email. Wire into Supabase Auth email templates (confirmation, magic link, password reset). Without this, Supabase Auth defaults to its built-in low-throughput emailer and bounces under demo conditions.
- **Upstash Redis** for guest cart sessions, rate-limit on AI gateway routes, and idempotency keys for Workflows steps. Optional but cheap.

Update §3 Folder Structure to include `apps/web/lib/email/resend.ts` and `apps/web/lib/redis/upstash.ts`. Update §10 Documentation Updates to add specs:

- `[docs/specs/transactional-email-spec.md](c:\Users\PC\Downloads\Grad project dPlan\docs\specs\transactional-email-spec.md)`
- `[docs/specs/redis-cache-and-ratelimit-spec.md](c:\Users\PC\Downloads\Grad project dPlan\docs\specs\redis-cache-and-ratelimit-spec.md)`

---

## 14. API Keys, Subscriptions, And Cost Map

This is the lookup card. Use it to provision accounts in Gate 0 and to forecast graduation-day cost. All numbers are May 2026 list prices; verify before locking.

### 14.1 Required Accounts And Keys (Tier 1 — Locked For Version One)

| Service | Plan for demo | Recurring cost | Variable cost | Env keys (.env.local) | Notes |
| --- | --- | --- | --- | --- | --- |
| **Supabase** | Free → Pro if real users sign up | $0 free / **$25/mo** Pro | None on free; Pro adds 8 GB DB, 100 GB egress | `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY` | Service role key is server-only, never client-bundled. |
| **Vercel** | Hobby (free) for preview-only OR Pro | $0 / **$20/mo** | Functions, Workflows, Queues per-execution | `VERCEL_URL` (auto), no key needed for self-deploy | Hobby is fine if no commercial use. |
| **OpenAI API** | Pay-as-you-go | $0 base | GPT-5.4: **$2.50 / $15.00 per 1M in/out**; GPT-5.4-mini: $0.75/$4.50; GPT-5-nano: $0.05/$0.40; gpt-image-1: ~$0.04–0.08/image | `OPENAI_API_KEY` | Set hard usage limit at $20 in dashboard. |
| **Anthropic API** | Pay-as-you-go | $0 base | Claude Sonnet 4.6: **$3.00 / $15.00 per 1M**; Haiku 4.5: $1.00/$5.00; Opus 4.7: $5.00/$25.00 | `ANTHROPIC_API_KEY` | Used for `normalizeBrandBrief` + `generateCopyBlocks`. |
| **Google AI Studio (Gemini)** | Free dev tier + paid | $0 dev | Gemini 2.5 Flash: **$0.30 / $2.50 per 1M**; 2.5 Flash-Lite: $0.10/$0.40 | `GOOGLE_GENERATIVE_AI_API_KEY` | Cheap fallback for catalog generation. |
| **Replicate** | Pay-as-you-go | $0 | Flux Ultra 1.1: **~$0.04–0.06 / image** (≈3–8 s) | `REPLICATE_API_TOKEN` | Photoreal product images. |
| **Sentry** | Developer (free) | $0 | 5K errors / 10K perf events / 50 replays per month | `SENTRY_DSN`, `SENTRY_AUTH_TOKEN`, `SENTRY_ORG`, `SENTRY_PROJECT` | Sufficient for graduation. Team plan $26/mo if exceeded. |
| **Resend** | Free dev | $0 | 3K emails/mo, 100/day | `RESEND_API_KEY` | Wire as Supabase Auth SMTP. |
| **Upstash Redis** | Free | $0 | 10K commands/day, 256 MB | `UPSTASH_REDIS_REST_URL`, `UPSTASH_REDIS_REST_TOKEN` | Cart sessions, AI route rate limits, Workflows idempotency. |
| **GitHub** | Free | $0 | n/a | n/a | Repo + Actions for CI typecheck/lint/test. |

**Required-tier total recurring**: **$0/mo on the all-free configuration**, **$45/mo** if Supabase Pro + Vercel Pro are turned on.

### 14.2 Optional Accounts (Tier 2 — Nice For Polish)

| Service | Why | Cost | Env keys |
| --- | --- | --- | --- |
| **Cloudflare R2** | Cheaper artifact/image blob storage with free egress | $0 first 10 GB / mo, then $0.015/GB | `R2_ACCOUNT_ID`, `R2_ACCESS_KEY_ID`, `R2_SECRET_ACCESS_KEY`, `R2_BUCKET` |
| **Tavily** | Brand-context web search during intake | Free 1K searches/mo, $50/mo Pro | `TAVILY_API_KEY` |
| **Firecrawl** | Site/scrape input for brand intake | Free 500 pages, paid from ~$20/mo | `FIRECRAWL_API_KEY` |
| **Helicone** OR **Langfuse** | Dedicated LLM-trace dashboard alongside Sentry | Free dev tiers | `HELICONE_API_KEY` or `LANGFUSE_PUBLIC_KEY`+`LANGFUSE_SECRET_KEY` |
| **Inngest** | Backup background runtime if Vercel Workflows misbehaves | Free 100K executions/mo, Pro $75/mo | `INNGEST_EVENT_KEY`, `INNGEST_SIGNING_KEY` |
| **Trigger.dev** | OSS alternative; self-hostable | Free + Apache 2.0 | `TRIGGER_API_KEY` |
| **Cloudflare** | DNS + CDN for production preview domain | Free | n/a (use dashboard) |

### 14.3 Refused For Version One (Do Not Provision)

| Service | Why refused |
| --- | --- |
| **Stripe** | Decision `0002` excludes payments. |
| **StackBlitz WebContainers** | §13.2 — not for ecommerce, enterprise-only contract, documented failure modes. |
| **Shopify Storefront / Hydrogen** | Future-only path; only enable if a real dev store is provided. |
| **Vendor payouts / Adyen / Tap** | Out of scope. |
| **Twilio / SMS** | Not needed; email is enough. |
| **AWS / GCP raw infra** | Vercel + Supabase already cover compute + DB. |

### 14.4 Per-Run Cost Estimate (Worst Case, Real Providers, No Mocks)

A single end-to-end "approved user generates a store" run on the recommended routing matrix:

| Step | Provider/model | Tokens or units (est.) | $ |
| --- | --- | --- | --- |
| `normalizeBrandBrief` | Claude Sonnet 4.6 | ~3K in / ~2K out | $0.039 |
| `createStorePlan` | GPT-5.4 | ~5K in / ~6K out | $0.103 |
| `generateCatalog` (12 products) | GPT-5.4 | ~6K in / ~10K out | $0.165 |
| `generateCopyBlocks` | Claude Sonnet 4.6 | ~5K in / ~4K out | $0.075 |
| `generateProductImage` × 12 | Flux Ultra 1.1 (Replicate) | 12 images | $0.60 |
| `reviewSafety` | Haiku 4.5 | ~2K in / ~1K out | $0.007 |
| `summarizeVerification` | Haiku 4.5 | ~3K in / ~1K out | $0.008 |
| **Total per run** | | | **≈ $1.00** |

A 10-credit demo grant covering 10 full runs costs ~$10 in real provider spend. The mock provider drops this to $0 for non-graded test runs.

### 14.5 Graduation-Day Budget (Single-Day Demo)

| Bucket | $ |
| --- | --- |
| 5 fresh end-to-end runs (real providers) | $5 |
| Idle Vercel + Supabase + Sentry + Upstash + Resend + Replicate | $0 (free tiers) |
| Buffer for surprise admin runs | $5 |
| **Total demo day** | **≈ $10** |

### 14.6 Monthly Standby Cost (Project Idle)

| Item | $ |
| --- | --- |
| Vercel Hobby + Supabase Free + Sentry Dev + Resend Free + Upstash Free + GitHub Free | $0 |
| OpenAI / Anthropic / Google / Replicate (zero usage) | $0 |
| **Idle total** | **$0/mo** |

If Pro tiers are turned on for headroom: **~$45/mo** (Supabase $25 + Vercel $20).

### 14.7 `.env.example` Skeleton (Lockfile For Gate A)

```dotenv
# --- core ---
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
DATABASE_URL=postgresql://...
# --- AI providers ---
OPENAI_API_KEY=
ANTHROPIC_API_KEY=
GOOGLE_GENERATIVE_AI_API_KEY=
REPLICATE_API_TOKEN=
# --- email + cache ---
RESEND_API_KEY=
UPSTASH_REDIS_REST_URL=
UPSTASH_REDIS_REST_TOKEN=
# --- observability ---
SENTRY_DSN=
SENTRY_AUTH_TOKEN=
SENTRY_ORG=
SENTRY_PROJECT=
# --- optional ---
R2_ACCOUNT_ID=
R2_ACCESS_KEY_ID=
R2_SECRET_ACCESS_KEY=
R2_BUCKET=
TAVILY_API_KEY=
FIRECRAWL_API_KEY=
HELICONE_API_KEY=
INNGEST_EVENT_KEY=
INNGEST_SIGNING_KEY=
# --- demo control ---
AI_PROVIDER_MODE=mock|live          # mock for offline demo fallback
DEMO_FIXTURE_PATH=tooling/fixtures
```

### 14.8 Provisioning Order (Day-One Checklist For Gate 0)

1. Create dev Supabase project → copy 3 keys.
2. Create Vercel project (link to GitHub repo, do not deploy yet) → import env vars.
3. Create OpenAI org + project, set $20 hard limit → key.
4. Create Anthropic Console org → key.
5. Create Google AI Studio project → key.
6. Create Replicate account → token.
7. Create Sentry org + project (Next.js platform) → DSN + auth token.
8. Create Resend account, verify a domain or use Resend test domain → key. Switch Supabase Auth SMTP to Resend.
9. Create Upstash Redis db (region near Vercel) → REST URL + token.
10. Optionally: Cloudflare R2, Tavily, Firecrawl.
11. Drop everything into Vercel project env vars **and** local `.env.local` (gitignored).
12. Author `[docs/references/api-keys-and-costs.md](c:\Users\PC\Downloads\Grad project dPlan\docs\references\api-keys-and-costs.md)` mirroring this section so the table lives in the repo too.

---

## 15. Future-Future Backlog (Parking Notes)

Side notes captured here so they are not lost. **Not Version One. Not Version Two.** Do not pull any of these into a gate without an explicit decision in `docs/decisions/`.

### 15.1 Per-Tenant Custom Backend Extensions (FUTURE-FUTURE)

**The ask.** "Not the same backend we built once would be the same for everyone obligatory." Some users will eventually want custom server-side behavior beyond the platform-built defaults: custom webhooks, custom integrations (Klaviyo, ShipStation, custom CRM/ERP), custom pricing or tax rules, custom data syncs, custom region/locale logic, region-specific auth, etc.

**Why this is parked, not in V1.** It directly tensions the platform-vs-generated boundary in `[docs/architecture/extensibility-boundaries-review.md](c:\Users\PC\Downloads\Grad project dPlan\docs\architecture\extensibility-boundaries-review.md)` ("no per-store generated cart, auth, RLS, or checkout logic"). Letting tenants ship raw code per store re-creates the exact failure modes documented in `[docs/research/bolt-new-scaling-limitations-discussion.txt](c:\Users\PC\Downloads\Grad project dPlan\docs\research\bolt-new-scaling-limitations-discussion.txt)`. Version One must not relax this rule.

**Likely shape when we eventually build it.**

- A controlled, allow-list-based **Tenant Extensions Registry** — not raw code per store.
- Three layered modes, in order of risk:
  1. **Declarative integrations**: a curated registry of pre-built adapters (Klaviyo, Mailchimp, ShipStation, custom webhook URL, Zapier hook, Stripe-when-payments-reopen). Tenant configures via UI, no code.
  2. **Server-side extension hooks**: a small set of named hook points (`onOrderCreated`, `onCartUpdated`, `onProductViewed`, `onCheckoutCompleted`) where tenants can plug a registered integration or a JSON-shaped transform. Still no arbitrary code.
  3. **Sandboxed tenant functions** (highest risk, last to ship): per-tenant code runs in a hardened sandbox-as-a-service — `e2b.dev`, **Cloudflare Sandbox SDK** (your `~/.codex/skills/sandbox-sdk` skill), or a Cloudflare Worker per tenant with strict CPU/memory/egress quotas. Each function is reviewed and deployed by Super Admin, never auto-published. Refuse WebContainers for the same reasons in §13.2.
- All three modes share: per-tenant quota, per-tenant secrets vault (Vercel/Supabase encrypted env), audit log via `admin_actions`, kill switch per tenant, observable in Sentry, billable as separate credit type (e.g., "extension credits") so it doesn't pollute generation credits.
- A new contract `tenant_extension_v1` that mirrors the artifact pattern — declarative config persisted in Postgres, validated by Zod, never mutated at runtime by AI.

**Earliest place this could live.** Version Three (post-graduation), behind ADR `0007-tenant-extensions` (not yet written). Until then, this is a parking note only.

**Why we do not block Version One on this.** The graduation thesis is "structured AI generation that does not collapse like Bolt/Lovable." Per-tenant custom backend is an orthogonal product feature, not part of that thesis. Parking it preserves architectural discipline and graduation focus.

### 15.2 Other Items Already Parked Elsewhere

These are documented in `[docs/specs/project-scope-v1-and-roadmap.md](c:\Users\PC\Downloads\Grad project dPlan\docs\specs\project-scope-v1-and-roadmap.md)` under "Future-Ready Extension Points" and stay parked:

- Stripe / live payments (decision `0002`).
- Real Shopify Storefront / Hydrogen / UCP / Cart MCP / Checkout MCP path.
- Visual builder with controlled inspector + section reordering.
- Live ad campaign launch and creative orchestration.
- Marketplace / add-on ecosystem.
- Vendor payouts and commission settlement.
- Real-customer order operations, fulfillment, tax, shipping, returns.
- Live in-browser code preview (would use `e2b` or Cloudflare Sandbox SDK, never WebContainers — §13.2).

### 15.3 Rule For Adding To This Section

Any "future-future" item added here must include:

- A one-paragraph ask in the user's own framing.
- The reason it is parked (which V1 invariant it would violate).
- A rough shape of what it would look like when built.
- The earliest realistic version it could land in.

This keeps the parking lot useful instead of becoming a wishlist dumping ground.

---

## 16. Done Means (Graduation Bar)

- A stranger signs up at the preview URL.
- Super Admin (you) approves them and grants 10 credits inside the lazy dashboard.
- They submit a brand prompt; AI returns a structured brief with follow-ups in <30 s.
- They start Store Generation; the Vercel Workflow runs steps with visible progress.
- A storefront with home, listing, detail, cart, mock checkout, confirmation is live at a unique slug.
- The verification report is green; admin store-approval flips status to `demo_ready`.
- Sentry shows traces, token costs, and zero unresolved errors.
- The same demo runs offline using fixtures.
- All `docs/` updates above are merged.
- The graduation script (per `grad-demo-script` skill) reads in 6–8 minutes.
