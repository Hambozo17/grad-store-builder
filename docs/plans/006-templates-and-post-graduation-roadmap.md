# Templates UX + Post-Graduation Investment-Grade Roadmap

> Discussion document for the supervisor meeting. Two real questions, answered directly, with everything needed to talk through the project's path from a graduation demo to an investable online product.

---

## 0. Executive Summary (One-Page TL;DR For The Supervisor)

**Question 1: "Are we ready for the model to show templates the user can select alongside the prompt?"**

**Honest answer.** Architecturally yes, product-wise not yet. The Version One plan has a **template registry** baked into Gate D — one strong base template plus six category presets (fashion, beauty, electronics, home, fitness, gifts). What it does **not** have today is a **user-facing template gallery UX** where the user browses, previews, and picks a template before or alongside writing the brand prompt. Section 1 below specs that gallery as a clean addition that fits inside Gate C and Gate D without breaking the existing plan. Effort estimate: **3–4 extra days**, no architectural rewrites.

**Question 2: "We are seeking investment, the website will go live online, give the whole professional-grade recommendations."**

**Honest answer.** The graduation bar is a credible demo, not an investable product. To go from demo-grade to investor-grade we need a **Version Two** that adds: real payments, custom domains, multi-store per organization, theme customization, real product image upload, store-owner analytics, SEO, marketing site, pricing tiers, compliance (GDPR, ToS, privacy), trust pages (security, status), and investor-facing collateral (one-pager, deck, demo video, metrics dashboard). Sections 2–10 lay this out by category with cost estimates, refused items, and a phased post-graduation timeline.

**One-sentence pitch we should be able to defend to an investor.**

> *"We turn a brand prompt into a verifiable, multi-tenant, production-grade e-commerce store in under three minutes — every store rendered by the platform, every AI call audited, every credit traceable. Today we generate the store. Tomorrow we host the business."*

---

## PART 1 — TEMPLATES UX: GALLERY + SELECTION + AI MATCHING

### 1.1 Where Templates Live Today (V1 Plan)

The current 7-gate plan treats templates as a **backend resource** consumed by Gate D step 3 (`createStorePlan`):

> *"Template registry seeded with one strong base template + 6 category presets (fashion, beauty, electronics, home, fitness, gifts)."*
> — `.cursor/plans/version-one-advanced-plan_5edfc7dd.plan.md` §5 Gate D

There is **no UX** in V1 for the user to browse, preview, or pick a template. The AI silently picks one based on the brand prompt's inferred category. The Brand Intake spec (`docs/specs/brand-prompt-intake-spec.md`) only has a free-text `visualStyle` field, no gallery.

### 1.2 The Gap Three Real Users Will Hit

1. **"I have a vibe but no words for it."** A user knows they want their store to look like a clean Apple-style minimalist site, but can't describe that in a paragraph. A gallery solves this in one click.
2. **"I want to see what I'm getting before I commit credits."** Users will not spend a credit on a black-box generation. Templates make the output range concrete.
3. **"I picked the wrong category."** The AI may infer "fashion" from a prompt about handmade jewelry but the user wanted the "beauty" preset look. A gallery lets them override.

### 1.3 Proposed Three Modes (Hybrid Is The Right Answer)

| Mode | Who picks the template | When in the flow | Default? |
| --- | --- | --- | --- |
| **A. Auto** | AI infers from prompt (current V1) | Hidden from user | Yes, fastest path |
| **B. User-picked** | User browses gallery, picks one | Before writing the prompt | Optional, advanced users |
| **C. AI-suggested-then-confirmed** | AI suggests 2–3 templates after intake; user confirms one | Mid-flow, after the brief is normalized | **Recommended default for V1.5** |

**Why C wins.** It keeps the prompt-first ethos (low friction) and adds a confidence-building moment (user sees three real previews). It also makes the AI's choice **visible and auditable**, which is the whole point of the project.

### 1.4 Proposed UX (Frame By Frame)

