# glossary/

Shared terminology. The agent and the team should mean the same thing by the same word.

## Layout

```
glossary/
├── README.md
└── {term}.md               ← optional, for terms that need long entries
```

For most terms, append to `glossary.md` (one entry per term). Promote a term to its own file only when it needs >~30 lines of explanation.

## Entry shape

```markdown
## term

**Type**: client | internal | concept | acronym
**Aliases**: alt-spelling, abbreviation
**Owner**: @handle

Short definition. What it means here, specifically. Avoid restating dictionary definitions.

### Examples

- ...

### See also

- [related-term](#related-term)
```

## Why this exists

Models hallucinate when the prompt and the brain disagree on what a word means. The glossary is the tiebreaker.
