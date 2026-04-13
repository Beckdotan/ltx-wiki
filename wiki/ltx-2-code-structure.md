---
title: LTX-2 Code Structure
type: technical
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/github-code-structure-architecture.md
  - raw/github-ltx-2-official.md
tags:
  - github
  - code-structure
  - ltx-2
  - monorepo
  - architecture
  - python
---

# LTX-2 Code Structure

The [[github-official-repositories|Lightricks/LTX-2]] repository is organized as a monorepo with three independent packages. This represents a significant structural evolution from the [[ltx-video-code-structure|flat LTX-Video layout]], reflecting the increased complexity of joint audio-video generation.

## Monorepo Layout

```
LTX-2/
├── packages/
│   ├── ltx-core/               # Core model & inference primitives
│   │   └── src/ltx_core/
│   │       ├── components/      # Schedulers, guiders, noisers, patchifiers
│   │       ├── conditioning/    # Latent preparation & conditioning
│   │       ├── guidance/        # Attention control perturbations
│   │       ├── loader/          # Model loading & LoRA fusion
│   │       ├── model/
│   │       │   ├── transformer/  # Main diffusion transformer (14B video + 5B audio)
│   │       │   ├── video_vae/    # Video encoder/decoder
│   │       │   ├── audio_vae/    # Audio encoder/decoder & vocoder
│   │       │   └── upsampler/    # Spatial upscaling (x1.5, x2)
│   │       ├── quantization/    # FP8 quantization backends
│   │       └── text_encoders/
│   │           └── gemma/       # Gemma 3 text encoder (12B)
│   ├── ltx-pipelines/          # High-level inference pipelines (8 total)
│   └── ltx-trainer/            # Training & fine-tuning (LoRA, full ft, IC-LoRA)
├── pyproject.toml
├── uv.lock
├── README.md
└── LICENSE
```

## Package Breakdown

### ltx-core

The foundational package containing all model implementations and inference primitives.

**`components/`** -- Standard diffusion protocol components:
- Schedulers, guiders, noisers, patchifiers
- Core building blocks shared across all pipelines

**`conditioning/`** -- Latent preparation and conditioning logic for text, image, audio, and multi-keyframe inputs.

**`guidance/`** -- Attention control perturbations, including the bimodal CFG (see [[ltx-2-architecture]]).

**`loader/`** -- Model loading infrastructure:
- `SingleGPUModelBuilder` for loading `.safetensors` files
- Multi-LoRA adapter fusion with memory staging

**`model/`** -- Core model definitions:
- **`transformer/`** -- The [[ltx-2-architecture|asymmetric dual-stream DiT]]: 14B-parameter video stream + 5B-parameter audio stream with bidirectional cross-modal attention
- **`video_vae/`** -- Video encoder/decoder (inherits 1:192 compression from LTX-Video)
- **`audio_vae/`** -- Causal audio VAE producing 1D latent space, plus vocoder for waveform reconstruction
- **`upsampler/`** -- Spatial upscaling models (x1.5 and x2 variants)

**`quantization/`** -- [[fp8-quantization|FP8 quantization]] backends:
- TensorRT-LLM scaled matrix multiplication (`fp8-scaled-mm`)
- Simple FP8 casting approach (`fp8-cast`)

**`text_encoders/gemma/`** -- Gemma 3 (12B) text encoder:
- Multi-layer feature extraction across all decoder layers
- Separate embeddings optimized for video vs. audio conditioning
- Thinking tokens for improved prompt adherence

### ltx-pipelines

Eight ready-to-use high-level inference pipelines. See [[github-official-repositories#available-pipelines-8-total]] for the full list. These compose ltx-core components into end-to-end generation workflows.

### ltx-trainer

Training and fine-tuning tools:
- [[lora-training|LoRA training]] on LTX-2 and LTX-2.3 models
- Full fine-tuning (base dev model is fully trainable in bf16)
- [[ic-lora|IC-LoRA]] training for video-to-video transformations
- Audio-video joint training support
- Training can complete in under 1 hour for motion, style, or likeness adaptation

See [[ltx-video-trainer]] for more on both the LTX-Video and LTX-2 trainers.

## Dependency Stack

- Python (managed via `uv`)
- PyTorch with CUDA 12.8+
- Optional: xFormers, Flash Attention 2/3
- Gemma 3 text encoder weights
- safetensors format checkpoints

## Optimization Strategies

- DistilledPipeline for speed (8+4 steps)
- FP8 quantization with `fp8-cast` or `fp8-scaled-mm` flags
- Flash Attention 3 for Hopper GPUs
- Reducing inference steps from 40 to 20-30 via gradient estimation
- Disable memory cleanup between stages when VRAM permits

## Key Architectural Differences from LTX-Video

| Aspect | [[ltx-video-code-structure|LTX-Video]] | LTX-2 |
|--------|-----------|-------|
| Structure | Flat Python package | Monorepo with 3 packages |
| Package manager | pip | uv |
| Model streams | Single (video only) | Dual (video + audio) |
| Text encoder | T5-XXL | Gemma 3 (12B) |
| Pipelines | Implicit in `pipelines/` | Explicit `ltx-pipelines` package |
| Trainer | Separate repo (LTX-Video-Trainer) | Bundled in monorepo |

## Related Pages

- [[github-official-repositories]] -- All official repositories
- [[ltx-video-code-structure]] -- Original LTX-Video layout
- [[ltx-2-architecture]] -- Dual-stream DiT architecture details
- [[ltx-2-overview]] -- LTX-2 model overview
- [[ltx-video-trainer]] -- Training tools