**Step 1.** Approved user lands in `/intake`. Two top-level options visible:

```
[ Just describe your brand ]   [ Browse template gallery ]
       (Mode A — fastest)             (Mode B — optional)
```

**Step 2 (Mode A or C).** User writes the prompt, follow-up questions appear, brief reaches `ready_for_generation`.

**Step 3 (Mode C, new).** Before "Start generation", show:

```
┌──────────────────────────────────────────────────────────────┐
│  Based on your brand, three templates fit best:              │
│                                                              │
│  [Preview 1]   [Preview 2]   [Preview 3]                     │
│   "Minimal"    "Editorial"   "Showcase"                      │
│   ●●●●●        ●●●●○         ●●●○○                           │
│   match score  match score   match score                     │
│                                                              │
│  [ Use Minimal ]  [ Use Editorial ]  [ Use Showcase ]        │
│  [ Show me more ]  [ Surprise me — let AI decide ]           │
└──────────────────────────────────────────────────────────────┘
```

**Step 4.** User clicks one. Choice is recorded in `BrandIntakeBrief.templatePreference`. Gate D's `createStorePlan` now respects the choice.

**Step 5 (Mode B, gallery first).** User opens `/templates`, filters by category, vibe, color, tags. Each card shows a real screenshot, a 2-second hover preview animation, and a one-line description. Picking a template pre-fills the intake form with template metadata (suggested tone, suggested catalog size, suggested color palette as starting point).

### 1.5 Schema Changes (Non-Breaking)

**New tables in `packages/db/schema/templates.ts`:**

```ts
templates (
  id uuid pk,
  template_key text unique,            // "minimal-v1", "editorial-v1", "showcase-v1"
  name text,
  category text[],                     // ["fashion", "beauty"]
  vibe_tags text[],                    // ["minimal", "luxe", "warm", "playful"]
  color_palette jsonb,                 // { primary, secondary, accent, neutrals[] }
  font_pairing jsonb,                  // { heading, body }
  layout_kind text,                    // "grid", "editorial", "single-product", "lookbook"
  cover_image_url text,
  preview_url text,                    // /templates/[key]/preview with stub data
  status text,                         // "active", "beta", "deprecated"
  created_at timestamptz
)

template_inspirations (                // user-uploaded or user-picked references
  id uuid pk,
  brief_id uuid fk → brand_intake_briefs,
  source_kind text,                    // "template_pick", "external_url", "uploaded_image"
  template_key text nullable,
  external_url text nullable,
  image_url text nullable,
  weight numeric default 1.0,          // for AI to weigh
  created_at timestamptz
)
```

RLS rule on `template_inspirations`: same tenancy filter as the parent `brand_intake_briefs`.

**Extension to `BrandIntakeBrief` Zod schema** in `packages/contracts/intake.ts`:

```ts
templatePreference: z.object({
  mode: z.enum(["auto", "user_picked", "ai_suggested_confirmed"]).default("auto"),
  templateKey: z.string().nullable(),
  presetKey: z.string().nullable(),
  matchScore: z.number().min(0).max(1).nullable(),
  inspirationIds: z.array(z.string().uuid()).default([]),
}).optional()
```

### 1.6 AI Gateway Changes

Add one new task to the routing matrix:

| Task | Primary | Fallback |
| --- | --- | --- |
| **`suggestTemplates`** (new) | Claude Haiku 4.5 (cheap, fast, ranks well on JSON) | Gemini 2.5 Flash |

The task takes the normalized brief plus the `templates` table (cached as a small JSON list, ~12 KB) and returns 3 templates with match scores 0–1 and a one-sentence reason each.

`createStorePlan` is extended to accept an optional `templatePreference.templateKey`. When present, the planner is instructed to honor the template's `layout_kind`, `color_palette` starting point, `font_pairing`, and section structure — not invent a new layout.

### 1.7 Verification Additions (Gate E)

Add **two new checks** to the verification runner's 9 checks (becomes 11):

