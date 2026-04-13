---
title: LTXV-13B Model
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://www.prnewswire.com/news-releases/lightricks-launches-13b-parameters-ltx-video-model-breakthrough-rendering-approach-generates-high-quality-efficient-ai-video-30x-faster-than-comparable-models-302447660.html
  - https://techstartups.com/2025/05/06/lightricks-advances-ai-video-capabilities-with-new-13b-parameter-ltxv-model/
  - https://venturebeat.com/ai/exclusive-lightricks-bets-on-open-source-ai-video-to-challenge-big-tech
  - https://dataconomy.com/2025/05/14/lightricks-unveils-13b-ltx-video-model-for-hq-ai-video-generation/
  - https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
tags:
  - ltx-video
  - 13b
  - architecture
  - dit
---

# LTXV-13B Model

The LTXV-13B is a 13-billion-parameter video generation model launched by Lightricks on May 6, 2025, as part of [[ltx-video-097|LTX Video v0.9.7]]. It represents a 6.5x scale-up from the previous 2B parameter models and was described as the "most advanced and efficient AI video generation model to date."

## Architecture

- **Parameters:** 13 billion
- **Base architecture:** DiT (Diffusion Transformer)
- **Key innovation:** [[multiscale-rendering]] -- a layered generation process that enables 30x faster rendering
- **VAE:** 1:192 compression ratio with denoising decoder
- **Rendering:** Motion-first approach that prioritizes temporal consistency before adding visual detail

## Performance Claims

- Render times up to **30x faster** than competing systems of comparable size
- Real-time generation capability maintained despite the 6.5x parameter increase
- Comparable quality to much larger models (e.g., MovieGen 30B)
- Optimized for consumer-grade GPUs (NVIDIA RTX cards)

## Improvements Over 2B

- Significantly improved visual quality and coherence
- Better prompt following and understanding
- Breakthrough physical understanding
- More detailed and realistic generation
- Enhanced motion consistency
- Support for up to 60 seconds of continuous video

## Hardware Requirements

| Variant | Estimated VRAM | Recommended GPU |
|---------|---------------|-----------------|
| 13B dev | 40GB+ | A100 80GB, H100 |
| 13B distilled | 24GB+ | A100 40GB, RTX 4090 |
| 13B FP8 | 16GB+ | RTX 4090 |
| 13B distilled-fp8 | ~16GB | RTX 4090 |

## Competitive Landscape

The 13B launch positioned Lightricks as a significant player in the open-source AI video generation space:

| Model | Parameters | Approach |
|-------|-----------|----------|
| LTXV-13B | 13B | Open-source, multiscale rendering |
| HunyuanVideo | 13B | Comparable size, slower rendering |
| MovieGen (Meta) | 30B | Larger but closed, slower |
| CogVideoX | 2B-5B | Smaller open-source |
| Sora (OpenAI) | Unknown | Closed-source |
| Veo (Google) | Unknown | Closed-source |
| Runway Gen-3 | Unknown | Closed-source |

## Open Source Strategy

- Released as open-source to challenge Big Tech's closed models
- Free to license for enterprises with under $10 million annual revenue
- Available within LTX Studio (flagship storytelling platform)
- Integrated across the Lightricks product portfolio

## Media Coverage

The announcement received coverage from PR Newswire, VentureBeat, Tech Startups, Dataconomy, Morningstar, The AI Journal, and multiple international outlets. VentureBeat framed it as "Lightricks bets on open-source AI video to challenge Big Tech."

## Timeline Significance

This was the foundational release for the modern LTXV ecosystem:
- First jump beyond 2B parameters
- Introduction of [[multiscale-rendering]]
- Establishment of the [[ltxv-model-variants|dual dev/distilled + FP8 model release pattern]]
- Set the foundation for [[ltx-video-098|v0.9.8]] two months later

## See Also

- [[ltx-video-097]] -- the release that introduced the 13B model
- [[ltx-video-098]] -- continued evolution of the 13B line
- [[multiscale-rendering]] -- the rendering approach enabling practical 13B inference
- [[ltxv-model-variants]] -- all available variants of the 13B model
