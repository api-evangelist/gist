---
name: Summarize an authorized publisher URL
description: >-
  Create a summarization request for a URL on one of your publisher group's authorized domains,
  then stream the generated summary back over Server-Sent Events.
api: openapi/gist-answers-api-openapi.yml
operations:
  - POST /v1/summaries
  - GET /v1/summaries/{summaryId}
  - GET /v1/publishers
generated: '2026-08-12'
method: generated
source: openapi/gist-answers-api-openapi.yml + https://platform.gist.ai/reference/post_v1-summaries
---

# Summarize an authorized publisher URL

A create-then-stream pair, with one rule that trips up most first integrations: the URL must be on
a domain your publisher group owns, matched **exactly**.

## Before you start

- Base URL: `https://api.gist.ai`.
- `Authorization: Bearer <api-key>` is required. The reference notes this endpoint accepts "a
  valid API key (public or secret)" — the docs never define that distinction, so use the key you
  were issued.
- These two operations do **not** take `X-User-ID`.

## Steps

1. **Know your authorized domains.** `GET /v1/publishers` returns your publisher group
   (`id`, `name`, `description`, `publishers`). `GET /v1/publishers/{id}` returns one publisher
   including its `sites`. Those are the domains summarization will accept.
   - Both are cached server-side in Redis with a 1-hour TTL, and cache headers come back on the
     response — a repeat call inside the hour is served from cache, not refreshed.
   - `GET /v1/publishers/{id}` returns 403 if the publisher is not in your group.

2. **Create the summary.** `POST /v1/summaries` with:
   - `url` (required) — must exactly match an authorized publisher domain. **Subdomains are not
     automatically allowed.**
   - `length`, `medium`, `style` (optional) to shape the output.
   - The 200 returns `summaryId`.
   - `403 {"error": "..."}` means the domain is not authorized for your organization — check
     step 1 before assuming the URL is bad.
   - `400 {"error": "...", "status": ...}` means the request parameters were invalid. Note this
     endpoint uses a **different** error envelope from the rest of the API.
   - `404` means there was no content at the URL.

3. **Stream the summary.** `GET /v1/summaries/{summaryId}` responds `text/event-stream`. The
   reference recommends `EventSource` in the browser. A 400 means the `summaryId` was malformed;
   a 404 means it does not exist.

## Errors and limits

- `429` on either call; back off using `X-RateLimit-Reset`.
- There is no idempotency key, so a retried `POST /v1/summaries` creates a second summary. If a
  create times out, do not blind-retry.
- Full catalog: `errors/gist-problem-types.yml`.
