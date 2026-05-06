# Version One Research Synthesis

## Scope

This synthesis applies the existing research to the current authenticated Version One scope.

Version One includes:

- Supabase Auth and profiles.
- Multi-tenant organization/store isolation.
- Admin-granted credits.
- Lazy Super Admin dashboard.
- AI adapter gateway.
- Brand Prompt Intake.
- Store Generation.
- Generated artifacts.
- Storefront, cart, mock checkout, and verification.
- Polished UI/UX.

## Key Lessons Applied

| Research Signal | Version One Requirement |
| --- | --- |
| Lovable-style RLS and tenant leaks are dangerous | RLS and tenant authorization are core requirements, not future work. |
| AI-generated auth/admin logic is risky | Auth, roles, Super Admin, credits, and RLS stay platform-owned. |
| Credit burn loops punish users | Credits are admin-granted and generation has reserve/deduct/refund states. |
| Beautiful UI can hide broken functionality | Verification is required before demo-ready. |
| Context drift breaks large generation flows | Store contracts and generated artifacts are persisted. |
| Monolithic rewrites cause maintenance collapse | Generated output is config/content/tokens/catalog/report, not app code. |
| Browser sandboxes can be fragile | Use normal Next.js/Vercel/local execution, not WebContainer as core architecture. |
| Shopify competitors already exist | Shopify remains optional future path; Version One is prompt-to-store orchestration. |

## Added Risks From New Scope

- Tenant data leakage through weak RLS or client-only authorization.
- Super Admin actions without audit trail.
- Credit balance mismatches if generation fails mid-run.
- AI provider failure blocking the live demo.
- UI scope growing into a full analytics product.

## Required Mitigations

- Server-side auth/role checks for protected actions.
- RLS policies reviewed before Supabase writes.
- Credit ledger is append-only.
- Mock AI and JSON fixture fallback.
- Lazy Super Admin only: approvals, grants, stores, runs, reports.
- No Stripe/live payment path in Version One.
