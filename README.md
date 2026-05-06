# Grad Store Builder — Version One

An authenticated, multi-tenant AI e-commerce store builder for a graduation project (2026).

An approved user writes a brand prompt; the system generates a full storefront with catalog, cart, mock checkout, and automated verification — all observable, all safe for a live demo.

---

## What This Repo Contains

This repository is the **planning, architecture, and documentation workspace** for Version One. It covers every decision, spec, contract, research note, and implementation plan before code is written.

| Folder | Contents |
| --- | --- |
| `docs/architecture/` | System overview, data flow, tenancy boundaries, MCP/skills architecture |
| `docs/specs/` | Product and technical specs: auth, credits, AI gateway, commerce, UI/UX, contracts |
| `docs/decisions/` | Architecture Decision Records (ADRs) |
| `docs/plans/` | Implementation plans, sequencing, backlog, demo-readiness runbook |
| `docs/prompts/` | Reusable prompts for planning, research synthesis, and image generation |
| `docs/research/` | Competitor analysis (Bolt.new, Stunning.so, Lovable, v0), technical deep-dives |
| `docs/references/` | Stable reference docs, imported proposal, Shopify guide |
| `docs/notes/` | Working notes |
| `docs/logs/` | Dated change summaries |
| `docs/archive/` | Superseded drafts |
| `docs/latex/` | LaTeX source and compiled PDFs for the supervisor deck |
| `Images/` | Generated architecture diagrams (10 images, one per gate + bonuses) |
| `.cursor/plans/` | The advanced 7-gate implementation plan |

---

## The Plan: 7 Enforceable Gates

The project replaces a flat 28-phase outline with 7 named gates, each with binary pass/fail criteria. Every gate ends with a tag, a docs log, and a green checklist.

| Gate | Theme | Days | Tag on Pass |
| --- | --- | --- | --- |
| **0** | Source Confirmation | 1 | `v0.0-gate-0-source-confirmed` |
| **A** | Foundation (monorepo, auth, DB, Sentry) | 3–4 | `v0.1-gate-a-foundation-pass` |
| **B** | Tenancy + Credits + Admin | 4–5 | `v0.2-gate-b-tenancy-credits-pass` |
| **C** | AI Gateway + Brand Intake | 5–6 | `v0.3-gate-c-ai-intake-pass` |
| **D** | Store Generation Pipeline (Vercel Workflows) | 6–7 | `v0.4-gate-d-generation-pass` |
| **E** | Storefront + Cart + Mock Checkout + Verification | 5–6 | `v0.5-gate-e-storefront-verify-pass` |
| **F** | Demo Readiness + Super Admin Polish | 3–4 | `v1.0-gate-f-graduation-ready` |

**Total: ~33 working days to graduation bar.**

---

## 2026 Stack

| Layer | Technology |
| --- | --- |
| **App** | Next.js 16 App Router, React 19 |
| **Repo** | Turborepo + pnpm workspaces |
| **Auth** | Supabase Auth (`@supabase/ssr`) |
| **DB** | Supabase Postgres + RLS |
| **ORM** | Drizzle ORM with `pgPolicy` |
| **AI** | Vercel AI SDK v6, `Output.object()`, `ToolLoopAgent` |
| **Models** | GPT-5.4, Claude Sonnet 4.6, Haiku 4.5, Gemini 2.5 Flash |
| **Images** | GPT-Image-1 + Flux Ultra 1.1 (Replicate) |
| **Orchestration** | Vercel Workflows (GA April 2026) |
| **UI** | shadcn/cli v4 + Tailwind v4 + Radix |
| **Observability** | Sentry + `@sentry/opentelemetry` + AI SDK telemetry |
| **Verification** | Playwright + Chrome DevTools MCP |
| **Deploy** | Vercel preview + Fluid Compute |

---

## AI Task Routing Matrix

Every AI task has a primary model and a documented fallback, plus a mock provider for offline demo safety.

