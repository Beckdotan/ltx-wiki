---
title: Multiscale Rendering
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://www.prnewswire.com/news-releases/lightricks-launches-13b-parameters-ltx-video-model-breakthrough-rendering-approach-generates-high-quality-efficient-ai-video-30x-faster-than-comparable-models-302447660.html
  - https://ltx.studio/blog/ltxv-models
  - https://ltx.io/model/ltx-2
tags:
  - ltx-video
  - multiscale-rendering
  - architecture
  - performance
---

# Multiscale Rendering

Multiscale rendering is a breakthrough technique introduced with the [[ltxv-13b|LTXV-13B model]] in [[ltx-video-097|v0.9.7]] (May 2025). It is a layered generation process that delivers both speed and quality by mimicking how professional artists and filmmakers work -- starting with broad strokes before layering in increasingly fine details.

## How It Works

### Step-by-Step Process

1. **Coarse Pass (Low Detail Draft)** -- The model drafts the video at lower detail, capturing coarse motion patterns with fewer computational resources and establishing the fundamental motion and composition of the scene.

2. **Progressive Refinement** -- The coarse draft guides subsequent generation stages. The model progressively adds structural details, lighting effects, and micro-motion (subtle movements, textures). Each refinement pass adds more detail without recomputing everything.

3. **Built-in Upscaling** -- For a target 1080p output, the model first renders at 960x540 then upscales 2x to full resolution. The heavy generation work happens at half resolution.

### Motion-First Approach

LTXV prioritizes motion first, then infuses detail:

- Dynamic, believable movement is established before other details
- This ensures temporal consistency across the video
- Fine visual details are added in the refinement passes

## Performance Benefits

- **30x faster** rendering than comparable-sized models (e.g., HunyuanVideo 13B, MovieGen 30B)
- Reduced computational requirements per frame
- Enables real-time generation on consumer GPUs
- Maintains visual realism despite speed gains

## Comparison to Traditional Approaches

### Traditional Sequential Latent Diffusion
- Calculates at full resolution throughout the diffusion process
- Each step processes the full resolution
- Higher computational cost, slower generation

### Multiscale Rendering
- Works at multiple resolutions simultaneously
- Lower resolution passes handle motion and structure
- Higher resolution passes add detail
- Total computation is less than a full-resolution-only approach

## Mix Mode

The "mix" model variant (e.g., `ltxv-13b-0.9.7-mix`, `ltxv-13b-0.9.8-mix`) implements multiscale rendering by combining two [[ltxv-model-variants|model variants]]:

- Uses the **distilled model** for the initial low-resolution pass (fast, fewer steps)
- Uses the **dev model** for high-resolution refinement (higher quality, more steps)
- Balances speed and quality in a single workflow

## Relationship to Other Components

### Spatial Upscaler
The [[ltxv-spatial-upscaler]] can be used as an additional post-processing step beyond the built-in multiscale rendering:
- Multiscale rendering handles the internal generation process
- The spatial upscaler provides a further 2x upscaling on top of that

### VAE Integration
- Multiscale rendering works within the compressed latent space enabled by the VAE's 1:192 compression ratio
- The VAE's denoising decoder handles the final pixel-space output

## Impact on Model Design

Multiscale rendering is a key reason the [[ltxv-13b|13B model]] achieves practical inference speeds:
- Without it, a 13B DiT model at full resolution would be prohibitively slow
- The technique effectively reduces the computational complexity of the generation process
- Makes it feasible to run on consumer GPUs despite the large parameter count

## See Also

- [[ltxv-13b]] -- the model architecture that uses multiscale rendering
- [[ltx-video-097]] -- the release that introduced multiscale rendering
- [[ltxv-spatial-upscaler]] -- complementary upscaling beyond built-in multiscale
- [[ltxv-model-variants]] -- the mix variant that implements the dual-model workflow