10. **Template fidelity**: rendered storefront uses the chosen `template_key`'s layout and font_pairing. Probed via DOM snapshot comparison against template fixture.
11. **Template metadata persisted**: `store_artifacts.body.templateKey` matches the brief's `templatePreference.templateKey`.

### 1.8 Effort & Risk

| Item | Effort | Risk | Mitigation |
| --- | --- | --- | --- |
| Templates table + 6 seeded entries with cover screenshots | 1 day | Low | Use the existing 6 category presets |
| Gallery UI + filters + previews | 1 day | Low | shadcn cards, no custom design |
| AI `suggestTemplates` task + matching logic | 0.5 day | Low | Haiku is cheap and fast |
| `BrandIntakeBrief` extension + planner respect | 0.5 day | Low | Backwards-compatible (optional field) |
| 2 new verification checks | 0.5 day | Low | DOM snapshot, no flaky pixel diff |
| Cover screenshots (manual or generated) | 0.5 day | Medium | Use Playwright to capture from `preview_url` |

**Total: 3–4 days.** Slot it as **Gate C+** (between Gate C and Gate D) without disrupting existing branches.

### 1.9 What This Does NOT Do (Discipline Statement)

This feature does **not** open the door to:

- Drag-and-drop visual editing (still parked, V3+)
- Per-tenant CSS overrides (still parked)
- Per-tenant React component injection (refused, see V1 plan §13.2)
- WebContainer-style live code preview (refused)

The user picks a template; the **platform** still owns the rendering, the layout, and the cart engine. This is the same boundary as V1.

---

## PART 2 — INVESTMENT-GRADE ROADMAP (V2 AND BEYOND)

> Everything below is **post-graduation**. None of it lands in V1. It is the answer to "what does this project look like as a real online business worth investor money?"

### 2.1 The Honest Gap Between V1 And A Funded Startup

| Dimension | V1 (Graduation Bar) | V2 (Soft Launch) | V3 (Funded) |
| --- | --- | --- | --- |
| **Auth** | Supabase Auth, manual approval | Supabase Auth, automated approval, Google/Apple SSO | Enterprise SSO (SAML), 2FA, magic links |
| **Tenancy** | One org per user | Multiple orgs, invite teammates | Roles, audit log surface, SCIM |
| **Stores per org** | 1 | 3 (Free) / 10 (Pro) / Unlimited (Business) | Same |
| **Payments** | Mock checkout, admin credits | Real Stripe checkout for store owners + Stripe subscriptions for our SaaS | Stripe Connect + tax + invoicing |
| **Domains** | Vercel preview slug | Custom subdomain (`store.ourapp.com`) | Custom apex domain with SSL via Vercel |
| **Templates** | 1 base + 6 presets, AI-picked | Gallery of 20+ templates, user-picked, hybrid mode | 60+ templates, partner-contributed, theme marketplace |
| **Customization** | None beyond AI | Color picker, font picker, logo upload, hero image upload | Section reordering (controlled), saved theme variants |
| **Catalog** | AI-generated 12 products | AI-generated + user upload, CSV import, image upload per product | Bulk operations, inventory sync, supplier integration |
| **Checkout** | Mock | Real Stripe checkout, real order persistence, refunds | Subscriptions for store owners, post-purchase upsells |
| **Analytics** | Sentry only | Plausible/PostHog for store visits, conversions, funnel | Cohort analysis, attribution, custom reports |
| **SEO** | Basic meta from artifacts | Per-store SEO editor, sitemap, OG tags, structured data | Schema.org product markup, performance score badge |
| **Email** | Resend transactional only | Welcome emails, abandoned cart, order confirmation | Marketing campaigns (curated only), automation rules |
| **Mobile** | Responsive web | Responsive web + PWA install prompt | Native wrapper if data justifies it |
| **Languages** | English (default) | English + Arabic + a third based on demand | Full i18n framework, RTL templates |
| **Compliance** | None enforced | GDPR cookie banner, privacy/ToS pages, data export/delete | DPA available, EU data residency option |
| **Trust** | Sentry observability | Public status page, security page | SOC2 readiness (Drata/Vanta), pen test, bug bounty |

