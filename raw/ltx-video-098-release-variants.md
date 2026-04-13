# LTX-Video 0.9.8: Latest Release and All Variants

**Source:** https://huggingface.co/Lightricks/LTX-Video (main model page, lists 0.9.8 as current)

---

## Overview

Version 0.9.8 is the latest major release of LTX-Video as of early 2025. It continues the 13B model line introduced in 0.9.7 and adds a lightweight 2B distilled variant, making the ecosystem more accessible.

Note: Many 0.9.8 model pages on Hugging Face are gated (require authentication to access), so detailed changelogs for this version are limited.

## Available Variants

### 13B Models

| Model | Type | Key Feature |
|-------|------|-------------|
| `ltxv-13b-0.9.8-dev` | Full quality | Highest quality, requires most VRAM |
| `ltxv-13b-0.9.8-mix` | Multi-scale | Balanced speed-quality, multi-scale rendering workflow |
| `ltxv-13b-0.9.8-distilled` | Fast | Faster inference, slight quality reduction |
| `ltxv-13b-0.9.8-fp8` | Quantized | FP8 quantization for better memory efficiency |
| `ltxv-13b-0.9.8-distilled-fp8` | Fast + Quantized | Combined optimizations |

### 2B Models (New in 0.9.8)

| Model | Type | Key Feature |
|-------|------|-------------|
| `ltxv-2b-0.9.8-distilled` | Lightweight | Light VRAM usage, accessible on consumer GPUs |
| `ltxv-2b-0.9.8-distilled-fp8` | Lightweight + Quantized | Most efficient variant overall |

## What Changed from 0.9.7

Based on the model page information:
- **2B distilled variant added:** The 0.9.8 release brings back a 2B model (previously last at 0.9.6), now updated with the latest training improvements
- **Mix mode carried forward:** The multi-scale rendering workflow from 0.9.7 continues
- **ICLoRA adapters updated:** Depth, Pose, Canny, and Detailer adapters available for 0.9.8
- **Upscalers updated:** Spatial and temporal upscalers updated for 0.9.8
- **Quality improvements:** General refinement over 0.9.7

## VRAM Estimates

| Model | Estimated VRAM |
|-------|---------------|
| 13B dev | ~24+ GB |
| 13B distilled | ~16-20 GB |
| 13B FP8 | ~12-16 GB |
| 2B distilled | ~8-12 GB |
| 2B distilled-fp8 | ~6-8 GB |

## Usage with Diffusers

```python
import torch
from diffusers import LTXConditionPipeline, LTXLatentUpsamplePipeline
from diffusers.utils import export_to_video, load_image

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev", 
    torch_dtype=torch.bfloat16
)
pipe.to("cuda")

# Image-to-Video
image = load_image("penguin.png")
video = load_video(export_to_video([image]))
condition = LTXVideoCondition(video=video, frame_index=0)

output = pipe(
    conditions=[condition],
    prompt="A cute little penguin takes out a book and starts reading it",
    negative_prompt="worst quality, inconsistent motion, blurry, jittery, distorted",
    width=832,
    height=480,
    num_frames=96,
    num_inference_steps=30,
    generator=torch.Generator().manual_seed(0),
).frames

export_to_video(output, "output.mp4", fps=24)
```

## CLI Usage

```bash
python inference.py \
  --prompt "A cute little penguin takes out a book and starts reading it" \
  --input_image_path IMAGE_PATH \
  --height 480 \
  --width 832 \
  --num_frames 96 \
  --seed 0 \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

## Online Demos (0.9.8)

- **LTX-Studio (13B-mix):** https://app.ltx.studio/motion-workspace?videoModel=ltxv-13b
- **LTX-Studio (13B distilled):** https://app.ltx.studio/motion-workspace?videoModel=ltxv
- **Fal.ai (13B full):** https://fal.ai/models/fal-ai/ltx-video-13b-dev/image-to-video
- **Fal.ai (13B distilled):** https://fal.ai/models/fal-ai/ltx-video-13b-distilled/image-to-video
- **Replicate:** https://replicate.com/lightricks/ltx-video

## Configuration Files

Available pipeline configs for 0.9.8:
- `configs/ltxv-13b-0.9.8-dev.yaml`
- `configs/ltxv-13b-0.9.8-distilled.yaml`
- `configs/ltxv-13b-0.9.8-mix.yaml`
- `configs/ltxv-2b-0.9.8-distilled.yaml`

## License

All 0.9.8 variants use the **LTX-Video Open-Weights License** (same as 0.9.6+).
