# Active Stage Research Synthesis

> Supersession note, 2026-05-05: this synthesis was originally written for the older two-stage anonymous MVP. Its failure-pattern lessons still apply, but active scope is now authenticated Version One: Supabase Auth, tenant isolation, admin-granted credits, lazy Super Admin, AI adapter gateway, Brand Prompt Intake, Store Generation, cart/mock checkout, verification, and polished UI/UX. Use `docs/research/version-one-research-synthesis.md` for the current scope interpretation.

## Scope And Source Caution

This synthesis extracts lessons for the two active graduation MVP stages only:

1. Brand Prompt Intake.
2. Store Generation.

It does not expand the MVP into creative generation, campaigns, production dashboards, subscriptions, add-ons, live Shopify, or production commerce operations.

The research sources mix technical review, competitive scan, market analysis, and inferred postmortem commentary. Treat repeated patterns across sources as stronger signals. Treat exact metrics, vendor claims, social sentiment, and single-review judgments as directional rather than definitive.

## 1. Competitor/Tool Summary Table

| Tool/source | Relevant facts from research | Inference for Brand Prompt Intake | Inference for Store Generation |
| --- | --- | --- | --- |
| Stunning.so | Prompt-first app builder; dashboard has prompt textarea, uploads, voice input, templates, project tabs, i18n, and database viewer. Technical review praised prompt quality, WebContainer-style preview, and prompt-injection resistance, but noted infrastructure weakness and UI glitches. | Keep intake low-friction, multilingual-aware, and template-aware. Support simple prompts first, with optional richer inputs. | Use templates and visible generation status, but avoid browser-sandbox dependence for MVP. Add prompt-injection checks and visible verification results. |
| Lovable.dev | Strong rapid MVP generation, React/Supabase stack, but research highlights complexity wall, schema migration problems, RLS/security failures, service-role exposure risk, and technical debt after export. | Intake must produce explicit constraints and assumptions so later stages do not silently invent backend behavior. | Use deterministic schema, human-reviewed migrations, server-only secrets, and modular platform-owned logic. |
| v0.dev | Excellent UI/component generation, but research highlights UI-to-full-stack gap, hallucinated data contracts, monolithic files, overwritten manual logic, state-management issues, shadow deployment/source-control risks, accessibility gaps, and token burn. | Intake should normalize data contracts before UI generation. Avoid treating attractive UI as proof of store readiness. | Store Generation must generate from contracts, not from UI guesses. Keep source-owned artifacts and verification before demo-ready status. |
| Bolt.new | Fast browser-based full-stack prototyping using WebContainers, but research highlights context rot, full-file rewrites, token burn, WebContainer memory limits, schema divergence, and export-to-IDE pressure for real projects. | Intake should keep briefs compact, structured, and reusable rather than relying on long chat history. | Generate small config/seed artifacts, not repeated whole-app rewrites. Avoid WebContainer-heavy architecture for this project. |
| Shopify prompt-to-shop apps | Existing Shopify apps already provide AI search/chat, product cards, add-to-cart, recommendations, and sometimes checkout guidance. Closest competitors include Talkom, Perla, Shoply, Carti, SetuBridge, Edis, FitCart, Sprinix, and Smart Product Finder AI. | Intake should understand commerce-specific intent, audience, category, variants, budget, and constraints, not generic app prompts. | Generated stores should demonstrate ecommerce-native flows: product cards, variants, cart, and checkout handoff. Do not claim uniqueness. |
| Shopify platform direction | Shopify has strong commerce primitives: catalog, variants, inventory, checkout, orders, payments, discounts, shipping, taxes, and agentic commerce direction. | Optional Shopify intent can be captured as a future field, but should not be required. | Keep Shopify as an adapter path later; core MVP should remain Supabase/Stripe-test/mock based. |

## 2. Failure Patterns To Avoid

| Failure pattern | Fact from research | Inference for active stages |
| --- | --- | --- |
| One-shot generation as product strategy | Lovable, v0, and Bolt research all warn that prompt-only generation works for demos but fails when complexity grows. | Treat AI as an orchestrated assistant, not the owner of architecture. |
| Visual completeness mistaken for functional completeness | v0 research describes the UI/full-stack "70% problem." | Store Generation must verify schema, data, cart, checkout, and UI together. |
| Context rot and architecture amnesia | Bolt and v0 research describe context degradation and forgotten patterns over long sessions. | Keep stage contracts short, explicit, versioned, and stored outside chat history. |
| Monolithic/generated file rewrites | v0 and Bolt research describe full-file rewrites, code deletion, and layout breakage. | Generate config, copy, theme tokens, and seed data; keep React/cart/schema code platform-owned. |
| Schema drift and unsafe migrations | Lovable and Bolt research describe conflicting schema changes, non-idempotent migrations, and seed order failures. | Use deterministic schema and idempotent seeds; do not let AI create arbitrary migrations per store. |
| Security as generated afterthought | Lovable research highlights RLS failures and service-role key exposure. | Server-only secrets, no production credentials, simple RLS in dev, no auth schema edits. |
| Token/iteration waste | v0 and Bolt research connect monoliths and broken fix loops to high token burn. | Smaller artifacts and deterministic templates reduce regeneration cost and review surface. |
| Browser sandbox dependency | Bolt/Stunning research shows WebContainers are powerful but expensive/resource-limited. | Do not make WebContainers part of MVP architecture. Use normal Next.js dev/test flow. |
| Weak infrastructure when traffic spikes | Stunning review notes crash risk without CDN/load balancing. | Keep demo low-risk now; later deployment plan should include preview health checks and production hardening. |
| Prompt injection and instruction leakage | Stunning review explicitly tested prompt injection. | Treat user prompt as data; validate contracts and never let prompt text modify platform rules. |

