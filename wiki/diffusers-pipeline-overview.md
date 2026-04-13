---
title: Diffusers Pipeline Overview
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/docs/diffusers/main/api/pipelines/ltx_video
  - https://huggingface.co/docs/diffusers/main/api/pipelines/ltx2
  - https://github.com/huggingface/diffusers/blob/main/docs/source/en/api/pipelines/ltx_video.md
tags:
  - diffusers
  - pipeline
  - api
  - python
  - overview
---

# Diffusers Pipeline Overview

LTX Video has first-class integration with the HuggingFace Diffusers library. This page catalogs all available pipeline classes for both LTX-Video (v1) and LTX-2.

## LTX-Video (v1) Pipelines

### LTXPipeline -- Text-to-Video

The base pipeline for text-to-video generation. Works with the original 2B model.

- **Import:** `from diffusers import LTXPipeline`
- **Model:** `Lightricks/LTX-Video`
- **Details:** [[ltx-pipeline-text-to-video]]

### LTXImageToVideoPipeline -- Image-to-Video

Generates video conditioned on an input image using the original API.

- **Import:** `from diffusers import LTXImageToVideoPipeline`
- **Model:** `Lightricks/LTX-Video`
- **Details:** [[ltx-image-to-video-pipeline]]

### LTXConditionPipeline -- Multi-Condition (0.9.5+)

The most flexible pipeline, supporting text, image, and video conditioning with frame-level control. Also supports text-only generation without passing `conditions`.

- **Import:** `from diffusers import LTXConditionPipeline`
- **Condition class:** `LTXVideoCondition`
- **Models:** `Lightricks/LTX-Video-0.9.5` and all later versions
- **Details:** [[ltx-condition-pipeline]]

### LTXLatentUpsamplePipeline -- Latent Upscaler (0.9.7+)

Upscales latent representations by 2x for higher resolution output. Used as Stage 2 in the [[two-stage-inference-pattern]].

- **Import:** `from diffusers import LTXLatentUpsamplePipeline`
- **Model:** `Lightricks/ltxv-spatial-upscaler-0.9.7` or `0.9.8`
- **Details:** [[ltx-latent-upsample-pipeline]]

### LTXI2VLongMultiPromptPipeline -- Long Video + Multi-Prompt

Generates long-duration videos using temporal sliding windows and per-window prompt scheduling.

- **Import:** `from diffusers import LTXI2VLongMultiPromptPipeline`
- **Model:** `Lightricks/LTX-Video-0.9.8-13B-distilled`
- **Details:** [[ltx-long-multi-prompt-pipeline]]

## LTX-2 Pipelines

### LTX2Pipeline

Main pipeline for LTX-2 audio-video generation.

- **Import:** `from diffusers.pipelines.ltx2 import LTX2Pipeline`
- **Model:** `Lightricks/LTX-2`

### LTX2LatentUpsamplePipeline

Upsampler for LTX-2 latents.

- **Import:** `from diffusers.pipelines.ltx2 import LTX2LatentUpsamplePipeline`

## Pipeline Component Architecture

Every LTX Video pipeline consists of five shared components:

1. **[[ltxv-transformer-architecture|Transformer]]** (`LTXVideoTransformer3DModel`) -- the core diffusion model
2. **[[ltxv-video-vae|VAE]]** (`AutoencoderKLLTXVideo`) -- video encoder/decoder with 1:192 compression
3. **Text Encoder** (`T5EncoderModel`) -- Google T5 v1.1 XXL
4. **Tokenizer** (`T5TokenizerFast`)
5. **[[ltxv-schedulers|Scheduler]]** (`FlowMatchEulerDiscreteScheduler`)

Components can be loaded independently:

```python
from diffusers import AutoModel, AutoencoderKLLTXVideo
import torch

transformer = AutoModel.from_pretrained(
    "Lightricks/LTX-Video", subfolder="transformer", torch_dtype=torch.bfloat16
)
vae = AutoencoderKLLTXVideo.from_pretrained(
    "Lightricks/LTX-Video", subfolder="vae", torch_dtype=torch.float32
)
```

## Shared Parameters

All LTX-Video pipelines accept these common parameters:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `prompt` | None | Text prompt(s) guiding generation |
| `negative_prompt` | None | Prompts to suppress undesired content |
| `height` | 512 | Output height in pixels (divisible by 32) |
| `width` | 704 | Output width in pixels (divisible by 32) |
| `num_frames` | 161 | Video frames to generate (8n+1 pattern) |
| `num_inference_steps` | 50 | Denoising steps |
| `guidance_scale` | 3.0 | CFG scale; 1.0 for distilled models |
| `output_type` | "pil" | "pil", "np", "pt", or "latent" |
| `decode_timestep` | 0.0 | VAE decode timestep (0.05 for v0.9.1+) |
| `decode_noise_scale` | None | Noise interpolation at decode (0.025 for v0.9.1+) |
| `max_sequence_length` | 128 | Max tokenizer length (256 for LTXConditionPipeline) |

## Utility Functions

```python
from diffusers.utils import (
    export_to_video,    # Export PIL frames to MP4
    load_image,         # Load image from path or URL
    load_video,         # Load video as list of PIL frames
)
```

## Recommended Settings by Model Type

| Setting | Distilled Model | Full Model |
|---------|----------------|------------|
| `guidance_scale` | 1.0 | 5.0 |
| `decode_timestep` (v0.9.1+) | 0.05 | 0.05 |
| `image_cond_noise_scale` (v0.9.1+) | 0.025 | 0.025 |
| `dtype` | torch.bfloat16 | torch.bfloat16 |

## See Also

- [[python-installation-setup]] -- how to install Diffusers
- [[ltxv-model-variants]] -- which model to use with which pipeline
- [[ltxv-memory-optimization]] -- reducing VRAM usage
- [[two-stage-inference-pattern]] -- recommended generation workflow
- [[ltx-native-pytorch-api]] -- alternative native PyTorch API
