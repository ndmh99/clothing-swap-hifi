# GUIDE — Terminology + How to Fill the Template

This guide explains **every term used by the schema**, and gives a practical, low-friction workflow for filling out the JSON so you can get consistent, photorealistic clothing swaps.

> Quick orientation:
> - Use `default_prompt.json` if you want a **ready-to-run** prompt with strong defaults.
> - Use `template.json` if you want a **schema skeleton** you can customize (same shape as `default_prompt.json`, mostly empty values).

---

## Table of Contents

- [1) Core Concepts](#1-core-concepts)
- [2) Terminology Glossary](#2-terminology-glossary)
- [3) Which File Should I Use?](#3-which-file-should-i-use)
- [4) Fastest Workflow: Copy → Fill → Run](#4-fastest-workflow-copy--fill--run)
- [5) Field-by-Field Guide (template.json)](#5-field-by-field-guide-templatejson)
- [6) Field-by-Field Guide (default_prompt.json)](#6-field-by-field-guide-default_promptjson)
- [7) Best Practices (Photorealism + Consistency)](#7-best-practices-photorealism--consistency)
- [8) Troubleshooting](#8-troubleshooting)
- [9) Suggested “Fill Templates”](#9-suggested-fill-templates)

---

## 1) Core Concepts

You always have **two images**:

- **TARGET (Image 1)**: the person + scene you want to keep. Only the garment changes.
- **SOURCE (Image 2)**: the garment you want to transfer. This defines the garment’s design.

The model’s job is to produce a result that looks like:

- The person in the target image was **originally photographed** wearing the source garment.
- Nothing else changes (identity, pose, accessories, background, camera look).

---

## 2) Terminology Glossary

### Images and roles

- **target_image**: the target input (the “keep everything” photo).
- **source_garment_image**: the source input (the “take garment from here” photo).
- **id**: the token your tool uses to reference an image input. Common options:
  - `image_1` / `image_2`
  - `target_image` / `source_garment_image`

If your tool expects `image_1` and `image_2`, set IDs to match.

### Garment-related terms

- **garment type**: the garment category/name (e.g., `dress`, `blazer`, `hoodie`).
  - In `default_prompt.json`, the schema is written so the model can often infer the garment without you specifying it.
  - In the blank template (`template.json`), set `inputs.source_garment_image.garment.type` if you want, or leave it empty if your tool/model can infer it.

- **attributes_to_transfer / must_transfer**: the **design DNA** of the garment.
  - **structure**: cut, silhouette, neckline, sleeves, hem, darts, pleats.
  - **material**: fabric look, sheen, thickness, stiffness, weave.
  - **visual_details**: patterns, logos, embroidery, buttons, zippers, seams.

### Preservation terms

- **preserve.person**: what must remain unchanged about the person (identity/face/hair/body/pose).
- **preserve.scene**: what must remain unchanged about the environment (background/lighting/camera/composition).
- **preserve.accessories**: what must remain unchanged (jewelry, glasses, hats, bags, etc.).

### Constraint system

- **constraints**: rules the model must follow.
- **negative_constraints**: a “do not generate” list (artifacts to avoid).
- **priority** levels:
  - **critical**: absolutely must not break (identity preservation, clean boundaries, garment fidelity)
  - **high**: very important realism (lighting integration, fabric physics)
  - **medium**: finishing (grain/DOF, compression match)

### Integration & realism terms

- **color correction**: adjust garment brightness/contrast/saturation/white balance to match target lighting.
- **contact shadows**: subtle shadows where garment touches skin/body.
- **occlusion/layering**: hair/straps/accessories overlapping correctly (in front/behind).
- **camera match**: grain, sharpness, depth-of-field, compression artifacts consistent with target.

---

## 3) Which File Should I Use?

### Use `default_prompt.json` when you want results fast
- It’s already filled with strong defaults.
- Best for typical “swap this garment onto that person” workflows.

### Use `template.json` when you need a clean blank template
`template.json` is a schema skeleton with the same top-level keys as `default_prompt.json`.

Best for:
- building your own generator
- converting into another prompt format
- creating multiple variants with consistent structure

---

## 4) Fastest Workflow: Copy → Fill → Run

### Step A — Decide your input IDs
Check what your tool calls the two images.

- If your tool uses `image_1` and `image_2`:
  - Set the target image id to `image_1`
  - Set the source image id to `image_2`

### Step B — Pick the starting JSON
- Start from `default_prompt.json` for quickest success.
- Only customize fields if you have a specific reason.

### Step C — Run it as an edit/inpaint
For most editors, you get best results if you:

- Mask the **garment region** on the target image.
- Allow a little buffer around boundaries (neckline, sleeves, hem) to give the model space to blend shadows.

---

## 5) Field-by-Field Guide (template.json)

`template.json` is a schema/template. Here’s what each part means and how to fill it.

### `meta`
- **name**: your internal name for the format.
- **version**: bump if you change structure.
- **description**: short human summary.

### `inputs.target_image`
- **id**: set to what your tool expects (commonly `image_1`).
- **role**: should remain `target`.
- **description**: optional.
- **preserve**:
  - `person`: list the person aspects you want guaranteed unchanged.
  - `scene`: list the scene/camera aspects you want unchanged.
  - `accessories`: list accessories to keep.

Tip: If you don’t know what to put here, you can leave these arrays empty in the template and rely on your downstream prompt or tool defaults—but the more you specify, the safer the swap.

### `inputs.source_garment_image`
- **id**: set to what your tool expects (commonly `image_2`).
- **role**: should remain `garment_source`.
- **garment.type**:
  - Optionally set a simple value like `dress`, `jacket`, `shirt`.
  - If you leave it blank, `attributes_to_transfer` still tells the model what to preserve.
- **garment.attributes_to_transfer.structure/material/visual_details**:
  - Fill with bullet-like strings describing what must carry over.
  - Keep them **observable** (don’t invent hidden parts).

Example entries:
- structure: `crew neckline`, `long sleeves`, `cropped hem`
- material: `matte knit`, `medium thickness`, `soft drape`
- visual_details: `ribbed cuffs`, `visible seam lines`, `embroidered logo on left chest`

### `instructions.primary_operation`
- **action**: keep `clothing_swap`.
- **summary**: a short statement like “Replace only the main visible outer garment on the target person.”
- **priority**: pick one of `critical`, `high`, `medium`.
- **notes**: any extra reminders (e.g., “final must look like an original photo”).

### `instructions.constraints[]`
In `template.json`, constraints are already scaffolded as `C1`–`C8`. Fill in each one’s `priority`, `description`, and `rules`.

Recommended constraint names to include in your derived prompt:
- identity preservation (critical)
- garment fidelity (critical)
- clean boundaries + contact shadows (critical)
- lighting integration (high)
- fabric physics (high)
- camera match (medium)

### `negative_constraints`
- A list of errors to explicitly forbid.
- Organize into categories like:
  - anatomy
  - garment_structure
  - texture_and_material
  - compositing_and_integration

### `output`
Define how the output should look.
- `output.quality.style`: `photorealistic`
- `output.quality.integration`: “seamless/indistinguishable”
- `output.quality.resolution`: match your pipeline (or keep it generic)
- `output.success_criteria`: concrete pass/fail checks (see Section 7)

---

## 6) Field-by-Field Guide (default_prompt.json)

`default_prompt.json` is the filled “ready prompt”. Here’s how to tweak it safely.

### `inputs.target_image.preserve`
This is your safety net.

- If you’re seeing identity drift (face changes), add/keep:
  - `identity`, `face`, `facial_expression`, `skin_tone`, `skin_texture`
- If you’re seeing pose drift (arms/hands change), keep:
  - `pose`, `hands`, `fingers`, `body_proportions`
- If background changes, keep:
  - `background`, `environment`, `composition`, `camera_angle`, `framing`

### `inputs.source_garment_image.garment`
- **type** is intentionally descriptive; you usually don’t need to change it.
- **attributes_to_transfer** is the most important section when the model is “almost right” but missing details.
  - Add/keep only details that are visible in the source.

### `instructions.constraints[]`
Each constraint has:
- **id**: stable identifier (C1, C2, …).
- **priority**: controls strictness.
- **rules**: plain-language rules.

Safe edits:
- Tighten rules if the model changes non-garment elements.
- Add a rule if a specific artifact repeats (e.g., “no collar hallucination”).

Avoid:
- Adding conflicting rules (e.g., “never change lighting” while also requiring scene-matched lighting on the new garment). The garment must adapt to the existing lighting.

### `negative_constraints`
If a specific artifact appears often, add it to the right category.
Examples:
- `halo_edges_or_glows_around_garment`
- `sticker_or_cutout_appearance`
- `broken_or_misaligned_seams`

### `output.quality`
If your pipeline isn’t truly “8k-like”, you can tone down:
- `resolution` → `high_resolution`

---

## 7) Best Practices (Photorealism + Consistency)

### Masking / edit region
- Mask the garment on the target image.
- Include boundary buffer areas (neckline, cuffs, hem) for blending.
- Avoid masking the face/hair unless the garment covers them.

### When the garment looks “pasted on”
- Strengthen **lighting integration** and **contact shadow** rules.
- Strengthen **camera match** (grain/DOF) if edges look too clean.

### When the garment design changes
- Strengthen **garment fidelity** rules.
- Add specific structure details (neckline shape, hem length).
- Add unique identifiers (pattern placement, visible fasteners).

### Simple success checklist
A good result should satisfy:
- Same person, same face, same pose.
- Same background/camera framing.
- Same garment design as reference.
- Lighting + shadows consistent with target.
- No halos, no cutout edges.

---

## 8) Troubleshooting

### Problem: Face or body changes
- Increase emphasis on identity preservation.
- Ensure you’re editing/masking only the garment region.
- Add more preserve items under `inputs.target_image.preserve.person`.

### Problem: Wrong neckline / sleeves / hem
- Add the missing structural item to `attributes_to_transfer.structure`.
- Add a negative constraint forbidding the wrong structure if it repeats.

### Problem: Halos / edge glow
- Add/strengthen the negative constraint: `halo_edges_or_glows_around_garment`.
- Strengthen edge/boundary fidelity + contact shadows.
- Slightly expand mask region so blending can happen naturally.

### Problem: Fabric looks like plastic / too smooth
- Add/strengthen texture negative constraints.
- Add material details: `matte`, `woven texture`, `visible weave` (only if visible in source).
- Add camera match rules to apply grain/noise.

---

## 9) Suggested “Fill Templates”

These are compact snippets you can copy into your own derived prompt.

### A) Minimal “garment type + key details”
Use when the swap is correct but missing signature features.

- garment type: `dress`
- structure: `square neckline`, `puff sleeves`, `mid-calf hem`
- material: `satin sheen`, `light drape`
- visual details: `floral print`, `covered buttons`, `visible waist seam`

### B) Minimal negative constraints add-on
Use when you see repeated artifacts:

- `halo_edges_or_glows_around_garment`
- `sticker_or_cutout_appearance`
- `visible_inpainting_or_blending_artifacts`
- `broken_or_misaligned_seams`

---