This is the table the supervisor will want to walk through line by line.

### 2.2 Product Features To Add (V2)

#### 2.2.1 Required For Real Customers

1. **Real payments for store owners.** Stripe Checkout (start) → Stripe Connect (when stores want to take real money themselves). The platform takes a transaction fee on Connect.
2. **Custom domains.** Vercel's domain API for subdomain (`<slug>.brand.app`). Apex (`brandowner.com`) requires DNS verification flow. Auto-SSL via Vercel.
3. **User uploads.**
   - Brand assets: logo (SVG/PNG), favicon, hero image, "About" image.
   - Product images: 1–10 per product, drag-drop, auto-WebP/AVIF conversion via Cloudflare Images or Vercel's image optimizer.
   - Mood-board uploads in intake (5 images max) that feed the AI as visual references.
4. **Theme customization beyond AI.**
   - Color picker (constrained to a 12-token palette, not free RGB).
   - Font pair picker from a curated list of 12 pairings.
   - Layout swap within a template (grid ↔ list, hero kind, footer kind).
   - **Refuse** free-form CSS until at least V3.
5. **Multi-store per organization.** Free=1, Pro=3, Business=unlimited. Each store has its own `store_id`, slug, and artifacts; org-level credit pool.
6. **Catalog editing.** AI generates the seed; user edits titles, descriptions, prices, variants, images. CSV import for power users.
7. **SEO editor.** Per-page meta title, meta description, OG image, slug. Auto-generated sitemap and robots.txt.
8. **Order operations.** Order list, order detail, mark fulfilled, refund (Stripe-side), customer notification email.
9. **Store-owner analytics.** Visits, unique visitors, top products, conversion rate, revenue. Use **PostHog** or **Plausible** (privacy-friendly, GDPR-clean).
10. **Onboarding flow.** Templates as the very first screen. Empty state "Create your first store" CTA. Sample data seeding option.

#### 2.2.2 Nice For Polish (Same V2)

1. **Saved drafts and versioning.** Every generation produces a versioned snapshot (already in V1's artifact pattern; expose it in UI).
2. **Preview mode toggle.** "View as customer" vs "View as owner".
3. **Soft delete + restore.** 30-day trash bin for deleted stores.
4. **Export.** Download a store's artifacts as JSON. (No code export; that's a V3 decision.)

#### 2.2.3 Refused Until V3 Or Later

- **Drag-and-drop visual builder.** Different product, different team.
- **Real Shopify two-way sync.** Optional commerce path; only when a paying customer asks for it.
- **Per-tenant custom backend code.** Already parked in V1 plan §15.1.
- **AI ad-creative generation + paid-media campaign launch.** Different product entirely.
- **Marketplace / vendor payouts.** Multi-sided is a different business; refuse for now.

### 2.3 Business / Go-To-Market

#### 2.3.1 Pricing (Recommended Starter)

| Plan | Monthly | Annual | Stores | Credits/mo | Custom domain | Analytics retention | Support |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **Free** | $0 | $0 | 1 | 5 | No (subdomain only) | 7 days | Docs only |
| **Pro** | **$29** | **$290** | 3 | 50 | Yes | 90 days | Email |
| **Business** | **$99** | **$990** | Unlimited | 200 | Yes + multi-domain | 365 days | Email + chat |
| **Enterprise** | Talk to us | — | Unlimited | Custom | Custom | Custom | Dedicated CSM |

Notes:

- **Annual discount = 2 months free** (industry standard).
- **Credits + subscription hybrid.** Subscription buys monthly credits; extra credits are a one-time top-up (e.g., $10 = 100 credits).
- **Free → Pro conversion trigger:** "Connect a custom domain" or "publish more than one store."
- Avoid "freemium with crippled features" feel; cap on **quantity** (stores, credits), not **quality**.

