# sops/

Standard Operating Procedures. Human-and-agent-readable runbooks for recurring operational tasks.

## Layout

```
sops/
└── {sop_name}.md           ← one SOP per file, kebab-case filename
```

## Conventions

- SOPs describe **how we do a thing**, not **what a thing is** (that's the glossary).
- Frontmatter: `title`, `owner`, `last_reviewed`, `applies_to` (skills, roles, or projects).
- Keep them tight. If an SOP grows past ~200 lines, it probably wants to be split or promoted to a skill.
- Cite SOPs from skills via `read_knowledge_file("sops/{name}.md")`.
