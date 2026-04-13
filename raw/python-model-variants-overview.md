# LTX Video - Model Variants and Capabilities Overview

> **Source:** https://huggingface.co/Lightricks/LTX-Video
> **Source:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video

## Model Family Overview

LTX-Video is a DiT-based (Diffusion Transformer) video generation model developed by Lightricks. It generates high-quality videos in real-time at up to 1216x704 resolution, 30 FPS, faster than playback speed.

## All Model Variants

### 2B Parameter Models (Consumer GPU Friendly)

| Model | ID | Pipeline | Notes |
|-------|-----|----------|-------|
| LTX-Video (Original) | `Lightricks/LTX-Video` | `LTXPipeline`, `LTXImageToVideoPipeline` | Original 2B model, 50-step generation |
| LTX-Video 0.9.5 | `Lightricks/LTX-Video-0.9.5` | `LTXConditionPipeline` | Adds multi-condition support |
| LTX-Video 2B 0.9.8 Distilled | `Lightricks/ltxv-2b-0.9.8-distilled` | `LTXConditionPipeline` | Low VRAM, 4-10 step generation |

### 13B Parameter Models (High Quality)

| Model | ID | Pipeline | Notes |
|-------|-----|----------|-------|
| LTX-Video 0.9.7 Dev | `Lightricks/LTX-Video-0.9.7-dev` | `LTXConditionPipeline` | First 13B model, spatial upscaler |
| LTX-Video 0.9.7 Distilled | `Lightricks/LTX-Video-0.9.7-distilled` | `LTXConditionPipeline` | 13B distilled, 4-10 steps |
| LTX-Video 0.9.8 Dev | `Lightricks/LTX-Video-0.9.8-dev` | `LTXConditionPipeline` | Highest quality, latest |
| LTX-Video 0.9.8 13B Distilled | `Lightricks/LTX-Video-0.9.8-13B-distilled` | `LTXConditionPipeline`, `LTXI2VLongMultiPromptPipeline` | Fast, long video support |

### FP8 Quantized Variants

Available for most models as 8-bit quantized versions for reduced memory usage.

### Auxiliary Models

| Model | ID | Purpose |
|-------|-----|---------|
| Spatial Upscaler 0.9.7 | `Lightricks/ltxv-spatial-upscaler-0.9.7` | 2x latent upscaling |
| Spatial Upscaler 0.9.8 | `Lightricks/ltxv-spatial-upscaler-0.9.8` | 2x latent upscaling |
| Latent Upsampler 0.9.8 | `a-r-r-o-w/LTX-0.9.8-Latent-Upsampler` | Alternative upsampler |

### LoRA Adapters

| LoRA | ID | Trigger Word | Description |
|------|-----|-------------|-------------|
| Cakeify LoRA | `Lightricks/LTX-Video-Cakeify-LoRA` | "CAKEIFY" | Object-to-cake transformation |
| Depth Control | Various | - | Depth-conditioned generation |
| Pose Control | Various | - | Pose-conditioned generation |
| Canny Control | Various | - | Edge-conditioned generation |

### GGUF Quantized (Community)

| Model | ID | Quantization |
|-------|-----|-------------|
| LTX-Video GGUF | `city96/LTX-Video-gguf` | Q3_K_S, Q4_K_S, etc. |

## Generation Modes

### Text-to-Video
- Supported by all models
- Use `LTXPipeline` (original) or `LTXConditionPipeline` (0.9.5+)
- Text-only generation works with `LTXConditionPipeline` without passing `conditions`

### Image-to-Video
- Supported by all models
- Use `LTXImageToVideoPipeline` (original) or `LTXConditionPipeline` (0.9.5+)
- Single image conditions at any frame position

### Video-to-Video
- Supported by v0.9.5+
- Use `LTXConditionPipeline` with video conditioning
- `denoise_strength` controls deviation from input

### Multi-Condition Generation
- Supported by v0.9.5+
- Use `LTXConditionPipeline` with multiple `LTXVideoCondition` objects
- Mix images and videos at different frame positions

### Long Video with Multi-Prompt
- Supported by v0.9.8 distilled
- Use `LTXI2VLongMultiPromptPipeline`
- Temporal sliding windows with autoregressive fusion
- Multi-prompt scene transitions

## Technical Specifications

### Resolution Support
- Must be divisible by 32
- Recommended: up to 720x1280 (or 1280x720)
- Auto-pads if not meeting divisibility requirements
- Best results at 480x704, 512x768, or 576x1024

### Frame Count
- Must follow pattern: 8n+1 (e.g., 9, 17, 25, ..., 161, 257, 361)
- Maximum varies by model and VRAM
- v0.9.8: Supports up to 361 frames (long video)

### Output
- 24-30 FPS video
- PIL images, NumPy arrays, PyTorch tensors, or latent output
- Export via `export_to_video()` utility

### Compression
- Video-VAE with 1:192 pixel-to-latent ratio
- Spatial compression: 32x per dimension
- Temporal compression: varies by configuration
- Timestep-aware decoding (v0.9.1+)

## Model Selection Guide

### By Use Case

| Use Case | Recommended Model | Steps | VRAM |
|----------|-------------------|-------|------|
| Quick prototyping | `ltxv-2b-0.9.8-distilled` | 4-8 | 8-10 GB |
| Production quality | `LTX-Video-0.9.8-dev` | 30-50 | 24-40 GB |
| Fast production | `LTX-Video-0.9.8-13B-distilled` | 4-10 | 16-24 GB |
| Long videos | `LTX-Video-0.9.8-13B-distilled` | 4-10 | 16-24 GB |
| Consumer GPU | `LTX-Video` + FP8 + offloading | 50 | 10 GB |
| API deployment | `ltxv-2b-0.9.8-distilled` | 4-8 | 8-10 GB |

### By VRAM Budget

| VRAM | Best Configuration |
|------|-------------------|
| 8 GB | 2B + FP8 + group offloading |
| 10 GB | 2B + FP8 + leaf-level offloading |
| 16 GB | 2B BF16 or 13B + FP8 + offloading |
| 24 GB | 13B + FP8 or 2B BF16 + torch.compile |
| 40+ GB | 13B BF16 + torch.compile |

## Version History

| Version | Date | Key Changes |
|---------|------|-------------|
| 0.9 | 2024 | Original 2B model release |
| 0.9.1 | 2024 | Timestep-aware VAE |
| 0.9.5 | 2025 | Multi-condition support, LTXConditionPipeline |
| 0.9.7 | 2025 | 13B model, spatial upscaler, distilled variants |
| 0.9.8 | 2025 | Long video support, tone mapping, 2B distilled |

## Limitations

- Not designed for factual information generation
- May amplify societal biases present in training data
- Prompt adherence depends on prompt quality
- Perfect prompt-to-video matching not guaranteed
- English prompts only
- Best with elaborate, descriptive prompts
