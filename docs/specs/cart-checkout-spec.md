# Cart And Mock Checkout Spec

## Goal

Version One proves the buyer path without collecting money. Cart and mock checkout are platform-owned and reused by every generated store.

## Cart Requirements

- Add product to cart.
- Require variant selection when variants exist.
- Remove item.
- Increase/decrease quantity.
- Enforce quantity minimum of 1.
- Enforce inventory or demo quantity cap.
- Recalculate subtotal from unit price snapshots.
- Show empty cart state.
- Persist cart across page navigation and refresh according to selected persistence mode.
- Handle unavailable product/variant gracefully.

## Mock Checkout Requirements

- Display cart summary.
- Clearly label the flow as mock/demo checkout.
- Do not collect card numbers or payment data.
- Support success and cancel states.
- Create a `mock_checkout_session` record if Supabase persistence is enabled.
- Route to confirmation on success.
- Clear or preserve cart according to documented demo behavior.

## Tests

- Empty cart.
- Add two products.
- Add variant product.
- Quantity update.
- Remove item.
- Inventory cap.
- Refresh persistence.
- Mock checkout success.
- Mock checkout cancel.
- Mobile cart and checkout path.

## Out Of Scope

- Stripe.
- Live payment forms.
- Real order fulfillment.
- Shopify checkout.
