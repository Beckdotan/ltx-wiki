# LTX Video Version History and Community Timeline

## Source
- **Platform**: Aggregated from Wikipedia, GitHub, HuggingFace, news coverage
- **Date range**: November 2024 - April 2026
- **URLs**:
  - https://en.wikipedia.org/wiki/LTX-2 (Wikipedia article)
  - https://github.com/Lightricks/LTX-Video (Official repository)
  - https://huggingface.co/Lightricks/LTX-Video (HuggingFace model)
  - https://ltx.studio/blog/ltxv-models (Official LTXV models blog post)
  - https://techstartups.com/2025/05/06/lightricks-advances-ai-video-capabilities-with-new-13b-parameter-ltxv-model/
  - https://siliconangle.com/2025/05/06/lightricks-shakes-ai-video-creation-powerful-open-source-model/
  - https://siliconangle.com/2025/07/16/lightricks-latest-release-allows-creators-direct-longform-ai-generated-videos-real-time/
  - https://markets.chroniclejournal.com/chroniclejournal/article/tokenring-2025-10-23-lightricks-unveils-ltx-2-the-first-complete-open-source-ai-video-foundation-model-revolutionizing-content-creation
  - https://crepal.ai/blog/aivideo/ltx-2-3-vs-ltx-2-upgrade-guide/
  - https://aivid.video/blog/ltx-23-vs-ltx-2-the-ultimate-upgrade-for-ai-video-creation

## Version Timeline

### November 2024 - LTX Video 0.9.0 (Initial Release)
- Lightricks publicly released first text-to-video model
- 2 billion parameter Diffusion Transformer (DiT) architecture
- Resolution: 768x512 at 24 FPS
- Speed: 5 seconds of video in 4 seconds (real-time generation)
- Open source release generated immediate community excitement
- Available on HuggingFace and ComfyUI from day one

### Late 2024 - LTX Video 0.9.1
- Incremental improvement
- Community comparisons with CogVideoX 1.5 began
- "LTXV is faster but CogVideoX higher quality in some scenarios"

### Early 2025 - LTX Video 0.9.5
- Day-1 ComfyUI support announced via official blog
- Quality improvements with enhanced prompt adherence
- Community adoption accelerated significantly

### April 2025 - LTX Video 0.9.6
- New distilled model: 15x faster inference than non-distilled
- Improved quality: smoother motion, finer details
- More realistic and coherent video outputs
- Community particularly excited about distilled model speed

### May 2025 - LTX Video 0.9.7 / LTXV-13B
- Major milestone: 13 billion parameter model (6x increase from 2B)
- "Huge step from the 2B model with noticeably higher quality"
- Supports keyframes, camera/character motion, multi-shot sequences
- Multiscale rendering for better fine details
- Runs on local consumer GPUs (with optimizations)
- Community contributed Q8 kernel optimizations for consumer hardware
- Trained on licensed data from Getty Images and Shutterstock
- Multiple Hacker News front-page discussions

### July 2025 - 60-Second Barrier Broken
- LTX Video broke the 60-second barrier for generated video
- "Redefining AI video as a longform medium"
- Enabled real-time direction of long-form AI-generated videos
- Hacker News discussion generated significant interest

### LTX Video 0.9.8 (Final 0.9.x Release)
- Final version before transition to LTX-2 architecture
- Both 2B and 13B variants available
- Distilled versions available (ltxv-13b-0.9.8-distilled)
- LoRA variant released (ltxv-13b-0.9.7-distilled-lora128)

### October 2025 - LTX-2 Announced
- Lightricks announced major new model, renamed to LTX-2
- Represented "major leap forward from LTXV 0.9.8"
- 19 billion parameters (14B video + 5B audio)
- First DiT-based audio-video foundation model

### January 6, 2026 - LTX-2 Open Sourced
- Model weights, code, and benchmarks released to community
- Apache 2.0 license
- Native 4K at up to 50 FPS
- Synchronized audio and video generation in single pass
- Consumer GPU compatible
- CEO Zeev Farbman personally championed open-source release
- "AI video shouldn't be locked behind closed systems"
- Massive community reception across all platforms

### March 5, 2026 - LTX-2.3 Released
- 22 billion parameters
- Rebuilt VAE with redesigned latent space
- Sharper textures, better fine details than any prior open model
- Improved motion consistency (drift issues from LTX-2 fixed)
- Portrait/face detail significantly improved
- Stronger prompt adherence for complex multi-sentence prompts
- Native audio generation built into same pass as video
- LTX Desktop app released simultaneously
- Apache 2.0 license maintained
- 8.2/10 rating from Awesome Agents review
- "First open-weights model to credibly remove the quality asterisk"

## Community Milestones
- 5 million+ total downloads before LTX-2.3
- 796,000 monthly HuggingFace downloads
- Wikipedia article created for LTX-2
- Featured in NVIDIA's official video generation guide
- Multiple cloud platforms added support (Replicate, fal.ai, Modal, WaveSpeed)
- Active LoRA ecosystem on Civitai and HuggingFace
- Official community trainer repository maintained by Lightricks

## Company Context
- **Lightricks**: Israeli tech company, founded 2013
- Known for Facetune and Videoleap consumer apps
- Backed by $150M+ in investment
- CEO Zeev Farbman's personal advocacy for open source AI
- Partnership with Banodoco for ADOS hackathon
- Ethical training: licensed data from Getty Images and Shutterstock
