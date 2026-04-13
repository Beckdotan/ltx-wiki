# Hugging Face Model Card: LTX-2.3 (Lightricks/LTX-2.3)

## Sources
- https://huggingface.co/Lightricks/LTX-2.3
- https://huggingface.co/Lightricks/LTX-2.3-fp8
- https://ltx.io/model/ltx-2-3
- https://github.com/Lightricks/LTX-2

---

## Overview

LTX-2.3 is a DiT-based audio-video foundation model developed by Lightricks. It is a significant update to LTX-2 with improved audio and visual quality and enhanced prompt adherence. It generates synchronized video and audio within a single model, with open weights and practical local execution capabilities.

- **Developer:** Lightricks
- **Model Type:** Diffusion-based audio-video foundation model
- **Parameters:** 22B
- **Language:** English
- **Paper:** [LTX-2: Efficient Joint Audio-Visual Foundation Model](https://arxiv.org/abs/2601.03233)
- **License:** LTX-2 Community License Agreement
- **Monthly downloads:** 1,658,413+
- **Spaces using LTX-2.3:** 81+
- **Adapters:** 20 models
- **Finetunes:** 27 models
- **Quantizations:** 14 models

---

## Model Checkpoints

| Name | Description |
|------|-------------|
| `ltx-2.3-22b-dev` | Full model, flexible and trainable in bf16 |
| `ltx-2.3-22b-distilled` | Distilled version, 8 steps, CFG=1 |
| `ltx-2.3-22b-distilled-lora-384` | LoRA version applicable to full model |
| `ltx-2.3-spatial-upscaler-x2-1.1` | 2x spatial upscaler for higher resolution |
| `ltx-2.3-spatial-upscaler-x1.5-1.0` | 1.5x spatial upscaler |
| `ltx-2.3-temporal-upscaler-x2-1.0` | 2x temporal upscaler for higher FPS |

---

## Technical Specifications

- **Framework:** PyTorch >= 2.7
- **Python:** >= 3.12
- **CUDA:** > 12.7
- Width and height: Must be divisible by 32
- Frame count: Must be divisible by 8 + 1

---

## Supported Tasks

- Text-to-video
- Image-to-video
- Video-to-video
- Image-text-to-video
- Audio-to-video
- Text-to-audio
- Video-to-audio
- Audio-to-audio
- Multi-modal combinations

---

## Installation

### PyTorch Codebase

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync
source .venv/bin/activate
```

### Repository Structure (Monorepo)
- `ltx-core`: Model definition and architecture
- `ltx-pipelines`: Inference pipelines
- `ltx-trainer`: Training capabilities

### ComfyUI
Built-in LTXVideo nodes available via ComfyUI Manager. See https://docs.ltx.video/open-source-model/integration-tools/comfy-ui

### Diffusers
Support via Hugging Face Diffusers library: https://huggingface.co/docs/diffusers/

---

## Required Downloads for Local Inference

From https://huggingface.co/Lightricks/LTX-2.3:

**Core Models:**
- `ltx-2.3-22b-dev.safetensors` (development version)
- `ltx-2.3-22b-distilled.safetensors` (distilled variant)

**Spatial Upscalers:**
- `ltx-2.3-spatial-upscaler-x2-1.1.safetensors`
- `ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors`

**Temporal Upscaler:**
- `ltx-2.3-temporal-upscaler-x2-1.0.safetensors`

**Text Encoder:**
- Gemma 3 12B text encoder (quantized)

**Control LoRAs:**
- Camera control
- Pose control
- Motion tracking
- Union control (depth/edges)

---

## API Access

- **API Playground:** https://console.ltx.video/playground/
- **API variants:** `ltx-2-3-fast` (rapid iteration) and `ltx-2-3-pro` (production quality)
- **Resolutions:** Both 720p and 1080p supported
- **ComfyUI and Fal:** Fully supported

---

## Training

The base (dev) model is fully trainable.

### LoRA Training
- Duration: Under 1 hour for motion, style, or likeness training
- Instructions: https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md
- Supports motion, style, and sound+appearance training

---

## Limitations

- Not intended for factual information generation
- May amplify existing societal biases
- May fail to match prompts perfectly
- Can generate inappropriate or offensive content
- Lower audio quality when generating audio without speech
- Prompt adherence variable based on prompting style

---

## Citation

```bibtex
@article{hacohen2025ltx2,
  title={LTX-2: Efficient Joint Audio-Visual Foundation Model},
  author={HaCohen, Yoav and Brazowski, Benny and Chiprut, Nisan and others},
  journal={arXiv preprint arXiv:2601.03233},
  year={2025}
}
```
