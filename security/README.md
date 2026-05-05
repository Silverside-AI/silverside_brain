# security/

Security incidents, postmortems, and the threat-model log for the platform.

The platform's threat model assumes prompt injection will eventually succeed. The defense is to **bound the blast radius**, not prevent the attack. This folder is where we record what happened when bounds were tested.

## Layout

```
security/
└── incidents/
    └── YYYY-MM-DD-{slug}.md     ← one file per incident
```

## Incident frontmatter (required)

```yaml
---
incident_id: 2026-04-25-canary-leak-acme
date_detected: 2026-04-25
severity: low | medium | high | critical
category: prompt_injection | brain_poisoning | malicious_skill_pr | excessive_agency | credential_exposure | other
detected_by: canary_check | user_report | audit | other
projects_affected: [acme-corp]
skills_affected: [draft_response]
runs_affected: [2026-04-25-1530-abc123]
status: open | mitigated | resolved
owner: @handle
---
```

## Body

- **Summary** — one paragraph, plain language.
- **Timeline** — what happened, in order, with timestamps.
- **Detection** — how we found out. Which control caught it. Which control should have caught it earlier.
- **Mitigation** — what we did to stop the bleeding.
- **Root cause** — the actual reason, not the proximate trigger.
- **Action items** — linked PRs / issues. Each must be assigned and dated.

## Rules

- Every canary leak, denied tool call that should have been allowed (or vice versa), approval-bypass attempt, or cost-cap-hit pattern gets an incident file.
- No agent ever writes here. Humans only.
