# evals/

Golden-set inputs and rubrics for every skill. One folder per skill, mirroring `skills/`.

## Layout

```
evals/
└── {skill_name}/
    ├── golden_set.jsonl    ← inputs + expected outputs / pass criteria
    ├── rubric.md           ← grader prompt or scoring criteria
    └── results/            ← dated run outputs (gitignored if large; index here)
```

## Rules

- A skill cannot ship without an eval folder. CI rejects PRs that add a `SKILL.md` without a paired golden set.
- A PR cannot merge if its eval pass rate regresses vs. `main`.
- Evals run via GitHub Actions on every PR that touches `skills/**` or `evals/**`.
- Keep golden sets small and meaningful. ~10–30 cases per skill is the right order of magnitude for MVP.
