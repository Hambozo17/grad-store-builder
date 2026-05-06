# Version One — Image-Generation Prompts (One Per Gate)

Source plan: `[.cursor/plans/version-one-advanced-plan_5edfc7dd.plan.md](.cursor/plans/version-one-advanced-plan_5edfc7dd.plan.md)`

Purpose: produce a coherent set of slides/diagrams for the supervisor ("doctor") discussion. Each prompt below renders one workflow image. All share one visual identity so the deck looks unified.

How to use:

1. Pick any image generator (GPT-Image-1, Midjourney v7, FLUX 1.1 Ultra, Imagen 4, Nano Banana, Sora-Image, Ideogram 3.0).
2. Always paste the **Shared Style Block** first, then the gate's specific prompt.
3. Render at **16:9, 1920×1080** for slide-deck use.
4. Use the suggested filename so they sort in order in the slide deck folder.

---

## Shared Style Block (paste before every gate prompt)

```
Modern flat-isometric technical workflow infographic, slide-deck quality, 16:9 aspect ratio, 1920x1080.
Background: deep navy (#0B1F3F) with a subtle radial gradient fading to charcoal in the corners; thin grid lines at 6% opacity.
Color system: primary nodes in electric teal (#00E0D6); AI / model nodes in warm violet (#7C5CFF) with a soft outer glow; success highlights in emerald (#22C55E); warning / risk callouts in amber (#F59E0B); database nodes in slate blue (#3B82F6); user / human nodes in soft white.
Typography: geometric sans-serif (Inter / SF Pro feel), crisp white for titles, light grey (#CBD5E1) for secondary labels, monospace tags for keys/IDs.
Iconography: 2px outlined glyphs, rounded corners on every container (12px radius), slight drop shadow under elevated cards, dotted lines for asynchronous edges, solid arrows for synchronous edges, double-line arrows for AI-call edges.
Composition: left-to-right flow with numbered circular badges (1, 2, 3...) on each step; swimlanes labeled at the far left when multiple actors are involved.
Mood: professional, academic, futuristic-but-credible, no clutter, no stock-photo people, no emoji, no flag icons, no fake brand logos.
Top-left title bar: bold white headline + thin teal underline. Bottom-right corner: small monospace tag "Grad Store Builder · Version One · 2026".
Render as one single coherent image, not a collage.
```

---

## 0. Master System Architecture (open the deck with this one)

Filename suggestion: `00-system-architecture.png`

Caption: *"End-to-end architecture: browser → Next.js 16 on Vercel → Vercel Workflows → AI Gateway (multi-provider) → Supabase Postgres with RLS, observable through Sentry + OTel."*

Prompt (append after Shared Style Block):

```
Title at top: "System Architecture — Grad Store Builder V1".

Render a single isometric workflow with FOUR horizontal swimlanes stacked top to bottom:

Lane 1 — "Browser" (soft white): three rounded cards labeled "Builder UI", "Super Admin UI", "Generated Storefront", each with a small monitor icon.

Lane 2 — "apps/web · Next.js 16 on Vercel Fluid Compute" (teal): one wide rounded container holding four pill-shaped sub-blocks: "App Router + Server Actions", "Auth + Tenant Guards", "/api/ai routes", "/api/workflows trigger". Add a small Vercel triangle glyph in the corner.

Lane 3 — "Durable Orchestration · Vercel Workflows" (violet glow): one large rounded card titled "generation.run" containing nine numbered steps left-to-right: 1 Validate, 2 Reserve Credit, 3 Plan, 4 Catalog, 5 Copy, 6 Images, 7 Persist, 8 Verify, 9 Deduct/Refund. Connect them with solid teal arrows.

Lane 4 — "Platform Services" (slate blue): five cards side by side: "AI Gateway · packages/ai-gateway" (violet), "Drizzle ORM · packages/db" (teal), "Supabase Postgres + RLS" (slate, with a database cylinder icon), "Supabase Auth" (slate, with a key icon), "Sentry + OpenTelemetry" (amber, with a bell icon).

Off to the right side, vertically stacked outside the lanes, render an "AI Providers" cluster with five small glowing violet pills: "OpenAI GPT-5.4 / gpt-image-1", "Claude Sonnet 4.6", "Claude Haiku 4.5", "Gemini 2.5 Flash", "Replicate · Flux Ultra 1.1", plus a smaller dashed grey pill labeled "Mock fixture provider · demo fallback".

Draw double-line violet arrows from the AI Gateway card to each provider pill. Draw dotted amber arrows from App Router, AI Gateway, and Workflows down into Sentry to indicate trace flow. Draw a thick teal arrow from Vercel Workflows down into Drizzle ORM and from Drizzle into Supabase Postgres.

No clutter, only labeled edges. Keep negative space generous.
```

