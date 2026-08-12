---
name: Ask Gist Answers and read the attribution
description: >-
  Ask a question against a publisher's licensed corpus with the Gist Answers API, stream the
  answer back, then fetch the citations and the per-source credit distribution for that turn.
api: openapi/gist-answers-api-openapi.yml
operations:
  - POST /v1/chat
  - GET /v1/chat/response/{threadId}/{turnId}
  - GET /v1/chat/citations/{threadId}/{turnId}
  - GET /v1/chat/attributions/{threadId}/{turnId}
generated: '2026-08-12'
method: generated
source: openapi/gist-answers-api-openapi.yml + https://platform.gist.ai/reference/get_v1-health
---

# Ask Gist Answers and read the attribution

Gist Answers is a two-call product: you create a turn, then you stream it. The interesting part
is the third call — the attribution split, which is what makes this API different from a generic
chat endpoint.

## Before you start

- Base URL: `https://api.gist.ai`. The published spec's `servers` block says `/v1`, which names no
  host; every path below already carries the `/v1` prefix.
- Every call needs two headers:
  - `Authorization: Bearer <api-key>` — issued per Publisher Group by ProRata at onboarding.
  - `X-User-ID: <your-domain-or-org-id>` — the same value a publisher puts in the widget's
    `user-id` attribute.
- There is **no idempotency key**. `POST /v1/chat` creates a turn every time you call it. Do not
  blind-retry on a timeout; list the thread first and check whether the turn landed.

## Steps

1. **Create the turn.** `POST /v1/chat` with a JSON body.
   - Required: `user_prompt`.
   - Optional: `thread_id` to continue an existing conversation, `temperature`, `inclusion_list`
     to narrow the corpus.
   - The 200 returns `thread_id`, `turn_id`, `thread_title`, `citations`, `attributions` and
     `response_time`. Keep `thread_id` and `turn_id` — every later call is keyed on that pair.
   - A 400 here means `user_prompt` was missing.

2. **Stream the answer.** `GET /v1/chat/response/{threadId}/{turnId}`.
   - Responds `text/event-stream` with `{event, data}` frames. Read it with an SSE client
     (`EventSource` in a browser).
   - A 400 means the id pair is malformed; a 404 means the thread or turn does not exist.

3. **Fetch the citations.** `GET /v1/chat/citations/{threadId}/{turnId}` returns the source
   documents behind the answer. Note that the published spec declares this 200 body with no
   properties, so treat the shape as undocumented and read defensively.

4. **Fetch the attribution.** `GET /v1/chat/attributions/{threadId}/{turnId}` returns four
   parallel credit distributions for the same turn:
   - `credit_dist` — the overall split
   - `domain_credit_dist` — by source domain
   - `document_credit_dist` — by individual document
   - `publisher_credit_dist` — by publisher
   This is the number the 50/50 revenue share is computed against. Surface it; do not discard it.

5. **Offer follow-ups.** `POST /v1/questions/related` with `thread_id`, `turn_id` and `question`
   (all required) returns a `questions` array. `num_recommended_queries` and `max_words_question`
   tune the output.

## Errors and limits

- `401 {"error":"Unauthorized","message":"Missing or invalid Authorization header","statusCode":401}`
  is returned for **any** `/v1/*` path on this host, including paths that do not exist. A 401 is
  not evidence that the endpoint you called is real.
- `429` carries `X-RateLimit-Limit`, `X-RateLimit-Remaining` and `X-RateLimit-Reset` (a Unix
  timestamp). Back off until `X-RateLimit-Reset`. The numeric limit is set per organization and is
  not published.
- Full catalog: `errors/gist-problem-types.yml`. Conventions: `conventions/gist-conventions.yml`.
