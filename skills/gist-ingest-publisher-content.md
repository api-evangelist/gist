---
name: Push publisher content into the Gist Content Network
description: >-
  Submit articles to ProRata's ingest API — one at a time for breaking news, or in bulk for
  archival backfill — instead of waiting for the ProRataInc crawler.
api: https://platform.gist.ai/docs/gist-content-api
operations:
  - POST /ingest/article
  - POST /ingest/multiple_articles
generated: '2026-08-12'
method: generated
source: https://platform.gist.ai/docs/gist-content-api
---

# Push publisher content into the Gist Content Network

ProRata prefers push over crawl: you control the timing, breaking news lands immediately, and
there is no crawl failure to debug.

## Before you start

- **The host is not published.** The developer hub documents the paths and payloads but never
  states the base URL. Ask your ProRata contact for it; do not guess from the domain. (Every
  `/v1/*` path on `api.gist.ai` returns 401 whether or not it exists, so probing there proves
  nothing.)
- `Authorization: Bearer <your-api-key>` on every request. The key is issued at the **Publisher
  Group** level, so one key covers every publication in the group.
- `Content-Type: application/json`.
- Default limit: **10 requests per minute.** Contact support for more.

## Steps

1. **Submit one current article.** `POST /ingest/article` with:
   - `publication_external_id` (required) — identifies the publication.
   - `publish_date` (required) — ISO 8601, e.g. `2025-06-27T14:45:00Z`.
   - `title` (required).
   - `content_type` (required) — one of `text`, `video`, `image`.
   - `external_url` (required) — the article URL on your site.
   - `content` (required when `content_type` is `text`) — the article HTML.
   - `canonical_url` (optional) — the original source for syndicated articles.
   - `article_thumbnail_url` (optional).
   - Success: `{"status": "success", "message": "Ingest was successful."}`.

2. **Backfill the archive.** `POST /ingest/multiple_articles` with a JSON **array**. Note the
   field names differ from the single-article endpoint — this is a real inconsistency in the API,
   not a typo:
   - `publication_domain` (required) — not `publication_external_id`.
   - `article_date` (required) — not `publish_date`.
   - `title`, `content`, `url` (required) — note `url`, not `external_url`.
   - `canonical_url` (optional).

3. **Sanitize before you send.** The docs require submitted HTML to be sanitized against XSS and
   to be well formed with no unsupported tags. ProRata states it reviews ingested content for
   security risks, but the obligation to sanitize is on the publisher.

## Alternatives to this API

If push does not fit, ProRata also ingests via the native WordPress REST API, ArcXP Content API,
Strapi Content API, Substack, RSS feeds, or file transfer. If you use the crawler path instead,
allowlist user-agent `ProRataInc` from `172.190.46.235` (production) and `172.171.95.51` (test).

## Errors

| Status | Meaning |
| --- | --- |
| 400 | Invalid or missing parameters |
| 401 | Unauthorized / invalid API key |
| 429 | Rate limit exceeded (10 req/min default) |
| 500 | Internal server error |

There is no idempotency key. Re-posting the same `external_url` is not documented as safe; treat
duplicate submission as an open question with your ProRata contact.