---

## 1. Gate 0 — Source Confirmation

Filename suggestion: `01-gate-0-source-confirmation.png`

Caption: *"Day one: lock the 2026 stack by source-confirming every library version through Context7 and OpenAI Docs MCPs before writing a line of code."*

Prompt:

```
Title at top: "Gate 0 — Source Confirmation · 1 day".
Subtitle in light grey: "Pin every version. Write nothing else.".

Render a horizontal radial diagram. In the centre place a large violet hexagon labeled "Context7 MCP + OpenAI Docs MCP" with a small magnifying-glass-over-book icon.

Around the hexagon, arrange ELEVEN smaller teal rounded pills in a half-circle, each labeled with a library and a target version tag in monospace:
"next.js@16", "@supabase/ssr", "drizzle-orm", "ai@6", "@ai-sdk/openai", "@ai-sdk/anthropic", "@ai-sdk/google", "@vercel/workflows", "shadcn-ui · cli v4", "@sentry/nextjs", "playwright".
Connect each pill to the central hexagon with a thin double-line teal arrow.

Below the radial, render a single emerald rounded "output" card labeled:
"docs/logs/2026-05-XX-source-confirmation.md"
with three checkmark bullet lines underneath:
"✓ versions pinned in package.json plan",
"✓ ADR 0003 stack-2026-lockin drafted",
"✓ ADR 0004 ai-task-routing-matrix drafted".

Bottom-right corner: an amber "Done when" tag with the text "pinned versions recorded + confirmation log exists".

No code blocks, no UI screenshots, just the radial flow and the output card.
```

---

## 2. Gate A — Foundation

Filename suggestion: `02-gate-a-foundation.png`

Caption: *"Scaffold the Turborepo, wire Supabase Auth + Drizzle + Sentry, deploy an empty preview, and prove RLS works with a negative cross-user test."*

Prompt:

```
Title at top: "Gate A — Foundation · 3–4 days".
Subtitle in light grey: "Empty app deploys to Vercel preview · login works · RLS proven.".

Render a left-to-right pipeline of FIVE numbered stage cards, each in a rounded teal container with a small icon and 2–3 bullet labels inside:

Stage 1 "Scaffold Monorepo" (icon: stacked cubes) — "Turborepo + pnpm", "apps/web · Next.js 16", "packages/{db, contracts, ai-gateway, generation, storefront, cart, verification, ui, config}".

Stage 2 "Bootstrap Web App" (icon: triangle/Vercel) — "App Router · React 19", "Tailwind v4 + shadcn/cli v4 (--base radix)", "Biome lint+format".

Stage 3 "Database Layer" (icon: cylinder) — "Drizzle wired to Supabase dev project", "drizzle-kit generate + migrate", "RLS enabled on profiles".

Stage 4 "Auth Layer" (icon: key) — "Supabase Auth via @supabase/ssr", "signup / login / logout", "pending-approval state".

Stage 5 "Observability" (icon: bell with waveform) — "@sentry/nextjs + @sentry/opentelemetry", "AI SDK telemetry hooks installed", "no AI calls yet".

Connect stages with solid teal arrows.

Below the pipeline, render an emerald "Done when" panel with four ticked checkboxes left-to-right:
"☑ empty app deploys to Vercel preview",
"☑ login works",
"☑ Sentry receives a test error",
"☑ negative cross-user test on profiles passes".

Top-right corner: a small violet badge "Branch: gate-a/foundation-scaffold".
Bottom-right corner: amber tag "Tag on pass: v0.1-gate-a-foundation-pass".
```

---

## 3. Gate B — Tenancy + Credits + Admin

Filename suggestion: `03-gate-b-tenancy-credits-admin.png`

