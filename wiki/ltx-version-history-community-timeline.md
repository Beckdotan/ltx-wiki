---
title: LTX Version History -- Community Timeline
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/social-ltx-version-history-community-timeline.md
  - raw/social-general-sentiment-consensus.md
tags:
  - version-history
  - timeline
  - releases
  - ltx-video
  - lightricks
---

# LTX Version History -- Community Timeline

This page tracks the version history of LTX Video models as observed and documented by the community, including community reaction at each milestone.

## Timeline

### November 2024 -- LTX Video 0.9.0 (Initial Release)

- 2 billion parameter Diffusion Transformer (DiT) architecture
- 768x512 at 24 FPS
- 5 seconds of video in 4 seconds (real-time generation)
- Available on HuggingFace and [[comfyui-ltx-workflows|ComfyUI]] from day one
- Community reaction: excitement about speed, curiosity about quality

### Late 2024 -- LTX Video 0.9.1

- Incremental improvement
- Community began comparing against CogVideoX 1.5
- Assessment: "LTXV is faster but CogVideoX higher quality in some scenarios"

### Early 2025 -- LTX Video 0.9.5

- Day-1 ComfyUI support announced
- Quality improvements with enhanced prompt adherence
- Community adoption accelerated significantly

### April 2025 -- LTX Video 0.9.6

- New distilled model: 15x faster inference
- Improved quality: smoother motion, finer details
- Community excited about distilled model speed

### May 2025 -- LTX Video 0.9.7 / LTXV-13B

- Major milestone: 13 billion parameters (6x increase from 2B)
- Community quote: "Huge step from the 2B model with noticeably higher quality"
- Supports keyframes, camera/character motion, multi-shot sequences
- Multiscale rendering for better fine details
- Q8 kernel optimizations contributed by community for consumer hardware
- Trained on licensed data from Getty Images and Shutterstock
- Multiple [[hackernews-ltx-discussions|Hacker News]] front-page discussions

### July 2025 -- 60-Second Barrier Broken

- LTX Video achieved 60+ second video generation
- Community described this as "redefining AI video as a longform medium"
- Enabled real-time direction of long-form AI-generated videos

### Mid-2025 -- LTX Video 0.9.8

- Final version in the 0.9.x line
- Both 2B and 13B variants available
- Distilled and LoRA variants released
- Prepared ground for the LTX-2 architecture shift

### October 2025 -- LTX-2 Announced

- Major model rename from "LTXV" to "LTX-2"
- 19 billion parameters (14B video + 5B audio)
- First DiT-based audio-video foundation model
- Described as a "major leap forward from LTXV 0.9.8"

### January 6, 2026 -- LTX-2 Open Sourced

- Model weights, code, and benchmarks released to community
- Apache 2.0 license
- Native 4K at up to 50 FPS
- [[ltx-audio-video-community-reception|Synchronized audio and video]] generation
- CEO Zeev Farbman personally championed the release
- Massive community reception across [[x-twitter-ltx-announcements|X/Twitter]], [[reddit-community-discussions|Reddit]], and [[hackernews-ltx-discussions|Hacker News]]

### March 5, 2026 -- LTX-2.3 Released

- 22 billion parameters
- Rebuilt VAE with redesigned latent space
- Sharper textures, better fine details than any prior open model
- Motion consistency improved (drift issues from LTX-2 fixed)
- Portrait/face detail significantly improved
- [[ltx-desktop-community-reception|LTX Desktop]] app released simultaneously
- Apache 2.0 license maintained
- [[blog-reviews-ltx|8.2/10 rating]] from Awesome Agents review
- Community assessment: "First open-weights model to credibly remove the quality asterisk"

Note: There is no LTX-2.1 or LTX-2.2. Versioning jumped directly from LTX-2 to LTX-2.3.

## Community Milestones

- 5 million+ total downloads before LTX-2.3
- 796,000 monthly HuggingFace downloads
- Wikipedia article created for LTX-2
- Featured in NVIDIA's official video generation guide
- Multiple cloud platforms added support (Replicate, fal.ai, Modal, WaveSpeed)
- Active LoRA ecosystem on Civitai and HuggingFace

## Company Context

- **[[lightricks-company]]**: Israeli tech company, founded 2013
- Known for Facetune and Videoleap consumer apps
- CEO Zeev Farbman's personal advocacy for open-source AI
- Ethical training: licensed data from Getty Images and Shutterstock

## See Also

- [[community-sentiment-overview]]
- [[ltx-adoption-metrics]]
- [[ltx-model-comparisons]]
