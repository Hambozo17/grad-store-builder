# MCP And Skills Architecture

## MCP Purpose

MCP servers connect agents to current docs, browser testing tools, project-scoped files, commerce/dev systems, deployment providers, observability, and optional future integrations.

## Skills Purpose

Skills define repeatable project workflows. For Version One they reduce re-explaining how to handle brand intake, store generation, schema design, UI quality review, cart/mock checkout testing, MCP evaluation, deployment, and the graduation demo.

## Active Default Profile

Use `core-dev` from `mcp-profiles.json` for normal planning and development:

- OpenAI Docs
- Context7
- Shopify Dev MCP
- shadcn
- Playwright
- Chrome DevTools
- Cloudflare Docs
- X Docs
- Project-scoped filesystem

## Version One MCP Rules

- Use Context7 before implementing syntax-sensitive library work: Next.js, React, Supabase Auth, RLS, validation, state, shadcn/Radix, tests, and AI SDK/provider adapters.
- Use OpenAI Docs MCP before implementing OpenAI API, structured outputs, tool calling, agent, or model behavior.
- Use Playwright and Chrome DevTools for UI/UX, responsive, accessibility, and flow verification.
- Use `commerce-dev` only for development Supabase/Prisma/Neon inspection or schema work.
- Do not activate Stripe in Version One. Payments are skipped; credits are manually granted by Super Admin.
- Use Shopify docs only for future optional path planning unless a real Shopify development store is intentionally provided.
- Use read-only or development-safe profile variants before enabling write tools.
- Never add full-device filesystem access.

## Expansion Profiles

- `commerce-dev`: Supabase, Prisma, Neon, and optional Shopify storefront template work in development only.
- `frontend-design`: Figma, shadcn, Playwright, Chrome DevTools.
- `research-brand`: Tavily, Firecrawl, X Docs.
- `automation`: n8n, Pipedream, Zapier, Make templates.
- `deploy-observe`: GitHub, Vercel, Netlify, Cloudflare, Sentry.
- `payments-future`: Stripe only after a later payment decision.
