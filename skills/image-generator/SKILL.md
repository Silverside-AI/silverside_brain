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

Use this skill when the user asks to generate, refine, or prepare an image generation request. This is a routing and instruction skill only: image execution runs through the hosted fal MCP server (`fal-ai`) from the sandbox orchestrator.

## Operating Flow

1. Preserve the user's prompt constraints, including subject, style, aspect ratio, text requirements, exclusions, and any seed or output format preferences.
2. Discover endpoints with the fal MCP `search_models` or `recommend_model`. Prefer the curated endpoint catalog (`fal_endpoint_catalog.yaml`) when an entry's `capabilities`, `use_when`, and examples match the request.
3. Inspect the selected endpoint with the fal MCP `get_model_schema` before building parameters.
4. Run all media generation through the host `generate_media` tool (it submits and waits for completion, so do not finish until it returns). It handles both quick images and long-running jobs like video; `run_model`/`submit_job`/`check_job` are not available. Only run paid models after the user has approved the final prompt, endpoint, and relevant cost or spend caveat.
5. For image-to-image or editing with user-attached references, upload the mounted file to fal first with `fal_client.upload_file('/workspace/uploads/<name>')` from a sandbox shell command, then pass the returned fal URL into the model's `image_url`-style field.
6. Return generated asset metadata or result links according to the host application flow. The backend re-uploads outputs to R2 automatically.

## Safety And Routing Rules

- Never invent endpoint IDs. Use verified `search_models`/`get_model_schema` results, or the curated endpoints in `fal_endpoint_catalog.yaml` when they match.
- Treat `fal-ai/nano-banana-pro` and `fal-ai/flux-2-pro` as the current curated image-generation allowlist, not as a comprehensive fal catalog.
- Do not call or mention a `generate_image` shortcut. Inspect endpoints with the fal MCP `get_model_schema`, then generate with `generate_media`.
- Ask before spending. Image generation can incur cost, so confirm intent before running a paid model.
- Keep prompt refinements faithful. Improve clarity and completeness without changing the user's requested content or constraints.

## Execution Flow

When handling fal work, use the fal MCP tools to inspect schemas and run approved jobs. Prefer catalog examples and presets over assumptions, then verify the final params against the endpoint schema. Omit optional fields unless the user requested them, the selected preset supplies them, or the schema clearly supports them.
