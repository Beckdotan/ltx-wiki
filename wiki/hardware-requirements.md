---
title: Hardware Requirements
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/dev-hardware-requirements.md
  - raw/ltx-video-hardware-requirements-and-inference.md
  - raw/ltx-video-model-sizes-inference-hardware.md
tags:
  - hardware
  - gpu
  - vram
  - performance
  - benchmarks
  - ltx-video
---

# Hardware Requirements

This page covers GPU VRAM requirements, software dependencies, performance benchmarks, and optimization strategies for running [[ltx-video-overview|LTX-Video]] models locally. For LTX-2/2.3 requirements see [[ltx2-system-requirements]].

## GPU VRAM Requirements by Model

| Model Variant | Parameters | Precision | Min VRAM | Recommended VRAM |
|---------------|-----------|-----------|----------|------------------|
| 13B Dev | 13B | BF16 | 24GB | 32GB+ |
| 13B Distilled | 13B | BF16 | 16GB | 24GB+ |
| 13B FP8 | 13B | FP8 | 12GB | 16GB+ |
| 13B Distilled FP8 | 13B | FP8 | 12GB | 16GB+ |
| 2B Dev (0.9.6) | 2B | BF16 | 12GB | 12GB+ |
| 2B Distilled | 2B | BF16 | 8GB | 12GB+ |
| 2B Distilled FP8 | 2B | FP8 | 6GB | 8GB+ |

VRAM requirements scale with output resolution and frame count. Values above are estimates for typical settings (480x832, 96 frames).

## Model File Sizes

### 13B Parameter Models

| File | Size | Precision |
|------|------|-----------|
| ltxv-13b-0.9.8-dev.safetensors | ~28.6 GB | bf16 |
| ltxv-13b-0.9.8-distilled.safetensors | ~28.6 GB | bf16 |
| ltxv-13b-0.9.8-dev-fp8.safetensors | ~15.7 GB | fp8 |
| ltxv-13b-0.9.8-distilled-fp8.safetensors | ~15.7 GB | fp8 |
| ltxv-13b-0.9.7-distilled-lora128.safetensors | ~1.33 GB | LoRA |

### 2B Parameter Models

| File | Size | Precision |
|------|------|-----------|
| ltxv-2b-0.9.8-distilled.safetensors | ~6.34 GB | bf16 |
| ltxv-2b-0.9.8-distilled-fp8.safetensors | ~4.46 GB | fp8 |

## Software Requirements

| Dependency | Minimum Version | Notes |
|-----------|----------------|-------|
| Python | 3.10.5+ | Required |
| PyTorch | 2.1.2+ | CUDA support required |
| CUDA | 12.2+ | For GPU inference |
| diffusers | 0.32.0+ (or latest git) | For pipeline API |
| transformers | Latest | For text encoder |
| accelerate | Latest | For device management |
| macOS (MPS) | PyTorch 2.3.0 or >= 2.6 | See [[apple-silicon-setup]] |

### System Requirements (Official)

- **System RAM:** 32 GB recommended
- **Storage:** 100 GB free space (for model files)
- **OS:** Windows 11, macOS 13+ (Ventura), Linux (Ubuntu 20.04+)

## Compatible GPUs

### Consumer GPUs

| GPU | VRAM | Compatible Models |
|-----|------|-------------------|
| RTX 4090 | 24GB | All variants |
| RTX 4080 | 16GB | 13B FP8, all 2B |
| RTX 4070 Ti | 12GB | 13B FP8, all 2B |
| RTX 4070 | 12GB | 13B FP8, all 2B |
| RTX 4060 Ti 16GB | 16GB | 13B FP8, all 2B |
| RTX 4060 | 8GB | 2B variants only |
| RTX 3090 | 24GB | All variants |
| RTX 3080 | 10-12GB | 13B FP8, all 2B |
| RTX 3070 | 8GB | 2B variants only |

### Data Center GPUs

| GPU | VRAM | Compatible Models |
|-----|------|-------------------|
| H100 | 80GB | All variants (fastest) |
| A100 | 40/80GB | All variants (recommended for production) |
| L40S | 48GB | All variants |
| A40 | 48GB | All variants |
| L4 | 24GB | All variants |
| A10G | 24GB | All variants |
| T4 | 16GB | 13B FP8, all 2B |

## Performance Benchmarks

### Inference Speed (97 frames, 768x512)

