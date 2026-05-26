---
name: test-recipe
version: 0.1.0
description: Test-only skill that proves sidecar files can guide the response.
allowed_tools: []
model_tier: fast
default_model: claude-haiku
fallback_model: claude-haiku
escalation_model: claude-sonnet
risk_tier: READ_ONLY
eval_pass_rate: null
owner: @platform-test
testing_only: true
sidecars:
  - checklist.md
---

# Test Recipe Skill

Use this skill only when the user asks to test sidecar loading, recipe execution,
or the Sandbox Skills Orchestrator.

## Required Output

When this skill is used:

1. Read `checklist.md` in this skill folder.
2. Respond with the exact marker `TEST_RECIPE_SKILL_USED`.
3. Summarize the checklist as three short bullets.
4. End with `sidecar_status: read`.

Do not call external tools for this skill. Keep the response short.
