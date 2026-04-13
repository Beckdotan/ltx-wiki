# LTX-Video 0.9.8 Release (Mid-July 2025)

**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.8-13B-distilled
**Source:** https://blog.fal.ai/new-ltxv-0-9-8-model-live-on-fal/
**Source:** https://aicreators.tools/model/video/79
**Source:** https://github.com/Lightricks/LTX-Video

## Release Details

- **Release date:** Mid-July 2025
- **Parameters:** 13B and 2B variants
- **Major focus:** Extended video generation, IC-LoRA support, new 2B distilled model

## Model Variants

### 13B Models
| Variant | File Size | Features |
|---------|-----------|----------|
| ltxv-13b-0.9.8-dev | ~28.6 GB | Highest quality |
| ltxv-13b-0.9.8-distilled | ~28.6 GB | Faster inference |
| ltxv-13b-0.9.8-dev-fp8 | ~15.7 GB | Quantized full model |
| ltxv-13b-0.9.8-distilled-fp8 | ~15.7 GB | Quantized distilled |

### 2B Models (New)
| Variant | File Size | Features |
|---------|-----------|----------|
| ltxv-2b-0.9.8-distilled | ~6.34 GB | New lightweight distilled |
| ltxv-2b-0.9.8-distilled-fp8 | ~4.46 GB | Quantized 2B |

## Key Changes from 0.9.7

### Extended Video Generation
- Videos up to 60 seconds of continuous generation
- Extended video capabilities for longer narrative content

### Improved Prompt Understanding
- Better comprehension of complex prompts
- Enhanced detail generation in response to descriptions

### IC-LoRA Support
- Works with official IC-LoRA control models
- New detailer model: LTX-Video-ICLoRA-detailer-13B-0.9.8
- IC-LoRA for depth, pose, and canny edge control

### New 2B Distilled Checkpoints
- 2 new distilled checkpoints (2B and 13B)
- IC-LoRA for detail enhancement included
- Both 13B and 2B share the same base model (ltxv-13b-0.9.8-dev)

### Performance
- Slightly faster than 0.9.7
- Maintains real-time generation capability

## Pipeline Details

- **Pipeline:** LTXConditionPipeline
- **Framework:** Diffusers compatible

### Key Inference Parameters
```python
num_inference_steps = 30      # Default for generation
denoise_strength = 0.4        # For upscaling refinement
decode_timestep = 0.05        # VAE decoding parameter
image_cond_noise_scale = 0.025  # Conditioning noise scaling
```

### Multi-Condition Generation
```bash
python inference.py --prompt "PROMPT" \
  --conditioning_media_paths PATH_1 PATH_2 \
  --conditioning_start_frames FRAME_1 FRAME_2 \
  --height HEIGHT --width WIDTH \
  --num_frames NUM_FRAMES --seed SEED
```

## Output Specifications

- **Resolution:** 1216x704 native, best under 720x1280
- **Frame Rate:** 30 FPS
- **Maximum Frames:** 257 (divisible by 8+1)
- **Duration:** Up to 60 seconds
- **Resolution divisibility:** Must be divisible by 32

## Upscaling Pipeline

- Compatible with LTXLatentUpsamplePipeline for 2x spatial upscaling
- Supports denoising-based refinement after upsampling
- VAE tiling enabled for memory efficiency: `pipe.vae.enable_tiling()`

## Conditioning Types

- Image-to-video (primary)
- Video-to-video
- Multiple conditions (images + video segments with target frame numbers)
- Conditioning strength adjustable per element (default: 1.0)

## Availability

- **Hugging Face:** https://huggingface.co/Lightricks/LTX-Video-0.9.8-13B-distilled
- **Fal.ai:** https://fal.ai/models/fal-ai/ltxv-13b-098-distilled
- **Replicate:** Available via lucataco/ltx-video-0.9.8-distilled
- **ComfyUI:** Full workflow support with dedicated nodes
- **Config file:** configs/ltxv-13b-0.9.8-distilled.yaml

## Known Issues

- Some users reported that generated videos make human skin appear plastic-like (GitHub Issue #230)
