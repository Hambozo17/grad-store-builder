# Generated Store Boundaries

## Rule

Generated stores are data-driven. The platform renders them from approved templates and generated artifacts. Store Generation must not generate independent applications per user.

## Generated Allowed

- Store config.
- Theme tokens.
- Catalog seed data.
- Copy blocks.
- Image prompts or media placeholders.
- Generation report.
- Verification report.

## Generated Not Allowed

- Auth code.
- RLS policies.
- Database migrations.
- Cart engine code.
- Mock checkout code.
- Payment code.
- Super Admin code.
- Secrets or credentials.
- Arbitrary React app files.

## Why

The research warns that one-shot AI generation collapses when backend contracts, security, and state become complex. Version One avoids that by keeping sensitive systems platform-owned and making AI outputs inspectable.

## Visual Editing Boundary

Future visual builder work may edit:

- section order from a registry,
- theme tokens,
- copy blocks,
- media placeholders,
- approved product display settings.

It may not mutate arbitrary DOM or generate new component logic without review.
