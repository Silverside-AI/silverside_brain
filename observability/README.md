# observability/

Operational audit log. The **selective** subset of agent runs that need a durable, dated, immutable record beyond their Postgres row.

> **This is not where learnings live.** Durable, topic-keyed memory goes in `projects/{id}/learnings/`. This folder is the audit trail — what happened, when, who did it, what it cost. The lesson extracted from those runs (if any) is appended to a topic file in `learnings/`. The live conversation is in Redis.

## When a run record is written here

`agent_runs` (Postgres) gets a row for **every** run, unconditional. A markdown file is written here only when at least one of these is true:

- A workflow completed end-to-end.
- `delivery_status` transitioned to `delivered` (the agent called `push_to_miro` or `push_to_air`).
- A skill failed and a workaround was discovered.
- A human corrected the agent.
- Novel parameters were used and they worked.

Pure compliance / "client preference identified" learnings without an operational trigger go to `projects/{id}/learnings/` only — not here.

## Layout

```
observability/
└── runs/
    └── YYYY-MM-DD/
        └── {run_id}.md         ← markdown with YAML frontmatter
```

## Run record frontmatter (required)

```yaml
---
run_id: 2026-04-25-1530-abc123
parent_run_id: null              # set if this is a sub-agent run
project_id: acme-corp
skill: draft_response
workflow: null
role: client_responder
user: @handle
channel: slack                   # slack | web | api | cron
trigger: manual
status: completed                # completed | failed | denied | awaiting_approval
started_at: 2026-04-25T15:30:00Z
ended_at: 2026-04-25T15:32:14Z
model:
  primary: claude-sonnet
  fallback_used: false
cost_usd: 0.0421
tokens:
  input: 12340
  output: 882
tools_called:
  - search_knowledge
  - write_learning
# security fields (required)
denied_tools: []
approval_requests: []
canary_leaks: []
cost_cap_hits: []
---
```

## Body

- One-line summary, then a free-form narrative of what happened, what was decided, and what was learned.
- Links to assets, child runs, and PRs as appropriate.
- This is the artifact `consolidate_learnings` reads from to update project `learnings/` and propose skill improvements.

## Rules

- Every run writes a record. No exceptions, even on failure.
- Postgres `run_records` is the index. The markdown here is the source of truth.
