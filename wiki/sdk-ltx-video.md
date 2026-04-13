---
title: SDK - LTX-Video (Original Repository)
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://github.com/Lightricks/LTX-Video
  - https://pypi.org/project/ltx-video/
  - https://docs.ltx.video/open-source-model/integration-tools/pytorch-api
tags:
  - sdk
  - ltx-video
  - python
  - installation
  - cli
---

# SDK - LTX-Video (Original Repository)

The official LTX-Video repository provides the Python inference package and scripts for the LTX-Video model family (up to 0.9.8). For the newer LTX-2/LTX-2.3 models, see [[sdk-ltx-2]].

- **Repository:** https://github.com/Lightricks/LTX-Video
- **PyPI:** https://pypi.org/project/ltx-video/

## Installation

### From GitHub (Recommended)

```bash
git clone https://github.com/Lightricks/LTX-Video.git
cd LTX-Video
python -m venv env
source env/bin/activate
python -m pip install -e .[inference-script]
```

### From PyPI

```bash
pip install ltx-video
```

### Requirements

- Python >= 3.10.5
- CUDA 12.2+
- PyTorch >= 2.1.2
- macOS: PyTorch 2.3.0 for MPS support

### Key Dependencies

- torch >= 2.1.0
- diffusers >= 0.28.2
- transformers >= 4.47.2
- sentencepiece >= 0.1.96
- huggingface-hub ~= 0.30

## Model Variants

| Model | Config File | Use Case |
|-------|-------------|----------|
| ltxv-13b-0.9.8-dev | `ltxv-13b-0.9.8-dev.yaml` | Highest quality, max VRAM |
| ltxv-13b-0.9.8-distilled | `ltxv-13b-0.9.8-distilled.yaml` | Faster, lower quality |
| ltxv-2b-0.9.8-distilled | `ltxv-2b-0.9.8-distilled.yaml` | Smallest, rapid iterations |
| FP8 variants | `*-fp8.yaml` | Quantized versions |

## Command-Line Usage

### Image-to-Video

```bash
python inference.py \
  --prompt "PROMPT" \
  --conditioning_media_paths IMAGE_PATH \
  --conditioning_start_frames 0 \
  --height HEIGHT --width WIDTH \
  --num_frames NUM_FRAMES \
  --seed SEED \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### Video Extension

```bash
python inference.py \
  --prompt "PROMPT" \
  --conditioning_media_paths VIDEO_PATH \
  --conditioning_start_frames START_FRAME \
  --height HEIGHT --width WIDTH \
  --num_frames NUM_FRAMES \
  --seed SEED \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

### Multi-Condition Generation

```bash
python inference.py \
  --prompt "PROMPT" \
  --conditioning_media_paths PATH1 PATH2 \
  --conditioning_start_frames FRAME1 FRAME2 \
  --height HEIGHT --width WIDTH \
  --num_frames NUM_FRAMES \
  --seed SEED \
  --pipeline_config configs/ltxv-13b-0.9.8-distilled.yaml
```

## Python Library API

```python
from ltx_video.inference import infer, InferenceConfig

infer(
    InferenceConfig(
        pipeline_config="configs/ltxv-13b-0.9.8-distilled.yaml",
        prompt="A cute penguin reading a book",
        height=480,
        width=832,
        num_frames=96,
        output_path="output.mp4",
    )
)
```

## Features

- Text-to-video generation
- Image-to-video transformation
- Video-to-video conversion
- Multi-keyframe conditioning
- Video extension (forward/backward)
- Synchronized audio+video generation
- Multiple resolution support
- Real-time capable with distilled models
- LoRA fine-tuning support
- IC-LoRA control models (depth, pose, canny)

## See Also

- [[sdk-ltx-2]] -- Newer LTX-2 monorepo with additional pipelines
- [[diffusers-integration]] -- Diffusers library integration
- [[python-conditioning]] -- Conditioning types and usage
