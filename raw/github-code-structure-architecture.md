# LTX Video Code Structure and Architecture

> **Sources:**
> - <https://github.com/Lightricks/LTX-Video> (LTX-Video original)
> - <https://github.com/Lightricks/LTX-2> (LTX-2 monorepo)
> - <https://arxiv.org/abs/2501.00103> (LTX-Video paper)
> - <https://arxiv.org/abs/2601.03233> (LTX-2 paper)

---

## LTX-Video (Original) Code Structure

```
LTX-Video/
├── .github/workflows/          # CI/CD pipelines
├── configs/                     # Model configuration YAMLs (per model variant)
├── docs/_static/               # Example GIFs and documentation assets
├── ltx_video/                  # Core Python package
│   ├── __init__.py
│   ├── inference.py            # Main inference API
│   ├── models/                 # Core model definitions (DiT, VAE)
│   ├── pipelines/              # Processing pipeline implementations
│   ├── schedulers/             # Noise scheduling components
│   └── utils/                  # Shared helper functions
├── tests/                      # Test suite
├── inference.py                # CLI inference entry point
├── pyproject.toml              # Package & dependency configuration
├── LICENSE                     # OpenRail-M
└── README.md
```

**Key dependencies:** transformers, diffusers, PyTorch, mediapy

---

## LTX-2 Monorepo Code Structure

LTX-2 is organized as a monorepo with three independent packages:

```
LTX-2/
├── packages/
│   ├── ltx-core/               # Core model & inference primitives
│   │   └── src/ltx_core/
│   │       ├── components/      # Diffusion components
│   │       │   # Schedulers, guiders, noisers, patchifiers
│   │       │   # Standard diffusion protocols
│   │       ├── conditioning/    # Latent preparation & conditioning
│   │       ├── guidance/        # Attention control perturbations
│   │       ├── loader/          # Model loading & LoRA fusion
│   │       │   # SingleGPUModelBuilder for .safetensors
│   │       │   # Multi-LoRA adapter fusion with memory staging
│   │       ├── model/
│   │       │   ├── transformer/  # Main diffusion transformer
│   │       │   │   # 14B-param video stream
│   │       │   │   # 5B-param audio stream
│   │       │   │   # Bidirectional cross-modal attention
│   │       │   ├── video_vae/    # Video encoder/decoder
│   │       │   ├── audio_vae/    # Audio encoder/decoder & vocoder
│   │       │   │   # Causal audio VAE producing 1D latent space
│   │       │   └── upsampler/    # Spatial upscaling (x1.5, x2)
│   │       ├── quantization/    # FP8 quantization backends
│   │       │   # TensorRT-LLM scaled matrix multiplication
│   │       │   # Simple FP8 casting approach
│   │       └── text_encoders/
│   │           └── gemma/       # Gemma 3 text encoder (12B)
│   │               # Multi-layer feature extraction
│   │               # Separate embeddings for video vs audio
│   ├── ltx-pipelines/          # High-level inference pipelines
│   │   # 8 ready-to-use pipelines (see LTX-2 doc)
│   │   # TI2VidTwoStages, Distilled, ICLora, etc.
│   └── ltx-trainer/            # Training & fine-tuning
│       # LoRA training
│       # Full fine-tuning
│       # IC-LoRA training
├── pyproject.toml
├── uv.lock
├── README.md
└── LICENSE
```

---

## Model Architecture Deep Dive

### Diffusion Transformer (DiT) Core

LTX-Video/LTX-2 is built on the **Diffusion Transformer (DiT)** architecture, combining iterative refinement from diffusion models with sequence modeling power of transformers.

### LTX-Video (Original) Architecture

**Key innovation -- holistic design:**
1. The Video-VAE and denoising transformer are integrated as interdependent components, not separate modules
2. The patchifying operation is relocated from the transformer's input to the VAE's input
3. This enables full spatiotemporal self-attention in a highly compressed latent space

**Video-VAE:**
- Compression ratio: **1:192**
- Spatiotemporal downscaling: **32 x 32 x 8 pixels per token**
- Latent depth: **128 channels**
- The VAE decoder performs both latent-to-pixel conversion AND the final denoising step simultaneously
- Produces clean output directly in pixel space, preserving fine details without a separate upsampler

**Transformer:**
- Full spatiotemporal self-attention over compressed latents
- Efficient due to high compression ratio
- Supports text conditioning for T2V and image conditioning for I2V
- Guidance: Classifier-Free Guidance (CFG) and Spatiotemporal Guidance (STG)

### LTX-2 Architecture (Evolution)

**Dual-Stream Transformer:**
- **Video stream:** 14B parameters -- handles spatial detail, motion consistency, temporal coherence
- **Audio stream:** 5B parameters -- manages sound generation, dialogue timing, environmental audio
- **Communication:** Bidirectional cross-attention layers between streams

**Audio VAE:**
- Causal audio Variational AutoEncoder
- Produces high-fidelity 1D latent space optimized for diffusion training/inference

**Text Conditioning (Enhanced):**
- Multi-token prediction module for improved prompt understanding and semantic stability
- **4x larger text connector** between language encoder (Gemma 3, 12B) and DiT backbone
- Separate embeddings optimized for video vs. audio conditioning

**Multimodal Backbone:**
- Shared attention mechanism across all modalities
- Text, image, and audio inputs each encoded into token representations
- All tokens concatenated into a single sequence
- Transformer self-attention attends across all modalities simultaneously
- Enables cross-modal relationship learning

**LoRA Support:**
- Low-Rank Adaptation for efficient customization
- Trainable rank decomposition matrices injected while base weights stay frozen
- 10+ control LoRA variants (camera, pose, motion, detail)

---

## Key Technical Parameters

| Parameter | LTX-Video | LTX-2 |
|-----------|-----------|-------|
| Architecture | DiT | Dual-Stream DiT |
| Model Size | 2B / 13B | 19B (14B video + 5B audio) / 22B (LTX-2.3) |
| Compression Ratio | 1:192 | Similar |
| VAE Downscaling | 32x32x8 | Similar + audio VAE |
| Latent Channels | 128 | Similar |
| Text Encoder | T5-based | Gemma 3 (12B) |
| Audio Support | No (added later) | Native synchronized |
| Max Resolution | 4K | 4K |
| Max FPS | 50 | 50 |
| Max Duration | 60s (v0.9.8) | 10s continuous with audio |
| Default Resolution | 1216x704 @ 30fps | Variable |

---

## Performance Benchmarks

**LTX-Video (Original):**
- 5 seconds of 24fps video at 768x512 in **2 seconds** on NVIDIA H100
- "Faster-than-real-time generation" -- outperforms all similar-scale models
- Distilled variants: **15x faster** inference with minimal quality loss
- LTX-2 claims **up to 50% lower compute cost** than competing models

**FP8 Quantization:**
- Reduces memory footprint significantly
- Dedicated Q8 CUDA kernels (Lightricks/LTX-Video-Q8-Kernels) for Ada architecture GPUs
- Supported via TensorRT-LLM scaled matrix multiplication or simple casting

---

## Dependency Stack

### LTX-Video
- Python 3.10.5+
- PyTorch >= 2.1.2
- CUDA 12.2 (or MPS on macOS)
- transformers, diffusers, mediapy
- Optional: LTX-Video-Q8-Kernels for FP8

### LTX-2
- Python (managed via `uv`)
- PyTorch with CUDA 12.8+
- Optional: xFormers, Flash Attention 2/3
- Gemma 3 text encoder weights
- safetensors format checkpoints
