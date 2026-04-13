# Community Benchmarks: LTX-2.3 Render Performance

**Source:** https://huggingface.co/Lightricks/LTX-2.3/discussions/16, https://huggingface.co/Lightricks/LTX-2.3/discussions/11

---

## Overview

Community members have extensively benchmarked LTX-2.3 on various hardware configurations, providing valuable real-world performance data.

## Hardware Benchmarks

### RTX 5090 Configurations

| Config | Resolution | Frames | Model | Time |
|--------|-----------|--------|-------|------|
| 96GB RAM + RTX 5090 | 1920x1088 | 241 | 22b-Dev two-stage | 222 seconds |
| 64GB RAM + RTX 5090 | 1280x720 | 481 | Dev FP8, 8 steps | 82 seconds |
| 64GB RAM + RTX 5090 | 1280x720 | 961 | Dev FP8, 8 steps | 564 seconds |
| 64GB RAM + RTX 5090 | 1920x1080 | 481 | Dev FP8, 8 steps | 547 seconds |

### Key Finding: Model Loading Overhead
Model loading time accounts for most of the processing time. Generating fewer frames only saves 2-5 seconds per run. Subsequent runs with the model already loaded are significantly faster.

## Quality Comparisons

### FP8 vs Full-Dev
- FP8 is **significantly faster** (~1 minute faster on first run, ~25 seconds on subsequent)
- Quality difference is **"minimal and negligible"**
- Output "virtually indistinguishable" from Full-Dev
- Consistent finding across LTX-2 and LTX-2.3

### NV4 (NVFP4) Quantization (Blackwell GPUs)
- Expected 200-300% speed improvement over FP8
- Trade-off: ~10% quality loss
- Real-world results on LTX-2: 280% faster for long-duration videos
- Artifacts more noticeable in high-action scenes than slow-motion content

## LTX-2.3 Quality Feedback

### Initial Issues (Discussion #11)
Users initially reported quality problems with LTX-2.3:
- "Not hitting the mark" quality
- Struggles with detail and warping
- Notable teeth anomalies
- Blotchy output

### Root Cause Identified
**Sigma values differ from LTX-2.** Users were applying LTX-2 settings to LTX-2.3.

### Solution
- Use official workflows from the LTX repository (not ComfyUI built-in templates)
- Use Kijai's transformers implementation
- Use LTX guidance parameter nodes

### After Fix
- "Significantly better outputs"
- "It's pretty insane"
- "Pretty stellar results" when using Flux1_krea_dev images
- "Waaaaaayyy better than I was getting in LTX-2"

## LTX-2.3 Distilled Performance

The 8-step distilled model delivers:
- Sub-1-minute render times on RTX 5090
- Quality sufficient for rapid iteration and prototyping
- Best speed-to-quality ratio in the LTX family
