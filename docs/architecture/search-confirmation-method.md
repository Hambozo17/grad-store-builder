# Search Confirmation Method

## Purpose

Use current official docs before implementing APIs, frameworks, auth, RLS, AI adapters, UI libraries, or deployment behavior. This prevents outdated syntax and old assumptions from entering implementation.

## Required Confirmation Sources

- OpenAI Docs MCP for OpenAI API, structured outputs, tool calling, model behavior, and SDK usage.
- Context7 for Next.js, React, Supabase, Auth, shadcn/Radix, testing, validation, and state libraries.
- Supabase docs or Context7 for Auth, RLS, service-role key handling, and database policies.
- Playwright and Chrome DevTools tools for browser verification.
- Vercel docs for deployment and environment behavior.
- Shopify official docs only when planning optional future Shopify work.

## Output Format

Record source confirmation in plans or logs:

- Topic.
- Source/tool.
- Date checked.
- Confirmed fact.
- Decision affected.
- Remaining uncertainty.

## Version One Priority Topics

- Supabase Auth session handling.
- RLS policy patterns for tenant isolation.
- Next.js route handlers and server-only environment variables.
- OpenAI structured output parsing/refusals.
- shadcn/Radix accessible UI primitives.
- Playwright locators and browser flow testing.
- Vercel preview environment variables.
