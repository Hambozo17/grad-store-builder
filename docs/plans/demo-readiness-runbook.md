# Demo Readiness Runbook

## Goal

Make the graduation demo repeatable even if external AI, database, or deployment services are unreliable.

## Required Demo Path

1. Open app preview.
2. Sign in as Super Admin.
3. Approve a demo user.
4. Grant demo credits.
5. Sign in as demo user.
6. Submit Brand Prompt Intake.
7. Run Store Generation.
8. Inspect generated artifacts and report.
9. Preview generated storefront.
10. Add products to cart.
11. Complete mock checkout.
12. Show verification report.
13. Super Admin approves generated store.
14. Show final demo-ready status.

## Fallback Modes

- Use mock AI provider if OpenAI/API access fails.
- Use JSON fixtures if Supabase dev data is unavailable.
- Use mock checkout always; no payment dependency.
- Use local app run if Vercel preview fails.
- Use pre-generated demo store if generation fails during live presentation.

## Required Checks Before Demo

- Auth flow works.
- User approval flow works.
- Credit grant and deduction work.
- Tenant isolation checks pass.
- Intake and generation fixture run works.
- Store preview renders on desktop and mobile.
- Cart add/remove/quantity works.
- Mock checkout success/cancel works.
- Verification report passes.
- No Stripe/live payment UI is visible.
- No production secrets are in docs/config.

## Demo Data

Keep at least one safe demo scenario:

- Category: handmade candles, skincare, gifts, or fashion accessories.
- Catalog size: small.
- Variants: scent/size or color/size.
- Region: Egypt/Cairo-ready.
- Language: English first, Arabic-ready fields preserved.

## Recovery Notes

If a live step fails, switch to the nearest fixture and explain that Version One stores contracts/artifacts precisely to make recovery and verification possible.