Caption: *"Multi-tenancy enforced at the database layer (RLS via Drizzle pgPolicy), an immutable credit ledger, and a lazy Super Admin queue for approving users and granting credits."*

Prompt:

```
Title at top: "Gate B — Tenancy · Credits · Admin · 4–5 days".
Subtitle in light grey: "RLS at the DB · immutable ledger · lazy Super Admin.".

Split the canvas into THREE vertical columns separated by thin vertical dividers.

COLUMN 1 — "Tenancy Schema (Drizzle + pgPolicy)" (header in teal):
Stack five small slate-blue database cards vertically, each shaped like a tiny table with a header bar:
"profiles", "organizations", "organization_members", "admin_actions", "memberships".
Below them, a violet rounded callout box labeled
"RLS policy:  organization_id IN ( SELECT organization_id FROM organization_members WHERE user_id = (SELECT auth.uid()) )"
in monospace.

COLUMN 2 — "Credits System (Immutable Ledger)" (header in teal):
At top, a slate-blue card "credit_balances" with a thin arrow pointing down to a wider slate-blue card "credit_ledger" tagged "append-only · immutable".
To the right of these, four small teal pill helpers arranged vertically:
"reserveCredits()", "deductCredits()", "refundCredits()", "grantCredits()".
Each pill points an arrow into "credit_ledger".
At the bottom of the column, an emerald invariant badge:
"Σ ledger.amount per org  ==  credit_balances.balance".

COLUMN 3 — "Lazy Super Admin UI" (header in teal):
Render three stacked browser-window mock cards (no real screenshot, just framed rectangles):
- "Pending Users Queue"
- "Credit Grant Form"
- "Admin Action Audit Log"
Each card has 3–4 placeholder line bars inside.
Above the cards, a small violet badge "server-side super-admin guard · never trust client".

Below all three columns, an emerald "Done when" footer band with four ticked items in one row:
"☑ pending user blocked",
"☑ approved user lands in builder",
"☑ ledger sum equals balance for every org",
"☑ cross-tenant access tests pass".

Top-right corner: small violet badge "Branch: gate-b/credits-ledger".
Bottom-right corner: amber tag "Tag on pass: v0.2-gate-b-tenancy-credits-pass".
```

---

## 4. Gate C — AI Gateway + Brand Intake

Filename suggestion: `04-gate-c-ai-gateway-brand-intake.png`

Caption: *"A typed AI Gateway built on AI SDK v6 with per-task model routing, mock-provider fallback, prompt-injection safety, and a Brand Intake UI that turns a raw prompt into a validated brief in under 30 seconds."*

Prompt:

```
Title at top: "Gate C — AI Gateway + Brand Intake · 5–6 days".
Subtitle in light grey: "AI SDK v6 · Output.object() · per-task routing · mock fallback.".

Render two stacked sections with a horizontal divider between them.

UPPER SECTION — "Brand Intake Flow" (40% of canvas height):
Render a left-to-right horizontal flow of FOUR cards:
1. White user-card "User pastes brand prompt" (icon: chat bubble) →
2. Teal card "Brand Intake UI · prompt-first form + advanced fields" (icon: form) →
3. Violet glowing card "AI Gateway · normalizeBrandBrief task" →
4. Emerald card "BrandIntakeBrief · status: ready_for_generation" (icon: stamped document).
Below card 3, a small amber callout: "follow-up questions + accept-assumptions flow · credit estimate visible".

LOWER SECTION — "AI Gateway Internals" (60% of canvas height):
Header in teal: "packages/ai-gateway".

On the left, render a vertical stack of SEVEN task pills (teal outlined):
"normalizeBrandBrief", "createStorePlan", "generateCatalog", "generateCopyBlocks", "reviewSafety", "summarizeVerification", "generateProductImage".

In the centre, a large violet rounded "Routing Matrix" card with a small grid icon. Inside it, render the following compact routing rows in monospace, each row showing primary then dashed-arrow fallback:
"normalizeBrandBrief  →  Claude Sonnet 4.6  ⇢  GPT-5.4",
"createStorePlan      →  GPT-5.4           ⇢  Claude Sonnet 4.6",
"generateCatalog      →  GPT-5.4           ⇢  Gemini 2.5 Flash",
"generateCopyBlocks   →  Claude Sonnet 4.6  ⇢  GPT-5.4",
"reviewSafety         →  Claude Haiku 4.5   ⇢  GPT-5-nano",
"summarizeVerification→  Claude Haiku 4.5",
"generateProductImage →  GPT-Image-1 + Flux Ultra 1.1  ⇢  static placeholder".

On the right, a vertical stack of FIVE provider pills (violet, glowing) plus one dashed-grey pill at the bottom:
"OpenAI", "Anthropic", "Google", "Replicate", and dashed: "Mock fixture provider".
Connect every task pill on the left through the routing matrix to the relevant provider pills on the right with thin violet double-line arrows; bend them to avoid overlap.

Below the routing matrix, render two horizontal sub-bars:
- Amber bar "safety.ts · prompt-as-data · secret redaction · output safeParse".
- Emerald bar "experimental_telemetry → Sentry AI Agents Insights · stable functionId per task".

Bottom emerald "Done when" footer with three ticks:
"☑ sample brief returns ready_for_generation in <30s",
"☑ prompt-injection attempts produce inert data",
"☑ mock provider drives full demo flow with zero network".

Top-right corner: violet badge "Branch: gate-c/ai-gateway-tasks".
Bottom-right corner: amber tag "Tag on pass: v0.3-gate-c-ai-intake-pass".
```

