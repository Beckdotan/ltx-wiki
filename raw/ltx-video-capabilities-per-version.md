# LTX-Video Capabilities Per Version

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.8-13B-distilled
**Source:** https://github.com/Lightricks/LTX-Video

## Capability Matrix

| Capability | v0.9.0 | v0.9.1 | v0.9.5 | v0.9.6 | v0.9.7 | v0.9.8 |
|------------|--------|--------|--------|--------|--------|--------|
| Text-to-video | Yes | Yes | Yes | Yes | Yes | Yes |
| Image-to-video | Yes | Improved | Improved | Yes | Yes | Yes (primary) |
| Video-to-video | -- | -- | -- | -- | Yes | Yes |
| Multi-keyframe conditioning | -- | -- | Yes (new) | Yes | Yes | Yes |
| Video extension (fwd/bwd) | -- | -- | Partial | Yes | Yes | Yes |
| IC-LoRA control | -- | -- | -- | -- | -- | Yes (new) |
| LoRA fine-tuning | -- | -- | -- | -- | Yes | Yes |
| Spatial upscaling (2x) | -- | -- | -- | -- | Yes | Yes |
| Temporal upscaling | -- | -- | -- | -- | -- | Yes |
| Training-free enhancement | -- | Yes (new) | -- | -- | -- | -- |

## Resolution and Frame Specifications

| Spec | v0.9.0 | v0.9.1 | v0.9.5 | v0.9.6 | v0.9.7 | v0.9.8 |
|------|--------|--------|--------|--------|--------|--------|
| Native resolution | 768x512 | 1216x704 | 1216x704 | 1216x704 | 1216x704 | 1216x704 |
| Max recommended | 768x512 | 720x1280 | 720x1280 | 720x1280 | 720x1280 | 720x1280 |
| FPS | 24 | 30 | 24-30 | 30 | 30 | 30 |
| Max frames | 257 | 257 | 257 | 257 | 257 | 257 |
| Max duration | ~5 sec | ~8.5 sec | ~8.5 sec | ~8.5 sec | ~60 sec | ~60 sec |
| Resolution divisibility | 32 | 32 | 32 | 32 | 32 | 32 |
| Frame divisibility | 8n+1 | 8n+1 | 8n+1 | 8n+1 | 8n+1 | 8n+1 |

## Model Size Progression

| Version | Parameters | Model Variants |
|---------|-----------|----------------|
| v0.9.0 | 2B (1.9B transformer) | Single model |
| v0.9.1 | 2B | Single model |
| v0.9.5 | 2B | Single model |
| v0.9.6 | 2B | Dev + Distilled (first time) |
| v0.9.7 | 13B (new) + 2B | Dev, Distilled, FP8, LoRA |
| v0.9.8 | 13B + 2B | Dev, Distilled, FP8, IC-LoRA detailer |

## Inference Step Recommendations

| Version/Variant | Recommended Steps | CFG Required | STG Required |
|-----------------|-------------------|--------------|--------------|
| v0.9.0 (2B) | 40-50 | Yes | Yes |
| v0.9.1 (2B) | 50 | Yes | Yes |
| v0.9.5 (2B) | 50 | Yes | Yes |
| v0.9.6-dev (2B) | ~30 | Yes | Yes |
| v0.9.6-distilled (2B) | 8 | No | No |
| v0.9.7-dev (13B) | 30 | Yes | Yes |
| v0.9.7-distilled (13B) | 8 | No | No |
| v0.9.8-dev (13B) | 30 | Yes | Yes |
| v0.9.8-distilled (13B) | 8 | No | No |

## Generation Speed Progression

| Version | Speed (H100) | Notes |
|---------|-------------|-------|
| v0.9.0 | 5 sec video in 4 sec (768x512, 24fps) | Faster than real-time |
| v0.9.1 | ~Same as 0.9.0 | Maintained speed |
| v0.9.5 | ~Same | Real-time at 768x512 |
| v0.9.6-distilled | 15x faster than dev | Real-time on H100 |
| v0.9.7 | 30x faster than comparable models | Multiscale rendering |
| v0.9.8 | Slightly faster than 0.9.7 | Incremental improvement |

## Key Feature Introductions by Version

### v0.9.0 (Nov 2024)
- First open-source DiT-based video model
- Real-time generation
- Text-to-video + image-to-video

### v0.9.1 (Dec 2024)
- Smoother motion and improved physics
- Cleaner visuals
- Enhanced image-to-video quality
- Training-free video enhancement

### v0.9.5 (Mar 2025)
- Multi-keyframe conditional support
- Commercial license (OpenRail-M)
- Longer video generation
- ComfyUI day-1 support

### v0.9.6 (~Apr 2025)
- Dev/Distilled split (first time)
- 15x faster distilled variant
- 8-step sampling
- No CFG/STG needed for distilled

### v0.9.7 (May 2025)
- 13B parameter model (6.5x increase)
- Multiscale rendering
- Spatial upscaler (2x)
- LoRA fine-tuning support
- FP8 quantization
- Up to 60 second videos

### v0.9.8 (Jul 2025)
- IC-LoRA control models (depth, pose, canny)
- Detailer model
- New 2B distilled checkpoints
- Video-to-video support
- Multi-condition generation
