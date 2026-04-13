# LTX Video: Developer Documentation, Quickstart, and Installation Guide

## Sources
- https://docs.ltx.video/welcome
- https://docs.ltx.video/quickstart
- https://docs.ltx.video/open-source-model/getting-started/quick-start
- https://docs.ltx.video/open-source-model/integration-tools/pytorch-api
- https://docs.ltx.video/models
- https://github.com/Lightricks/LTX-Video
- https://github.com/Lightricks/LTX-2
- https://ltx.io/model

---

## Official Documentation Hub

The primary documentation lives at **https://docs.ltx.video** and covers:

- API documentation and quickstart
- Authentication and pricing
- Open-source model guides
- Integration tools (ComfyUI, PyTorch API, Diffusers)
- Model reference

---

## Quickstart: Using the Official API

### Step 1: Get an API Key

Sign up at the Developer Console: https://console.ltx.video

### Step 2: Make Your First Request

**Text-to-Video:**
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

**Image-to-Video:**
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

### Step 3: Receive Your Video

Successful operations deliver the video file directly in MP4 format with metadata headers including a unique request identifier.

---

## Quickstart: Local Installation (LTX-Video v1)

### Requirements
- Python 3.10.5+
- CUDA 12.2+ (or PyTorch 2.1.2+)
- macOS: PyTorch 2.3.0 for MPS support

### Steps

```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video
python -m venv env
source env/bin/activate
python -m pip install -e .[inference-script]
```

### First Generation

```bash
python inference.py \
  --prompt "A cute little penguin takes out a book and starts reading it" \
  --height 480 \
  --width 832 \
  --num_frames 96 \
  --seed 0 \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

---

## Quickstart: Local Installation (LTX-2 / LTX-2.3)

### Requirements
- Python >= 3.12
- CUDA > 12.7
- PyTorch ~= 2.7

### Steps

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync --frozen
source .venv/bin/activate
```

### Download Models

From https://huggingface.co/Lightricks/LTX-2.3, download:
- Core model checkpoint (dev or distilled)
- Spatial upscaler
- Temporal upscaler
- Gemma 3 text encoder

---

## Quickstart: Diffusers

### Installation

```bash
pip install -U git+https://github.com/huggingface/diffusers
pip install torch transformers accelerate
```

### First Generation

```python
import torch
from diffusers import LTXPipeline
from diffusers.utils import export_to_video

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
pipeline.to("cuda")

video = pipeline(
    prompt="A woman with brown hair smiles at the camera",
    negative_prompt="worst quality, blurry",
    width=768,
    height=512,
    num_frames=161,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

---

## Quickstart: ComfyUI

### Installation

1. Download ComfyUI from https://www.comfy.org/download
2. Open ComfyUI Manager (Ctrl+M)
3. Install "LTXVideo" custom nodes
4. Download model checkpoints to appropriate folders
5. Load an example workflow and click Queue Prompt

---

## Quickstart: fal.ai

### Python

```bash
pip install fal-client
```

```python
import fal_client

result = fal_client.subscribe("fal-ai/ltx-2/text-to-video", arguments={
    "prompt": "A beautiful sunset over the ocean",
})
```

### JavaScript

```bash
npm install @fal-ai/client
```

```javascript
import { fal } from "@fal-ai/client";

const result = await fal.queue.submit("fal-ai/ltx-2/text-to-video", {
  input: { prompt: "A beautiful sunset over the ocean" }
});
```

---

## Quickstart: Replicate

```bash
pip install replicate
```

```python
import replicate

output = replicate.run("lightricks/ltx-video", input={
    "prompt": "A beautiful sunset over the ocean"
})
```

---

## All Integration Paths Summary

| Method | Best For | Setup Time |
|--------|----------|------------|
| **Official API** (api.ltx.video) | Production, managed service | Minutes |
| **fal.ai** | Serverless, pay-per-use | Minutes |
| **Replicate** | Quick prototyping | Minutes |
| **Segmind** | Serverless API | Minutes |
| **WaveSpeed AI** | Low-latency inference | Minutes |
| **Diffusers** (local) | Python developers, customization | 30 min |
| **LTX-Video repo** (local) | Full control, custom pipelines | 30 min |
| **LTX-2 repo** (local) | Audio-video, latest models | 30 min |
| **ComfyUI** | Visual workflow, node-based | 1 hour |
| **Modal** | Custom serverless deployment | 1 hour |
| **RunPod** | Cloud GPU rental | 1 hour |
| **LTX Desktop** | Desktop app experience | 15 min |

---

## Dimension Constraints (All Methods)

- **Width and Height:** Must be divisible by 32
- **Frame Count:** Must be (8n + 1): 1, 9, 17, 25, 33, 41, 49, 57, 65, 73, 81, 89, 97, 121, 161, 257...
- **Max recommended:** Below 720x1280 resolution, below 257 frames for v1; up to 4K for v2/v2.3

---

## Model Evolution

| Model | Parameters | Key Feature |
|-------|-----------|-------------|
| LTX-Video 0.9.x | 2B-13B | First real-time DiT video model |
| LTX-2 | 19B | Audio-video synchronized generation |
| LTX-2.3 | 22B | Improved quality, prompt adherence, 4K output |
