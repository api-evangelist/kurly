---
name: Send outbound orders to a Kurly fulfillment centre
description: >-
  Register SKUs, map them to a sales channel, place single or bulk outbound orders against Kurly's
  warehouse, read the fulfillment plan, and cancel — handling partial-success responses correctly.
api: openapi/kurly-kls-fulfillment-openapi.yml
operations:
  - issueToken
  - saveSku
  - updateSku
  - findSkuList
  - saveSalesChannelMapping
  - findSalesChannelMappingList
  - createOrderV2
  - createOrdersBulkV2
  - findOrders
  - findOperationPlan
  - findOperationPlansBulk
  - cancelOrderV2
  - cancelOrdersBulkV2
  - findInventories
generated: '2026-07-19'
method: generated
source: https://developers.kurly.com/docs/api/물류대행
---

# Send outbound orders to a Kurly fulfillment centre

Use this when Kurly holds the stock (물류대행) and you are instructing it to ship.

## 1. Authenticate

Call `issueToken` (`POST /auth/token`), then send `Authorization: Bearer {AccessToken}`.

## 2. Set up the goods master

- `saveSku` (`POST /v1/goods/skus`) registers SKUs; `updateSku` (`PUT`) amends them;
  `findSkuList` (`GET`) reads them back.
- **SKU registration returns `200 OK` even on partial success**, with a per-item result list. A
  `200` does not mean every SKU landed. Always walk the response body and reconcile per item — this
  is the most common integration bug on this endpoint.
- `saveSalesChannelMapping` (`POST /v1/goods/sales-channel-mapping`) maps your sales-channel product
  identifiers onto Kurly SKUs; `updateSalesChannelMapping` and `findSalesChannelMappingList` maintain
  and read them.

## 3. Check stock before promising it

`findInventories` (`GET /v1/inventories`) returns on-hand stock per SKU.

For movement history use the ledgers: `findLedgers` (`GET /v1/ledgers`) per SKU, or `findLotLedgers`
(`GET /v1/lot-ledgers`) per lot, both over a date range.

## 4. Place the order — use V2

Use `createOrderV2` (`POST /api/fulfillment/v2/orders`) for one order, or `createOrdersBulkV2`
(`POST /api/fulfillment/v2/orders/bulk`) for many.

- The V1 order APIs were **removed** in docs v1.3.6. If you are still calling them, migrate.
- Bulk caps at **20 orders per call**. Chunk larger batches.
- Bulk responses carry **per-item error detail (code and message) for the failures** — the batch does
  not fail as a unit. Process the response per item and only retry the entries that failed.

## 5. Read the fulfillment plan

`findOperationPlan` (`POST /api/fulfillment/v1/operation-plans`) or `findOperationPlansBulk`
(`POST /api/fulfillment/v1/operation-plans/bulk`) tell you how and when Kurly intends to fulfil the
order. Note these are `POST` lookups despite being reads.

## 6. Look up orders

`findOrders` (`GET /api/fulfillment/v1/orders`) takes query parameters `pageNumber`, `pageSize` and
`clientOrderCodes` — it moved from a request-body style in docs v1.3.0, so older integrations that
POST a body are broken.

The response is grouped into order, orderer, receiver, outbound and delivery sections.

## 7. Cancel

`cancelOrderV2` (`PUT /api/fulfillment/v2/orders/cancel`) or `cancelOrdersBulkV2`
(`PUT /api/fulfillment/v2/orders/bulk/cancel`), also capped at **20** with per-item error detail.

## Conventions that bite

- **Partial success is normal here.** SKU registration and bulk order register/cancel all return
  success envelopes containing per-item failures. Never infer success from the HTTP status alone.
- **`429` applies to every KLS API** with no published quota and no rate-limit headers — back off.
- **No webhooks.** Kurly does not push order or inventory state; poll.
- Error envelope is `{ code, message }`.
