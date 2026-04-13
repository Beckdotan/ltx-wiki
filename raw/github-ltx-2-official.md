# Lightricks/LTX-2 -- Official Repository

> **Source:** <https://github.com/Lightricks/LTX-2>
> **HuggingFace:** <https://huggingface.co/Lightricks/LTX-2>
> **Paper:** <https://arxiv.org/abs/2601.03233>
> **Playground:** <https://console.ltx.video/playground>

## Repository Stats

| Metric | Value |
|--------|-------|
| Stars | ~5,800 |
| Forks | ~891 |
| Watchers | 58 |
| Primary Language | Python (100%) |
| Total Commits | 27 |
| License | Available (see repo) |

## Description

LTX-2 is the official successor to LTX-Video. It is described as "the first DiT-based audio-video foundation model that contains all core capabilities of modern video generation in one model: synchronized audio and video, high fidelity, multiple performance modes, production-ready outputs, API access, and open access."

Released in January 2026 with fully open weights, inference pipelines, and training code.

## Repository Structure (Monorepo)

```
LTX-2/
├── packages/
│   ├── ltx-core/              # Core model implementations & inference primitives
│   │   └── src/ltx_core/
│   │       ├── components/     # Schedulers, guiders, noisers, patchifiers
│   │       ├── conditioning/   # Latent preparation & conditioning
│   │       ├── guidance/       # Attention control perturbations
│   │       ├── loader/         # Model loading & LoRA fusion
│   │       ├── model/
│   │       │   ├── transformer/  # Main diffusion transformer (14B video + 5B audio)
│   │       │   ├── video_vae/    # Video encoder/decoder
│   │       │   ├── audio_vae/    # Audio encoder/decoder & vocoder
│   │       │   └── upsampler/    # Spatial upscaling
│   │       ├── quantization/   # FP8 quantization backends
│   │       └── text_encoders/gemma/  # Gemma 3 text encoder
│   ├── ltx-pipelines/         # High-level ready-to-use inference pipelines
│   └── ltx-trainer/           # Training & fine-tuning tools (LoRA, full ft)
├── pyproject.toml
├── uv.lock
├── README.md
└── LICENSE
```

## Key Architecture

- **Dual-Stream Transformer:** 14B-parameter video stream + 5B-parameter audio stream
- **Bidirectional cross-modal attention** for synchronized generation
- **Text Encoder:** Gemma 3 (12B) with multi-layer feature extraction, producing separate embeddings for video vs. audio conditioning
- **4x larger text connector** between language encoder and DiT backbone (vs LTX-Video)
- **Audio VAE:** Causal audio Variational AutoEncoder producing a high-fidelity 1D latent space
- **Multimodal backbone:** Shared attention mechanism across text, image, and audio tokens concatenated into a single sequence

## Available Pipelines (8 total)

| Pipeline | Purpose | Notes |
|----------|---------|-------|
| TI2VidTwoStagesPipeline | Text/image-to-video | Recommended, 2x upsampling |
| TI2VidTwoStagesHQPipeline | High-quality two-stage | Uses res_2s sampler |
| TI2VidOneStagePipeline | Quick prototyping | Single-stage generation |
| DistilledPipeline | Fastest inference | 8 predefined sigmas |
| ICLoraPipeline | Video/image transformation | Uses distilled model |
| KeyframeInterpolationPipeline | Keyframe interpolation | Between image keyframes |
| A2VidPipelineTwoStage | Audio-to-video | Audio-conditioned generation |
| RetakePipeline | Selective regeneration | Specific time regions |

## Main Checkpoints

- `ltx-2.3-22b-dev.safetensors` (development version)
- `ltx-2.3-22b-distilled.safetensors` (distilled version)
- Spatial Upscalers: x1.5 and x2 versions
- Temporal Upscaler: x2 frame interpolation
- Distilled LoRA: `ltx-2.3-22b-distilled-lora-384.safetensors`

## Control LoRAs (10+ variants)

Camera control (dolly, jib), pose control, motion tracking, detail enhancement, and more.

## Installation

```bash
git clone https://github.com/Lightricks/LTX-2.git
uv sync --frozen
source .venv/bin/activate
# Optional: uv sync --extra xformers
```

## Optimization Strategies

- DistilledPipeline for speed (8+4 steps)
- FP8 quantization with `fp8-cast` or `fp8-scaled-mm` flags
- Flash Attention 3 for Hopper GPUs
- Reducing inference steps from 40 to 20-30 via gradient estimation
- Disable memory cleanup between stages when VRAM permits

## Links

- Website: <https://ltx.io>
- Documentation: <https://docs.ltx.video>
- Discord: <https://discord.gg/ltxplatform>
- ComfyUI: <https://github.com/Lightricks/ComfyUI-LTXVideo>
