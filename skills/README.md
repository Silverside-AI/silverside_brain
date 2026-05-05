# skills/

Versioned, eval-gated capabilities. One folder per skill.

## Layout

```
skills/
└── {skill_name}/
    ├── SKILL.md            ← prompt, instructions, examples, frontmatter
    └── CHANGELOG.md        ← optional, human-readable version history
```

## SKILL.md frontmatter (required)

```yaml
---
name: {skill_name}
version: 0.1.0
description: one-line summary the router can use
allowed_tools:               # per-skill tool allowlist (security baseline)
  - search_knowledge
  - read_knowledge_file
  - write_learning
model_tier: standard         # standard | fast | deep
default_model: claude-sonnet
fallback_model: claude-haiku
escalation_model: claude-opus
risk_tier: INTERNAL_WRITE    # READ_ONLY | INTERNAL_WRITE | EXTERNAL_WRITE | IRREVERSIBLE | SPEND
eval_pass_rate: null         # populated by CI from evals/{skill_name}/
owner: @handle
---
```

## Rules

- **No agent-merged skills, ever.** All skill PRs are reviewed and merged by humans.
- A skill cannot ship without a paired golden set in `evals/{skill_name}/`.
- A PR cannot merge if it regresses the eval pass rate.
- The `allowed_tools` list is the security boundary. Keep it minimal.
