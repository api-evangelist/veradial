---
name: veradial-subscribe-to-receptionist-events
description: Subscribe a public HTTPS endpoint to VeraDial receptionist events (call.screened, voicemail.received, sms.reply_sent, appointment.booked) through the Zapier Integration API REST Hooks, preview samples, and unsubscribe.
api: openapi/_ae-authored/veradial-openapi-generated.yml
operations: [get_me, get_lines, get_samples, post_hooks, delete_hooks_id]
generated: '2026-09-23'
method: generated
source: https://veradial.com/help/zapier-integration-api
---

# Subscribe to VeraDial receptionist events

Contract generated from VeraDial's documentation (not a provider-published OpenAPI). Base URL
`https://api.veradial.com/api/v1/integrations/zapier`; send `Authorization: Bearer vdk_...` (a key from
https://app.veradial.com/dashboard/integrations, Vera Plus required). Never put the key in a query string or URL.

1. `get_me` - confirm the key works; returns `{ id, name }`.
2. `get_lines` - list selectable lines; use `id` (not the phone number) as the subscription identity, or null for All lines.
3. `get_samples` with `kind` (and optional `lineId`) - preview up to three recent events in the live envelope shape.
4. `post_hooks` with `{ "targetUrl", "kind", "lineId" }` - returns `201 { id }`. Repeating the same kind and targetUrl reactivates and returns the same id (safe to retry).
5. `delete_hooks_id` - unsubscribe; idempotent.

Rules: delivery is at least once - store the envelope `id` before any non-repeatable action. Handle 403
INTEGRATIONS_NOT_ENTITLED, 404 LINE_NOT_FOUND, 409 ZAPIER_SUBSCRIPTION_LIMIT_REACHED (25 per account) and 400 for an
unsafe target URL (see ../errors/veradial-error-codes.yml). A 410 from your receiver deactivates the subscription.