#### 2.3.2 Acquisition (Bootstrapped, Not VC-Funded)

| Channel | Why | Who owns it | Cost |
| --- | --- | --- | --- |
| **SEO** (long-tail e.g., "AI store builder for handmade jewelry") | Compounding, founder-led | Founder + a writer | $0–$500/mo |
| **Product Hunt** launch | High-quality early users, press | Founder | $0 |
| **Indie Hackers** + **r/sideproject** + **r/Entrepreneur** | Authentic feedback | Founder | $0 |
| **YouTube tutorials** ("Build a store in 3 minutes with AI") | Long shelf life, demoable | Founder | Time only |
| **Twitter/X build-in-public** | Distribution + investor radar | Founder | $0 |
| **Templates as content** | Each template is a landing page that ranks | Designer + SEO | $200/template |
| **Affiliate program** | 30% recurring for 12 months | Stripe + Rewardful | $9/mo + payouts |
| **Newsletter** (own list) | Direct line to engaged users | Founder | $0–$30/mo (Buttondown) |

Skip until $1k MRR: paid ads, conferences, sponsored content. They burn capital before product-market fit is verified.

#### 2.3.3 Launch Sequence

1. **Week 0–4 post-graduation.** Polish, add custom domain support, add Stripe subscription, add template gallery (Part 1).
2. **Week 4–6.** Build a marketing site, write 5 SEO landing pages targeting category keywords, set up analytics.
3. **Week 6–8.** Closed beta with 20 hand-picked users (offer 6 months free in exchange for testimonials and bug reports).
4. **Week 8–12.** Public launch on Product Hunt + Indie Hackers + Twitter. Goal: 100 signups, 10 paying customers in month one.
5. **Month 4–6.** Optimize for retention (cohort retention is the metric that matters for fundraising).

### 2.4 Tech / Scale Upgrades (V2)

| Item | Why | Service / Library |
| --- | --- | --- |
| **CDN + image optimization** | Generated stores need fast images globally | Cloudflare Images or Vercel Image Optimization |
| **Object storage** | User uploads, exports | Supabase Storage (V1 OK), migrate to Cloudflare R2 at scale (no egress) |
| **Multi-region read replicas** | When customers complain about latency | Supabase read replicas (Pro+) |
| **Background queue** | Long-running tasks beyond Workflows (cron, cleanup) | Vercel Queues + Workflows together |
| **Rate limiting** | Per-IP, per-user, per-org on AI routes | Upstash Redis (already in V1) |
| **Cron jobs** | Trash cleanup, analytics rollups, free-tier credit reset | Vercel Cron |
| **Feature flags** | Roll out V2 features safely | **Cloudflare Flagship** or **PostHog feature flags** |
| **A/B testing** | Pricing experiments | PostHog A/B |
| **Snapshot / restore per store** | "Undo my last generation" | Already implicit in `store_artifacts` versioning; expose in UI |
| **Audit log surface** | Already in `admin_actions`; expose to org admins | UI work |
| **Backups** | Supabase auto-backs up; verify restore drill quarterly | Supabase Pro backups |
| **Disaster recovery runbook** | Investor due-diligence asks for this | Document in `docs/runbooks/` |

### 2.5 Compliance & Trust

These are **table stakes** for taking real customer money in 2026.

