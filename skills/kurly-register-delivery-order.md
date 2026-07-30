---
name: Register a Kurly delivery-agency order safely
description: >-
  Hand an order from your own warehouse to Kurly's delivery network (배송대행), using requestKey so a
  network retry can never double-register the shipment, then confirm it and get the waybill.
api: openapi/kurly-kls-delivery-agency-openapi.yml
operations:
  - issueToken
  - createDeliveryOrder
  - findDeliveryOrdersByRequestKeys
  - findInvoicePrintData
  - findDeliveryPolicies
generated: '2026-07-19'
method: generated
source: https://developers.kurly.com/docs/guide/배송대행/배송대행-연동-가이드
---

# Register a Kurly delivery-agency order

Use this when the shipper stores and picks goods themselves and only wants Kurly to deliver.

## Before you start

- KLS is **not self-service**. The shipper must already hold a `clientId` / `secretKey` pair from
  Kurly, and the calling host's egress IP must be on Kurly's allowlist. If you get a network-level
  rejection, the IP is the first thing to check — not the credentials.
- All documentation is Korean; field labels below use Kurly's own terms.

## 1. Get an access token

Call `issueToken` (`POST /auth/token`) with the `clientId` and `secretKey`.

Send the result on every subsequent call as `Authorization: Bearer {AccessToken}`.
A `400` here means the credential pair or the calling IP is wrong — do not retry in a loop.

## 2. Pick the delivery service

Call `findDeliveryPolicies` (`POST /api/delivery-agency/v1/delivery-policies`) to see which delivery
services the shipper is contracted for. Do this before registering, not after a failure.

## 3. Generate a requestKey — this is the important step

**Generate `requestKey` yourself, in your own system, one per order, before you call Kurly.**

- Max 50 characters, unique within the shipper.
- A value that has been used before can never be reused — including values from cancelled orders.
- If you omit it, Kurly generates one, **and you lose all duplicate protection**.
- If a call times out or the connection drops, **retry with the exact same `requestKey`**. Kurly
  registers the order once. Generating a fresh key on retry is the one mistake that produces
  duplicate shipments.

Store the `requestKey` against your own order record before sending the request, so a crash between
send and response is recoverable.

## 4. Register the order

Call `createDeliveryOrder` (`POST /v1/orders`).

Policy constraints Kurly enforces:

- **Delivery date** must be within **D+1**. Nothing further out is accepted.
- **Grouping** defaults to `NO_GROUPING`. Use `BY_RECEIVER` to consolidate one receiver's orders
  into a single box — Kurly treats orders as the same receiver only when delivery date, receiver
  name, address, contact, carrier and delivery type all match.
- **Delivery type** (`DAWN` / `DAY`) is resolved automatically with `DAWN` preferred when you do not
  specify it. Specify it explicitly if you care which one you get.
- `clientOrderCode` (your sales-channel order number) is **optional and may be duplicated**. It is a
  reference field, not an identifier. Never treat it as a key.

## 5. Confirm what was registered

Call `findDeliveryOrdersByRequestKeys` (`GET /v1/orders/by-request-keys`) with up to **50** keys per
call. This is the authoritative check after an ambiguous retry: if the order is there, the retry
succeeded and you must not send it again.

Use `findDeliveryOrdersByClientOrderCode` (`GET /v1/orders`) only for human-facing lookup by
sales-channel number, and expect more than one result.

## 6. Get the waybill

Call `findInvoicePrintData` (`POST /v1/invoices/print-data`) in bulk when the shipper prints their
own waybills. Keep the returned `invoiceNumber` — it is the key for tracking.

## Cancelling

Call `cancelDeliveryOrderByRequestKey` (`POST /v1/orders/cancel/by-request-key`). Orders already
progressed to outbound or in-transit cannot be cancelled. Cancelling an order that is **already
cancelled returns success** — it is idempotent, so a retried cancel is safe.

## Errors and pacing

- Error envelope is `{ code, message }`. Delivery-agency codes carry a `DA` prefix
  (`DA60400`, `DA60404`, `DA60500`). If you are matching bare numeric codes from before docs v1.3.8,
  your mapping is stale.
- `429` is returned across all KLS APIs with no published quota and no rate-limit headers. Back off
  on `429`.
- Kurly does **not** push events to you. There are no webhooks. Poll for state changes.
