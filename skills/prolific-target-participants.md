---
name: Target participants with filters and filter sets
description: Discover eligibility filters, count the eligible pool, and build reusable filter sets to target the right participants.
api: openapi/prolific-openapi-original.yml
operations: [get-filters, get-eligible-count, create-filter-set, get-participant-groups]
---

# Target participants with filters and filter sets

Authenticate with `Authorization: Token <your token>` against `https://api.prolific.com`.

## Steps

1. **Browse available filters** — `get-filters` (`GET /api/v1/filters/`) to see the demographic/behavioral filters you can screen on.
2. **Count the eligible pool** — `get-eligible-count` (`POST /api/v1/eligibility-count/`) with your chosen filters to confirm enough participants exist before you launch (avoids stalled recruitment).
3. **Save a reusable filter set** — `create-filter-set` (`POST /api/v1/filter-sets/`) so the same targeting can be reused/locked across studies.
4. **Optionally target a known group** — `get-participant-groups` (`GET /api/v1/participant-groups/`) to reuse curated participant groups (e.g. for longitudinal follow-ups).

## Rules

- Always run the eligibility count before publishing; over-narrow filters lead to slow or failed recruitment.
- Attach the `filter_set_id` or filters when creating/updating the study (see `prolific-launch-study.md`).
