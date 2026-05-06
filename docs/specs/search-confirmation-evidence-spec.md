# Search Confirmation Evidence Spec

## Goal

Before implementing syntax-sensitive or platform-sensitive work, confirm current facts through official docs or MCPs. Do not rely on memory for APIs that change.

## Required Sources

- OpenAI Docs MCP for OpenAI API, structured outputs, agents, tools, model behavior, and SDK usage.
- Context7 for Next.js, React, Supabase, Auth, validation, state libraries, shadcn/Radix, testing libraries, and provider SDKs.
- Supabase official docs or Context7 for Auth, RLS, database, migrations, and service-role key guidance.
- Playwright/Chrome DevTools docs/tools for UI, browser, accessibility, and flow verification.
- Shopify official docs only for future optional Shopify path work.
- Vercel docs for deployment and environment behavior.

## Evidence Output

For each implementation plan or major code change, record:

- Topic confirmed.
- Source used.
- Date checked.
- Decision affected.
- Remaining uncertainty.

## Version One Confirmation Priorities

- Supabase Auth and RLS.
- Next.js App Router route handlers and server-only environment variables.
- OpenAI structured outputs.
- shadcn/Radix component installation and accessible patterns.
- Playwright browser verification.
- Vercel preview env setup.

## Out Of Scope

- Broad web search for implementation syntax when official docs/MCPs are available.
- Community sentiment as a source of truth for API behavior.
