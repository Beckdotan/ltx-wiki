---
title: Sora (OpenAI)
type: competitor
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/competitor-product-sora.md
tags:
  - competitor
  - video-generation
  - sora
  - openai
  - proprietary
  - discontinued
---
# Sora (OpenAI)

Sora is OpenAI's text-to-video generation product, which launched to significant fanfare but is now being discontinued. The Sora app shuts down on April 26, 2026, and the API follows on September 24, 2026. OpenAI cited compute shortages, cost pressures, and a strategic shift toward core enterprise products as reasons.

## Shutdown Timeline
- **March 24, 2026:** OpenAI announces discontinuation
- **April 26, 2026:** Sora app and web experience shut down
- **September 24, 2026:** Sora API discontinued

The shutdown was so abrupt that Disney, which had committed $1 billion to a partnership, learned of the decision less than an hour before the public announcement.

## Model Versions

### Sora 2 (2025)
- Major upgrade with improved physics, audio, and controllability
- Available to ChatGPT Plus ($20/mo) subscribers
- Standard model at 720p

### Sora 2 Pro (2025-2026)
- Higher-quality variant requiring ChatGPT Pro ($200/mo)
- Up to 1024p resolution, enhanced audio quality

## Key Features (Pre-Shutdown)
- Text-to-video and image-to-video generation
- Storyboard mode for multi-shot sequences (up to 25 seconds per shot)
- Maximum duration: up to 35 seconds per clip
- Native audio generation (dialogue, sound effects, ambient audio)
- "Cameos" feature: insert real people into AI-generated environments with accurate voice
- Best-in-class physics simulation (water splashes, cloth motion, complex object interactions)
- World state persistence across multi-shot sequences

## Pricing (Before Shutdown)

| Access | Price |
|--------|-------|
| **ChatGPT Plus** | $20/mo (720p standard) |
| **ChatGPT Pro** | $200/mo (1024p high quality) |
| **API (sora-2)** | $0.10/second (720p) |
| **API (sora-2-pro 1024p)** | $0.50/second |

## Strengths (Historical)
- Best physics simulation in the industry
- Native audio generation with dialogue and sound effects
- Massive distribution via ChatGPT integration
- Strong controllability for multi-shot sequences
- Cinematic visual quality

## Weaknesses
- **Being shut down** -- no longer a viable option after April/September 2026
- Extremely expensive ($200/mo for Pro)
- Short duration (35 seconds max)
- No open-source option, no local deployment
- No production workflow beyond basic storyboard mode
- Revenue never materialized -- described as "a money pit that nobody was using"

## Market Impact of Shutdown

Sora's shutdown is the most dramatic event in the AI video space. Despite having the best physics simulation and strong audio capabilities, OpenAI could not make the economics work. This creates a significant market share opportunity for [[competitor-runway|Runway]], [[competitor-veo|Veo]], [[competitor-kling|Kling]], and [[ltx-studio]].

Teams that were using or evaluating Sora should consider [[ltx-studio]] as a primary alternative, especially given its script-to-export workflow, open-source model ([[ltx-2.3-model]]), and commitment to the video generation space as a core business.

## See Also
- [[competitor-landscape-overview]]
- [[ltx-studio]]
