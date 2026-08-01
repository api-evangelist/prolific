---
name: Subscribe to and verify Prolific webhooks
description: Discover event types, register a signing secret and subscription, confirm it, and verify inbound events.
api: openapi/prolific-openapi-original.yml
operations: [get-event-types, create-secret, create-subscription, confirm-subscription, get-events]
---

# Subscribe to and verify Prolific webhooks

Authenticate with `Authorization: Token <your token>` against `https://api.prolific.com`.

## Steps

1. **Discover event types** — `get-event-types` (`GET /api/v1/hooks/event-types/`). Event names follow a `noun.verb` pattern (e.g. `study.status.change`). This runtime list is the source of truth.
2. **Create a signing secret** — `create-secret` (`POST /api/v1/hooks/secrets/`). You'll use it to verify inbound signatures.
3. **Create the subscription** — `create-subscription` (`POST /api/v1/hooks/subscriptions/`) with your callback URL and the event types to receive.
4. **Confirm it** — `confirm-subscription` (`POST /api/v1/hooks/subscriptions/{subscription_id}/`) to complete the challenge so events start flowing.
5. **Audit deliveries** — `get-events` (`GET /api/v1/hooks/subscriptions/{subscription_id}/events/`) to inspect what was sent.

## Verifying inbound events

- Compute HMAC-SHA256 over `X-Prolific-Request-Timestamp` + raw body with your secret, base64-encode it, and constant-time compare against `X-Prolific-Request-Signature`.
- Deduplicate on the `X-Event-ID` header — delivery is at-least-once, so the same event may arrive more than once (idempotent handling).
- Order events by `X-Prolific-Request-Timestamp`. Persistent delivery failures disable the subscription; keep your endpoint returning 2xx quickly.
