---
name: Create and issue a Hubflo invoice
description: Create an invoice, add line items, then issue it to the client.
api: openapi/hubflo-openapi-original.yml
operations:
  - "POST /api/v2/invoices"
  - "POST /api/v2/invoices/{invoice_id}/line-items"
  - "GET /api/v2/invoices/{invoice_id}/line-items"
  - "POST /api/v2/invoices/{invoice_id}/issuances"
  - "GET /api/v2/invoices/{invoice_id}/payment-url"
---

# Create and issue a Hubflo invoice

Use this skill to bill a client end-to-end.

## Auth
- `Authorization: Bearer <token>`, `Content-Type: application/json`.
- Base URL: `https://app.hubflo.com/api/v2`.

## Steps
1. **Create the invoice** — `POST /api/v2/invoices`. Capture the returned invoice `id`.
2. **Add line items** — `POST /api/v2/invoices/{invoice_id}/line-items` for each item;
   verify with `GET /api/v2/invoices/{invoice_id}/line-items`.
3. **Issue it** — `POST /api/v2/invoices/{invoice_id}/issuances` to send the invoice to the client.
4. **Get the payment URL** — `GET /api/v2/invoices/{invoice_id}/payment-url` to share a pay link.

## Related webhook events
Subscribe (see the webhooks skill) to `invoice_created`, `invoice_issued`, `invoice_paid`,
and `invoice_overdue` to track lifecycle changes.

## Rules
- IDs are UUIDs; monetary/date fields follow the documented formats.
- Errors return `{status, title, message}`; handle 422 (validation) and 429 (rate limit).
- No idempotency key — avoid duplicate issuance on retry.
