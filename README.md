<div align="center">

# 👔 High-Fidelity Clothing Swap JSON Prompt

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
<img alt="Schema" src="https://img.shields.io/badge/Schema-JSON-blue.svg" />
<img alt="Focus" src="https://img.shields.io/badge/Focus-Photorealistic-orange.svg" />
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

**A scene-adaptive, photorealistic clothing-swap prompt schema for AI image editing**

*Seamlessly swap garments while preserving identity, pose, accessories, and environment*

[Features](#-features) • [How It Works](#-how-it-works) • [Quick Start](#-quick-start) • [Guide](#-guide) • [Design](#-design-philosophy)

---

</div>

## 📋 Table of Contents

- [👔 High-Fidelity Clothing Swap JSON Prompt](#-high-fidelity-clothing-swap-json-prompt)
  - [📋 Table of Contents](#-table-of-contents)
  - [✨ Features](#-features)
  - [🎯 How It Works](#-how-it-works)
  - [📥 Inputs](#-inputs)
  - [📚 Guide](#-guide)
  - [🚀 Quick Start](#-quick-start)
    - [Step 1: Prepare Your Files](#step-1-prepare-your-files)
    - [Step 2: Load Into Your Tool](#step-2-load-into-your-tool)
    - [Step 3: Execute the Swap](#step-3-execute-the-swap)
  - [🎨 Design Philosophy](#-design-philosophy)
    - [Priority Levels](#priority-levels)
    - [Core Components](#core-components)
  - [✅ Success Criteria](#-success-criteria)
  - [🔧 Technical Details](#-technical-details)
    - [Compatibility](#compatibility)
    - [Requirements](#requirements)
  - [📄 License](#-license)

---

## ✨ Features

<table>
  <tr>
    <td>🎯</td>
    <td><strong>Precision Garment Replacement</strong><br/>Replaces only the garment in the target image</td>
  </tr>
  <tr>
    <td>🛡️</td>
    <td><strong>Total Preservation</strong><br/>Keeps everything else from the target (face, hair, body, background, accessories)</td>
  </tr>
  <tr>
    <td>🎨</td>
    <td><strong>Scene-Adaptive Color Correction</strong><br/>Adjusts garment colors to match target scene lighting</td>
  </tr>
  <tr>
    <td>💡</td>
    <td><strong>Photorealistic Physics</strong><br/>Enforces realistic drape, seams, and contact shadows for seamless results</td>
  </tr>
</table>

---

## 🎯 How It Works

This prompt schema uses advanced AI image editing to transfer a garment from one image to another while maintaining photorealistic quality:

```
SOURCE (Image 2) → [Garment] → TARGET (Image 1)
          ↓                              ↓
  Reference Clothing              Person + Scene
                                         ↓
                             ✨ Photorealistic Result
```

The system prompt intelligently:
- 🔍 **Identifies** the garment in the source image
- 🎭 **Adapts** it to the target person's pose and body type
- 🌈 **Matches** lighting and color to the target environment
- 🧵 **Renders** realistic fabric physics and shadows

---

## 📥 Inputs
This repo ships two JSON files:

- `default_prompt.json` — ready-to-run, fully specified constraints.
- `template.json` — schema skeleton you can fill; defaults to `image_1` / `image_2` IDs.

| Input (ID) | Type | Description | Required |
|------------|------|-------------|----------|
| **`target_image`** | Image | **TARGET** - Person + scene to preserve | ✅ Yes |
| **`source_garment_image`** | Image | **SOURCE** - Garment reference to transfer | ✅ Yes |

> Note: Input IDs are configurable. Update `inputs.*.id` inside your JSON to match what your tool expects.
> - `template.json` defaults to `image_1` / `image_2`
> - `default_prompt.json` defaults to `target_image` / `source_garment_image`
>
> Garment naming/type is optional and lives under `inputs.source_garment_image.garment.type`.

---

## 📚 Guide

- New here? Start with the terminology + “how to fill” walkthrough in `GUIDE.md`.
- Want a ready-to-run prompt? Use `default_prompt.json`.
- Want a clean template/spec you can customize? Use `template.json`.

---

## 🚀 Quick Start

### Step 1: Prepare Your Files
Choose a starting JSON:

- Use `default_prompt.json` for fastest results.
- Use `template.json` if you want a schema skeleton to fill (or your tool expects `image_1` / `image_2`).

### Step 2: Load Into Your Tool
```bash
# Example workflow
1. Open your AI image editing tool (Stable Diffusion, Midjourney, etc.)
2. Load the JSON prompt schema
3. Upload your two images
```

### Step 3: Execute the Swap
1. 📋 Paste the JSON into your tool/workflow (diffusion edit, inpainting, GenFill, etc.)
2. 🖼️ Provide the two images as inputs:
   - **Image 1**: Target person and scene
   - **Image 2**: Source garment reference
3. ▶️ Run an edit/inpaint pass focused on the garment region
4. 🎉 Get your photorealistic result!

Tip: If your tool uses fixed image input names (`image_1` / `image_2`), start from `template.json` and then copy any extra constraints you want from `default_prompt.json`.

---

## 🎨 Design Philosophy

Our prompt schema uses **weighted constraints** to guide the AI model effectively:

### Priority Levels

```
🔴 CRITICAL → Must be perfect (identity, garment design)
🟡 HIGH     → Very important (lighting, drape, boundaries)
🟢 MEDIUM   → Desirable refinements (grain, compression artifacts)
```

### Core Components

<details>
<summary><strong>🧑 Identity Preservation</strong></summary>

- Maintains exact facial features
- Preserves body proportions
- Keeps original pose and gesture
- Retains all accessories (jewelry, watches, etc.)
</details>

<details>
<summary><strong>👗 Garment Fidelity</strong></summary>

- Accurate design transfer from source
- Correct patterns, textures, and colors
- Proper fit to target body shape
- Authentic fabric appearance
</details>

<details>
<summary><strong>🌈 Scene-Adaptive Color Correction</strong></summary>

- Matches target scene lighting
- Adjusts for warm/cool tones
- Preserves garment identity
- Natural color adaptation
</details>

<details>
<summary><strong>🧵 Physical Realism</strong></summary>

- Natural fabric drape and folds
- Realistic tension points
- Accurate contact shadows
- Proper seam alignment
</details>

<details>
<summary><strong>📸 Camera & Post Match</strong></summary>

- Grain consistency
- Depth of field matching
- Compression artifact alignment
- Overall photographic coherence
</details>

<details>
<summary><strong>🎯 Edge & Boundary Fidelity</strong></summary>

- No halos or artifacts
- Clean garment boundaries
- Seamless integration
- Natural occlusion
</details>

---

## ✅ Success Criteria

A successful clothing swap achieves:

- ✨ **Indistinguishable from reality** - The result looks like a genuine photograph
- 🎯 **Design accuracy** - The garment is clearly the same design as the reference
- 🛡️ **Complete preservation** - Zero changes to face, body, background, or accessories
- 🌟 **Natural integration** - No visible artifacts, halos, or inconsistencies
- 💡 **Proper lighting** - Garment lighting matches the scene perfectly

---

## 🔧 Technical Details

### Compatibility
- ✅ Stable Diffusion (with ControlNet)
- ✅ DALL-E 3
- ✅ Midjourney (with `/blend` or inpainting)
- ✅ Any GenFill/inpainting pipeline

### Requirements
- High-resolution input images (1024px+ recommended)
- Clear garment visibility in source image
- Similar pose/angle for best results

---

## 📄 License

This project is licensed under the **MIT License** - see below for details.

```
MIT License

Copyright (c) 2026 Hieu Nguyen

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

<div align="center">

**Made with ❤️ by NDMH Prompt Creator**

⭐ Star this repo if you find it helpful! ⭐

</div>