---
title: LTX Video Adoption Metrics
type: analysis
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/community-adoption-metrics.md
  - raw/adoption-metrics.md
  - raw/github-ltx-video-official.md
  - raw/github-ltx-2-official.md
  - raw/github-lightricks-other-official-repos.md
tags:
  - adoption
  - metrics
  - github
  - huggingface
  - downloads
  - community
---
# LTX Video Adoption Metrics

LTX Video has achieved significant open-source adoption across GitHub, HuggingFace, and community platforms. Combined monthly HuggingFace downloads exceed 3.8 million, with over 10,000 GitHub stars on the main repository and a [[discord-community]] of 70,000+ members.

## GitHub Repositories

| Repository | Stars | Forks | Notes |
|------------|-------|-------|-------|
| Lightricks/LTX-Video | ~10,000 | ~960 | Original LTXV, released Nov 2024 (78 open issues) |
| Lightricks/LTX-2 | ~5,800 | ~891 | Audio-video foundation model (monorepo, 27 commits) |
| Lightricks/ComfyUI-LTXVideo | ~3,400 | ~368 | Official [[comfyui-integration]] (32 watchers) |
| Lightricks/LTX-Desktop | ~1,400 | ~268 | Desktop app, latest v1.0.4 (Apr 2026) |
| Lightricks/LTX-Video-Trainer | ~424 | ~58 | LoRA and fine-tuning trainer (85 commits) |
| Lightricks/LTX-Video-Q8-Kernels | 81 | 18 | FP8 CUDA kernels (latest release 0.0.4, May 2025) |
| logtd/ComfyUI-LTXTricks | ~512 | ~23 | Community nodes (deprecated, merged into official) |

See [[github-official-repositories]] for detailed repository profiles and [[github-community-forks]] for the ~960 fork ecosystem.

## HuggingFace Downloads (Monthly)

| Model | Monthly Downloads | Likes | Discussions |
|-------|-------------------|-------|-------------|
| [[ltx-video-overview\|LTX-Video]] (2B) | 567,927 | 2,150 | 111 |
| [[ltx-2-overview\|LTX-2]] (19B) | 907,185 | 1,670 | 52 |
| LTX-2.3 (22B) | 1,658,413 | 942 | 37 |
| LTX-2.3-fp8 | 651,823 | 75 | 4 |
| LTX-2.3-nvfp4 | 19,346 | 62 | -- |
| **Total (all variants)** | **~3.8M+/month** | | |

## Lightricks HuggingFace Organization

| Metric | Value |
|--------|-------|
| Followers | 2,983 |
| Team members | 54 |
| Total models | 37 |
| Official Spaces | 8 |
| Datasets | 3 |
| Verified | Yes |

## Community-Created Derivative Ecosystem

Each model version has spawned a large ecosystem of community derivatives on HuggingFace:

| Category | LTX-Video | LTX-2 | LTX-2.3 |
|----------|-----------|-------|---------|
| Adapter models | 24 | 49 | 20 |
| Fine-tunes | 25 | 54 | 27 |
| Quantizations | 16 | 8 | 14 |
| Spaces using model | 100+ | 100+ | 81 |
| Merged versions | 2 | -- | -- |

Over 90 community models exist on HuggingFace, including GGUF quantizations (city96, pollockjj), FP8/INT8 variants, Diffusers format conversions (a-r-r-o-w), 12+ custom LoRAs, and specialized variants (anime, pixel art).

See also [[huggingface-spaces-ecosystem]] and [[civitai-community]] for platform-specific adoption details.

## Model Version Evolution

| Version | Parameters | Key Feature | Release Period |
|---------|-----------|-------------|----------------|
| LTX-Video 0.9.0 | 2B | First release, text-to-video | Late 2024 |
| LTX-Video 0.9.1 | 2B | Improved quality | Late 2024 |
| LTX-Video 0.9.5 | 2B | LoRA support added | Early 2025 |
| LTX-Video 0.9.6 | 2B | 15x faster, real-time | Mid 2025 |
| LTX-Video 0.9.7 | 13B | Distilled + dev versions | Mid 2025 |
| LTX-Video 0.9.8 | 13B | IC-LoRA, detailer | Late 2025 |
| [[ltx-2-overview\|LTX-2]] | 19B | Joint audio-video generation | Jan 2026 |
| LTX-2.3 | 22B | Improved quality + prompt adherence | Mar 2026 |

## Review Platform Presence

- **G2:** Generally positive ratings; users praise creative editing features
- **Trustpilot:** Polarized reviews; praise mixed with billing complaints about [[ltx-studio]]
- **Product Hunt:** Active listings for LTX Studio and LTX Desktop
- **Reddit:** Active discussions on r/StableDiffusion and r/comfyui; divided opinions with praise for speed and complaints about hardware requirements

## References

- [[github-official-repositories]] -- Detailed repository profiles
- [[github-community-forks]] -- Community fork ecosystem
- [[github-community-tools]] -- Community tools and wrappers
- [[huggingface-spaces-ecosystem]]
- [[civitai-community]]
- [[discord-community]]
- [[community-feedback]]
- [[creative-showcases]]
