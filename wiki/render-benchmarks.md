---
title: Render Benchmarks
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/community-project-render-benchmarks.md
tags:
  - community
  - benchmarks
  - performance
  - quantization
  - rtx-5090
  - ltx-2.3
---

# Render Benchmarks

Community members have extensively benchmarked [[ltx-2.3-model|LTX-2.3]] on various hardware configurations, providing valuable real-world performance data.

## RTX 5090 Benchmarks

| Config | Resolution | Frames | Model | Time |
|--------|-----------|--------|-------|------|
| 96 GB RAM + RTX 5090 | 1920x1088 | 241 | 22b-Dev two-stage | 222 seconds |
| 64 GB RAM + RTX 5090 | 1280x720 | 481 | Dev FP8, 8 steps | 82 seconds |
| 64 GB RAM + RTX 5090 | 1280x720 | 961 | Dev FP8, 8 steps | 564 seconds |
| 64 GB RAM + RTX 5090 | 1920x1080 | 481 | Dev FP8, 8 steps | 547 seconds |

### Model Loading Overhead

Model loading time accounts for most of the processing time. Generating fewer frames only saves 2-5 seconds per run. Subsequent runs with the model already loaded are significantly faster.

## Quantization Quality Comparisons

### FP8 vs Full-Dev

- FP8 is **significantly faster** (~1 minute faster on first run, ~25 seconds on subsequent)
- Quality difference is "minimal and negligible"
- Output "virtually indistinguishable" from Full-Dev
- Consistent finding across [[ltx-2-overview|LTX-2]] and LTX-2.3

### NV4 (NVFP4) Quantization (Blackwell GPUs)

- Expected 200-300% speed improvement over FP8
- Trade-off: ~10% quality loss
- Real-world results on LTX-2: 280% faster for long-duration videos
- Artifacts more noticeable in high-action scenes than slow-motion content

## LTX-2.3 Quality Issues and Fix

### Initial Reports

Users initially reported quality problems with LTX-2.3:

- "Not hitting the mark" quality
- Struggles with detail and warping
- Notable teeth anomalies
- Blotchy output

### Root Cause

**Sigma values differ from LTX-2.** Users were incorrectly applying LTX-2 settings to LTX-2.3.

### Solution

- Use official workflows from the LTX repository (not ComfyUI built-in templates)
- Use Kijai's transformers implementation
- Use LTX guidance parameter nodes

### After Fix

Community feedback improved dramatically:
- "Significantly better outputs"
- "Pretty stellar results" when using Flux1_krea_dev images
- "Waaaaaayyy better than I was getting in LTX-2"

## Distilled Model Performance

The 8-step distilled model delivers:

- Sub-1-minute render times on RTX 5090
- Quality sufficient for rapid iteration and prototyping
- Best speed-to-quality ratio in the LTX family

## See Also

- [[ltx-2-overview]] -- LTX-2 base model
- [[gguf-quantizations]] -- GGUF quantization variants for lower VRAM usage
- [[community-models-finetunes]] -- Full landscape of community model variants
- [[jetson-thor-deployment]] -- Edge hardware benchmark on Jetson AGX Thor
- [[turbo-space]] -- Flash Attention 3 optimization for faster inference
