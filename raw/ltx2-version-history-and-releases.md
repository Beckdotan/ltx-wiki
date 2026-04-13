# LTX-2 Version History and Releases

## Overview

LTX-2 is the second generation of Lightricks' video generation model family, representing a complete architectural overhaul from LTX Video (0.9.x). The name was changed from "LTXV" to "LTX-2" to reflect the magnitude of the changes. There is no version 2.1 or 2.2 -- versioning jumped from LTX-2 (base) to LTX-2.3.

## Release Timeline

### LTX-2 Announcement -- October 2025
- Lightricks announced LTX-2 as a rename from LTXV
- First DiT-based audio-video foundation model
- Native 4K resolution at up to 50 FPS
- Synchronized audio and video generation in one pass
- API access initially rolled out to early partners and teams
- **Not yet open source** at this point

### LTX-2 Full Open-Source Release -- January 6, 2026
- Complete codebase, weights, and tooling made publicly available
- **Parameters:** 19 billion (14B video + 5B audio)
- Apache 2.0 license for code; free for companies under $10M annual revenue
- Full access to model weights, inference pipelines, and training code
- First production-ready model combining truly open audio and video generation
- ArXiv paper published: 2601.03233
- **GitHub:** https://github.com/Lightricks/LTX-2
- **HuggingFace:** https://huggingface.co/Lightricks/LTX-2
- Download statistics: 84,353 downloads in December 2025 on Hugging Face; 795 GitHub stars at launch

### End-of-January LTX-2 Update -- Late January 2026
- Focused on builder-requested improvements for real-world workflows
- New IC-LoRA union control model supporting depth, pose, and edges automatically
- New ComfyUI nodes to remove Gemma text encoder from the critical path (addressing VRAM management pain point)
- Saveable/loadable text conditioning as .safetensors files
- Shaped by ongoing community feedback

### LTX-2.3 -- March 2026
- **Parameters:** 22 billion (upgraded from 19B)
- Accompanied by LTX Desktop, a desktop video editor for local execution
- Major engine upgrade across all dimensions:
  - **Sharper details:** Pores, fine baby hairs, eye corners survive motion better; improved denim, linen, brushed steel textures
  - **Stronger motion:** Up to 48 FPS for smoother motion without soap-opera look
  - **Cleaner audio:** Training set filtered for silence/noise/artifacts; new vocoder
  - **Native portrait:** Up to 1080x1920, trained on vertical data (not cropped from landscape)
  - **Better prompt understanding:** 4x larger text connector, gated attention architecture
  - **Text rendering improvements**
- New redesigned VAE for sharper fine details, more realistic textures, cleaner edges
- New spatial and temporal latent upscalers
- Two model variants: LTX-2.3 Fast and LTX-2.3 Pro
- **HuggingFace:** https://huggingface.co/Lightricks/LTX-2.3

## Version Comparison: LTX-2 vs LTX-2.3

| Feature | LTX-2 (Jan 2026) | LTX-2.3 (Mar 2026) |
|---------|-------------------|---------------------|
| Parameters | 19B | 22B |
| VAE | Original | Redesigned (sharper) |
| Portrait support | Limited | Native 1080x1920 |
| Max FPS | 50 | 50 (48 FPS practical for smooth motion) |
| Text connector | Standard | 4x larger, gated attention |
| Audio quality | Good | Filtered training data, new vocoder |
| Upscalers | Spatial only | Spatial (1.5x/2x) + Temporal |
| LTX Desktop | No | Yes (released alongside) |
| LoRA compatibility | LTX-2 LoRAs | Not backward compatible with LTX-2 LoRAs |

## No Intermediate Versions

There is no LTX-2.1 or LTX-2.2. The versioning went directly from LTX-2 (base) to LTX-2.3. The "end-of-January update" was an incremental drop of new IC-LoRA controls and ComfyUI nodes, not a version bump.

## Sources

- [LTX-2 Wikipedia](https://en.wikipedia.org/wiki/LTX-2)
- [LTX-2 Is Now Open Source -- LTX Blog](https://ltx.io/model/model-blog/ltx-2-is-now-open-source)
- [End-of-January LTX-2 Drop -- LTX Blog](https://ltx.io/model/model-blog/end-of-january-ltx-2-drop)
- [LTX-2.3 Release Blog](https://ltx.io/model/model-blog/ltx-2-3-release)
- [LTX-2.3 What's New -- WaveSpeedAI](https://wavespeed.ai/blog/posts/ltx-2-3-whats-new-2026/)
- [LTX 2.3 vs LTX 2 -- CrePal](https://crepal.ai/blog/aivideo/ltx-2-3-vs-ltx-2-upgrade-guide/)
- [Lightricks Open-Sources LTX-2 -- GlobeNewsWire](https://www.globenewswire.com/news-release/2026/01/06/3213304/0/en/Lightricks-Open-Sources-LTX-2-the-First-Production-Ready-Audio-and-Video-Generation-Model-With-Truly-Open-Weights.html)
