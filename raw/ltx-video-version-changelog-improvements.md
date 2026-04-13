# LTX-Video: Version-by-Version Changelog and Improvements

**Sources:** Compiled from all Hugging Face model cards and arxiv paper

---

## Version Progression Summary

### v0.9 (Late 2024) -- Initial Public Release
**New:**
- First public release of LTX-Video
- 2B parameter DiT-based video generation model
- Novel Video-VAE with 1:192 compression ratio (32x32x8, 128 channels)
- Denoising decoder (VAE performs final diffusion step in pixel space)
- Reconstruction GAN loss for VAE training
- Video DWT loss for high-frequency detail preservation
- T5-XXL text encoder with cross-attention conditioning
- RoPE with normalized fractional coordinates
- QK normalization with RMSNorm
- Per-token timestep conditioning for image-to-video
- Rectified-flow training with velocity prediction
- Multi-resolution training
- Text-to-video and image-to-video capabilities
- 768x512 at 24 FPS, up to 257 frames
- Faster-than-real-time generation (2s for 5s video on H100)
- Research paper published: arxiv.org/abs/2501.00103
- Open-source release on GitHub and Hugging Face
- Diffusers integration (`LTXPipeline`, `LTXImageToVideoPipeline`)

### v0.9.1 (Early 2025) -- Quality Refinement
**Improved:**
- Quality and stability refinements over v0.9
- Same 2B architecture
- Same API and capabilities

### v0.9.5 -- Further Refinement
**Improved:**
- Continued quality improvements
- Last version of the "original" 2B-only model line
- Bridge to the distillation era

### v0.9.6 -- Distillation Milestone
**New:**
- Distilled model variant introduced
- 15x speed improvement over dev model
- No STG/CFG guidance required for distilled variant
- Dramatically reduced inference steps
- Real-time video generation achieved

**Models Added:**
- `ltxv-2b-0.9.6-dev` (replaces earlier 2B model)
- `ltxv-2b-0.9.6-distilled` (new: 15x faster)

### v0.9.7 -- Major Scale-Up Release
**New:**
- **13B parameter model** -- massive quality improvement
- **FP8 quantization** -- reduced memory footprint
- **ICLoRA conditioning adapters:**
  - Depth map conditioning
  - Pose keypoint conditioning
  - Canny edge conditioning
  - Detailer (enhanced fine details)
- **Spatial upscaler** -- 2x resolution increase in latent space
- **Temporal upscaler** -- frame interpolation
- **Mix mode** -- multi-scale workflow combining dev + distilled
- **LoRA128 adapter** -- convert dev to distilled behavior
- **Multi-condition generation** -- condition on multiple images/videos at different frame positions
- **New Diffusers API:** `LTXConditionPipeline`, `LTXLatentUpsamplePipeline`, `LTXVideoCondition`

**Models Added:**
- `ltxv-13b-0.9.7-dev`
- `ltxv-13b-0.9.7-distilled`
- `ltxv-13b-0.9.7-fp8`
- `ltxv-13b-0.9.7-distilled-fp8`
- `ltxv-13b-0.9.7-mix`
- `ltxv-13b-0.9.7-distilled-lora128`
- `ltxv-13b-0.9.7-ICLoRA-depth`
- `ltxv-13b-0.9.7-ICLoRA-pose`
- `ltxv-13b-0.9.7-ICLoRA-canny`
- `ltxv-13b-0.9.7-ICLoRA-detailer`
- `ltxv-spatial-upscaler-0.9.7`
- `ltxv-temporal-upscaler-0.9.7`

### v0.9.8 -- Ecosystem Maturation
**New:**
- **2B distilled variant returns** with latest training improvements
- All 13B variants updated (dev, mix, distilled, FP8)
- **2B distilled-fp8** -- most efficient variant ever (~6-8 GB VRAM)
- ICLoRA adapters updated
- Upscalers updated

**Models Added:**
- `ltxv-13b-0.9.8-dev`
- `ltxv-13b-0.9.8-mix`
- `ltxv-13b-0.9.8-distilled`
- `ltxv-2b-0.9.8-distilled`
- `ltxv-13b-0.9.8-fp8`
- `ltxv-13b-0.9.8-distilled-fp8`
- `ltxv-2b-0.9.8-distilled-fp8`

## Key Milestones Timeline

| Version | Milestone |
|---------|-----------|
| 0.9 | First open-source real-time DiT video model |
| 0.9.1 | Quality refinement |
| 0.9.5 | Mature 2B model |
| 0.9.6 | Distillation breakthrough (15x speed) |
| 0.9.7 | 13B scale-up, FP8, ICLoRA, upscalers |
| 0.9.8 | Full ecosystem with 2B+13B variants |

## Architecture Constants (Unchanged Across Versions)

These fundamental design choices remain constant:
- DiT-based architecture (built on Pixart-alpha)
- Video-VAE with 32x32x8 compression, 128 channels
- T5-XXL text encoder
- RoPE positional embeddings
- Cross-attention text conditioning
- Per-token timestep conditioning for I2V
- Rectified-flow training framework
- Resolution divisible by 32
- Frame count divisible by 8 + 1
- Maximum ~257 frames
- 24-30 FPS output