## 3. Architecture Lessons

- Fact: The strongest repeated signal is that unbounded AI generation creates fragile systems once UI, state, database, and checkout behavior interact.
- Inference: The project should be presented as an orchestration platform with stage contracts: Brand Prompt Intake output becomes Store Generation input.
- Fact: Tools that generate visually impressive screens can still fail when backend contracts are hallucinated or missing.
- Inference: Store Generation should start from product/catalog data and page plan before UI rendering.
- Fact: Research repeatedly criticizes monoliths, context rot, and full-file rewrites.
- Inference: Platform code should be stable; generated output should mostly be JSON-like artifacts: brief, store plan, theme tokens, copy blocks, catalog seed, generation report.
- Fact: Browser-based WebContainers enable fast previews but can hit memory/cost limits.
- Inference: For a graduation MVP, normal local/dev Next.js execution is safer than building a browser code-running platform.
- Fact: Source-control opacity and shadow deploy flows create recovery risk.
- Inference: Generated artifacts must be stored in project-controlled files or database records and later deployed through normal preview flow.

## 4. UX Lessons

- Fact: Stunning's dashboard uses a large prompt box, upload affordances, voice input, templates, project organization, and language switching.
- Inference: Brand Prompt Intake should start with a simple prompt textarea, then expose optional advanced fields without making the first step heavy.
- Fact: Shopify competitive apps emphasize visual product cards, product grids, filters, add-to-cart, and one-click checkout paths rather than plain chat answers.
- Inference: Store Generation should demonstrate product-card and cart-first ecommerce UX, not a generic chatbot interface.
- Fact: Some tools include dashboards and marketplaces, but the current project scope excludes full dashboards and add-ons.
- Inference: MVP may show minimal generation status/reporting, but must not become a merchant dashboard.
- Fact: v0 research says AI-generated UI often misses accessibility and responsive polish.
- Inference: Generated store acceptance must include mobile, text-overflow, keyboard, and basic accessibility checks.
- Fact: Stunning supports Arabic/multilingual UX.
- Inference: Intake should preserve language and region assumptions; Store Generation should be able to produce at least a consistent primary language and avoid breaking RTL later.

## 5. Database/Cart/Checkout Lessons

- Fact: Lovable research highlights migration idempotency failures, seed sequencing failures, RLS misconfiguration, and protected auth schema corruption.
- Inference: The MVP needs a fixed commerce schema and deterministic seed order: store, generation run, products, variants, carts, cart items, checkout sessions/orders.
- Fact: Shopify's strongest advantage is mature commerce infrastructure and trusted checkout.
- Inference: Our custom MVP must be modest: stable product/catalog model, stable cart, and safe checkout handoff only.
- Fact: Shopify competitors often support variant-aware product discovery and add-to-cart.
- Inference: Product variants must be first-class in the Store Generation contract, even if the demo catalog is small.
- Fact: v0 and Bolt can generate UI that assumes wrong backend shapes.
- Inference: Storefront components should consume the generated data model, not invent prop shapes independently.
- Fact: Payment and credentials are high-risk areas.
- Inference: Stripe test mode should be optional and server-only; mock checkout should be the default fallback when credentials are absent.
- Fact: RLS scanners can miss semantic authorization errors.
- Inference: V1 should avoid production auth and multi-tenant authorization complexity; use dev-only synthetic data and simple policies.

## 6. Prompting And Orchestration Lessons

- Fact: Stunning's review suggests hidden system instructions and strong prompt engineering can improve output quality.
- Inference: Our prompts should add structured constraints behind the scenes, but the resulting contracts should be visible and inspectable.
- Fact: Lovable/v0/Bolt research shows "try to fix" loops and long chat histories can poison context.
- Inference: Each stage should be resumable from stored artifacts, not dependent on a long conversation.
- Fact: v0 research warns that the model may claim work was done when it was not.
- Inference: Every generated plan/artifact needs verification, not just AI self-reporting.
- Fact: Bolt research recommends reducing irrelevant context to protect coherence.
- Inference: Intake should pass only the normalized brief to Store Generation, not the entire raw conversation.
- Fact: Shopify prompt-to-shop competitors are ecommerce-specific, not general website builders.
- Inference: Prompts should encode commerce primitives: product type, variants, price, inventory, buyer intent, cart, checkout, trust elements.