| GPU | 13B Dev (30 steps) | 13B Distilled (20 steps) | 2B Distilled (20 steps) |
|-----|--------------------|--------------------------| ----------------------|
| H100 80GB | ~20s | ~15s | ~5s |
| A100 80GB | ~30s | ~20s | ~8s |
| RTX 4090 | ~45s | ~25s | ~10s |
| RTX 3090 | ~60s | ~35s | ~15s |

### Community Benchmarks

| Hardware | Config | Performance |
|----------|--------|-------------|
| RTX 4090 | 512x512, 121 frames | ~11 seconds |
| H100 (fal.ai) | 512x768, 121 frames | ~4 seconds |
| RTX 5090 | 1280x720, 121 frames | 80-95 seconds (LTX-2) |
| RTX 5090 | 1920x1088, 241 frames | 222 seconds (LTX-2.3) |
| RTX 3070 Ti (laptop) | Standard presets | ~300 seconds |

### Faster-Than-Real-Time (Paper)

The original 2B model on an H100 generates 5 seconds of 24 FPS video at 768x512 in 2 seconds -- faster than real-time playback.

### Speed Characteristics by Model Type

| Model | Inference Steps | CFG/STG Required | Speed Category |
|-------|---------------|------------------|----------------|
| 2B dev | 20-50 | Yes | Faster-than-real-time |
| 2B distilled | ~7 | No | 15x faster (real-time++) |
| 13B dev | 20-30 | Yes | Slower (highest quality) |
| 13B distilled | ~7 | No | Fast (comparable to 2B dev) |

## Resolution vs. VRAM Tradeoffs

| Available VRAM | Achievable Resolution | Frame Count |
|----------------|----------------------|-------------|
| 6 GB | 512x512 | ~50 frames |
| 8 GB | 512x640 | ~80 frames |
| 12 GB | 640x960 | ~120 frames |
| 16 GB | 1080p | ~161 frames |
| 24 GB | 1216x704 | 257 frames |
| 32+ GB | 720x1280 | 257 frames |

## Memory Optimization Strategies

### VAE Tiling

Reduces peak VRAM during video decoding:

```python
pipe.vae.enable_tiling()
```

### Model CPU Offloading

Moves model components to CPU when not in use:

```python
pipe.enable_model_cpu_offload()
```

### Sequential CPU Offloading

More aggressive offloading (slower but lower VRAM):

```python
pipe.enable_sequential_cpu_offload()
```

### FP8 Quantization

Use FP8 model variants for roughly 50% memory reduction with minimal quality loss. File size drops from ~28.6 GB to ~15.7 GB for 13B models.

### GGUF Quantization (Community)

Community-created GGUF quantizations from city96 and Unsloth provide further VRAM reduction beyond FP8 with various quantization levels (Q4, Q5, Q8). Designed for ComfyUI integration.

### Reduce Resolution + Upscale

Generate at 2/3 target resolution and upscale with the [[upscaler-pipelines|latent upscaler pipeline]].

### Reduce Frame Count

Fewer frames = less memory. Use the [[upscaler-pipelines|temporal upscaler]] if more frames are needed afterward.

## Cloud GPU Options

| Provider | GPUs Available | Approx. Cost |
|----------|---------------|--------------|
| AWS | A10G, A100 | $1-5/hour |
| Google Cloud | A100, T4 | $1-4/hour |
| Lambda Labs | A100, H100 | $1-3/hour |
| RunPod | Various | $0.5-3/hour |
| Vast.ai | Various | $0.3-2/hour |

## GPU Recommendations by Use Case

| Use Case | Recommended GPU | VRAM |
|----------|----------------|------|
| Quick prototyping (2B) | RTX 3060/4060 | 8-12 GB |
| Quality generation (2B) | RTX 3070/4070 | 12 GB |
| 13B development | RTX 3090/4090 | 24 GB |
| 13B production | A100/H100 | 40-80 GB |
| Real-time 13B | H100 | 80 GB |
| Low-budget 13B | RTX 4060 Ti 16GB | 16 GB (FP8) |

## See Also

- [[ltx2-system-requirements]] -- LTX-2/2.3 specific requirements
- [[apple-silicon-setup]] -- Running on Mac hardware
- [[hardware-accessibility]] -- Running on low-end consumer hardware
- [[installation-quickstart]] -- Getting started guides
- [[upscaler-pipelines]] -- Latent upscaling for lower VRAM usage
