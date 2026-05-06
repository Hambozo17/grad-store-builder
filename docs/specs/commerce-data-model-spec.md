# Commerce Data Model Spec

## Goal

Define the Version One commerce model for generated stores, synthetic catalogs, carts, mock checkout, and verification without production payment or fulfillment complexity.

## Core Entities

- `generated_stores`: tenant-owned store identity, slug, status, template, locale, currency.
- `generation_runs`: run lifecycle, input brief snapshot, credit usage, artifact references, status, errors.
- `store_artifacts`: config, theme tokens, catalog seed, copy blocks, generation report.
- `products`: stable product identity, name, slug, category, description, media placeholder, price minor units, currency, inventory state.
- `product_variants`: stable variant identity, SKU placeholder, option values, price override, inventory quantity.
- `carts`: tenant/store-specific demo cart session.
- `cart_items`: product/variant identity, quantity, unit price snapshot.
- `mock_checkout_sessions`: mock handoff, success/cancel, no payment collection.
- `verification_reports`: required checks, results, demo readiness.

## Data Rules

- Use UUID primary keys.
- Store money-like product prices as integer minor units plus currency, even though no payments occur.
- Stable IDs must be used for products and variants; never use names or slugs as cart identity.
- Synthetic data only.
- Seed order: organization, store, run, artifacts, products, variants, cart, cart items, mock checkout, verification report.
- Category presets may add optional fields, but may not alter schema per store.

## Tenant Rules

- Tenant-owned tables include `organization_id`.
- User-owned convenience views must still resolve through organization membership.
- RLS must block cross-tenant reads/writes.

## Out Of Scope

- Real orders.
- Tax, shipping, fulfillment, returns, refunds.
- Payment provider tables.
- Vendor payout and commission accounting.
