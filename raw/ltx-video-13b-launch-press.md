# LTXV-13B Launch - Press Coverage and Announcement Details

**Source:** https://www.prnewswire.com/news-releases/lightricks-launches-13b-parameters-ltx-video-model-breakthrough-rendering-approach-generates-high-quality-efficient-ai-video-30x-faster-than-comparable-models-302447660.html
**Source:** https://techstartups.com/2025/05/06/lightricks-advances-ai-video-capabilities-with-new-13b-parameter-ltxv-model/
**Source:** https://dataconomy.com/2025/05/14/lightricks-unveils-13b-ltx-video-model-for-hq-ai-video-generation/
**Source:** https://venturebeat.com/ai/exclusive-lightricks-bets-on-open-source-ai-video-to-challenge-big-tech

## Official Announcement

On May 6, 2025, Lightricks launched its 13-billion-parameter LTX Video model (LTXV-13B), described as a "breakthrough rendering approach" that generates high-quality, efficient AI video 30x faster than comparable models.

## Key Claims from Press Release

### Performance
- Render times up to **30x faster** than competing systems of comparable size
- Real-time generation capability maintained despite 6.5x parameter increase
- Multiscale rendering enables speed without quality sacrifice

### Architecture
- **13 billion parameters** -- the company's largest model to date
- Leverages "multiscale rendering" -- a layered generation process:
  1. Drafts in lower detail to capture coarse motion
  2. Progressively adds structure, lighting, and micro-motion
  3. Motion prioritized first, then detail infused
- Based on DiT (Diffusion Transformer) architecture

### Accessibility
- **Optimized for consumer-grade GPUs** -- NVIDIA RTX cards
- Democratizes AI video generation for personal devices
- Does not require cloud-scale infrastructure or professional-grade hardware
- Free to license for enterprises with under $10 million in annual revenue

### Open Source Strategy
- Released as open-source to challenge Big Tech's closed models
- Available within LTX Studio (flagship storytelling platform)
- Shared with open community
- Being integrated across the Lightricks product portfolio

## Industry Context

The launch positioned Lightricks as a significant player in the open-source AI video generation space, competing against:
- Closed models: Google Veo, OpenAI Sora, Runway Gen-3
- Open models: CogVideoX (2B/5B), HunyuanVideo (13B), MovieGen (30B)

The 13B model was notable for claiming comparable quality to much larger models (like MovieGen's 30B) while being significantly faster due to the multiscale rendering approach and efficient VAE architecture.

## Media Coverage

The announcement received coverage from:
- PR Newswire (official press release)
- VentureBeat ("Lightricks bets on open-source AI video to challenge Big Tech")
- Tech Startups
- Dataconomy
- Morningstar
- The AI Journal
- Multiple international outlets

## Significance for the LTXV Timeline

This was the most significant release in the LTXV line:
- First jump beyond 2B parameters
- Introduction of multiscale rendering architecture
- Establishment of the dual dev/distilled + FP8 model release pattern
- Set the foundation for the v0.9.8 release two months later
