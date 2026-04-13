# LTX Video Models by Lightricks - Complete Version Overview

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Source:** https://github.com/Lightricks/LTX-Video
**Source:** https://ltx.studio/blog/ltxv-models

## What is LTX-Video?

LTX-Video (LTXV) is the first DiT-based (Diffusion Transformer) video generation model capable of generating high-quality videos in real-time. Developed by Lightricks, it produces 30 FPS videos at a 1216x704 resolution faster than they can be watched. The model family supports text-to-video, image-to-video, and video-to-video generation.

## Version Timeline

| Version | Release Date | Parameters | Key Milestone |
|---------|-------------|------------|---------------|
| 0.9.0 | November 2024 | 2B | Initial open-source release |
| 0.9.1 | December 2024 | 2B | Smoother motion, improved physics |
| 0.9.5 | March 5, 2025 | 2B | Keyframe conditioning, commercial license |
| 0.9.6 | ~April 2025 | 2B (dev + distilled) | Distilled variant, 15x faster inference |
| 0.9.7 | May 6, 2025 | 13B + 2B | First 13B model, multiscale rendering, 30x faster than comparable |
| 0.9.8 | Mid-July 2025 | 13B + 2B | 60-second videos, IC-LoRA support, detailer model |

## Model Variants Available (as of 0.9.8)

### 13B Models
| Variant | File Size | Key Features |
|---------|-----------|--------------|
| ltxv-13b-0.9.8-dev | ~28.6 GB | Highest quality, requires more VRAM |
| ltxv-13b-0.9.8-distilled | ~28.6 GB | Faster inference, slight quality reduction |
| ltxv-13b-0.9.8-dev-fp8 | ~15.7 GB | Quantized full model, reduced VRAM |
| ltxv-13b-0.9.8-distilled-fp8 | ~15.7 GB | Quantized distilled, lowest VRAM for 13B |

### 2B Models
| Variant | File Size | Key Features |
|---------|-----------|--------------|
| ltxv-2b-0.9.8-distilled | ~6.34 GB | Lighter VRAM usage |
| ltxv-2b-0.9.8-distilled-fp8 | ~4.46 GB | Quantized 2B, minimal VRAM |
| ltxv-2b-0.9.6-dev | ~6.34 GB | Full-quality 2B |
| ltxv-2b-0.9.6-distilled | ~6.34 GB | 15x faster, real-time capable |

### Legacy Versions
| Variant | File Size | Notes |
|---------|-----------|-------|
| ltxv-13b-0.9.7-dev | ~28.6 GB | First 13B release |
| ltxv-13b-0.9.7-distilled | ~28.6 GB | First 13B distilled |
| ltxv-13b-0.9.7-dev-fp8 | ~15.7 GB | Quantized 13B |
| ltxv-13b-0.9.7-distilled-fp8 | ~15.7 GB | Quantized 13B distilled |
| ltxv-13b-0.9.7-distilled-lora128 | ~1.33 GB | LoRA weights only |

## Output Specifications

- **Resolution:** 1216x704 (native), supports up to 720x1280
- **Frame Rate:** 30 FPS
- **Maximum Frames:** 257 frames (must be divisible by 8 + 1)
- **Maximum Duration:** Up to 60 seconds (0.9.8+)
- **Height/Width:** Must be divisible by 32
- **Frame Count:** Must follow 8n + 1 pattern (9, 17, 25, 33, ..., 257)

## Capabilities

- Text-to-video generation
- Image-to-video generation
- Video-to-video generation
- Multi-keyframe conditioning (specify images/video at different frame indices)
- Video extension (forward and backward)
- IC-LoRA control models (depth, pose, canny edge)
- Spatial upscaling (2x resolution increase)
- Temporal upscaling (frame rate enhancement)
- Standard LoRA fine-tuning support

## Prompt Requirements

- English language only
- Elaborate, descriptive prompts perform best
- Detailed descriptions with visual elements, lighting, camera angles recommended
- Negative prompts supported (e.g., "worst quality, inconsistent motion, blurry, jittery, distorted")

## Hugging Face Repositories

- Main: https://huggingface.co/Lightricks/LTX-Video
- 0.9.1: https://huggingface.co/Lightricks/LTX-Video-0.9.1
- 0.9.5: https://huggingface.co/Lightricks/LTX-Video-0.9.5
- 0.9.7-dev: https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
- 0.9.7-distilled: https://huggingface.co/Lightricks/LTX-Video-0.9.7-distilled
- 0.9.8-13B-distilled: https://huggingface.co/Lightricks/LTX-Video-0.9.8-13B-distilled
- Spatial Upscaler: https://huggingface.co/Lightricks/ltxv-spatial-upscaler-0.9.7

## Licensing

Each model version has its own license file. Version 0.9.0 used the RAIL-M license (dated November 22, 2024). Version 0.9.5 introduced the OpenRail-M license allowing commercial use. The 13B model (0.9.7+) is free to license for enterprises with under $10 million in annual revenue.