---

## 5. Gate D — Store Generation Pipeline (Vercel Workflow)

Filename suggestion: `05-gate-d-generation-workflow.png`

Caption: *"A durable Vercel Workflow `generation.run` with nine retryable steps that turns a validated brief into a fully-persisted, schema-validated set of store artifacts — credits only deducted on success."*

Prompt:

```
Title at top: "Gate D — Store Generation Pipeline · 6–7 days".
Subtitle in light grey: "Vercel Workflow · generation.run · 9 retryable steps · credits deducted only on success.".

Render a horizontal swimlane diagram with NINE numbered step nodes inside a long rounded violet container labeled at the top "Vercel Workflow · generation.run · durable · idempotent · retry x3 exponential".

Each of the nine step nodes is a rounded teal rectangle with: a circular number badge (1–9), a 2-word title, a one-line subtitle, and a small icon. Connect them left to right with thick teal arrows. Above each retryable step, a small amber "↻ retry x3" tag.

Step 1 "Load Context" — "brief + tenant + ContextEnvelope" (icon: folder).
Step 2 "Reserve Credits" — "reserveCredits() → ledger row" (icon: lock).
Step 3 "createStorePlan" — "AI · GPT-5.4 · Output.object()" (violet inner glow, icon: blueprint).
Step 4 "generateCatalog" — "AI · GPT-5.4 · 12 products" (violet inner glow, icon: grid).
Step 5 "generateCopyBlocks" — "AI · Claude Sonnet 4.6" (violet inner glow, icon: pen).
Step 6 "generateProductImages" — "AI · GPT-Image-1 + Flux Ultra · parallel · mock fallback" (violet inner glow, icon: picture).
Step 7 "Persist Artifacts" — "store_artifacts + Supabase Storage" (icon: cylinder).
Step 8 "summarizeVerification" — "after Gate E pass" (violet inner glow, icon: checklist).
Step 9 "Deduct or Refund" — "deductCredits() / refundCredits()" (icon: scale).

Below the workflow lane, render a thin teal "Artifact Bus" bar showing five rounded artifact pills emerging from step 7 with downward arrows:
"store.config.json", "theme.tokens.json", "catalog.seed.json", "copy.blocks.json", "generation.report.json".
Each pill points down into a slate-blue cylinder labeled "store_artifacts · Supabase Postgres + Storage".

To the right of the workflow, a vertical "Template Registry" panel (teal outlined) listing one strong base template at top and six category preset chips below: "fashion", "beauty", "electronics", "home", "fitness", "gifts". An arrow from this panel points into Step 3.

Bottom emerald "Done when" footer with four ticks:
"☑ brief reaches generation.artifacts_created reliably",
"☑ credits deducted only on success",
"☑ mock-provider end-to-end in <10s",
"☑ every artifact validates against its Zod schema".

Top-right corner: violet badge "Branch: gate-d/workflows-generation-run".
Bottom-right corner: amber tag "Tag on pass: v0.4-gate-d-generation-pass".
```

---

## 6. Gate E — Storefront + Cart + Mock Checkout + Verification

