---
name: Create, cost, and launch a Prolific study
description: Draft a study, check its cost, validate it as a test study, then publish it to recruit real participants.
api: openapi/prolific-openapi-original.yml
operations: [create-study, get-study-cost, create-test-study, publish-study]
---

# Create, cost, and launch a Prolific study

Authenticate every request with `Authorization: Token <your token>` against `https://api.prolific.com`.

> WARNING: publishing a real study spends money. As soon as `publish-study` runs with action `PUBLISH`, participants can start and you are charged. There is no sandbox host — validate with a test study first.

## Steps

1. **Create a draft study** — `create-study` (`POST /api/v1/studies/`). Provide name, description, external study URL, reward, and `total_available_places`. The study is created in an unpublished/draft state.
2. **Check the cost** — `get-study-cost` (`GET /api/v1/studies/{id}/cost/`) to see the total charge (reward + fees) before committing. Confirm the workspace balance covers it (`get-workspace-balance`).
3. **Validate as a test study** — `create-test-study` (`POST /api/v1/studies/{id}/test-study`) and walk the participant experience end-to-end. Fix any issues on the draft with `update-study`.
4. **Publish** — `publish-study` (`POST /api/v1/studies/{id}/transition/`) with body `{"action": "PUBLISH"}`. To schedule instead, use `{"action": "SCHEDULE_PUBLISH", "publish_at": "<ISO 8601>"}`. Pause/stop later with `PAUSE` / `STOP`.

## Rules

- Errors come back as `{ "error": { "status", "error_code", "title", "detail" } }` (see `errors/prolific-problem-types.yml`); a 409 means the study is not in a state that allows the requested transition.
- Never call `publish-study` with `PUBLISH` on behalf of a user without explicit confirmation of the reward and place count.
