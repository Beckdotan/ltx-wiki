---
title: PromptGen
type: community-project
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/community-project-promptgen.md
tags:
  - community
  - prompting
  - prompt-engineering
  - ltx-video
  - mushroomfleet
---

# PromptGen

Community member **mushroomfleet** created a comprehensive prompt engineering toolkit for [[ltx-video-overview|LTX Video]], including a curated prompt collection and an automated prompt generator.

## Components

### Prompt Collection

- **Repository:** [DJZ-Nodes/prompts/LTXV-video-full.txt](https://github.com/MushroomFleet/DJZ-Nodes/blob/main/prompts/LTXV-video-full.txt)
- Contains all official demo prompts compiled in full (the official README had truncated versions)

### LTXV PromptGen Module

- **Repository:** [LLM-Base-Prompts/LTXV-PromptGen](https://github.com/MushroomFleet/LLM-Base-Prompts/tree/main/LTXV-PromptGen)
- Uses the ACE-HOLOFS technique for high-quality video prompt generation
- Mimics the author-specified prompt formats from the official demos
- Contains **110 prompts across 11 categories**
- Full documentation included

### Educational Content

- **YouTube walkthrough:** https://www.youtube.com/watch?v=s5c0IX8F0os
- Demonstrates the PromptGen approach and how to write effective LTX-Video prompts

## Adoption

[[lightricks-company]] adopted the PromptGen approach in their official LTX-Video-Playground Space on HuggingFace, including a simplified setup and image-to-video (I2V) version. This is notable as a case of community work being upstreamed into official tooling.

## Prompting Best Practices

The prompt collection established effective patterns for LTX-Video:

- **Visual contrast** -- e.g., "turquoise water vs. dark rocks"
- **Color specificity** -- e.g., "clear turquoise, white foam, green moss"
- **Texture and material details** -- e.g., "jagged rocks, foam, vegetation"
- **Spatial composition** -- e.g., "foreground, background, sky conditions"
- Elaborate, descriptive language emphasizing camera angles and scene dynamics
- English-language prompts required for best results

These patterns align with the general guidance that LTX-Video performs best with detailed prompts of ~200 words describing subject, action, environment, lighting, and camera angles.

## See Also

- [[ltx-video-overview]] -- LTX Video base model and prompt guidelines
- [[gemini-comfyui-workflow]] -- Another community approach to improving prompt quality via Gemini integration
