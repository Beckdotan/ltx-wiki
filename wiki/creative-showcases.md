---
title: Creative Showcases
type: analysis
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/creative-showcases.md
  - raw/creative-showcase-user-examples.md
  - raw/creative-showcase-official-lora-demos.md
tags:
  - showcase
  - examples
  - lora
  - prompts
  - creative
  - community
---
# Creative Showcases

Creative showcases for [[ltx-video-overview\|LTX Video]] span official galleries, community-shared examples with full settings, and official LoRA effect demonstrations. These showcases serve both as inspiration and as practical references for prompting and configuration.

## Official Showcase Galleries

- **LTX-2.3 Showcase** (ltx23.net/showcase): Curated gallery of masterworks featuring exact text prompts and camera angles for hyper-realistic sequences
- **LTX Studio Examples** (ltx.studio/ai-video-examples): Videos spanning ads, movies, cartoons, and more
- **MaxVideoAI Examples** (maxvideoai.com/examples/ltx): Community-curated examples with prompts and settings

## Community User Examples

A pinned discussion on the [[ltx-2-overview\|LTX-2]] HuggingFace model page serves as a community showcase where users share video generation examples complete with prompts, settings, and hardware configurations.

### Common Settings Patterns
- **Resolution:** 1280x720 is the community standard for quality/speed balance
- **Frames:** 121 frames (5 seconds at 24fps) is the most common output length
- **Text encoder:** Gemma 3 12B is universally used for LTX-2/2.3
- **Generation time:** 80-95 seconds for 121 frames on RTX 5090

### Model Variant Comparisons from Users
- FP4 vs BF16: Similar quality, FP4 slightly slower
- FP8 vs full-dev: "Minimal and negligible" quality difference
- GGUF Q4_K_M: Viable for lower-VRAM setups with acceptable quality

### Prompt Style
Community examples consistently use detailed, cinematic descriptions with specific camera direction language ("smartphone video," "close-up," "photorealistic"), material and texture descriptions, and mood/lighting specifications.

## Official LoRA Effect Demonstrations

[[lightricks]] released two creative LoRA demonstrations with public training datasets, showing the creative potential of LTX-Video fine-tuning.

### Squish LoRA
Generates "squish" effects -- animations where hands squeeze deformable objects shaped like various subjects. Prompt format: `SQUISH two hands squeezing a squeezable object that is shaped like [your object]`. Public training dataset available at Lightricks/Squish-Dataset on HuggingFace.

### Cakeify LoRA
Generates "cakeify" effects -- scenes where a person cuts a cake shaped like various objects. Prompt format: `CAKEIFY a person using a knife to cut a cake shaped like [your object]`. Public training dataset available at Lightricks/Cakeify-Dataset on HuggingFace.

### Significance of Official LoRAs
These demonstrations show that custom video effects can be trained with relatively small datasets, trigger word patterns enable easy activation, and creative applications extend well beyond standard video generation (viral effects, social media content). Both LoRAs work with the image-to-video pipeline and include [[comfyui-integration\|ComfyUI]] workflows.

## Types of Creative Content Demonstrated

### Cinematic Sequences
Character shots in futuristic environments, action shots with realistic water and particle effects, high-fidelity portrait videos with detailed facial expressions.

### Artistic Styles
Stop-motion style scenes with felt/wool characters and tactile textures, cardboard and twine miniature sets, animated cartoon-style content.

### Commercial Content
High-converting UGC and ad videos, music videos and trailers, social media content (Instagram, TikTok, YouTube Shorts).

## Technical Capabilities Demonstrated

| Capability | Detail |
|------------|--------|
| Resolution | Native 4K output |
| Frame rate | Up to 50 FPS |
| Camera movements | Dolly, crane, tracking shots |
| Audio-visual sync | Synchronized dialogue, music, sound effects (LTX-2+) |
| Text rendering | In-video text (LTX-2.3 improvement) |
| Clip duration | Up to 20 seconds with synchronized audio |

## Use Case Categories

- **Advertising:** High-performing AI UGC ads without creators or filming
- **Filmmaking:** Movie clips, trailers, and short films
- **Social media:** Platform-optimized content creation
- **Music videos:** Text-to-music-video pipelines
- **Storyboarding:** Rapid visual ideation and prototyping

## Community Showcases on Civitai

Community members share generated videos alongside their workflows on [[civitai-community\|Civitai]], including character consistency demos, video-to-video transformation showcases, IC-LoRA style transfer demonstrations, and multi-model VideoFlow workflows.

## References

- [[adoption-metrics]]
- [[community-feedback]]
- [[huggingface-spaces-ecosystem]]
- [[civitai-community]]
