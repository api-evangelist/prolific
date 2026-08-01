---
name: Review submissions and pay bonuses
description: List a study's submissions, approve or reject them, and pay optional bonuses.
api: openapi/prolific-openapi-original.yml
operations: [get-study-submissions, get-submission, transition-submission, bulk-approve-submissions, create-bonus-payments, pay-bonus-payments]
---

# Review submissions and pay bonuses

Authenticate with `Authorization: Token <your token>` against `https://api.prolific.com`.

## Steps

1. **List submissions for the study** — `get-study-submissions` (`GET /api/v1/studies/{id}/submissions/`). Page with `page`/`page_size`; filter by status (awaiting_review, completed, etc.).
2. **Inspect one** — `get-submission` (`GET /api/v1/submissions/{id}/`) to review the participant's response before deciding.
3. **Approve or reject** — `transition-submission` (`POST /api/v1/submissions/{id}/transition/`) with the approve/reject action. For volume, use `bulk-approve-submissions` (`POST /api/v1/submissions/bulk-approve/`).
4. **Bonus good work (optional)** — set up with `create-bonus-payments` (`POST /api/v1/submissions/bonus-payments/`), then execute with `pay-bonus-payments` (`POST /api/v1/bulk-bonus-payments/{id}/pay/`).

## Rules

- Approving and paying bonuses moves real money — confirm amounts first and check the workspace balance.
- Rejecting a submission has fairness implications for participants; only reject with a documented reason per Prolific policy.
- Handle 409 conflicts (submission already actioned) idempotently — re-fetch state before retrying.
