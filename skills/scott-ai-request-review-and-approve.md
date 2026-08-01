---
name: Request review on a snapshot and collect approvals
description: Open review requests on a Scott planspace snapshot, then read and submit review decisions.
api: openapi/scott-ai-openapi-original.json
operations: [create_review_requests, list_workspace_review_requests, submit_review_decision, get_workspace_reviews_summary]
---

# Request review on a snapshot and collect approvals

Base URL `https://api.tryscott.ai`. Bearer-JWT auth.

## Steps

1. **Open review requests** — `create_review_requests`
   (`POST /agent-loop/workspaces/{workspace_id}/review-requests`) targeting the
   committed snapshot and the reviewers.
2. **Track outstanding reviews** — `list_workspace_review_requests`
   (`GET /agent-loop/workspaces/{workspace_id}/review-requests`) or the rollup
   `get_workspace_reviews_summary`
   (`GET /agent-loop/workspaces/{workspace_id}/reviews/summary`).
3. **Submit a decision** — `submit_review_decision`
   (`POST /agent-loop/review-requests/{request_id}/decisions`) to approve or
   request changes.

## Rules
- Do not hand off to execution until the reviews summary shows approval.
- A reviewer may retract via `retract_review_decision`; re-check the summary.
- Handle `422` validation errors on malformed request bodies.
