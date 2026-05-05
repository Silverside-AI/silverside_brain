# silverside_brain

The brain repository for the Silverside Agent Platform. Markdown source of truth for projects, skills, workflows, roles, evals, run records, SOPs, glossary, and security incidents.

> **Files are the brain. The database is the index. The agent is the librarian.**
>
> Plain markdown is the source of truth. Postgres + pgvector indexes it for search. Agents read context from this repo at the start of work and write a learning summary back at the end.

## Folder map

| Folder | What lives here |
|---|---|
| `projects/` | Per-project knowledge packs — overview, context, decisions, consolidated learnings. Data isolation is enforced at the Postgres query layer. |
| `skills/` | Versioned, eval-gated capabilities. One folder per skill with `SKILL.md` + frontmatter (allowed tools, model tier, risk tier, eval pass rate). |
| `workflows/` | Declarative compositions of skills. One folder per workflow with `WORKFLOW.md`. |
| `roles/` | Pre-configured `(skills + system prompt + tool allowlist + default model)` bundles. The user picks a role; the agent is ephemeral. |
| `evals/` | Golden-set inputs and rubrics, one folder per skill. CI rejects PRs that regress eval pass rate. |
| `observability/runs/` | One markdown file per agent run, organized by `YYYY-MM-DD/`. Postgres holds the index; the markdown is the truth. |
| `sops/` | Standard Operating Procedures — runbooks for recurring operational tasks. |
| `glossary/` | Shared terminology so the agent and the team mean the same thing by the same word. |
| `security/incidents/` | Postmortems for prompt injection, brain poisoning, malicious skill PRs, excessive agency, credential exposure, and other security events. |
| `models.yaml` | Declarative model registry consumed by the tool layer. Adding a model is a PR to this file. |

Each folder has its own `README.md` describing conventions, frontmatter, and rules in detail.

## Non-negotiables

1. **No agent-merged content, ever.** Humans review and merge every PR. Agents may open PRs (consolidated learnings, proposed skill tweaks); humans always approve.
2. **Branch protection on `main`.** Required reviews, required eval CI, no force-push.
3. **Skills cannot ship without evals.** A `SKILL.md` PR without a paired `evals/{skill_name}/` golden set fails CI.
4. **No regression merges.** A PR that lowers a skill's eval pass rate vs. `main` cannot merge.
5. **Per-skill tool allowlists are the security boundary.** Keep them minimal. Risk tier (`READ_ONLY` → `SPEND`) must be set in frontmatter.
6. **Run records are mandatory.** Every meaningful agent run writes to `observability/runs/`, including failures.
7. **Security incidents are written by humans only.** Canary leaks, denied-tool anomalies, and approval-bypass attempts get a file in `security/incidents/`.

## Suggested CODEOWNERS (set up in GitHub when the repo goes private)

```
*                    @platform-leads
/skills/             @platform-leads @skill-reviewers
/workflows/          @platform-leads
/security/           @platform-leads
/models.yaml         @platform-leads
/observability/      # no owner — agents write here, humans audit
```

## How to use this repo

- **Reading**: agents call `read_knowledge_file(path)` and `search_knowledge(query, project_id)`. Humans browse it like any markdown wiki.
- **Writing**: agents call `write_learning(...)` which opens a PR (or appends to a per-run record) — never a direct commit to `main`.
- **Adding a skill**: open a PR with `skills/{name}/SKILL.md` + `evals/{name}/golden_set.jsonl` + `evals/{name}/rubric.md`. CI runs the eval. Humans review. Merge.
- **Adding a model**: edit `models.yaml`. Done. Capability-shaped tools (`generate_image`, etc.) pick it up automatically.

## Pointers

- Mission and overall architecture: `../mission.md`
- Architecture deep-dive: `../plan_md/silverside-platform-build-reference.md`
- MVP scope: `../plan_md/silverside-brain-mvp_1.md`
- Security model and phased controls: `../plan_md/silverside-security-implementation.md`
