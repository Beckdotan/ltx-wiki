---
title: Installation and Quickstart
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/dev-installation-quickstart.md
  - raw/dev-docs-quickstart-installation.md
  - raw/api-github-repository.md
tags:
  - installation
  - quickstart
  - setup
  - diffusers
  - comfyui
  - api
  - cli
---

# Installation and Quickstart

This page covers all methods for installing and running [[ltx-video-overview|LTX-Video]], from cloud APIs (no installation) to full local setup from source. For LTX-2/2.3 local setup, see the LTX-2 section below.

## All Integration Paths

| Method | Best For | Setup Time |
|--------|----------|------------|
| **Official API** (api.ltx.video) | Production, managed service | Minutes |
| **fal.ai** | Serverless, pay-per-use | Minutes |
| **Replicate** | Quick prototyping | Minutes |
| **Diffusers** (local) | Python developers, customization | 30 min |
| **LTX-Video repo** (local) | Full control, custom pipelines | 30 min |
| **LTX-2 repo** (local) | Audio-video, latest models | 30 min |
| **ComfyUI** | Visual workflow, node-based | 1 hour |
| **LTX Studio** (web) | No-code web interface | Minutes |
| **LTX Desktop** | Desktop app experience | 15 min |

## Method 1: Official API (No Installation)

Sign up at the Developer Console: https://console.ltx.video

### Text-to-Video

```bash
curl -X POST https://api.ltx.video/v1/text-to-video \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "A beautiful sunset over the ocean with gentle waves",
    "model": "ltx-2-3-pro",
    "duration": 8,
    "resolution": "1920x1080"
  }'
```

### Image-to-Video

```bash
curl -X POST https://api.ltx.video/v1/image-to-video \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_uri": "https://example.com/image.jpg",
    "prompt": "The scene comes to life with gentle motion",
    "model": "ltx-2-3-pro",
    "duration": 8,
    "resolution": "1920x1080"
  }'
```

## Method 2: Cloud APIs (fal.ai, Replicate)

### fal.ai (Python)

```bash
pip install fal-client
```

```python
import fal_client

result = fal_client.subscribe("fal-ai/ltx-2/text-to-video", arguments={
    "prompt": "A beautiful sunset over the ocean",
})
```

### fal.ai (JavaScript)

```bash
npm install @fal-ai/client
```

```javascript
import { fal } from "@fal-ai/client";

const result = await fal.queue.submit("fal-ai/ltx-2/text-to-video", {
  input: { prompt: "A beautiful sunset over the ocean" }
});
```

### Replicate

```bash
pip install replicate
```

```python
import replicate

output = replicate.run("lightricks/ltx-video", input={
    "prompt": "A cat playing with a ball of yarn",
})
```

## Method 3: Diffusers Library (Recommended for Local Python)

### Prerequisites

- Python 3.10.5+
- PyTorch >= 2.1.2
- CUDA 12.2+
- GPU with 8GB+ VRAM (2B) or 16GB+ VRAM (13B) -- see [[hardware-requirements]]

### Installation

```bash
python -m venv ltx-env
source ltx-env/bin/activate  # Linux/macOS
# ltx-env\Scripts\activate   # Windows

pip install -U git+https://github.com/huggingface/diffusers
pip install torch>=2.1.2 transformers accelerate
```

### Image-to-Video Example

```python
import torch
from diffusers import LTXConditionPipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition
from diffusers.utils import export_to_video, load_image, load_video

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev",
    torch_dtype=torch.bfloat16
).to("cuda")
pipe.vae.enable_tiling()

image = load_image("your_image.png")
video = load_video(export_to_video([image]))
condition = LTXVideoCondition(video=video, frame_index=0)

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

## Method 4: From Source (CLI)

### LTX-Video (v1)

```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video
python -m venv env
source env/bin/activate
python -m pip install -e ".[inference-script]"
```

```bash
python inference.py \
  --prompt "A cute little penguin takes out a book and starts reading it" \
  --height 480 \
  --width 832 \
  --num_frames 97 \
  --seed 42 \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### LTX-2 / LTX-2.3

