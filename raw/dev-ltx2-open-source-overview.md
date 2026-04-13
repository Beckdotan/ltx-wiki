# LTX-2 / LTX-2.3 Open Source Model Overview

**Source:** https://docs.ltx.video/open-source-model/getting-started/overview
**Fetched:** 2026-04-13

---

## Overview

LTX-2.3 is a DiT-based audio-video foundation model designed to generate synchronized video and audio within a single model. It represents the evolution of the original LTX-Video into a more capable, multimodal generation system.

---

## Key Features

### Video & Audio Generation
- Produces up to approximately 20 seconds of synchronized content
- Supports high resolutions and frame rates based on hardware
- Scales from quick iterations to premium-quality outputs

### Native Synchronization
Generates dialogue, lip movement, and ambient audio together in a single pass, eliminating the need for separate alignment processes.

### Motion & Consistency
Capable of rendering dynamic scenes with stable motion, consistent character identity, and strong frame-to-frame coherence.

### Customization & Control
Offers LoRA-based customization, camera-aware motion logic, and multimodal inputs including:
- Text
- Image
- Video
- Audio
- Depth maps

### Performance Efficiency
Features a compact latent space and step-distilled architecture enabling execution on consumer-grade GPUs without requiring specialized infrastructure.

---

## Model Variants

| Model | Parameters | Description |
|-------|-----------|-------------|
| LTX-2.3 (latest) | ~20B | Latest version with audio-video sync, portrait support |
| LTX-2 | ~19B | Audio-video model, landscape orientation |
| LTX-Video 0.9.8 | 13B / 2B | Original video-only model |

---

## Open Source vs API

| Feature | Open Source | Official API |
|---------|-----------|-------------|
| Self-hosted | Yes | No (cloud) |
| Full model access | Yes | No |
| Custom fine-tuning | Yes | No |
| LoRA / IC-LoRA | Yes | Via API features |
| Audio generation | Yes | Yes |
| No GPU required | No | Yes |
| Production SLA | No | Yes |
| Managed infrastructure | No | Yes |

---

## Getting Started Options

1. **ComfyUI** (Recommended for most users) - Visual node-based workflow
2. **LTX Desktop** - Standalone application with GUI
3. **PyTorch API** - Full programmatic control via `ltx-pipelines`
4. **Diffusers** - Simplified Python API via Hugging Face

---

## Resources

- **Research Paper:** https://arxiv.org/abs/2601.03233
- **API Developer Console:** https://console.ltx.video/playground/
- **Quick Start:** https://docs.ltx.video/open-source-model/getting-started/quick-start
- **System Requirements:** https://docs.ltx.video/open-source-model/getting-started/system-requirements
- **GitHub (LTX-2):** https://github.com/Lightricks/LTX-2
- **Hugging Face (LTX-2):** https://huggingface.co/Lightricks/LTX-2
- **Hugging Face (LTX-2.3):** https://huggingface.co/Lightricks/LTX-2.3