| Task | Primary | Fallback |
| --- | --- | --- |
| normalizeBrandBrief | Claude Sonnet 4.6 | GPT-5.4 |
| createStorePlan | GPT-5.4 | Claude Sonnet 4.6 |
| generateCatalog | GPT-5.4 | Gemini 2.5 Flash |
| generateCopyBlocks | Claude Sonnet 4.6 | GPT-5.4 |
| reviewSafety | Claude Haiku 4.5 | GPT-5-nano |
| summarizeVerification | Claude Haiku 4.5 | — |
| generateProductImage | GPT-Image-1 + Flux Ultra 1.1 | Static placeholder |

---

## Architecture Diagrams

Ten generated diagrams live in `Images/` and `docs/latex/figures/`. Two compiled PDF decks are ready for the supervisor discussion:

1. **`docs/latex/01-version-one-technical-deck.pdf`** — 13-page landscape A4 technical deck. Each gate on one page: image left, technical narrative right.

2. **`docs/latex/02-version-one-bilingual-pitch.pdf`** — 17-page portrait A4 bilingual pitch (English left, Arabic right). Simple language, pitch scripts, closing summary.

---

## Monorepo Layout (Target)

```
grad-store-builder/
├── apps/
│   └── web/                        Next.js 16 App Router
├── packages/
│   ├── contracts/                  Zod schemas (single source of truth)
│   ├── db/                         Drizzle schema + RLS + migrations
│   ├── ai-gateway/                 AI SDK v6 task adapters
│   ├── generation/                 Workflow orchestrator
│   ├── storefront/                 Pure renderer from artifacts
│   ├── cart/                       Platform-owned cart engine
│   ├── verification/               9-check verification runner
│   ├── ui/                         shadcn registry + tokens
│   └── config/                     tsconfig, biome, tailwind
├── tooling/
│   ├── playwright/                 E2E test fixtures
│   └── fixtures/                   Demo fallback artifacts
└── docs/                           This documentation set
```

---

## Graduation Bar

> A stranger signs up at the preview URL. Super Admin approves them and grants 10 credits. They submit a brand prompt; AI returns a structured brief in <30 seconds. The Vercel Workflow runs 9 generation steps with visible progress. A storefront with home, listing, detail, cart, mock checkout, and confirmation is live at a unique slug. The verification report is green. Sentry shows traces, token costs, and zero unresolved errors. The same demo runs offline using fixtures.

---

## Key Design Decisions

- **No WebContainers.** Generated stores are data-driven artifacts rendered by the platform, not independent per-tenant apps. This avoids every failure mode documented in the Bolt.new research.
- **No production payments.** Admin-granted credits + mock checkout only. Stripe is a future-only path.
- **RLS at the database, not in JavaScript.** Drizzle `pgPolicy` enforces tenant isolation; the app cannot bypass it.
- **Immutable credit ledger.** Every grant, reservation, deduction, and refund is an append-only row. The visible balance is the sum.
- **One AI gateway.** Every AI call passes through `packages/ai-gateway` for safety, telemetry, routing, and mock fallback.
- **Mock provider on every task.** The demo works offline from `tooling/fixtures/`.

---

## Cost

| Item | Cost |
| --- | --- |
| Per-run (worst case, real providers) | ~$1.00 |
| Demo day (5 runs + buffer) | ~$10 |
| Monthly idle (all free tiers) | $0 |

---

## Documentation

Full documentation lives in `docs/`. Start with:

- [`docs/specs/project-scope-v1-and-roadmap.md`](docs/specs/project-scope-v1-and-roadmap.md) — scope and roadmap
- [`docs/decisions/0002-version-one-authenticated-scope.md`](docs/decisions/0002-version-one-authenticated-scope.md) — why authenticated, not anonymous
- [`docs/architecture/system-overview.md`](docs/architecture/system-overview.md) — system architecture
- [`.cursor/plans/version-one-advanced-plan_5edfc7dd.plan.md`](.cursor/plans/version-one-advanced-plan_5edfc7dd.plan.md) — the full 7-gate implementation plan
- [`docs/plans/006-templates-and-post-graduation-roadmap.md`](docs/plans/006-templates-and-post-graduation-roadmap.md) — **Templates UX spec + investment-grade V2/V3 roadmap (supervisor discussion document)**

---

## License

Graduation project. All rights reserved.
