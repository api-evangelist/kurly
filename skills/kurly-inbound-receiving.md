---
name: Book and track a Kurly inbound receiving request
description: >-
  Register goods inbound to a Kurly fulfillment centre (입고), download the inbound label and
  transaction specification, and monitor receiving status and exception reports.
api: openapi/kurly-kls-fulfillment-openapi.yml
operations:
  - issueToken
  - createReceivingRequest
  - searchReceivingPlans
  - searchReceivingRequests
  - searchReceivingItemStatus
  - searchReceivingReports
  - cancelReceivingRequest
  - printReceivingLabel
  - downloadReceivingSpecification
generated: '2026-07-19'
method: generated
source: https://developers.kurly.com/docs/guide/물류대행/inbound/입고-정책-가이드
---

# Book and track a Kurly inbound receiving request

Use this when Kurly operates the warehouse (물류대행) and the shipper is sending stock in.

## 1. Authenticate

Call `issueToken` (`POST /auth/token`) with `clientId` / `secretKey`, then send
`Authorization: Bearer {AccessToken}`. The calling IP must be allowlisted with Kurly.

## 2. Register the inbound request

Call `createReceivingRequest` (`POST /openapi/v1/receiving-request`).

Send the `X-Idempotency-Key` header (with its companion `X-Timestamp`) and **reuse the same value on
any retry** — this is what stops a timeout from creating two bookings.

Kurly validates these fields individually and returns a specific error per field, so read the error
body rather than just the status:

- expiry date / manufacturing date
- quantity
- remarks
- lot number
- SKU must exist and must belong to the shipper's supplier

Check `searchReceivingPlans` (`POST /openapi/v1/receiving-request/plans/search`) first if you need
to align with an existing inbound plan.

## 3. Understand the cancellation window — it closes fast

The request status flow is `요청완료` (requested) → `확인완료` (confirmed) → work statuses.

- Cancel via `cancelReceivingRequest` (`PATCH /openapi/v1/receiving-request/cancel`) is possible
  **only while the request is still `요청완료`**.
- Once a Kurly operator confirms it (`확인완료`), or it is already `요청취소`, the API cannot cancel it.
- Multi-cancel is **all-or-nothing**: if a single item in the list is already confirmed, the entire
  request is rejected. Filter the list by status before calling, or expect a whole-batch failure.

Kurly also validates the cancel reason — missing or over-length reasons are rejected, as are
cancels outside the permitted time window.

## 4. Get the paperwork

- `printReceivingLabel` (`GET /openapi/v1/receiving-request/receiving-label/print`) returns the
  inbound label PDF for a receiving-request code. Omit the SKU code to get labels for every product
  on the request. It only works while the request is in a downloadable state
  (확인대기 / 입고대기 / 입고중) — call it before receiving completes.
- `downloadReceivingSpecification` (`GET /openapi/v1/receiving-request/specification`) returns the
  transaction specification (거래명세서).

## 5. Monitor receiving

Status is tracked at two levels; read both, they do not mean the same thing.

- **Request level** via `searchReceivingRequests` (`POST /openapi/v1/receiving-request/search`):
  `입고중` (inspection started) → `입고완료` (all items inspected) or `강제종료` (force-closed after an
  exception such as missing goods).
- **Item level** via `searchReceivingItemStatus`
  (`POST /openapi/v1/receiving-request/items/status/search`):
  `입고대기` → `입고중` → `입고완료` per SKU.

There are no webhooks — poll these endpoints. Pace polling against the 429 rate limit.

## 6. Handle exception reports

Call `searchReceivingReports` (`POST /openapi/v1/receiving-request/reports/search`).

- Reports are issued **per issue type**, so one inbound can produce many rows — 10 products with two
  issue types each yields 20 report entries.
- **A report can be issued after `입고완료`.** Report status runs independently of work status, so do
  not stop polling reports when receiving completes.
- Report statuses: `리포트 미발행` (none), `리포트 발행` (issued), `리포트 처리완료` (resolved, final),
  `리포트 발행취소` (voided by Kurly).
- Issued reports are **immutable**. A correction appears as a void plus a new report, not an edit —
  reconcile on report identity, not by diffing content.

## Errors

Envelope is `{ code, message }`. Inbound validation errors identify the failing field. `429` applies
across all KLS APIs with no published quota and no rate-limit headers.
