# Other Official Lightricks Repos Related to LTX

> **Source:** <https://github.com/lightricks>

Lightricks has 153 total public repositories. The following are directly related to the LTX Video ecosystem.

---

## 1. Lightricks/ComfyUI-LTXVideo

> **URL:** <https://github.com/Lightricks/ComfyUI-LTXVideo>

| Metric | Value |
|--------|-------|
| Stars | ~3,400 |
| Forks | ~368 |
| Watchers | 32 |
| Languages | Python (93.7%), JavaScript (6.3%) |

**Description:** Custom nodes extending ComfyUI for the LTX-2 video generation model. LTX-2 is built into ComfyUI core; this repo provides additional nodes and workflows.

**Key Features:**
- Text-to-video and image-to-video generation
- Video-to-video refinement (detailing)
- IC-LoRA controlled generation (depth, pose, edges)
- Motion tracking and camera control
- Two-stage pipeline with spatial and temporal upsampling
- Union IC-LoRA (unified depth + edge control on downsampled latents)
- Tiled VAE decoding for efficiency
- Looping sampler for seamless video
- Low VRAM optimization options

**Supported Workflows (LTX-2.3):**
- Single-stage text/image-to-video (full and distilled)
- Two-stage distilled with upsampling
- IC-LoRA union control
- Motion tracking I2V

**System Requirements:** CUDA GPU with 32GB+ VRAM, 100GB+ disk space

**Installation:** Via ComfyUI Manager -- search for "LTXVideo" and install. Models download automatically on first use.

---

## 2. Lightricks/LTX-Video-Trainer

> **URL:** <https://github.com/Lightricks/LTX-Video-Trainer>

| Metric | Value |
|--------|-------|
| Stars | 424 |
| Forks | 58 |
| Watchers | 5 |
| Language | Python (100%) |
| License | Apache-2.0 |
| Commits | 85 |

**Description:** Community trainer for Lightricks' LTX Video model, enabling LoRA training, full fine-tuning, and video-to-video transformation workflows on custom datasets.

**Training Modes:**
- Standard LoRA training
- IC-LoRA (In-Context LoRA) training
- Full fine-tuning
- Control adapter training

**Supported Models:** LTX-Video (original) and LTXV 13B variant

**Pre-trained Examples:**
- Cakeify LoRA, Squish LoRA (standard)
- Depth map, Human pose, Canny edge (IC-LoRA control adapters)

**Note:** For LTX-2 training, users are directed to the separate ltx-trainer package within the LTX-2 monorepo.

**Documentation:** Quick Start, Dataset Prep, Training Modes, Configuration Reference, Troubleshooting in `docs/` directory.

**Authors:** Matan Ben Yosef, Naomi Ken Korem, Tavi Halperin (2025)

---

## 3. Lightricks/LTX-Desktop

> **URL:** <https://github.com/Lightricks/LTX-Desktop>

| Metric | Value |
|--------|-------|
| Stars | ~1,400 |
| Forks | ~268 |
| Language | TypeScript (72.2%), Python (25.5%), Shell, PowerShell, JS, CSS |
| License | Apache-2.0 |
| Status | Beta |
| Latest Release | v1.0.4 (April 3, 2026) |

**Description:** Open-source desktop app for generating videos with LTX models. Supports local GPU processing on Windows/Linux with NVIDIA GPUs, and API mode for macOS/unsupported hardware.

**Features:**
- Text-to-video, image-to-video, audio-to-video generation
- Video edit generation (Retake)
- Built-in video editor with timeline
- Generate multiple takes per clip, switch non-destructively
- Project-based video editing

**Architecture (Three Layers):**
1. **Renderer:** TypeScript + React UI, calls backend over HTTP at localhost:8000
2. **Electron Main Process:** Manages lifecycle, OS integration, ffmpeg export, Python backend
3. **Backend:** FastAPI Python server orchestrating model generation and GPU execution

**System Requirements:**
- Windows/Linux local: NVIDIA GPU with CUDA and >=16GB VRAM, 160GB+ disk
- macOS: Apple Silicon, API-only mode (requires LTX API key)

---

## 4. Lightricks/LTX-Video-Q8-Kernels

> **URL:** <https://github.com/Lightricks/LTX-Video-Q8-Kernels>

| Metric | Value |
|--------|-------|
| Stars | 81 |
| Forks | 18 |
| Languages | Python (38%), CUDA (34.8%), C++ (18.2%), C (9%) |
| Latest Release | 0.0.4 (May 6, 2025) |

**Description:** Specialized CUDA kernels for FP8-quantized inference of LTX-Video models. Reduces memory requirements and computational overhead compared to full-precision.

**Requirements:** CUDA Toolkit 12.8 or later. For ComfyUI integration, apply the "LTXVideo Q8 Patcher" node to loaded models.

---

## 5. Lightricks/ltx-analytics-agents

> **URL:** <https://github.com/Lightricks/ltx-analytics-agents>

| Metric | Value |
|--------|-------|
| Stars | 3 |
| Language | Python |

**Description:** Product data agents for LTX. Likely internal analytics tooling.

---

## Summary of All Official LTX-Related Repos

| Repository | Stars | Purpose |
|-----------|-------|---------|
| Lightricks/LTX-Video | ~10,000 | Original model & inference |
| Lightricks/LTX-2 | ~5,800 | Next-gen audio-video model (monorepo) |
| Lightricks/ComfyUI-LTXVideo | ~3,400 | ComfyUI integration nodes |
| Lightricks/LTX-Desktop | ~1,400 | Desktop video generation app |
| Lightricks/LTX-Video-Trainer | 424 | LoRA & fine-tuning toolkit |
| Lightricks/LTX-Video-Q8-Kernels | 81 | FP8 CUDA kernels |
| Lightricks/ltx-analytics-agents | 3 | Analytics tooling |
