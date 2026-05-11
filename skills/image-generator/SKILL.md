---
name: image-generator
version: 1.0.0
description: Skill for routing approved image-generation requests through the generator specialist and generic FAL tools.
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
3. For FAL-backed image generation, use the curated endpoint catalog first. Pick an endpoint whose `capabilities`, `use_when`, and examples match the request.
4. Have the generator specialist inspect the selected endpoint schema with `fal_get_schema` before preparing a submission. Use `fal_search_endpoints` only when the curated catalog does not fit the user request.
5. Submit paid generation through `fal_submit` only after the user has approved the final prompt, endpoint, and relevant cost or spend caveat.
6. Poll with `fal_poll` after submission and return generated asset metadata or result links according to the host application flow.

## Safety And Routing Rules

- Never invent endpoint IDs. Use verified endpoint search/schema results, or the curated endpoints in `fal_endpoint_catalog.yaml` when they match backend support.
- Treat `fal-ai/nano-banana-pro` and `fal-ai/flux-2-pro` as the current curated image-generation allowlist, not as a comprehensive FAL catalog.
- Do not call or mention a `generate_image` shortcut. Image execution must use `fal_get_schema`, `fal_submit`, and `fal_poll`.
- Do not claim this skill executes FAL calls directly. The skill gives domain guidance; the backend and generator-native FAL tools perform the work.
- Ask before spending. Image generation can incur cost, so confirm intent before submission.
- Keep prompt refinements faithful. Improve clarity and completeness without changing the user's requested content or constraints.

## Internal Specialist Flow

When `generator_agent` handles FAL work, it must use generator-native FAL tools to inspect schemas, submit approved jobs, and poll results. Prefer catalog examples and presets over assumptions, then verify the final params against the endpoint schema. Omit optional fields unless the user requested them, the selected preset supplies them, or the schema clearly supports them.
