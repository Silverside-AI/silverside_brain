# roles/

Pre-configured agent bundles. The user picks a role; the agent is ephemeral.

A role = `(skills + system prompt + tool allowlist + default model)`.

## Layout

```
roles/
└── {role_name}/
    └── ROLE.md             ← system prompt + bundle config in frontmatter
```

## ROLE.md frontmatter

```yaml
---
name: {role_name}
version: 0.1.0
description: who this role is and when to pick it
skills:                      # skills available to this role
  - search_knowledge
  - draft_response
allowed_tools:               # union must be a subset of the skills' tool allowlists
  - search_knowledge
  - read_knowledge_file
  - write_learning
default_model: claude-sonnet
fallback_model: claude-haiku
owner: @handle
---
```

## Notes

- Roles are how non-technical users start a task. They should read like a job title.
- Keep role count small. Many skills, few roles.