Filename suggestion: `06-gate-e-storefront-verification.png`

Caption: *"The platform-owned storefront renders home / listing / detail / cart / mock checkout / confirmation purely from artifacts, then a verification runner blocks `demo_ready` on schema, RLS, routes, cart, checkout, responsive, a11y, and injection checks."*

Prompt:

```
Title at top: "Gate E — Storefront · Cart · Mock Checkout · Verification · 5–6 days".
Subtitle in light grey: "Pure renderer over artifacts · platform-owned cart · 9 verification checks block demo_ready.".

Split the canvas into TWO halves with a thin vertical divider.

LEFT HALF — "Storefront Render Path" (header in teal):
Top: a slate-blue cylinder "store_artifacts (from Gate D)" with five small artifact chips inside.
Below, an arrow into a teal "packages/storefront · pure renderer" container.
From the renderer, six rounded browser-window page mocks fan out (no real screenshots, just framed wireframes):
1. "Home", 2. "Product Listing", 3. "Product Detail", 4. "Cart", 5. "Mock Checkout", 6. "Confirmation".
Each mock has 3–4 placeholder line bars and a tiny "200 OK" tag in the corner.

To the right of the renderer, a teal vertical bar labeled "packages/cart · platform-owned engine" with four bullet labels:
"variant selection", "qty caps", "subtotal + persistence", "empty state".
And a smaller violet pill below it: "mock_checkout_sessions · no card data".

RIGHT HALF — "Verification Runner" (header in teal):
A large violet rounded "packages/verification" container holding NINE numbered check chips arranged in a 3x3 grid:
1. "Schema presence",
2. "Tenant ownership probe (impersonate other org)",
3. "Credit-ledger invariant",
4. "All required routes 200",
5. "Cart add/remove/qty/subtotal · Playwright",
6. "Mock checkout success + cancel",
7. "Responsive · 360 / 768 / 1280 / 1920",
8. "Playwright a11y axe scan",
9. "Prompt-injection / secret-leak grep on artifacts".

Each chip has a small ☑ slot to the right that lights emerald when passing.

Below the grid, render a decision diamond: "All 9 green?".
A green arrow labeled "Yes" leads to an emerald pill "verification_reports.status = passed → demo_ready eligible".
A red arrow labeled "No" leads to an amber pill "store stays out of demo_ready · refund credits".

Bottom emerald "Done when" footer:
"☑ 100% of verification checks pass",
"☑ failed verification keeps store out of demo_ready".

Top-right corner: violet badge "Branch: gate-e/verification-runner".
Bottom-right corner: amber tag "Tag on pass: v0.5-gate-e-storefront-verify-pass".
```

---

## 7. Gate F — Demo Readiness + Super Admin Polish

Filename suggestion: `07-gate-f-demo-readiness.png`

Caption: *"Final gate: Super Admin store-approval, offline demo fixtures, locked preview env, Sentry release + alerts, and a graduation script that runs in 6–8 minutes."*

Prompt:

```
Title at top: "Gate F — Demo Readiness · Graduation Bar · 3–4 days".
Subtitle in light grey: "A stranger signs up · you approve · they generate a store · everything works on a fresh device.".

Render a circular demo-day journey with SEVEN numbered nodes arranged clockwise around the canvas like a clock face. Each node is a soft white rounded card with an icon and one short label. Connect them with a clockwise emerald arrow loop.

12 o'clock (1): "Stranger opens preview URL" (icon: globe).
2 o'clock (2): "Sign up · Supabase Auth" (icon: key).
4 o'clock (3): "Super Admin approves · grants 10 credits" (icon: shield with check).
5 o'clock (4): "Brand prompt → ready_for_generation in <30s" (icon: chat bubble).
7 o'clock (5): "Vercel Workflow runs · visible progress" (icon: gear).
9 o'clock (6): "Storefront live at unique slug · cart · mock checkout · confirmation" (icon: shopping bag).
11 o'clock (7): "Verification green · admin flips status = demo_ready" (icon: trophy).

In the centre of the clock, render a violet rounded "Graduation Bar" hub card listing five tick items in monospace:
"☑ Super Admin store-approval queue",
"☑ tooling/fixtures · offline demo fallback",
"☑ Vercel preview env locked (dev keys only · no prod payments)",
"☑ Sentry release tagged · AI-error alert armed",
"☑ demo runbook + 6-8 min graduation script".

Outside the clock, in the four corners, render four small badges:
- Top-left amber: "AI_PROVIDER_MODE = mock | live".
- Top-right teal: "Branch: gate-f/super-admin-store-approval".
- Bottom-left teal: "Branch: gate-f/demo-fallback-fixtures".
- Bottom-right emerald: "Tag on pass: v1.0-gate-f-graduation-ready".

Bottom centre footer banner in emerald:
"Done when a stranger can sign up, get approved, generate a store, and complete the full flow on a fresh device using only the preview URL.".
```

