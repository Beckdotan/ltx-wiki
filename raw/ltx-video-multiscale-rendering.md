# LTX-Video Multiscale Rendering Architecture

**Source:** https://www.prnewswire.com/news-releases/lightricks-launches-13b-parameters-ltx-video-model-breakthrough-rendering-approach-generates-high-quality-efficient-ai-video-30x-faster-than-comparable-models-302447660.html
**Source:** https://ltx.studio/blog/ltxv-models
**Source:** https://ltx.io/model/ltx-2

## What is Multiscale Rendering?

Multiscale rendering is a breakthrough technique introduced with the LTXV-13B model (v0.9.7, May 2025). It is a layered generation process that delivers both speed and quality by mimicking how professional artists and filmmakers work -- starting with broad strokes before layering in increasingly fine details.

## How It Works

### Step-by-Step Process

1. **Coarse Pass (Low Detail Draft)**
   - The model first drafts the video at lower detail
   - Captures coarse motion patterns using fewer computational resources
   - Establishes the fundamental motion and composition of the scene

2. **Progressive Refinement**
   - The coarse draft guides subsequent generation stages
   - The model progressively adds:
     - Structural details
     - Lighting effects
     - Micro-motion (subtle movements, textures)
   - Each refinement pass adds more detail without recomputing everything

3. **Built-in Upscaling**
   - For a target 1080p output, the model first renders at 960x540
   - Then upscales 2x to full resolution
   - This means the heavy generation work happens at half resolution

### Motion-First Approach

LTXV prioritizes motion first, then infuses detail using the multiscale process:
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
- Calculates pixels frame-by-frame at full resolution
- Each diffusion step processes the full resolution
- Higher computational cost, slower generation

### Multiscale Rendering
- Works at multiple resolutions simultaneously
- Lower resolution passes handle motion and structure
- Higher resolution passes add detail
- Total computation is less than full-resolution-only approach

## Relationship to Other Components

### Spatial Upscaler
- The spatial upscaler (ltxv-spatial-upscaler-0.9.7) can be used as an additional post-processing step
- It provides a further 2x upscaling beyond the built-in multiscale rendering
- Workflow: Multiscale render at base -> Optional spatial upscale 2x -> Optional refinement

### VAE Integration
- The multiscale rendering works within the compressed latent space enabled by the VAE's 1:192 compression
- The VAE's denoising decoder handles the final pixel-space output

## Impact on Model Design

Multiscale rendering is a key reason the 13B model achieves practical inference speeds:
- Without it, a 13B DiT model at full resolution would be prohibitively slow
- The technique effectively reduces the computational complexity of the generation process
- Makes it feasible to run on consumer GPUs despite the large parameter count