## 7. Shopify-Specific Positioning Lessons

- Fact: The Shopify scan says prompt-to-shop and AI shopping assistant ideas already exist.
- Inference: Do not claim uniqueness or empty market.
- Fact: Existing apps compete around AI search, visual product cards, add-to-cart, recommendations, support, and analytics.
- Inference: For the graduation project, position V1 as prompt-to-store generation from brand briefs, not as a Shopify chatbot.
- Fact: Shopify's mature commerce stack is a strength for future paths.
- Inference: Capture Shopify as an optional future adapter: Storefront API, UCP, Catalog, Cart MCP, Checkout MCP, and checkout referral only after credentials and scope are intentional.
- Fact: Merchants complain about too many half-integrated tools.
- Inference: Future Shopify work should focus on clean integration and measurable commerce outcomes, not another floating chat widget.
- Fact: Category-specific shopping flows are strategically useful: gift finder, bundle builder, routine builder, comparison, cart builder.
- Inference: Store Generation templates should support category presets now; prompt-to-shop behavior can remain roadmap.

## 8. Requirements That Should Be Added To Specs

### Brand Prompt Intake

- Add explicit source-quality fields: `facts`, `assumptions`, `unknowns`, and `confidence`.
- Require product category, audience, region/language, variants, price positioning, checkout mode, and hard constraints before `readyForStoreGeneration`.
- Add prompt-injection handling: user text must be treated as brand/content data, not instruction text.
- Add language/region normalization, including future RTL-readiness.
- Add category-specific follow-up prompts for fashion, beauty, electronics, home, fitness, and gifts.
- Add a "scope overflow" classifier that moves campaign/dashboard/billing/creative requests into future extension notes.

### Store Generation

- Require a `store_generation_plan` before any storefront artifact is considered generated.
- Require fixed platform-owned cart, checkout, schema, and template modules.
- Require generated artifacts to be stored separately from platform code.
- Require deterministic schema/seed order and product/variant stable IDs.
- Require cart verification: add, remove, quantity update, subtotal, empty state, variant selection, persistence, and unavailable item behavior.
- Require checkout fallback: mock checkout when Stripe test credentials are absent.
- Require verification report with route, data, schema, cart, checkout, mobile, accessibility, and prompt-injection checks.
- Require no production credentials, no real customer data, no production Shopify by default.

## 9. Risks That Should Be Added To Plans

| Risk | Active stage affected | Plan mitigation |
| --- | --- | --- |
| Intake accepts vague input and Store Generation guesses too much | Brand Prompt Intake, Store Generation | Blocking follow-ups for product/audience; assumptions recorded with confidence. |
| Prompt injection alters orchestration rules | Brand Prompt Intake | Treat raw prompt as data and validate normalized contract before handoff. |
| Store looks complete but cart/checkout do not work | Store Generation | Browser test full buyer path before demo-ready status. |
| AI-generated schema diverges from UI assumptions | Store Generation | Data model first, deterministic schema, generated UI consumes approved data shape. |
| Generated code becomes unmaintainable | Store Generation | Generate artifacts/config first; platform-owned code remains stable. |
| Supabase security mistakes | Store Generation | Development DB only, no service-role frontend exposure, no auth schema edits, simple RLS review. |
| Stripe/Shopify credentials block demo | Store Generation | Mock checkout default; Shopify future-only unless explicitly provided. |
| Scope creep into dashboards/campaigns/billing | Both | Extension fields only; do not build future systems in V1. |
| Overclaiming market uniqueness | Proposal, positioning | Position as graduation implementation of orchestration, not unique Shopify category. |
| Demo instability from external services | Store Generation | Synthetic seed data, local/preview fallback, no live dependencies required. |

## 10. Suggested Future Roadmap Items

These are future-ready only and should not enter the active MVP until Brand Prompt Intake and Store Generation are complete.

- Creative Generation adapter that consumes `visualDirection`, product metadata, and media prompts.
- Human review workflow for approving normalized brief, generated store plan, catalog seed, and preview.
- Shopify Storefront adapter for real catalog sync, product availability, cart creation, and checkout referral.
- Agentic commerce exploration for UCP, Catalog, Cart MCP, and Checkout MCP after official-doc review and real store credentials.
- Generation dashboard for run history, assumptions, warnings, verification status, and artifact previews.
- Category template packs for fashion, beauty, electronics, home, fitness, and gifts.
- Prompt-to-shop layer for product discovery and cart-building inside generated stores.
- Observability and demo deployment hardening: health checks, preview diagnostics, and later CDN/cache plan.
- Billing/credits only after generation cost and usage patterns are measurable.
- Marketplace/add-ons only after templates and extension contracts are stable.

## Planning Takeaway

The actionable project strategy is: keep the user-facing experience simple, but make the internal pipeline strict. Brand Prompt Intake should normalize messy business language into a compact contract. Store Generation should transform that contract through deterministic templates, schema, cart, checkout, and verification. The MVP wins by proving orchestration discipline, not by imitating broad app builders or Shopify production apps.
