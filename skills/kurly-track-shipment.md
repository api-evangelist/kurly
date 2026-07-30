---
name: Track a Kurly Nextmile shipment
description: >-
  Look up delivery progress for a waybill and interpret Kurly's tracking status vocabulary, including
  which states are terminal and why stages go missing.
api: openapi/kurly-kls-delivery-tracking-openapi.yml
operations:
  - issueToken
  - findDeliveryTracking
  - findInvoicePrintData
generated: '2026-07-19'
method: generated
source: https://developers.kurly.com/docs/guide/배송추적/배송-추적-API-연동-가이드
---

# Track a Kurly Nextmile shipment

Shared across both KLS services — usable whether Kurly fulfils from its own centre (물류대행) or only
delivers (배송대행).

## 1. Authenticate

Call `issueToken` (`POST /auth/token`), then send `Authorization: Bearer {AccessToken}`.

## 2. Get the waybill number

For delivery-agency orders, `findInvoicePrintData` (`POST /v1/invoices/print-data`) returns the
`invoiceNumber`. That number is the tracking key.

## 3. Query tracking

Call `findDeliveryTracking`
(`GET /v1/couriers/{courier}/trackings/{invoiceNumber}`) with the carrier and the waybill number.
Kurly documents Nextmile (NM), CJ Logistics (CJ대한통운) and Lotte (롯데택배) as carriers.

The response carries `trackingEvents` — an accumulating event list, not a single status.

## 4. Interpret the status vocabulary

Events accumulate in this order:

| code | label | meaning |
|---|---|---|
| `PREPARE_FOR_DELIVERY` | 배송접수 | Accepted, awaiting pickup |
| `PICK_UP_COMPLETE` | 집화완료 | Driver has collected from the sender |
| `ARRIVAL_AT_THE_PICK_UP_OFFICE` | 집화영업소도착 | Arrived at pickup branch |
| `DEPART_FROM_THE_MAIN_TERMINAL` | 간선TML출발 | Departed toward line-haul terminal |
| `ARRIVAL_AT_THE_MAIN_TERMINAL` | 간선TML도착 | Arrived at line-haul terminal |
| `ARRIVAL_AT_DELIVERY_OFFICE` | 배송영업소도착 | Arrived at final delivery branch |
| `START_DELIVERY` | 배송출발 | Out for delivery |
| `DELIVERY_COMPLETED` | 배송완료 | **Terminal** — delivered |
| `DELIVERY_FAILED` | 미배송 | **Terminal** — delivery failed |

**Do not require every stage.** Kurly states explicitly that not all stages always appear and
intermediate ones can be skipped depending on the shipment. A state machine that demands the full
sequence will stall on perfectly normal deliveries.

Derive current status from the **latest** event, and treat only `DELIVERY_COMPLETED` and
`DELIVERY_FAILED` as terminal — stop polling on those.

## 5. Poll, because nothing is pushed

Kurly does not push tracking updates to external systems; there are no webhooks. Poll this endpoint
on a schedule and back off on `429`, which applies across all KLS APIs with no published quota.

A reasonable pattern: poll open shipments on a fixed interval, drop each one from the polling set as
soon as it reaches a terminal state.