| Item | Required | When | How |
| --- | --- | --- | --- |
| **Privacy policy** | Yes (legal) | Before public launch | Use a generator like **Termly** or **Iubenda** (~$10/mo); have a lawyer review once |
| **Terms of service** | Yes (legal) | Before public launch | Same source, lawyer review |
| **Cookie consent banner** | Yes (GDPR) | Before EU traffic | **CookieScript** or **Osano** free tier |
| **GDPR data export + delete** | Yes (legal in EU/UK) | Before EU launch | Build it once: download org data as JSON, hard delete on request |
| **DPA (Data Processing Agreement)** | If selling to EU businesses | When first EU business asks | Template from Stripe Atlas or **DocuSign Agreement Cloud** |
| **Status page** | Trust signal | Soft launch | **BetterStack** or **Statuspage** ($29+/mo) |
| **Security page** | Trust signal | Soft launch | One-page summary of how we secure data; link from footer |
| **SOC2 Type 1** | Required for enterprise customers | When first $20k+ deal asks | **Drata** or **Vanta** (~$8k–$15k first year) |
| **Pen test** | Recommended pre-public-launch | Before Product Hunt | One-time, ~$3k–$5k via **Cobalt** or **HackerOne** |
| **Bug bounty** | Optional | After 100 paying customers | **HackerOne** managed |
| **Insurance** | Cyber liability + E&O | When revenue justifies | **Vouch** or **Embroker** |

### 2.6 Investor-Facing Collateral

This is what will be asked for in any serious investor conversation.

| Asset | Format | Why | Who builds it |
| --- | --- | --- | --- |
| **One-pager** | PDF, A4, design | Sent ahead of meetings | Founder + designer |
| **Pitch deck (10–12 slides)** | PDF/Pitch.com | The meeting itself | Founder |
| **Demo video (90 seconds)** | MP4, hosted on Loom + landing page | Async due diligence | Founder + screen recorder |
| **Live demo URL** | Real product | The call | Already built (V1 + V2) |
| **Public roadmap page** | Notion/GitHub Project | Trust signal | Founder |
| **Public changelog** | RSS-able | Velocity signal | Founder |
| **Founder bio + LinkedIn** | Web | Trust + diligence | Founder |
| **Testimonials** | Web | Social proof, post-launch | Founder collects |
| **Press kit** | Folder with logos, screenshots, founder photo | Journalists, partners | Founder + designer |
| **Metrics dashboard** | Internal Notion/Mode/Hex | Updated weekly | Founder |
| **Data room** | Drive folder, gated by NDA when needed | Late-stage diligence | Founder |

#### 2.6.1 The 12-Slide Deck (Recommended Order)

1. **Cover.** Logo + one-line pitch.
2. **The problem.** "Setting up a real store takes 40+ hours of decisions; AI builders today produce broken demos that collapse at scale (cite Bolt, Lovable, Stunning research already in `docs/research/`)."
3. **The insight.** "E-commerce stores share 95% of their structure. Generate the data; let the platform render the store. No per-tenant code = no failure modes."
4. **The product.** Three screenshots: intake → generation → live store.
5. **Demo.** 30-second video embed + live URL.
6. **Why now.** AI SDK v6, Vercel Workflows GA, Drizzle + RLS maturity — the stack that makes this feasible only crystallized in 2026.
7. **Market.** Shopify TAM ($600B+ GMV), no-code site builders ($14B+), AI tooling overlap. Cite sources.
8. **Business model.** Free / Pro / Business / Enterprise; credits + subscription hybrid; transaction fee on Connect.
9. **Traction.** Signups, paying customers, MRR, retention cohorts — even small numbers are credible if growing.
10. **Roadmap.** V1 → V2 → V3 from this document.
11. **Team.** Founder, advisors, supervisor (academic credibility helps in some markets).
12. **Ask.** "Raising $X to reach $Y MRR in Z months" or "Currently bootstrapped, open to angel conversations."

### 2.7 Metrics That Matter For Investors

Track these from day one, even at small numbers. Investors care more about **trends** than **absolute values**.

