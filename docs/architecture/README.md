# Architecture

Use this folder for system-level structure: product flow, data flow, tenancy, contracts, MCP/skills setup, folder organization, generated artifacts, and technical boundaries.

## Start Here

- `workspace-map.md`
- `system-overview.md`
- `mcp-and-skills-architecture.md`
- `extensibility-boundaries-review.md`
- `orchestration-data-flow.md`
- `generated-store-boundaries.md`
- `search-confirmation-method.md`

## Active Architecture Position

Version One is an authenticated multi-tenant builder, not an anonymous prototype. The architecture should use a Next.js full-stack app with Node.js runtime server modules, Supabase Auth, Supabase Postgres/RLS, AI provider adapters, generated artifacts, mock checkout/handoff, and browser-tested UI.

The highest-risk boundaries are tenant isolation, credit integrity, AI output contracts, prompt injection resistance, and avoiding arbitrary generated application code.
