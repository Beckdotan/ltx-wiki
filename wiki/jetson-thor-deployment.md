---
title: Jetson Thor Deployment
type: community-project
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/community-project-jetson-thor-deployment.md
tags:
  - community
  - edge-deployment
  - jetson
  - nvidia
  - ltx-2
  - unified-memory
---

# Jetson Thor Deployment

Community member **Divhanthegray** successfully ran the full [[ltx-2-overview|LTX-2]] 19B distilled model on an NVIDIA Jetson AGX Thor edge device, creating an open-source pipeline with complete memory lifecycle management.

## Key Details

- **Creator:** Divhanthegray
- **Repository:** github.com/divhanthelion/ltx2
- **Source discussion:** https://huggingface.co/Lightricks/LTX-2/discussions/47

## Hardware and Software

- **Device:** NVIDIA Jetson AGX Thor
- **Memory:** 128 GB unified CPU-GPU memory
- **Software:** JetPack 7.0, CUDA 13.0
- **Base image:** NGC PyTorch Docker container with sm_110 build

## Performance

- **Output:** 1920x1088 resolution, 161 frames (~6.7 seconds), 24 FPS with synchronized audio
- **Processing time:** ~15 minutes for diffusion + 2 minutes for VAE decode per clip
- **Inference steps:** 8 (distilled model)

## Key Technical Challenges

### Unified Memory Pitfalls

On Jetson Thor, CPU and GPU share the same physical RAM, which invalidates standard offloading strategies:

- `enable_model_cpu_offload()` is useless (CPU and GPU share the same memory)
- `tensor.to("cpu")` is a no-op
- Requires explicit `del` + `gc.collect()` + `torch.cuda.empty_cache()` (called twice)

### Page Cache Management

- SafeTensors uses mmap for weight loading
- Calling `drop_caches` while models are alive causes the kernel to evict weight pages
- Results in segfaults on next forward pass

### VAE Decode

- Must use `torch.no_grad()` for VAE decode
- Without it, PyTorch builds autograd graphs across 15+ spatial tiles
- On unified memory, this segfaults instead of producing a clean OOM error

## Memory Lifecycle

The pipeline follows a strict manual memory management sequence:

```
Load everything -> Diffuse -> Delete transformer/text encoder/scheduler/connectors
-> Decode audio -> Delete audio components -> VAE decode (under no_grad())
-> Delete everything -> Flush page cache -> Encode video
```

## Repository Features

- `generate.py` -- Main pipeline with memory management
- `decode_latents.py` -- Standalone decoder for recovering from failed runs (auto-saves latents)
- Batch rendering scripts with progress tracking and ETA
- [[camera-control-loras|Camera control LoRA]] support (dolly, jib, static movements)
- Optional FP8 quantization (reduces transformer memory ~50%)
- Post-processing pipeline: RIFE frame interpolation + Real-ESRGAN upscaling (Dockerized)

## Limitations

- 8 inference steps only (distilled model); frame interpolation helps compensate
- Negative prompts do not work (distilled model uses CFG=1.0)
- 1080p is the quality ceiling; higher resolution produces spatial tiling seams
- ~15 minutes per clip (expected for 19B model on edge hardware)
- At 64 GB unified memory, would need FP8 + pre-computed text embeddings (very tight)

## Significance

This project demonstrates that LTX-2's full 19B model can run entirely on edge hardware, enabling fully local and offline video+audio generation without cloud dependencies. The documented unified memory pitfalls apply broadly to any Jetson deployment of large diffusion models.

## See Also

- [[ltx-2-overview]] -- LTX-2 base model
- [[render-benchmarks]] -- Community performance benchmarks
- [[camera-control-loras]] -- Camera control LoRAs used in this pipeline
