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
- `api-reference/` — Auto-generated endpoint docs from OpenAPI spec
- `concepts/` — Urantia Book concept explainers
- `quotes/` — Curated quote collections
- `blog/` — Tutorial articles
## Commands

- `mint dev` — Local dev server (requires `npm i -g mint`)
- Auto-deploys via Mintlify GitHub app on push to default branch
