---
title: "LTX-2.3 HuggingFace Model Card"
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/Lightricks/LTX-2.3
  - https://huggingface.co/Lightricks/LTX-2.3-fp8
  - https://ltx.io/model/ltx-2-3
  - https://github.com/Lightricks/LTX-2
tags:
  - huggingface
  - model-card
  - ltx-2-3
  - open-source
  - diffusion-transformer
  - audio-video
  - lightricks
---

# LTX-2.3 HuggingFace Model Card

LTX-2.3 is the latest DiT-based audio-video foundation model from [[lightricks]], a significant update to [[ltx-2-huggingface|LTX-2]] with improved audio quality, visual fidelity, and prompt adherence. It has 22 billion parameters and generates synchronized video and audio within a single model.

## Key Facts

| Property | Value |
|----------|-------|
| Developer | Lightricks |
| Architecture | Diffusion Transformer (DiT) |
| Parameters | 22B |
| Language | English prompts |
| License | LTX-2 Community License Agreement |
| Paper | [LTX-2: Efficient Joint Audio-Visual Foundation Model](https://arxiv.org/abs/2601.03233) |
| Monthly downloads | 1,658,413+ |
| Spaces | 81+ |
| Adapters | 20 |
| Fine-tunes | 27 |
| Quantizations | 14 |
| GitHub | https://github.com/Lightricks/LTX-2 |

## Model Checkpoints

| Checkpoint | Description |
|-----------|-------------|
| `ltx-2.3-22b-dev` | Full model, flexible and trainable in bf16 |
| `ltx-2.3-22b-distilled` | Distilled version, 8 steps, CFG=1 |
| `ltx-2.3-22b-distilled-lora-384` | LoRA version applicable to full model |
| `ltx-2.3-spatial-upscaler-x2-1.1` | 2x spatial upscaler |
| `ltx-2.3-spatial-upscaler-x1.5-1.0` | 1.5x spatial upscaler |
| `ltx-2.3-temporal-upscaler-x2-1.0` | 2x temporal upscaler for higher FPS |

## Improvements Over LTX-2

- Improved audio quality
- Enhanced visual fidelity
- Better prompt adherence
- Increased parameter count (22B vs 19B)
- Additional 1.5x spatial upscaler option

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

## Technical Requirements

| Property | Value |
|----------|-------|
| Python | >= 3.12 |
| CUDA | > 12.7 |
| PyTorch | >= 2.7 |
| Resolution | Width/height divisible by 32 |
| Frames | Divisible by 8+1 |

## Installation

Shares the same codebase as LTX-2:

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

Also available via ComfyUI (built-in LTXVideo nodes) and Hugging Face Diffusers.

## Required Downloads for Local Inference

From https://huggingface.co/Lightricks/LTX-2.3:

- **Core models**: `ltx-2.3-22b-dev.safetensors`, `ltx-2.3-22b-distilled.safetensors`
- **Spatial upscalers**: `ltx-2.3-spatial-upscaler-x2-1.1.safetensors`, `ltx-2.3-spatial-upscaler-x1.5-1.0.safetensors`
- **Temporal upscaler**: `ltx-2.3-temporal-upscaler-x2-1.0.safetensors`
- **Text encoder**: Gemma 3 12B (quantized)
- **Control LoRAs**: Camera control, pose control, motion tracking, union control (depth/edges)

## Training and Fine-tuning

The base `dev` model is fully trainable. LoRA training for motion, style, or likeness (sound+appearance) can complete in under 1 hour using the [LTX-2 Trainer](https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md).

## API Access

LTX-2.3 powers the `ltx-2-3-fast` and `ltx-2-3-pro` model tiers on the [[ltx-video-api]]. See [[ltx-video-api-models]] for API-specific details and [[ltx-video-api-pricing]] for costs.

The API playground is available at https://console.ltx.video/playground/.

## Citation

```bibtex
@article{hacohen2025ltx2,
  title={LTX-2: Efficient Joint Audio-Visual Foundation Model},
  author={HaCohen, Yoav and Brazowski, Benny and Chiprut, Nisan and others},
  journal={arXiv preprint arXiv:2601.03233},
  year={2025}
}
```

## Limitations

- Not intended for factual information generation.
- May amplify existing societal biases.
- May fail to match prompts perfectly.
- Can generate inappropriate or offensive content.
- Lower audio quality when generating audio without speech.
- Prompt adherence variable based on prompting style.

## Related Pages

- [[ltx-video-huggingface]]
- [[ltx-2-huggingface]]
- [[ltx-video-api]]
- [[ltx-video-api-models]]
- [[ltx-video-api-pricing]]
- [[ltx-studio]]
