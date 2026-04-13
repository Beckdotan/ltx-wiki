---
title: Open-Sora
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/competitor-model-open-sora.md
  - raw/open-source-comparison.md
  - raw/related-work-and-comparisons.md
tags:
  - competitor
  - hpc-ai-tech
  - video-generation
  - open-source
  - research
---

# Open-Sora

Open-Sora is a community-driven open-source project by HPC-AI Tech (Colossai team) that aims to reproduce and democratize video generation capabilities comparable to OpenAI's Sora. The project achieved a major milestone with Open-Sora 2.0, a commercial-level model trained for only $200K. It is distinct from [[open-sora-plan]], a separate project from Peking University.

## Key Facts

- **Developer:** HPC-AI Tech (Colossai team)
- **Parameters:** 1B (v1.3), 11B (v2.0)
- **Max resolution:** 768px (supports 16:9, 9:16, 1:1, 2.39:1)
- **FPS:** 24 fps
- **Duration:** Up to approximately 5 seconds (128 frames)
- **VRAM:** 40 GB+ recommended
- **License:** MIT (most permissive in the space)
- **Training cost:** $200,000 from scratch (Open-Sora 2.0)

## Versions

### Open-Sora 1.3 (February 2025)
- 1 billion parameters
- Initial release in the 1.x series

### Open-Sora 2.0 (March 2025)
- 11 billion parameters
- Commercial-grade quality
- VBench gap to OpenAI's Sora narrowed from 4.52% to just 0.69%
- Full training code and checkpoints released

## Capabilities

- Text-to-video generation
- Image-to-video generation
- Multiple aspect ratio support (including cinematic 2.39:1)
- Motion Intensity Control (score 1-7)

## Strengths

- **Near-Sora quality** -- VBench gap narrowed to 0.69%
- **MIT license** -- most permissive license among major video generation models
- **Extremely cost-efficient** -- commercial-grade model for $200K training cost
- **Full reproducibility** -- training code and checkpoints available
- **Multiple aspect ratios** including cinematic 2.39:1

## Weaknesses

- **Lower max resolution** -- 768px is well below [[wan-video]] and [[ltx-2-overview|LTX-2]]
- **Short clips** -- approximately 5 seconds max
- **No native audio**
- **High VRAM requirements** -- 40 GB+ for optimal performance
- **Not fully production-ready** -- better suited for research

## Comparison to LTX

The [[related-work-and-comparisons|LTX-Video paper]] reports that LTX-Video significantly outperforms Open-Sora Plan in human evaluation. Open-Sora 2.0's MIT license is the most permissive in the space, and its $200K training cost demonstrates remarkable cost-efficiency. However, it trails [[ltx-2-overview|LTX-2]] significantly in resolution (768px vs native 4K), audio (none vs native synchronized), speed, and VRAM efficiency (40 GB+ vs approximately 8 GB quantized).

## See Also

- [[open-sora-plan]]
- [[open-source-video-generation-landscape]]
