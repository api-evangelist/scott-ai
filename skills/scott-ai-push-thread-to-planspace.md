---
name: Push an agent thread into a Scott planspace
description: Turn the current coding-agent conversation into a shareable Scott planspace so teammates can review the design intent.
api: openapi/scott-ai-openapi-original.json
operations: [create_workspace, create_turn, create_snapshot, commit_snapshot]
---

# Push an agent thread into a Scott planspace

Base URL `https://api.tryscott.ai`. Authenticate with an HTTP bearer JWT
(`Authorization: Bearer <jwt>`); obtain it via `POST /auth/oauth/exchange`
(Stytch OAuth) or a workspace agent token.

## Steps

1. **Create or upsert the planspace** — `create_workspace`
   (`POST /agent-loop/workspaces`). For plugin pushes include a `scott_push`
   payload: the call is idempotent on `(captured_by_user_id, source_kind,
   source_id)`, so re-pushing the same thread returns the same planspace and
   appends new turns rather than duplicating it. Do **not** invent a new
   workspace per push.
2. **Append the thread turns** — `create_turn`
   (`POST /agent-loop/workspaces/{workspace_id}/turns`) to record the agent
   activity. `scott_push` turns are appended past the current chain tip.
3. **Snapshot the design** — `create_snapshot`
   (`POST /agent-loop/workspaces/{workspace_id}/snapshots`) to capture the
   reviewable state.
4. **Commit the snapshot** — `commit_snapshot`
   (`PATCH /agent-loop/workspaces/{workspace_id}/snapshots/{snapshot_id}/commit`)
   once the push is complete.

## Rules
- Idempotency is by push source identity, not an `Idempotency-Key` header.
- Handle `422` validation errors (`detail[]` array) before retrying.
