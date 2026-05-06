# Planning Backlog

## Immediate Version One Plans

1. Source confirmation pass for Next.js, React, Supabase Auth/RLS, OpenAI structured outputs, shadcn/Radix, Playwright, and Vercel.
2. App scaffold plan for a Next.js full-stack builder.
3. Auth, users, profiles, roles, and tenant isolation implementation plan.
4. Credits and Super Admin approval implementation plan.
5. UI/UX design system and quality plan.
6. AI adapter gateway implementation plan.
7. Brand Prompt Intake implementation plan.
8. Store Generation implementation plan.
9. Commerce data model and Supabase development schema plan.
10. Cart and mock checkout behavior plan.
11. Verification runner plan.
12. Demo readiness runbook.
13. Graduation presentation and live demo script.

## Research To Convert Into Decisions

- Lovable production limitations: RLS, auth, multi-tenant leaks, schema drift, credit-burn loops.
- v0 and Bolt limitations: visual-only prototypes, monolithic rewrites, context drift, sandbox/resource problems.
- Stunning.so UX lessons: prompt-first builder, dashboard clarity, prompt-injection resistance.
- Shopify scan: position Version One as store generation, not a live Shopify app or chatbot.

## Specs To Keep Current

- Version One scope and roadmap.
- Auth/users/tenancy.
- Credits/admin approval.
- AI adapter gateway.
- UI/UX quality.
- Prompt output contracts.
- Brand Prompt Intake.
- Store Generation.
- Commerce data model.
- Cart/mock checkout.
- Search confirmation evidence.

## Rules

- Promote durable requirements to `docs/specs/`.
- Promote accepted tradeoffs to `docs/decisions/`.
- Add factual logs after significant updates.
- Do not reintroduce Stripe/live payments into Version One.
- Do not treat visual builder, Shopify, campaigns, marketplace, or vendor payouts as active Version One requirements.
