---
name: image-generator
version: 0.1.0
description: Test skill for routing approved image-generation requests through the generator specialist.
allowed_tools:
  - generator_agent
model_tier: standard
default_model: claude-sonnet
risk_tier: SPEND
owner: silverside_brain
sidecars:
  - fal_endpoint_catalog.yaml
  - references/text-to-image.md
---

# Image Generator

Use this skill when the user asks to generate, refine, or prepare an image generation request. This is a routing and instruction skill only: backend Python owns FAL mechanics, validation, submission, polling, and result handling.

## Operating Flow

1. Preserve the user's prompt constraints, including subject, style, aspect ratio, text requirements, exclusions, and any seed or output format preferences.
2. Route generation work through `generator_agent`.
3. For FAL-backed image generation, have the generator specialist inspect or search the target endpoint, then get the endpoint schema before preparing a submission.
4. Submit a paid generation request only after the user has approved the final prompt, endpoint, and relevant cost or spend caveat.
5. Poll for the result after submission and return the generated asset metadata or result link according to the host application flow.

## Safety And Routing Rules

- Never invent endpoint IDs. Use verified endpoint search/schema results, or the curated test hints in `fal_endpoint_catalog.yaml` when they match backend support.
- Treat `fal-ai/nano-banana-pro` and `fal-ai/flux-2-pro` as curated test hints, not as a comprehensive FAL catalog.
- Do not claim this skill executes FAL calls directly. The skill gives domain guidance; the backend and generator-native FAL tools perform the work.
- Ask before spending. Image generation can incur cost, so confirm intent before submission.
- Keep prompt refinements faithful. Improve clarity and completeness without changing the user's requested content or constraints.

## Internal Specialist Flow

When `generator_agent` handles FAL work, it may use generator-native FAL tools to search endpoints, inspect schemas, submit approved jobs, and poll results. Prefer schema-derived fields over assumptions, and omit optional fields unless the user requested them or the schema clearly supports them.
