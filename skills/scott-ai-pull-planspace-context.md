---
name: Pull reviewed planspace context into execution
description: Load a reviewed Scott planspace back into a fresh coding-agent thread so execution follows the agreed design intent.
api: openapi/scott-ai-openapi-original.json
operations: [list_workspaces, get_workspace_context, get_workspace_state]
---

# Pull reviewed planspace context into execution

Base URL `https://api.tryscott.ai`. Bearer-JWT auth.

## Steps

1. **Find the planspace** — `list_workspaces`
   (`GET /agent-loop/workspaces`) returns the workspaces the user belongs to,
   ordered by most recent turn activity. Pick the target `workspace_id`.
2. **Fetch execution context** — `get_workspace_context`
   (`GET /agent-loop/workspaces/{workspace_id}/context`) to pull the reviewed
   design intent the agent should build against.
3. **Check current state** — `get_workspace_state`
   (`GET /agent-loop/workspaces/{workspace_id}/state`) to confirm review status
   before starting execution.

## Rules
- Only begin execution against context whose reviews are resolved
  (cross-check with the review skill).
- Handle `422` validation errors on bad `workspace_id`.
