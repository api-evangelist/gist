# Gist

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Gist is an AI brand-visibility, answer-engine and content-monetization platform built by ProRata.
It ships Gist GEO, Gist Ads and Gist Answers, and it has a real, public developer surface behind
them at the [Gist Developer Hub](https://platform.gist.ai/docs/about-gist-services).

What this profile holds, all harvested from public URLs on 2026-08-12:

- **OpenAPI** for the Gist Answers API ("Prorata API Service") — 16 operations covering chat,
  streaming completions, citations, per-source attribution, threads, questions, publishers and
  URL summarization. Verbatim copy in `openapi/_original/`; the working copy carries a repaired
  `servers` block (the published one is the relative string `/v1`, which names no host) and the
  repair is recorded in `overlays/`.
- **llms.txt** served by the provider at `platform.gist.ai/llms.txt`, saved verbatim in `llms/`.
- **JSON Schemas** from ProRata's proposed fractional-attribution extension to the Content
  Telemetry standard, in `json-schema/`.
- **Packages** — the first-party Gist Ads SDKs for iOS/macOS (Swift Package Manager) and Android
  (JitPack), plus the unpinned CDN web component, with their real currency recorded.
- Derived and searched artifacts for authentication, conventions, errors, rate limits, plans,
  lifecycle, conformance, the data model, components, sandbox environments, a candidate MCP tool
  set with its crosswalk, and four packaged Agent Skills.
- Verified absences: no `/.well-known/` document on any host, no agent card, no MCP server, no
  status page, no changelog, no published pricing, no trust center. See
  `well-known/gist-well-known.yml`, `lifecycle/gist-lifecycle.yml` and
  `conformance/gist-conformance.yml`.

Backed by: mayfield — https://gist.ai
