# LTX-Video Installation & Quickstart Guide

**Source:** https://huggingface.co/Lightricks/LTX-Video, https://github.com/Lightricks/LTX-Video
**Date:** 2026-04-13

---

## Overview

This guide covers all methods for installing and getting started with LTX-Video, from the simplest (cloud APIs) to the most involved (local installation from source).

---

## Method 1: Cloud API (No Installation Required)

### fal.ai
```bash
pip install fal-client
```
```python
import fal_client

result = fal_client.subscribe(
    "fal-ai/ltx-video/image-to-video",
    arguments={
        "prompt": "A cat playing with a ball of yarn",
        "image_url": "https://example.com/cat.jpg",
    },
)
print(result["video"]["url"])
```

### Replicate
```bash
pip install replicate
```
```python
import replicate

output = replicate.run(
    "lightricks/ltx-video",
    input={
        "prompt": "A cat playing with a ball of yarn",
        "image": "https://example.com/cat.jpg",
    }
)
print(output)
```

---

## Method 2: Diffusers Library (Recommended for Local Use)

### Prerequisites
- Python 3.10.5+
- PyTorch >= 2.1.2
- CUDA 12.2+
- GPU with 8GB+ VRAM (2B models) or 16GB+ VRAM (13B models)

### Installation
```bash
# Create virtual environment
python -m venv ltx-env
source ltx-env/bin/activate  # Linux/macOS
# ltx-env\Scripts\activate   # Windows

# Install diffusers (latest from GitHub for newest features)
pip install -U git+https://github.com/huggingface/diffusers
pip install torch>=2.1.2 transformers accelerate
```

### Quickstart: Image-to-Video
```python
import torch
from diffusers import LTXConditionPipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_image, load_video

# Load model (downloads automatically on first run)
pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev",
    torch_dtype=torch.bfloat16
).to("cuda")
pipe.vae.enable_tiling()

# Prepare input image
image = load_image("your_image.png")
video = load_video(export_to_video([image]))
condition = LTXVideoCondition(video=video, frame_index=0)

# Generate video
result = pipe(
    conditions=[condition],
    prompt="A detailed description of the video you want",
    negative_prompt="worst quality, inconsistent motion, blurry",
    width=768,
    height=512,
    num_frames=97,
    num_inference_steps=30,
    generator=torch.Generator().manual_seed(42),
).frames[0]

export_to_video(result, "output.mp4", fps=24)
```

---

## Method 3: From Source (CLI Tool)

### Installation
```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video
python -m venv env
source env/bin/activate
python -m pip install -e ".[inference-script]"
```

### Quickstart: Image-to-Video (CLI)
```bash
python inference.py \
  --prompt "A cat playing with a ball of yarn" \
  --input_image_path cat.png \
  --height 512 \
  --width 768 \
  --num_frames 97 \
  --seed 42 \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

---

## Method 4: ComfyUI

### Installation
```bash
# In your ComfyUI custom_nodes directory
cd ComfyUI/custom_nodes
git clone https://github.com/Lightricks/ComfyUI-LTXVideo.git
cd ComfyUI-LTXVideo
pip install -r requirements.txt
```

Then load one of the pre-built workflow JSON files from the repository.

---

## Method 5: LTX Studio (Web UI, No Code)

Visit https://app.ltx.studio/ and sign up for an account. Use the motion workspace to generate videos through the web interface.

---

## Choosing a Model Variant

| Your Situation | Recommended Model | Config/ID |
|---------------|-------------------|-----------|
| Best quality, have 24GB+ VRAM | 13B Dev | `Lightricks/LTX-Video-0.9.8-dev` |
| Good quality, faster speed | 13B Distilled | `ltxv-13b-0.9.8-distilled` |
| 12-16GB VRAM | 13B FP8 | `ltxv-13b-0.9.8-fp8` |
| 8-12GB VRAM | 2B Distilled | `ltxv-2b-0.9.8-distilled` |
| Under 8GB VRAM | 2B FP8 | `ltxv-2b-0.9.8-distilled-fp8` |
| Real-time generation | 2B v0.9.6 | `ltxv-2b-0.9.6-distilled` |

---

## Resolution and Frame Count Guidelines

### Valid Resolutions (must be divisible by 32)
Common good resolutions:
- 512 x 768 (landscape)
- 768 x 512 (portrait)
- 480 x 832 (widescreen)
- 832 x 480 (tall)
- 704 x 1216 (HD-like)

### Valid Frame Counts (must be 8n + 1)
- 9, 17, 25, 33, 41, 49, 57, 65, 73, 81, 89, 97, 105, ...
- Maximum recommended: 257 frames
- At 24 FPS: 97 frames = ~4 seconds of video

---

## Prompt Tips

1. **Be descriptive** - Include details about scene, lighting, camera movement, colors
2. **Specify style** - Mention "cinematic", "photorealistic", "animated", etc.
3. **Describe motion** - Explicitly state what should move and how
4. **Use negative prompts** - "worst quality, inconsistent motion, blurry, jittery, distorted"

---

## Troubleshooting

### Out of Memory
- Use FP8 model variant
- Reduce resolution or frame count
- Enable `pipe.vae.enable_tiling()`
- Enable `pipe.enable_model_cpu_offload()`
- Use 2B model instead of 13B

### Slow Generation
- Use distilled model variant
- Reduce `num_inference_steps` (20-30 is usually sufficient)
- Use lower resolution with upscaler pipeline

### Poor Quality
- Use more descriptive prompts
- Increase `num_inference_steps`
- Use the dev (non-distilled) model
- Apply the latent upscaler pipeline
