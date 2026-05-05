# projects/

Per-project knowledge packs. Everything an agent needs to do good work for a specific client or internal initiative lives here.

## Layout

```
projects/
└── {project_id}/
    ├── PROJECT.md          ← overview, stakeholders, north-star outcomes
    ├── context/            ← brand voice, references, exemplars, do/don't notes
    ├── decisions/          ← dated decision records (why we chose X over Y)
    └── learnings/          ← durable, topic-keyed memory written by write_learning
```

## Conventions

- `{project_id}` is `kebab-case` and stable for the life of the engagement.
- `PROJECT.md` has YAML frontmatter (`project_id`, `status`, `owner`, `created`, `tags`).
- Project data isolation is enforced at the Postgres query level (see security plan). Cross-project references are explicit and reviewed.

## How `learnings/` works (important — read before designing `write_learning`)

`learnings/` is **durable memory**, not a transcript log. Three rules:

1. **Topic-keyed, not date-keyed.** Filenames are slugs like `warm-tones-preference.md`, `air-export-resolution-quirk.md`, `mood-board-references.md` — not `2026-04-25-some-conversation.md`.
2. **Append, don't duplicate.** When the consolidator identifies a learning that fits an existing topic, it **appends a dated section** to that topic's file. A new file is created only when the learning is genuinely a new topic.
3. **Distilled, not raw.** Each entry is the *lesson*, not the back-and-forth that produced it. The conversation that produced it lives in Redis (active) or — if it crossed an audit trigger — in `observability/runs/`.

### Recommended `learnings/` file shape

```markdown
---
topic: warm-tones-preference
project_id: acme-corp
created: 2026-04-25
last_updated: 2026-05-12
related_runs: [2026-04-25-1530-abc123, 2026-05-12-0901-def456]
tags: [color, photography, brand]
---

# Warm tones preference

One-line summary of the learning.

## 2026-04-25 — initial discovery
What was learned and from which run. Keep terse.

## 2026-05-12 — refined
Updated nuance from a later run.
```

### What goes here vs. `observability/runs/`

| Trigger | Destination |
|---|---|
| Client preference identified, novel-params-that-worked, human correction, brand voice nuance | `projects/{id}/learnings/{topic}.md` (append or create) |
| Workflow completed, delivery happened, failure-with-workaround, audit event | `observability/runs/YYYY-MM-DD/{run_id}.md` |

Some runs hit both. Most runs hit neither (the DB row in `agent_runs` is the unconditional record). The consolidator decides.

## Lifecycle

- Agents read project context (`PROJECT.md`, `context/`, `decisions/`, relevant `learnings/`) at the start of a run.
- The consolidator step writes back to `learnings/` only when a durable lesson was identified.
- Live conversation state stays in Redis (~1h TTL) and is never persisted here verbatim.
