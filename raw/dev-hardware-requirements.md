# LTX-Video Hardware Requirements

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Date:** 2026-04-13

---

## Overview

LTX-Video has different hardware requirements depending on the model variant chosen. This document covers GPU VRAM requirements, software dependencies, and optimization strategies.

---

## GPU VRAM Requirements by Model

| Model Variant | Minimum VRAM | Recommended VRAM | Notes |
|---------------|-------------|-----------------|-------|
| 13B Dev | 16GB | 24GB+ | Full quality, highest resource usage |
| 13B Distilled | 16GB | 24GB+ | Similar to dev, fewer inference steps |
| 13B FP8 | 12GB | 16GB+ | Quantized, ~50% memory savings |
| 2B Distilled | 8GB | 12GB+ | Lightweight |
| 2B Distilled FP8 | 6GB | 8GB+ | Minimum resource model |
| 2B v0.9.6 Distilled | 8GB | 12GB+ | Real-time capable |

---

## Software Requirements

| Dependency | Minimum Version | Notes |
|-----------|----------------|-------|
| Python | 3.10.5+ | Required |
| PyTorch | 2.1.2+ | CUDA support required |
| CUDA | 12.2+ | For GPU inference |
| diffusers | 0.32.0+ (or latest git) | For pipeline API |
| transformers | Latest | For text encoder |
| accelerate | Latest | For device management |

---

## Compatible GPUs

### NVIDIA Consumer GPUs
| GPU | VRAM | Compatible Models |
|-----|------|-------------------|
| RTX 4090 | 24GB | All variants |
| RTX 4080 | 16GB | 13B FP8, all 2B variants |
| RTX 4070 Ti | 12GB | 13B FP8, all 2B variants |
| RTX 4070 | 12GB | 13B FP8, all 2B variants |
| RTX 4060 Ti 16GB | 16GB | 13B FP8, all 2B variants |
| RTX 4060 | 8GB | 2B variants only |
| RTX 3090 | 24GB | All variants |
| RTX 3080 | 10-12GB | 13B FP8, all 2B variants |
| RTX 3070 | 8GB | 2B variants only |

### NVIDIA Data Center GPUs
| GPU | VRAM | Compatible Models |
|-----|------|-------------------|
| A100 | 40/80GB | All variants (recommended for production) |
| A10G | 24GB | All variants |
| A40 | 48GB | All variants |
| L4 | 24GB | All variants |
| T4 | 16GB | 13B FP8, all 2B variants |
| H100 | 80GB | All variants (fastest) |

---

## Memory Optimization Strategies

### 1. VAE Tiling
Reduces peak VRAM during video decoding:
```python
pipe.vae.enable_tiling()
```

### 2. Model CPU Offloading
Moves model components to CPU when not in use:
```python
pipe.enable_model_cpu_offload()
```

### 3. Sequential CPU Offloading
More aggressive offloading (slower but lower VRAM):
```python
pipe.enable_sequential_cpu_offload()
```

### 4. FP8 Quantization
Use FP8 model variants for ~50% memory reduction with minimal quality loss.

### 5. Reduce Resolution
Generate at lower resolution and use the latent upscaler:
- Generate at 2/3 target resolution
- Upscale with `LTXLatentUpsamplePipeline`
- Denoise at full resolution

### 6. Reduce Frame Count
Fewer frames = less memory:
- 33 frames (~1.4s at 24fps) uses much less than 97 frames (~4s)
- Use temporal upscaler if more frames needed

---

## Performance Benchmarks (Approximate)

### Generation Speed (97 frames, 768x512)
| GPU | 13B Dev (30 steps) | 13B Distilled (20 steps) | 2B Distilled (20 steps) |
|-----|--------------------|--------------------------| ----------------------|
| RTX 4090 | ~45s | ~25s | ~10s |
| RTX 3090 | ~60s | ~35s | ~15s |
| A100 80GB | ~30s | ~20s | ~8s |

*Times are approximate and depend on specific configuration.*

### 2B v0.9.6 Distilled (Real-time Capable)
- Claims 15x faster than base model
- No STG/CFG required (single forward pass)
- Can achieve real-time generation on high-end GPUs

---

## Cloud GPU Options

For users without a local GPU:

| Provider | GPU | Hourly Cost (approx) |
|----------|-----|---------------------|
| AWS | A10G, A100 | $1-5/hour |
| Google Cloud | A100, T4 | $1-4/hour |
| Lambda Labs | A100, H100 | $1-3/hour |
| RunPod | Various | $0.5-3/hour |
| Vast.ai | Various | $0.3-2/hour |

---

## Related Links

- **Model Hub:** https://huggingface.co/Lightricks/LTX-Video
- **GitHub:** https://github.com/Lightricks/LTX-Video
- **Diffusers Optimization Docs:** https://huggingface.co/docs/diffusers/optimization/opt_overview