---

## 8. Bonus — Gate Roadmap (use as the "table of contents" slide)

Filename suggestion: `08-bonus-gate-roadmap.png`

Caption: *"Seven enforceable gates replace the old flat 28-phase outline; each gate ends with a tag, a docs/logs entry, and a green done-when checklist."*

Prompt:

```
Title at top: "Version One Roadmap · 7 Enforceable Gates".
Subtitle in light grey: "Each gate is binary pass/fail · tagged on merge · logged in docs/logs/.".

Render a horizontal timeline ribbon spanning the full width.

On the ribbon, place SEVEN evenly-spaced milestone diamonds, each with a coloured glow and a number badge above and a tag below:

Diamond 1 (teal) — Top label "Gate 0 · Source Confirmation · 1d", bottom tag "v0.0-gate-0-source-confirmed".
Diamond 2 (teal) — Top label "Gate A · Foundation · 3-4d", bottom tag "v0.1-gate-a-foundation-pass".
Diamond 3 (teal) — Top label "Gate B · Tenancy + Credits + Admin · 4-5d", bottom tag "v0.2-gate-b-tenancy-credits-pass".
Diamond 4 (violet, glowing — AI gate) — Top label "Gate C · AI Gateway + Brand Intake · 5-6d", bottom tag "v0.3-gate-c-ai-intake-pass".
Diamond 5 (violet, glowing — AI gate) — Top label "Gate D · Generation Pipeline · 6-7d", bottom tag "v0.4-gate-d-generation-pass".
Diamond 6 (teal) — Top label "Gate E · Storefront + Verification · 5-6d", bottom tag "v0.5-gate-e-storefront-verify-pass".
Diamond 7 (emerald, larger, with subtle confetti rays) — Top label "Gate F · Demo Readiness · 3-4d", bottom tag "v1.0-gate-f-graduation-ready".

Connect the diamonds with a thick teal arrow that turns emerald between Diamond 6 and Diamond 7.

Above the ribbon, in light grey, an axis showing approximate cumulative day count: "D1 · D5 · D10 · D16 · D23 · D29 · D33".

Below the ribbon, three swimlane bands in muted colours indicating focus area:
- Top band (teal): "Infra & Schema" spanning Gates 0, A, B.
- Middle band (violet): "AI & Orchestration" spanning Gates C, D.
- Bottom band (emerald): "Storefront, Verification, Demo" spanning Gates E, F.

Bottom-right corner badge: "Total: ~33 working days to graduation bar".
```

---

## 9. Bonus — Per-Task AI Routing Matrix (one-slide deep dive)

Filename suggestion: `09-bonus-ai-routing-matrix.png`

Caption: *"Every AI task is bound to a primary model with a documented fallback, plus a mock provider for offline demo safety."*

Prompt:

