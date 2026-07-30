# Kurly

Kurly (컬리, operated by The Farmers Front) is a South Korean online grocery and lifestyle commerce
company, known for Market Kurly and its overnight "dawn delivery" (샛별배송) model built on a
cold-chain fulfillment network.

**Kurly Logistics Services (KLS)** opens that network to contracted shipper clients (화주사) as a B2B
Open API, documented at [developers.kurly.com](https://developers.kurly.com/) (Korean only).

## The API surface

| Service | Korean | What it does | Ops |
|---|---|---|---|
| Fulfillment | 물류대행 | Kurly holds and ships the shipper's stock — goods master, inbound receiving, inventory and ledgers, outbound orders, fulfillment plans | 24 |
| Delivery Agency | 배송대행 | Shipper ships from their own warehouse over Kurly's Nextmile last-mile network | 7 |
| Delivery Tracking | 배송추적 | Shared waybill tracking, usable by both services | 1 |
| Authentication | 인증 | clientId/secretKey → Bearer access token | 1 |

## What integrators need to know

- **Access is contract-gated.** There is no self-service signup. A shipper submits its outbound IP
  allowlist via Kurly's Google Form and receives `clientId` / `secretKey` within 5 business days.
- **No webhooks, by stated policy.** Kurly does not push data to external systems and recommends
  periodic polling.
- **Idempotency is real but per-domain.** Delivery-agency order registration uses a `requestKey`
  body field (max 50 chars, unique per shipper, never reusable, server-generated if omitted).
  Inbound receiving uses an `X-Idempotency-Key` header with `X-Timestamp`. Retries must reuse the
  same key.
- **Partial success is normal.** SKU registration and bulk order register/cancel return `200` with
  per-item failures in the body. Never infer success from the status code alone.
- **Rate limiting is signalled but unquantified.** `429` applies across all APIs; no numeric quotas
  and no rate-limit headers are published.
- **The test environment is security-gated** — security training, a signed pledge, and VPN/firewall
  setup are required before an admin account is issued.

## Artifacts in this repository

| Artifact | Path | Method |
|---|---|---|
| OpenAPI (4 specs, 33 operations) | `openapi/` | generated from Kurly's published reference |
| Overlays | `overlays/` | generated |
| Authentication profile | `authentication/` | searched |
| Conventions (incl. idempotency) | `conventions/` | searched |
| Error codes | `errors/` | searched |
| Vocabulary (tracking + inbound states) | `vocabulary/` | searched |
| Data model | `data-model/` | derived |
| Lifecycle | `lifecycle/` | searched |
| Changelog (16 releases) | `changelog/` | searched |
| Rate limits | `rate-limits/` | searched |
| Sandbox | `sandbox/` | searched |
| Conformance | `conformance/` | derived |
| Domain security | `security/` | probed |
| Well-known probe | `well-known/` | searched (verified absence) |
| MCP candidate tools | `mcp/` | derived |
| Agent skills (4) | `skills/` | generated |
| Arazzo workflows (3) | `arazzo/` | generated |
| Agentic access | `agentic-access/` | generated |
| llms.txt | `llms/` | generated |

The OpenAPI documents are an API Evangelist reconstruction: every path, method, summary and response
status comes from Kurly's own rendered API reference. Request and response **schemas render
client-side** on developers.kurly.com and are deliberately **not** reproduced — treat each
operation's `externalDocs` URL as authoritative for payload shapes.

## Not present, and why

No SDKs or CLI (Kurly publishes none — its GitHub org
[thefarmersfront](https://github.com/thefarmersfront) carries no KLS client libraries), no AsyncAPI
or webhook catalog (no event surface exists, by policy), no OAuth scopes (auth is a bearer
credential exchange, not OAuth), no status page, no published deprecation policy, no vulnerability
disclosure programme, and no trust centre or compliance certifications — each verified rather than
assumed.

- Contact: logistics-dev@kurlycorp.com
- Backed by: hongshan
