---
name: Subscribe to Hubflo webhook events
description: Register a webhook target for chosen event types, activate it, and inspect deliveries.
api: openapi/hubflo-openapi-original.yml
operations:
  - "POST /api/v2/webhooks"
  - "POST /api/v2/webhooks/{webhook_target_id}/activation"
  - "GET /api/v2/webhooks/{webhook_target_id}/webhook_deliveries"
  - "DELETE /api/v2/webhooks/{webhook_target_id}/activation"
  - "DELETE /api/v2/webhooks/{id}"
---

# Subscribe to Hubflo webhook events

Use this skill to receive real-time notifications when Hubflo resources change.

## Auth
- `Authorization: Bearer <token>`, `Content-Type: application/json`.
- Base URL: `https://app.hubflo.com/api/v2`.

## Steps
1. **Create the webhook** — `POST /api/v2/webhooks` with body `{ "url": "<https endpoint>",
   "event_names": ["contact_created", ...] }`. `url` is required. Capture the returned target id.
2. **Activate** — `POST /api/v2/webhooks/{webhook_target_id}/activation`.
3. **Inspect deliveries** — `GET /api/v2/webhooks/{webhook_target_id}/webhook_deliveries` to see
   delivery attempts (paginated with `page`/`per_page`).
4. **Deactivate / delete** — `DELETE .../activation` to pause, or `DELETE /api/v2/webhooks/{id}`.

## Subscribable events
`authenticated_contact_added_to_workspace`, `comment_created`, `company_created`,
`company_updated`, `contact_created`, `contact_invited`, `contact_signed_up`, `contact_updated`,
`file_added_to_task`, `file_uploaded`, `file_uploaded_by_contact`, `folder_created`,
`form_assignment_created`, `form_submission_completed`, `invoice_created`, `invoice_issued`,
`invoice_overdue`, `invoice_paid`, `message_sent`, `message_sent_by_contact`,
`project_stage_created`, `project_stage_updated`, `seal_submission_completed`,
`seal_submission_created`, `task_completed`, `task_completed_by_contact`, `task_created`,
`task_created_by_contact`, `task_overdue`.

## Rules
- Your endpoint must be publicly reachable over HTTPS and respond quickly.
- Errors return `{status, title, message}`; watch for 422 on invalid event names.
- Rate limit: 1000 requests/minute.
