# UI/UX Quality Spec

## Goal

The Version One app must feel like a premium creation studio: clean, focused, fast, and trustworthy. Beautiful UI alone is not enough; every polished screen must support the underlying workflow.

## Product Surfaces

- Auth screens.
- Pending approval state.
- Builder dashboard.
- Brand Prompt Intake.
- Generation run timeline.
- Store preview.
- Generated artifact/report views.
- Cart/mock checkout demo flow.
- Lazy Super Admin dashboard.

## Design Principles

- Clear hierarchy and restrained density.
- Strong empty states and loading states.
- Status-first UX: approval, credits, run state, verification state.
- No generic marketing landing page as the main product surface.
- No decorative complexity that hides broken behavior.
- Buttons, inputs, menus, tabs, toggles, and tables must use consistent design tokens.
- Responsive layouts must work on mobile, tablet, and desktop.
- No text overlap, clipping, or layout shift in core flows.

## Interaction Requirements

- Users always know whether they are pending approval, out of credits, generating, failed, awaiting admin approval, or demo-ready.
- Credit cost is visible before generation.
- Generation progress is inspectable.
- Verification failures are actionable.
- Super Admin actions require confirmation when they change approval or credits.

## Verification

Use Playwright and Chrome DevTools for:

- Login and protected-route flow.
- Mobile/desktop viewport checks.
- Keyboard navigation and focus visibility.
- Text overflow and long-name cases.
- Cart/mock checkout flow.
- Super Admin approval/credit flows.
- Basic accessibility checks.

## Out Of Scope

- Arbitrary visual builder editing.
- Drag-and-drop layout editing.
- Heavy animation that harms clarity or performance.
