---
name: Create a company and add a contact in Hubflo
description: Create a company record, then create a contact linked to it, using the Hubflo API.
api: openapi/hubflo-openapi-original.yml
operations:
  - "POST /api/v2/companies"
  - "GET /api/v2/companies/{id}"
  - "POST /api/v2/contacts"
  - "GET /api/v2/contacts/{id}"
---

# Create a company and add a contact in Hubflo

Use this skill to onboard a new client company and its primary contact.

## Auth
- Send `Authorization: Bearer <token>` on every request (org API key from Settings > Integrations).
- Send `Content-Type: application/json`.
- Base URL: `https://app.hubflo.com/api/v2`.

## Steps
1. **Create the company** — `POST /api/v2/companies` with the company fields in the JSON body.
   Up to 100 tags may be supplied. To set a custom field, add its `serialization_name` key
   with the desired value. Capture the returned company `id` (UUID).
2. **(Optional) Verify** — `GET /api/v2/companies/{id}` to confirm the record.
3. **Create the contact** — `POST /api/v2/contacts` with the contact fields; associate it with
   the company using the documented company reference field. Custom fields work the same way.
4. **(Optional) Verify** — `GET /api/v2/contacts/{id}`.

## Rules
- IDs are UUIDs; dates are `YYYY-MM-DD`, date-times are ISO 8601 UTC.
- Errors return `{status, title, message}` — handle 401 (bad token), 422 (validation), 429 (rate limit).
- Rate limit: 1000 requests/minute; a breach returns 429 and blocks for one minute.
- No idempotency key is supported — guard against duplicate creates on retry.
