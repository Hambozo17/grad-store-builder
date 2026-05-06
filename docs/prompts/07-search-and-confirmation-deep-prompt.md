# Search And Confirmation Deep Prompt

```text
Perform a source-confirmation pass for Version One before implementation.

Verify current facts through official docs/MCPs:
- OpenAI structured outputs and SDK behavior.
- Next.js App Router, route handlers, server-only env handling.
- Supabase Auth, profiles, RLS, service-role key safety.
- shadcn/Radix accessible UI patterns.
- Playwright browser testing.
- Vercel preview deployment/env vars.
- Shopify only for future optional path notes.

Read:
- docs/specs/search-confirmation-evidence-spec.md
- docs/specs/project-scope-v1-and-roadmap.md
- docs/architecture/search-confirmation-method.md
- mcp-profiles.json

Output:
1. Confirmation verdict.
2. Verified facts with source links/tool names.
3. Project assumptions.
4. Implementation risks.
5. Missing decisions.
6. Docs that need updates.

Do not change Version One scope during confirmation.
```
