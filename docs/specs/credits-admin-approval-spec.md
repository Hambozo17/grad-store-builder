# Credits And Admin Approval Spec

## Goal

Version One uses manual Super Admin credit grants instead of Stripe or paid billing. Credits control AI generation usage and make the demo safe.

## Required Capabilities

- Super Admin can approve users.
- Super Admin can grant credits to a user/organization.
- Store generation checks available credits before starting.
- Credits are reserved at run start, deducted on success, and refunded on system failure.
- Every credit movement is stored in an immutable ledger.
- Super Admin can approve or reject generated stores before they are marked demo-ready/published.

## Data Model

- `credit_balances`: current balance per organization.
- `credit_ledger`: immutable events: `grant`, `reserve`, `deduct`, `refund`, `adjustment`.
- `admin_actions`: approval, rejection, suspension, credit grant, store approval.

## Credit Policy

- Credits are integer units, not money.
- Default demo grant can be small, such as 10 credits.
- Credit cost per generation should be deterministic and visible before run start.
- Failed validation should not deduct credits.
- AI/provider failures should refund reserved credits.
- User-cancelled runs may either refund or deduct based on a documented run status.

## Super Admin Dashboard

The dashboard should be lazy and practical:

- User approval queue.
- Credit grant form.
- Store approval queue.
- Generation run list.
- Verification report view.
- Admin action log.

It should not become a full analytics or business dashboard in Version One.

## Tests

- Unapproved user cannot spend credits.
- Approved user with zero credits cannot start generation.
- Credit grant updates balance and ledger.
- Successful generation deducts credits.
- Failed system generation refunds reserved credits.
- Store cannot become demo-ready until verification passes and admin approval is complete.

## Out Of Scope

- Stripe.
- Purchased credits.
- Subscriptions.
- Invoices, taxes, refunds, or production billing.
