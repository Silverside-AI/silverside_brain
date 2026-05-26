---
name: test-echo
version: 0.1.0
description: Test-only skill that echoes the user's request with a deterministic marker.
allowed_tools: []
model_tier: fast
default_model: claude-haiku
fallback_model: claude-haiku
escalation_model: claude-sonnet
risk_tier: READ_ONLY
eval_pass_rate: null
owner: @platform-test
testing_only: true
---

# Test Echo Skill

Use this skill only when the user asks to test skill loading, echo behavior, or the
Sandbox Skills Orchestrator.

## Required Output

When this skill is used, respond with:

1. The exact marker `TEST_ECHO_SKILL_USED`.
2. A one-sentence confirmation that the skill was loaded from the skills repository.
3. A concise echo of the user's request.

Do not call external tools for this skill. Keep the response short.
