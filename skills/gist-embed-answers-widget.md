---
name: Embed the Gist Answers widget on a publisher site
description: >-
  Add AI search to a website with the gist-chat-widget / gist-search-widget custom elements,
  choose one of the three supported layouts, theme it with CSS custom properties, and verify the
  in-answer ad unit renders correctly.
api: https://platform.gist.ai/docs/quick-start-using-widgets
operations:
  - <gist-chat-widget>
  - <gist-search-widget>
generated: '2026-08-12'
method: generated
source: >-
  https://platform.gist.ai/docs/quick-start-using-widgets,
  https://platform.gist.ai/docs/displaying-search-results-on-new-page,
  https://platform.gist.ai/docs/customizing-look-and-feel,
  https://platform.gist.ai/docs/ads-within-gist-answers
---

# Embed the Gist Answers widget on a publisher site

This is the no-code path. It needs two values from ProRata and no server work.

## Before you start

- `api-key` — your ProRata API key.
- `user-id` — your domain or organization identifier.
- Pick the environment by swapping the CDN host: `https://cdn.sandbox.gist.ai` while testing,
  `https://cdn.gist.ai` in production. There is no test key — sandbox and production use the same
  credential against a different host.
- Content ingestion must already be set up, or the widget has nothing of yours to answer from.

## Choose a layout

1. Floating search box, answer in a modal — the simplest, and the one below.
2. Floating search box, answer on a standalone results page.
3. Inline search box, answer on a standalone results page.

## Steps — layout 1 (floating box, modal answer)

1. Load the module in `<head>`:
   ```html
   <script type="module" src="https://cdn.gist.ai/chatWidget.js"></script>
   ```
2. Place the element in `<body>` where the box should live:
   ```html
   <gist-chat-widget
     api-key="your-api-key"
     user-id="your-domain"
     sticky-prompt
     floating
     default-placeholder="Search with AI…">
   </gist-chat-widget>
   ```
   `floating` is load-bearing: it pins the box to the bottom centre of the viewport **and** is
   what makes the answer render in a modal.

## Steps — layouts 2 and 3 (separate results page)

1. On the main page, load `searchWidget.js` and place:
   ```html
   <gist-search-widget
     api-key="your-api-key"
     user-id="your-domain"
     target-url="/search-results.html"
     default-placeholder="Search with AI…"
     sticky-prompt>
   </gist-search-widget>
   ```
   Add `floating` if you want the box pinned rather than inline.
2. Create the results page, load `chatWidget.js` there, and place a `<gist-chat-widget>` with the
   same `api-key` and `user-id`.

## Theming

Override CSS custom properties page-level, inline-scoped, or from an external stylesheet — for
example `--gist-color-action-primary`, `--gist-color-surface-widget`, `--gist-font-family-base`,
`--gist-layout-input-radius`. The full documented list is in `components/gist-components.yml`.

## Verify the ad unit

Gist Ads may be inserted inside the answer automatically, with no publisher configuration. Before
going live, confirm on your own page that:

1. "Sponsored" appears top-left.
2. The advertiser's name appears top-right.
3. The ad body is not clipped.
4. The call to action is visible and the link works.
5. Two horizontal rules separate the ad from the answer.

## Operational note

`chatWidget.js` is served unpinned — there is no version in the URL and no npm package — so a new
build reaches every embedding site at once. If you need release control, mirror the bundle
yourself and monitor the `Last-Modified` header on the CDN copy.
