# Shopify Docs Guide

Use `https://shopify.dev/docs` as the official source for Shopify work in this project.

## Areas

- Apps: use for Shopify admin apps, extensions, App Bridge, Polaris, Functions, webhooks, billing, and App Store requirements.
- Storefronts: use for themes, Liquid, Hydrogen, Oxygen, Storefront API, Customer Account API, Storefront Web Components, and mobile commerce.
- Agents: use for agentic commerce, UCP, profiles/authentication, Catalog, Cart MCP, Checkout MCP, and checkout handoff.
- Changelog: check before relying on API behavior or version-specific checkout/cart/storefront features.

## Project Defaults

- Use `shopify-dev-mcp` for up-to-date Shopify docs and API schema context.
- Treat Shopify Storefront MCP as optional until a real store domain exists.
- Keep the graduation demo on safe test/dev systems unless Shopify store credentials are intentionally provided.
- Do not hardcode store domains, access tokens, customer data, or checkout credentials.

## Useful Commands From Shopify Docs

```powershell
npm i -g @shopify/cli@latest
shopify app init
shopify theme init
shopify hydrogen init
```

Run these only when the task is to create a real Shopify app, theme, or Hydrogen storefront.
