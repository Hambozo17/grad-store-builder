# Graduation Proposal Refined Scope

## 1. Project Title

AI-Orchestrated Multi-Tenant E-Commerce Store Builder

## 2. Academic Abstract

This project implements a multi-tenant AI-assisted e-commerce store builder. Users sign up, are approved by a Super Admin, receive generation credits, and use a polished builder interface to describe a brand, product type, and target audience. The system normalizes the prompt into a structured brand brief, generates a deterministic store plan, creates catalog/config/content artifacts, renders an isolated storefront, and verifies the cart and mock checkout flow before marking the store demo-ready. The technical focus is orchestration across authentication, tenant isolation, credit control, AI structured outputs, store generation, UI rendering, and verification. Version One intentionally excludes live payments, live Shopify, live ad campaigns, and production customer operations.

## 3. Problem Statement

Small merchants, students, and early-stage founders can describe an e-commerce idea in simple language, but turning that idea into a working store requires many connected tasks: account setup, brand clarification, product modeling, storefront design, cart behavior, checkout handoff, data isolation, and quality verification. Many AI builders produce attractive prototypes, but research shows they often break down around multi-tenant data boundaries, schema safety, RLS, context drift, and claims of completion without verification. This project solves a graduation-suitable version of that problem by building a controlled, inspectable orchestration platform instead of a one-shot generator.

## 4. Version One Deliverables

Version One will deliver:

- Supabase Auth-based login/signup.
- User profile and tenant ownership records.
- Super Admin approval and credit grants.
- Admin action audit trail.
- AI adapter gateway with provider/task boundaries.
- Brand Prompt Intake and normalization.
- Store Generation pipeline.
- Generated store artifacts: config, theme tokens, catalog seed, copy blocks, report.
- Tenant-owned generated storefront.
- Cart and mock checkout/handoff.
- Verification report.
- Polished builder and Super Admin UI.
- Deployment/demo runbook.

## 5. Explicit Out Of Scope

- Stripe, live payment collection, paid subscriptions, and purchased credits.
- Production Shopify integration.
- Live ad spend and campaign launch.
- Production customer accounts and real order fulfillment.
- Marketplace/add-ons.
- Arbitrary drag-and-drop builder.
- Vendor payout/commission operations.

## 6. Technical Value

The core technical value is orchestration with safety: the system coordinates Auth, tenant isolation, credit accounting, AI output schemas, generated artifacts, storefront rendering, cart/mock checkout behavior, verification, and admin approvals. The project demonstrates that AI generation can be constrained by contracts and tests instead of trusted blindly.

## 7. Evaluation Criteria

- User can sign up and log in.
- Super Admin can approve the user and grant credits.
- Approved user can create a tenant-owned generated store.
- AI normalization produces a structured brand brief with assumptions and confidence.
- Store Generation produces a plan and artifacts before rendering.
- Generated storefront includes home, listing, detail, cart, mock checkout/handoff, and confirmation.
- Cart add/remove/quantity/subtotal behavior works.
- Mock checkout never collects payment data.
- Verification report passes before demo-ready status.
- RLS/authorization checks prevent one user from seeing another user's stores.
- UI is clean, responsive, accessible, and polished enough for a serious demo.

## 8. Risk Mitigation

| Risk | Mitigation |
| --- | --- |
| Tenant data leaks | RLS, tenant IDs, server-side authorization checks, negative tests |
| Credit abuse | Ledger-based deductions, admin grants only, audit trail |
| AI output drift | Structured schemas, task adapters, validators, stored artifacts |
| Pretty UI without function | Verification gate before demo-ready |
| Scope creep | Payments, Shopify, campaigns, drag-and-drop, and marketplace stay future-only |
| External services fail | Mock AI fixture and mock checkout fallback |

## 9. Documentation Updates Required

- Keep `project-scope-v1-and-roadmap.md` synchronized with this scope.
- Keep `prompt-output-contracts.md` synchronized with the contracts.
- Treat auth/tenancy, credits/admin, AI gateway, UI/UX, commerce model, cart/mock checkout, and verification specs as Version One requirements.