Requires Python >= 3.12, CUDA > 12.7, PyTorch ~= 2.7:

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync --frozen
source .venv/bin/activate
```

Download models from https://huggingface.co/Lightricks/LTX-2.3:
- Core model checkpoint (dev or distilled)
- Spatial upscaler
- Temporal upscaler
- Gemma 3 text encoder

### CLI Arguments Reference

| Argument | Description |
|----------|-------------|
| `--prompt` | Text description of desired video |
| `--input_image_path` | Conditioning image (image-to-video) |
| `--conditioning_media_paths` | Multiple conditioning images/videos |
| `--conditioning_start_frames` | Target frame indices for each conditioning media |
| `--height` | Output height (divisible by 32) |
| `--width` | Output width (divisible by 32) |
| `--num_frames` | Number of frames (must be 8n+1) |
| `--seed` | Random seed for reproducibility |
| `--pipeline_config` | Path to YAML pipeline configuration |

### Pipeline Configuration Files

| Config File | Model | Description |
|-------------|-------|-------------|
| `ltxv-13b-0.9.8-dev.yaml` | 13B dev | Highest quality, most VRAM |
| `ltxv-13b-0.9.8-distilled.yaml` | 13B distilled | Balanced speed/quality |
| `ltxv-13b-0.9.8-fp8.yaml` | 13B FP8 | Reduced memory usage |
| `ltxv-2b-0.9.8-distilled.yaml` | 2B distilled | Lightweight |
| `ltxv-2b-0.9.8-distilled-fp8.yaml` | 2B distilled FP8 | Minimal resources |

## Method 5: ComfyUI

```bash
cd ComfyUI/custom_nodes
git clone https://github.com/Lightricks/ComfyUI-LTXVideo.git
cd ComfyUI-LTXVideo
pip install -r requirements.txt
```

Then load one of the pre-built workflow JSON files from the repository and click Queue Prompt.

Alternative: Open ComfyUI Manager (Ctrl+M), search for "LTXVideo", and install the custom nodes directly.

## Method 6: LTX Studio (Web UI)

Visit https://app.ltx.studio/ and sign up. No installation or code required.

## Choosing a Model Variant

| Your Situation | Recommended Model | Model ID |
|---------------|-------------------|----------|
| Best quality, 24GB+ VRAM | 13B Dev | `Lightricks/LTX-Video-0.9.8-dev` |
| Good quality, faster speed | 13B Distilled | `ltxv-13b-0.9.8-distilled` |
| 12-16GB VRAM | 13B FP8 | `ltxv-13b-0.9.8-fp8` |
| 8-12GB VRAM | 2B Distilled | `ltxv-2b-0.9.8-distilled` |
| Under 8GB VRAM | 2B FP8 | `ltxv-2b-0.9.8-distilled-fp8` |
| Real-time generation | 2B v0.9.6 | `ltxv-2b-0.9.6-distilled` |

## Resolution and Frame Count Constraints

### Valid Resolutions (divisible by 32)

- 512 x 768 (landscape)
- 768 x 512 (portrait)
- 480 x 832 (widescreen)
- 832 x 480 (tall)
- 704 x 1216 (HD-like)

### Valid Frame Counts (8n + 1)

9, 17, 25, 33, 41, 49, 57, 65, 73, 81, 89, 97, 105, ..., 257

At 24 FPS: 97 frames is roughly 4 seconds of video. Maximum recommended: 257 frames. Up to 4K for LTX-2/2.3.

## Troubleshooting

### Out of Memory

- Use an FP8 model variant
- Reduce resolution or frame count
- Enable `pipe.vae.enable_tiling()`
- Enable `pipe.enable_model_cpu_offload()`
- Use a 2B model instead of 13B

### Slow Generation

- Use a distilled model variant
- Reduce `num_inference_steps` (20-30 is usually sufficient)
- Use lower resolution with [[upscaler-pipelines]]

### Poor Quality

- Write more descriptive prompts -- see [[prompt-engineering]]
- Increase `num_inference_steps`
- Use the dev (non-distilled) model
- Apply the [[upscaler-pipelines|latent upscaler pipeline]]

## Repository Structure

```
LTX-Video/
  inference.py              # Main inference script
  configs/                  # Pipeline YAML configurations
  src/ltx_video/            # Core library
  pyproject.toml            # Package configuration
```

## Related Projects

- **LTX-Video:** https://github.com/Lightricks/LTX-Video
- **LTX-2:** https://github.com/Lightricks/LTX-2
- **ComfyUI-LTXVideo:** https://github.com/Lightricks/ComfyUI-LTXVideo
- **LTX-Video-Trainer:** https://github.com/Lightricks/LTX-Video-Trainer
- **Diffusers Integration:** https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video

## See Also

- [[hardware-requirements]] -- GPU and VRAM guidance
- [[prompt-engineering]] -- Writing effective prompts
- [[upscaler-pipelines]] -- Spatial and temporal upscaling
- [[apple-silicon-setup]] -- Running on Mac hardware
- [[tutorials-and-community-guides]] -- External tutorials and guides