| Metric | Why | Target by Month 6 |
| --- | --- | --- |
| **Signups / week** | Top of funnel | 20+/week |
| **Activation rate** | % of signups who generate ≥1 store | 50%+ |
| **Free → Paid conversion** | % of activated users who subscribe within 30 days | 5%+ |
| **MRR** | Predictable revenue | $1k+ |
| **Net Revenue Retention** | Are existing customers expanding | 100%+ |
| **Logo Churn** | % of customers cancelling | < 5%/mo |
| **CAC** | Cost to acquire one paying customer | < $50 (organic) |
| **LTV** | Lifetime value | 3x CAC minimum |
| **NPS** | Customer love | 30+ |
| **Time to first store** | Onboarding friction | < 5 min |
| **Generation success rate** | Product reliability | 95%+ |
| **Per-run AI cost** | Margin discipline | < $1.20 |

### 2.8 Cost Curve (Honest)

| Stage | Users | Stores | AI cost/mo | Infra cost/mo | Total/mo |
| --- | --- | --- | --- | --- | --- |
| **Idle (post-grad)** | 0 | 0 | $0 | $0 (all free tiers) | **$0** |
| **Soft launch** | 50 | 30 | $30 | $0 (still free tiers) | **$30** |
| **Early traction** | 500 | 300 | $300 | $45 (Supabase Pro + Vercel Pro) | **$345** |
| **Growing** | 5,000 | 3,000 | $3,000 | $200 (+R2 + Postgres replicas) | **$3,200** |
| **Scaling** | 50,000 | 30,000 | $30,000 | $2,000 (multi-region, dedicated DB) | **$32,000** |

Notes:

- AI cost is the biggest variable. **Mock provider for non-paying users** keeps free-tier cost near zero.
- **Per-run cost target: < $1.20**. At Pro pricing of $29/mo with 50 included credits (≈ 50 runs), that is $60 of provider cost vs $29 of revenue — meaning we **must** keep heavy free-tier usage on the mock provider and only charge real AI on paid plans, OR raise prices, OR negotiate volume discounts (OpenAI/Anthropic offer them at $5k+/mo spend).

### 2.9 Domain, Brand, And Marketing Site

#### 2.9.1 Pick A Real Name

V1 working name (`grad-store-builder`) is a placeholder. Before public launch we need:

- A **brandable .com** (preferred) or .ai/.app/.so as fallback.
- 4–8 letters, easy to spell aloud, not a generic noun.
- Trademark check via **Namecheckr** + **USPTO TESS** (US) and the relevant national registry.
- LinkedIn, X, Instagram, GitHub handles available.

#### 2.9.2 Marketing Site Structure (Separate From The App)

Hosted at `www.brand.app`; the app at `app.brand.app`.

```
/                          Landing (hero + 3 features + 3 templates + pricing + testimonials)
/templates                 Public template gallery (each template is its own SEO page)
/pricing                   Pricing table + FAQ
/showcase                  Real customer stores (with permission)
/blog                      SEO content
/docs                      Product documentation (Mintlify or Fumadocs)
/changelog                 Public release log
/security                  Trust page
/privacy                   Privacy policy
/terms                     ToS
/about                     Team + mission + supervisor credit
/contact                   Form
```

#### 2.9.3 Brand System For The Platform Itself

Different from the templates we generate for customers. The platform brand needs:

- Logo (wordmark + mark)
- Brand colors (3–5)
- Type system (heading + body)
- Voice & tone guide
- A 24-page brand book, kept in `docs/brand/`

Hire a designer for one week (~$1.5k–$3k) — do not generate this with AI.

### 2.10 Phased Post-Graduation Timeline

This is the timeline to walk through with the supervisor when they ask "what's next?"

| Phase | Duration | Milestone | Cost |
| --- | --- | --- | --- |
| **V1 — Graduation** | Now → Day 33 | Pass Gate F (current plan) | $0 + ~$10 demo day |
| **V1.5 — Templates UX** | Day 34–37 | Part 1 of this doc shipped | $0 |
| **V2.0 — Soft launch** | Day 38 → Month 4 | Custom domains, Stripe subs, marketing site, 20 closed-beta users | ~$45/mo + ~$3k one-time (legal, design, pen test) |
| **V2.1 — Public launch** | Month 4–6 | Product Hunt + 100 signups + 10 paying | ~$345/mo |
| **V2.5 — Retention focus** | Month 6–9 | Cohort retention > 60% at month 3, NPS > 30 | ~$345/mo |
| **V3.0 — Investment-ready** | Month 9–12 | $1k–$3k MRR, deck + metrics + data room | ~$345–$3k/mo |

