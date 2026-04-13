# Community Project: LTX-2 on NVIDIA Jetson Thor

**Source:** https://huggingface.co/Lightricks/LTX-2/discussions/47

---

## Overview

Community member **Divhanthegray** successfully ran the full LTX-2 19B distilled model on an NVIDIA Jetson AGX Thor edge device, creating an open-source pipeline with complete memory lifecycle management.

**Repository:** github.com/divhanthelion/ltx2

## Hardware & Software

- **Device:** NVIDIA Jetson AGX Thor
- **Memory:** 128GB unified CPU-GPU memory
- **Software:** JetPack 7.0, CUDA 13.0
- **Base Image:** NGC PyTorch Docker container with sm_110 build

## Performance

- **Output:** 1920x1088 resolution, 161 frames (~6.7 seconds), 24fps with synchronized audio
- **Processing Time:** ~15 minutes for diffusion + 2 minutes for VAE decode per clip
- **Inference Steps:** 8 (distilled model limitation)

## Key Technical Challenges Solved

### 1. Unified Memory Issues
On Jetson Thor, CPU and GPU share the same physical RAM, which means:
- `enable_model_cpu_offload()` is useless (CPU and GPU share RAM)
- `tensor.to("cpu")` is a no-op
- Requires explicit `del` + `gc.collect()` + `torch.cuda.empty_cache()` (called twice)

### 2. Page Cache Management
- SafeTensors uses mmap for weight loading
- Calling `drop_caches` while models are alive causes kernel to evict weight pages
- Results in segfaults on next forward pass

### 3. VAE Decode Critical Issue
- Must use `torch.no_grad()` for VAE decode
- Without it, PyTorch builds autograd graphs across 15+ spatial tiles
- On unified memory, this segfaults instead of OOMing cleanly (required 4+ hours of debugging)

## Manual Memory Lifecycle
```
Load everything -> Diffuse -> Delete transformer/text encoder/scheduler/connectors
-> Decode audio -> Delete audio components -> VAE decode (under no_grad())
-> Delete everything -> Flush page cache -> Encode video
```

## Repository Features

- `generate.py` - Main pipeline with memory management
- `decode_latents.py` - Standalone decoder for recovering from failed runs (auto-saves latents)
- Batch rendering scripts with progress tracking and ETA
- Camera control LoRA support (dolly, jib, static movements)
- Optional FP8 quantization (reduces transformer memory ~50%)
- Post-processing pipeline: RIFE frame interpolation + Real-ESRGAN upscaling (Dockerized)

## Limitations Documented

- 8 inference steps only (distilled model limitation); frame interpolation helps
- Negative prompts don't work (distilled model uses CFG=1.0)
- 1080p is the quality ceiling; higher resolution produces spatial tiling seams
- ~15 minutes per clip, expected for 19B model on edge device
- At 64GB unified memory, would need FP8 + pre-computed text embeddings (very tight)

## Significance

This project demonstrates that LTX-2's full 19B model can run entirely on edge hardware, enabling fully local/offline video+audio generation without cloud dependencies. It also provides valuable documentation of unified memory pitfalls that apply broadly to Jetson deployments of large diffusion models.
