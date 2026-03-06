---
name: urantia-papers-api
description: Access and search the Urantia Book via REST API. Use when retrieving paragraphs, searching text, browsing papers, getting audio narration, or building RAG pipelines over the Urantia Papers.
license: MIT
compatibility: Requires network access to api.urantia.dev. No authentication or API keys needed.
metadata:
  author: kelsonic
  version: "1.0"
---

# Urantia Papers API

Free, open REST API for structured access to all 197 papers, 1,626 sections, and 14,500+ paragraphs of the Urantia Book — with full-text search and multi-voice audio narration.

## Base URL

```
https://api.urantia.dev
```

No authentication required. Rate limit: 100 requests/minute per IP.

## Capabilities

- **Search** the full text of the Urantia Book with ranked results
- **Retrieve** any paragraph by reference in three auto-detected ID formats
- **Browse** the table of contents, papers, and sections
- **Get context** around a paragraph for RAG applications
- **Access audio** narration with multiple TTS models and voices
- **Generate typed clients** from the OpenAPI spec at `/openapi.json`

## Endpoints

| Method | Path | Description |
|--------|------|-------------|
| GET | `/toc` | Full table of contents (parts and papers) |
| GET | `/papers` | List all 197 papers with metadata |
| GET | `/papers/:id` | Single paper with all paragraphs |
| GET | `/papers/:id/sections` | Sections within a paper |
| GET | `/paragraphs/random` | Random paragraph |
| GET | `/paragraphs/:ref` | Paragraph by any ID format |
| GET | `/paragraphs/:ref/context?window=3` | Paragraph with surrounding context (window: 1-10) |
| POST | `/search` | Full-text search with pagination |
| GET | `/audio/:paragraphId` | Audio URLs for a paragraph |

## Paragraph Reference Formats

The API accepts three reference formats — auto-detected from the string:

| Format | Example | Structure |
|--------|---------|-----------|
| globalId | `1:2.0.1` | `partId:paperId.sectionId.paragraphId` |
| standardReferenceId | `2:0.1` | `paperId:sectionId.paragraphId` |
| paperSectionParagraphId | `2.0.1` | `paperId.sectionId.paragraphId` |

## Workflows

### Search and retrieve passages

1. `POST /search` with `{"q": "your query", "type": "and", "limit": 10}`
2. Use `standardReferenceId` from each result to fetch context
3. `GET /paragraphs/:ref/context?window=3` for surrounding paragraphs

### RAG pipeline (recommended)

1. `GET /toc` — understand the book structure
2. `POST /search` — find relevant passages for the user's question
3. `GET /paragraphs/:ref/context?window=3` — expand each result with surrounding paragraphs
4. Feed the collected passages as context to your LLM

### Browse and read

1. `GET /toc` — get the full table of contents
2. `GET /papers/:id` — read an entire paper
3. `GET /papers/:id/sections` — get sections within a paper

### Get audio for a passage

1. Look up a paragraph via `GET /paragraphs/:ref`
2. The response includes an `audio` field with URLs keyed by model and voice
3. Or use `GET /audio/:paragraphId` for just the audio data

## Search

Request body for `POST /search`:

```json
{
  "q": "search terms",
  "type": "and",
  "limit": 10,
  "page": 1,
  "paperId": null,
  "partId": null
}
```

Search modes:
- `and` (default) — all words must appear. Best for specific queries.
- `or` — any word can appear. Best for broad exploration.
- `phrase` — exact phrase match. Best for quoting specific text.

Optional filters: `paperId` (0-196) and `partId` (1-5) narrow the scope.

## Audio

Every paragraph has audio narration. The `audio` field is a nested object keyed by model and voice:

```json
{
  "audio": {
    "tts-1-hd": {
      "nova": { "format": "mp3", "url": "https://audio.urantia.dev/tts-1-hd-nova-1:2.0.1.mp3" }
    }
  }
}
```

Models: `tts-1-hd`, `tts-1`. Voices: `nova`, `echo`, `onyx`, `alloy`, `fable`, `shimmer`. Full coverage with `tts-1-hd/nova`.

## Book Structure

The Urantia Book contains 197 papers organized in four parts plus a Foreword:

- **Foreword** — Paper 0
- **Part I: The Central and Superuniverses** — Papers 1-31
- **Part II: The Local Universe** — Papers 32-56
- **Part III: The History of Urantia** — Papers 57-119
- **Part IV: The Life and Teachings of Jesus** — Papers 120-196

## Constraints

- Rate limit: 100 requests/minute per IP (429 response if exceeded)
- All responses are JSON
- The `/paragraphs/random` endpoint is never cached; all other endpoints are CDN-cached
- Response envelope: `{ data, meta: { page, limit, total, totalPages } }` for paginated endpoints

## Documentation

- Interactive docs: https://api.urantia.dev/docs (Swagger UI)
- Full documentation: https://urantia.dev
- OpenAPI 3.1 spec: https://api.urantia.dev/openapi.json
