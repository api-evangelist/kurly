# Kurly

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Kurly (컬리, operated by The Farmers Front) is a South Korean online grocery and lifestyle commerce company, known for Market Kurly and its overnight "dawn delivery" (샛별배송) model built on a cold-chain fulfillment network. Kurly Logistics Services (KLS) opens that network to contracted shipper clients (화주사) as a B2B Open API spanning three service families: fulfillment (물류대행), where Kurly holds and ships the shipper's stock, covering goods master, inbound receiving, inventory and ledgers, outbound orders and fulfillment plans; delivery agency (배송대행), where the shipper ships from their own warehouse over Kurly's Nextmile last-mile network; and shared delivery tracking. Access is contract-gated — clientId and secretKey are issued only after IP allowlist registration — and Kurly states as policy that KLS does not push data to external systems, so integrations poll rather than subscribe to webhooks.

**Areas:** Company · Technology · Logistics · Fulfillment · Supply Chain · Delivery · E-Commerce · Grocery · Shipping · Order Management · Inventory · Tracking

## Kurly on APIs.io

Everything profiled for Kurly is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/kurly/](https://apis.io/providers/kurly/)** — or go straight to one of the APIs or properties below.

### APIs (3)

- [Kurly Authentication API](https://apis.io/apis/kurly/kurly-authentication-api/) — The Authentication API from Kurly — 1 operation(s) for authentication.
- [Kurly 배송운영 정책 API](https://apis.io/apis/kurly/kurly-default-api/) — The 배송운영 정책 API from Kurly — 26 operation(s) for 배송운영 정책.
- [Kurly Delivery Tracking (배송추적) API](https://apis.io/apis/kurly/kurly-delivery-tracking-api/) — The Delivery Tracking (배송추적) API from Kurly — 1 operation(s) for delivery tracking (배송추적).

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Arazzo workflows (3)

Multi-step workflows that chain operations into a business task.

- [Kurly delivery-agency order through to delivery tracking](https://apis.io/arazzos/kurly/kurly-delivery-order-to-tracking/)
- [Kurly fulfillment outbound order flow](https://apis.io/arazzos/kurly/kurly-fulfillment-order-flow/)
- [Kurly fulfillment inbound receiving flow](https://apis.io/arazzos/kurly/kurly-inbound-receiving-flow/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Kurly Logistics Services (KLS) Open API](https://apis.io/servers/kurly/kurly-kls/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Kurly Agentic Access](https://apis.io/agentic-access/kurly/kurly-agentic-access/)

#### Vocabulary (1)

The provider's domain terms, defined.

- [Kurly Vocabulary](https://apis.io/vocabularies/kurly/kurly-vocabulary/)

#### Collections (3)

Postman and Open Collection request collections.

- [Kurly Logistics Services (KLS) Open API Authentication API](https://apis.io/collections/kurly/open-kurly-authentication-api/)
- [Kurly Logistics Services (KLS) Open API Authentication 배송운영 정책 API](https://apis.io/collections/kurly/open-kurly-default-api/)
- [Kurly Logistics Services (KLS) Open API Authentication Delivery Tracking (배송추적) API](https://apis.io/collections/kurly/open-kurly-delivery-tracking-api/)

#### Overlays (4)

OpenAPI overlays, including conversational phrasing for each operation.

- [Kurly Auth Overlay](https://apis.io/overlays/kurly/kurly-auth-overlay/)
- [Kurly Delivery Agency Overlay](https://apis.io/overlays/kurly/kurly-delivery-agency-overlay/)
- [Kurly Delivery Tracking Overlay](https://apis.io/overlays/kurly/kurly-delivery-tracking-overlay/)
- [Kurly Fulfillment Overlay](https://apis.io/overlays/kurly/kurly-fulfillment-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Kurly Authentication](https://apis.io/security/kurly/kurly-authentication/)
- [Kurly Domain Security](https://apis.io/security/kurly/kurly-domain-security/)

#### Rate limits (1)

Published request limits.

- [Kurly Rate Limits](https://apis.io/rate-limits/kurly/kurly-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://kurly.com)
- [Documentation](https://developers.kurly.com/docs/intro)
- [Getting started](https://developers.kurly.com/docs/intro)
- [API reference](https://developers.kurly.com/docs/api)
- [Sign up](https://developers.kurly.com/docs/auth)
- [Support](https://developers.kurly.com/docs/faq)
- [Changelog](https://developers.kurly.com/changelog)
- [Blog](https://helloworld.kurly.com/)
- [GitHub](https://github.com/thefarmersfront)

## API Evangelist rating

**Kin Score 32.8** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 78.6 |
| Contract quality | 46.9 |
| Contract governance | 19.7 |
| Operational transparency | 18.4 |
| Developer ergonomics | 31.5 |
| Access clarity | 13.2 |

**Agent readiness 34.3** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | verified |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | yes |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 16.7** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/kurly/](https://apis.io/providers/kurly/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Kurly: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/kurly/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
