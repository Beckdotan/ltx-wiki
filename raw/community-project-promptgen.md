# Community Project: LTX-Video PromptGen by mushroomfleet

**Source:** https://huggingface.co/Lightricks/LTX-Video/discussions/7

---

## Overview

Community member **mushroomfleet** created a comprehensive prompt engineering toolkit for LTX-Video, including a curated prompt collection and an automated prompt generator.

## Components

### 1. Prompt Collection
- **Repository:** [DJZ-Nodes/prompts/LTXV-video-full.txt](https://github.com/MushroomFleet/DJZ-Nodes/blob/main/prompts/LTXV-video-full.txt)
- Contains all official demo prompts compiled in full (the official README had truncated versions)

### 2. LTXV PromptGen Module
- **Repository:** [LLM-Base-Prompts/LTXV-PromptGen](https://github.com/MushroomFleet/LLM-Base-Prompts/tree/main/LTXV-PromptGen)
- Uses ACE-HOLOFS technique for high-quality video prompt generation
- Mimics the author-specified prompt formats from the official demos
- Contains **110 prompts across 11 categories**
- Full documentation included

### 3. Educational Content
- **YouTube walkthrough:** https://www.youtube.com/watch?v=s5c0IX8F0os
- Demonstrates the PromptGen approach and how to write effective LTX-Video prompts

## Impact

Lightricks adopted the PromptGen approach in their official LTX-Video-Playground Space on HuggingFace, including a simplified setup and image-to-video (i2v) version.

## Prompting Best Practices Established

The prompt collection demonstrates effective patterns for LTX-Video:
- **Visual contrast** (e.g., "turquoise water vs. dark rocks")
- **Color specificity** (e.g., "clear turquoise, white foam, green moss")
- **Texture and material details** (e.g., "jagged rocks, foam, vegetation")
- **Spatial composition** (e.g., "foreground, background, sky conditions")
- Elaborate, descriptive language emphasizing camera angles and scene dynamics
- English-language prompts required for best results
