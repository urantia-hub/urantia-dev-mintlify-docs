# urantia-dev-mintlify-docs

Mintlify-powered documentation for the Urantia Papers API (https://urantia.dev).

## Tech Stack

- Framework: Mintlify
- Config: docs.json
- Content: MDX files
- API spec: api-reference/openapi.json (synced from production)

## Structure

- `index.mdx` — Homepage
- `quickstart.mdx` — Getting started guide
- `use-cases.mdx` — Use case examples
- `papers.mdx`, `paragraphs.mdx`, `entities.mdx`, `audio.mdx`, `cdn.mdx` — Data guides
- `mcp-servers.mdx` — MCP server setup (API + Docs servers)
- `ai-agents.mdx` — AI agent integration guide
- `sdks/` — TypeScript SDK docs (split into 3 pages)
  - `overview.mdx` — Install, package comparison, "Using Both Together", demo link
  - `api.mdx` — @urantia/api usage patterns, endpoint groups, error handling
  - `auth.mdx` — @urantia/auth OAuth flows (redirect/popup/server), session management, scopes
- `api-reference/` — Auto-generated endpoint docs from OpenAPI spec (17 endpoints)
- `concepts/` — Urantia Book concept explainers (14 pages)
- `quotes/` — Curated quote collections (15 themes)
- `blog/` — Tutorial articles (4 posts)

## Navigation (docs.json)

5 tabs: Guides, API Reference, Concepts, Quotes, Blog

Guides tab groups: Getting Started, Data, SDKs (Overview, @urantia/api, @urantia/auth), Integrations (MCP Servers, AI Agents), Donate, Legal

## Commands

- `mint dev` — Local dev server (requires `npm i -g mint`)
- Auto-deploys via Mintlify GitHub app on push to default branch

## Notes

- No changelog — intentionally removed as unnecessary
- Config file is `docs.json` (not `mint.json`)
- Old single `sdks.mdx` was split into `sdks/` directory with 3 focused pages