If the project never raises external capital, V2.0 is still a real, profitable side-business. Investment is **optional**, not required.

### 2.11 Discipline Statement: What We REFUSE Even For Investors

This is the slide that makes serious investors trust the founder.

- **No WebContainers.** Different product. Different failure modes. (V1 plan §13.2 already on record.)
- **No per-tenant arbitrary code.** Until V3 with sandbox-as-a-service, never raw code. (V1 plan §15.1.)
- **No "AI does everything" magic claims.** Every AI call is auditable, every output validated, every credit reconciled.
- **No fake testimonials.** No fake customer logos. No fake metrics screenshots.
- **No paid acquisition before product-market fit.** Burning angel money on Facebook ads before retention is proven is a known failure mode.
- **No race to multi-cloud / Kubernetes.** Vercel + Supabase covers up to several thousand customers. Multi-cloud is engineering theatre at this stage.
- **No selling user data.** Ever.
- **No "we will be the next Shopify" claims.** Different problem, different game. We are the **on-ramp** to e-commerce, not the platform that runs $600B of GMV.

---

## 3. The Discussion Checklist (Bring This Page To The Meeting)

Walk the supervisor through these in order:

1. ☐ **Start with the demo.** Run the V1 plan demo (or a recorded version) so the supervisor sees what already exists.
2. ☐ **Show Part 1 §1.1 — §1.3.** Acknowledge the gap; explain the three modes; make clear which is the recommended default.
3. ☐ **Show Part 1 §1.4 frame-by-frame UX.** Get verbal feedback on whether the supervisor agrees with the "AI suggests, user confirms" hybrid as the default.
4. ☐ **Show Part 1 §1.8 effort table.** Confirm we can fit Templates UX into the post-graduation 4-day window without disrupting Gate F.
5. ☐ **Switch to Part 2 §2.1 — the V1 → V2 → V3 table.** This is the centerpiece of the conversation. Walk it row by row.
6. ☐ **Show §2.3.1 Pricing.** Ask the supervisor to challenge the numbers; defend with cost curve §2.8.
7. ☐ **Show §2.6.1 — the 12-slide deck order.** Confirm the supervisor agrees this is the structure investors expect in the local market.
8. ☐ **Show §2.7 Metrics.** Ask the supervisor to confirm targets are realistic for our market.
9. ☐ **Show §2.10 timeline.** Get a verbal "yes/adjust" on the post-graduation phase plan.
10. ☐ **Close with §2.11 — the refusal slide.** This is the discipline statement; it closes the conversation on the right note.

---

## 4. Cross-References

- V1 plan: `.cursor/plans/version-one-advanced-plan_5edfc7dd.plan.md`
- Boundary discipline: `docs/architecture/extensibility-boundaries-review.md`
- Brand intake spec (extended in §1.5): `docs/specs/brand-prompt-intake-spec.md`
- Store generation pipeline: `docs/specs/store-generation-pipeline-spec.md`
- Commerce data model: `docs/specs/commerce-data-model-spec.md`
- Refusal of WebContainers: V1 plan §13.2
- Future-future backlog (per-tenant extensions): V1 plan §15.1
- Cost matrix (V1 baseline): V1 plan §14
- Research that backs the "no per-tenant code" decision: `docs/research/bolt-new-scaling-limitations-discussion.txt`, `docs/research/lovable-production-limitations-review.txt`, `docs/research/stunning-so-technical-review-transcript-breakdown.txt`

---

*Status: discussion document, not yet an accepted decision. Convert into ADR `0007-templates-ux.md` and ADR `0008-post-graduation-roadmap-v2.md` after the supervisor meeting confirms direction.*
