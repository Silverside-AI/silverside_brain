# Text To Image Recipe

Use this reference to prepare image-generation requests before handing them to `generator_agent`.

## Prompt Refinement

1. Restate the user's intent in a concise generation prompt.
2. Preserve required subject matter, composition, style, mood, colors, aspect ratio, text, and exclusions.
3. Add clarifying visual detail only when it supports the user's stated goal.
4. Keep any negative constraints explicit, such as "no text," "no watermark," or "avoid distorted hands."
5. Ask a short clarification when a missing constraint would materially change the output.

## Endpoint Selection

- Use live endpoint search and schema inspection before submit.
- Prefer `fal-ai/nano-banana-pro` for the current curated test path when the user needs backend-supported controls such as multiple images, resolution, or web-search-assisted generation.
- Prefer `fal-ai/flux-2-pro` for the current curated test path when a straightforward high-quality text-to-image request fits the supported image-size options.
- Treat endpoint choices as hints until confirmed by the generator specialist and backend validation.
- Do not choose unverified endpoint IDs from memory.

## Approval Checklist

Before submission, confirm:

- The final prompt.
- The selected endpoint.
- Any requested optional fields, such as aspect ratio, image size, seed, output format, image count, or resolution.
- The spend caveat, because generation may incur cost.
