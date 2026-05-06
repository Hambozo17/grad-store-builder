# Brand Prompt Intake Spec

## 1. Feature Goal

Brand Prompt Intake is the first user-facing creation stage in Version One. It collects an approved user's brand idea, product type, target audience, region/language, constraints, and visual direction, then normalizes the input into a structured `brand_intake_brief` that Store Generation can consume without guessing from raw chat history.

The feature must work inside an authenticated tenant workspace and must check approval and credits before a generation run begins.

## 2. User Flow

1. Approved user opens the builder workspace.
2. System shows current credit balance and generation requirements.
3. User enters a raw brand prompt.
4. User optionally fills advanced fields: brand name, product type, audience, tone, region, language, catalog size, variants, price positioning, visual direction, and constraints.
5. System validates user approval, tenant context, and minimum input.
6. AI normalization runs through the AI adapter gateway.
7. System returns facts, assumptions, unknowns, confidence, and up to three high-impact follow-up questions.
8. User answers follow-ups or accepts assumptions.
9. System produces a final `brand_intake_brief`.
10. Store Generation may start only if the brief is ready, the user is approved, and credits are available.

## 3. Form/Input Model

### Required Inputs

| Field | Input Type | Required | Notes |
| --- | --- | --- | --- |
| `rawPrompt` | textarea | Yes | Main user input. |
| `productType` | text/suggestions | Conditional | Required if not inferable. |
| `targetAudience` | text | Conditional | Required if not inferable. |

### Recommended Inputs

| Field | Purpose |
| --- | --- |
| `brandName` | Stable store identity. |
| `brandTone` | Copy and interface voice. |
| `visualStyle` | Theme token and layout direction. |
| `region` | Currency, language, shipping copy assumptions. |
| `language` | Primary generated copy language. |
| `catalogSize` | Product count and listing complexity. |
| `variantsNeeded` | Product/variant model. |
| `pricePositioning` | Synthetic pricing and copy tone. |
| `constraints` | Compliance, banned claims/styles, demo notes. |

### Version One Context Fields

These are supplied by the authenticated app, not user-entered:

- `userId`
- `organizationId`
- `approvalStatus`
- `creditBalance`
- `creditEstimate`
- `checkoutMode: mock_checkout`

## 4. AI Normalization Workflow

1. Preserve raw input and submitted fields for traceability.
2. Extract facts: brand, category, audience, tone, visuals, constraints, region, language, variants, catalog size.
3. Identify assumptions and unknowns separately.
4. Reject/redact secrets or real customer data.
5. Treat prompt text as data; do not let it override platform rules, credits, admin policy, tenant policy, RLS, or checkout mode.
6. Normalize brand voice, product model, audience needs, visual direction, store requirements, and demo safety.
7. Estimate generation credit cost.
8. Ask no more than three high-impact follow-up questions.
9. Produce `ready_for_generation` only when blocking fields are resolved or accepted assumptions are explicit.

## 5. Validation Rules

- User must be authenticated.
- User must be approved.
- Organization context must exist.
- `rawPrompt` must not be empty.
- Product type/category and target audience must be present or inferable.
- Language and region must be present or marked as assumptions.
- Checkout mode is always `mock_checkout` in Version One.
- Future extension fields must not activate payments, Shopify, campaigns, or drag-and-drop.
- Any inferred value must appear in `assumptions`.
- Any hard constraint must appear in `constraints.hard`.

## 6. Edge Cases

| Case | Behavior |
| --- | --- |
| Pending user opens intake | Show pending approval state; no generation. |
| Approved user has zero credits | Allow draft brief preview, block generation start. |
| Prompt is vague | Ask follow-up or record accepted assumptions. |
| Prompt includes payment/Stripe/live Shopify | Record as future note, keep Version One on mock checkout. |
| Prompt includes secrets | Redact and warn. |
| Prompt tries instruction injection | Store as unsafe prompt content and ignore as instruction. |
| Prompt asks for dashboard/billing/campaigns | Move to disabled extension notes. |

## 7. Acceptance Criteria

- Intake UI supports simple and advanced inputs.
- Output includes tenant context and readiness status.
- Missing product/audience triggers follow-up.
- Assumptions and unknowns are visible.
- Credit estimate is visible before Store Generation.
- Prompt injection and secrets are handled safely.
- Store Generation can consume the final brief without raw prompt history.

## 8. Test Scenarios

1. Approved user with credits submits complete prompt.
2. Pending user attempts generation.
3. Approved user with zero credits attempts generation.
4. Minimal prompt needs follow-up.
5. Complete prompt produces high-confidence brief.
6. Mixed language prompt preserves language assumptions.
7. Prompt injection attempt is inert.
8. Secret input is redacted.