```
Title at top: "AI Task Routing Matrix · 2026".
Subtitle in light grey: "Per-task primary model · documented fallback · always-available mock provider.".

Render a clean Sankey-style flow diagram going LEFT TO RIGHT in three columns.

LEFT COLUMN — "Tasks" (header in teal):
Seven teal outlined task pills stacked vertically:
"normalizeBrandBrief", "createStorePlan", "generateCatalog", "generateCopyBlocks", "reviewSafety", "summarizeVerification", "generateProductImage".

MIDDLE COLUMN — "Primary Model" (header in violet):
Five violet glowing model pills stacked vertically:
"Claude Sonnet 4.6", "GPT-5.4", "Claude Haiku 4.5", "GPT-Image-1", "Flux Ultra 1.1 · Replicate".

RIGHT COLUMN — "Fallback" (header in amber):
Five amber outlined fallback pills stacked vertically:
"GPT-5.4", "Claude Sonnet 4.6", "Gemini 2.5 Flash", "GPT-5-nano", "Static placeholder".

Draw flowing teal ribbons (not straight lines, slightly curved Sankey style) from each task to its primary model, and dashed amber ribbons from each task to its fallback. Specifically:
- normalizeBrandBrief → Claude Sonnet 4.6  ⇢ GPT-5.4
- createStorePlan → GPT-5.4 ⇢ Claude Sonnet 4.6
- generateCatalog → GPT-5.4 ⇢ Gemini 2.5 Flash
- generateCopyBlocks → Claude Sonnet 4.6 ⇢ GPT-5.4
- reviewSafety → Claude Haiku 4.5 ⇢ GPT-5-nano
- summarizeVerification → Claude Haiku 4.5 (no fallback ribbon)
- generateProductImage → GPT-Image-1 + Flux Ultra 1.1 ⇢ Static placeholder

Below the three columns, a horizontal dashed-grey lane labeled:
"Mock fixture provider · always available · drives offline demo".
Draw thin dotted grey arrows from every task on the left into this lane.

Bottom-right corner: small monospace tag "Refs: docs/decisions/0004-ai-task-routing-matrix.md".
```

---

## 10. Bonus — Monorepo Folder Map (architectural overview)

Filename suggestion: `10-bonus-monorepo-layout.png`

Caption: *"Turborepo + pnpm: one app today (`apps/web`), nine reusable packages, and explicit room for `apps/worker` and `apps/admin-portal` later — without rewrites."*

Prompt:

```
Title at top: "Monorepo Layout · Turborepo + pnpm".
Subtitle in light grey: "One app today, room for more · contracts and DB shared across all packages.".

Render an isometric folder-tree diagram on a deep navy background.

Centre-top: a large rounded teal container labeled "grad-store-builder/" with two sub-sections inside:

Sub-section A "apps/" (left half):
- One filled teal card "web/  ·  Next.js 16 App Router" with four small chips inside: "(builder)", "(admin)", "(storefront)", "api/".
- Two ghosted dashed-outline cards beside it labeled "worker/  · future" and "admin-portal/  · future" rendered at 40% opacity to indicate extension points.

Sub-section B "packages/" (right half):
Render NINE rounded teal package cards in a 3x3 grid, each with a folder glyph:
1. "db/ · Drizzle schema + RLS",
2. "contracts/ · Zod schemas (single source of truth)",
3. "ai-gateway/ · AI SDK v6 task adapters",
4. "generation/ · Workflow orchestrator",
5. "storefront/ · pure renderer",
6. "cart/ · platform cart engine",
7. "verification/ · checks runner",
8. "ui/ · shadcn registry + tokens",
9. "config/ · tsconfig · biome · tailwind".

Below the main container, three small slate-blue cards in a row:
"tooling/playwright", "tooling/fixtures · demo fallback", "docs/".

Top-right corner of the canvas: three monospace config-file chips floating outside the container:
"turbo.json", "pnpm-workspace.yaml", "mcp-profiles.json".

Draw thin teal lines connecting "contracts/" to every other package card (it is the shared dependency).
Draw thin violet lines connecting "ai-gateway/" to "generation/" only.
Draw thin slate lines connecting "db/" to "generation/", "storefront/", "cart/", "verification/".

Bottom-right corner: small emerald tag "First package added: contracts/  ·  Last: verification/".
```

---

## Quick Reference — Order For The Slide Deck

1. `08-bonus-gate-roadmap.png` (table of contents)
2. `00-system-architecture.png` (the big picture)
3. `10-bonus-monorepo-layout.png` (where the code lives)
4. `09-bonus-ai-routing-matrix.png` (the AI strategy)
5. `01-gate-0-source-confirmation.png`
6. `02-gate-a-foundation.png`
7. `03-gate-b-tenancy-credits-admin.png`
8. `04-gate-c-ai-gateway-brand-intake.png`
9. `05-gate-d-generation-workflow.png`
10. `06-gate-e-storefront-verification.png`
11. `07-gate-f-demo-readiness.png`

This order mirrors how the discussion should flow with the supervisor: scope → architecture → folders → AI strategy → then walk through each gate sequentially.
