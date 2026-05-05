# workflows/

Declarative compositions of skills. One folder per workflow.

## Layout

```
workflows/
└── {workflow_name}/
    └── WORKFLOW.md         ← description + YAML composition block
```

## WORKFLOW.md shape

```yaml
---
name: {workflow_name}
version: 0.1.0
description: one-line summary
trigger: manual              # manual | scheduled | event
---

## Steps

steps:
  - id: classify
    skill: classify_intent
  - id: research
    skill: search_knowledge
    parallel:
      - skill: pull_brand_context
      - skill: pull_recent_runs
  - id: draft
    skill: draft_response
    depends_on: [research]
  - id: review
    skill: human_review
    when: "{{ steps.draft.confidence < 0.8 }}"
```

## Rules

- Workflows compose skills. They do not contain prompts of their own.
- Tool risk in a workflow is the **union** of the risk tiers of its skills. Approval gates apply at the workflow level too.
- Workflow chaining is a known excessive-agency vector — keep workflows shallow and reviewed.
