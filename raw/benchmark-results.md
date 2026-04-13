# LTX Video Benchmark Results and Performance

## Overview

This document consolidates all available benchmark results and performance comparisons for the LTX model family, spanning from the original LTX-Video through LTX-2 and LTX-2.3.

## LTX-Video (2B) Benchmark Results

### Human Evaluation (from arXiv paper)

Conducted with 1,000 prompts and 20 human participants, measuring win rates against competing models:

| Model | Text-to-Video Win Rate | Image-to-Video Win Rate |
|-------|----------------------|------------------------|
| **LTX-Video** | **85%** | **91%** |
| PyramidFlow | 51% | 35% |
| CogVideoX 2B | 38% | 47% |
| Open-Sora Plan | 20% | 20% |

### VBench Score
- **Total score:** ~71.93 (baseline LTX-Video)
- Note: Later model versions (LTXV-13B, LTX-2) improved significantly on this benchmark

### Speed Performance (LTX-Video 2B)
- **H100 GPU:** 121 frames at 768x512 pixels, 20 diffusion steps in ~2 seconds
- **Equivalent to:** 5 seconds of 24 fps video generated in 2 seconds (faster than real-time)
- **RTX 4090:** Near-real-time performance (approximately 4 seconds for same output)

### Architecture Comparison (from paper)

| Aspect | LTX-Video | CogVideoX | PyramidFlow | MovieGen |
|--------|-----------|-----------|-------------|----------|
| Model Size | 1.9B | 2B/5B | 2B | 30B |
| VAE Compression | 1:192 | 1:48 | 1:96 | 1:96 |
| Latent Channels | 128 | 16 | 16 | 16 |
| Input Patchifier | 1x1x1 | 2x2x1 | 2x2x1 | 2x2x1 |
| Attention | Self+Cross | Self-only | Self-only | Self+Cross |

## LTXV-13B Performance

### Speed
- Generates 5 seconds of video in approximately 4 seconds
- Up to 30x faster than comparable-scale models
- Runs on consumer-grade hardware (not limited to enterprise GPUs)
- Supports up to 60-second video generation (from v0.9.8)

### Resolution
- 1216x704 at 30 FPS (real-time generation)
- Progressive multiscale rendering enables higher effective quality

## LTX-2 (19B) Benchmark Results

### Inference Speed

| Model | Modality | Parameters | Seconds/Step |
|-------|----------|-----------|--------------|
| Wan 2.2-14B | Video Only | 14B | 22.30s |
| **LTX-2** | **Audio + Video** | **19B** | **1.22s** |

**18x speedup** despite generating both audio and video with more total parameters.

### Artificial Analysis Rankings (November 6, 2025)
- **3rd place** in Image-to-Video generation
- **4th place** in Text-to-Video generation
- Surpasses proprietary Sora 2 Pro and open-source Wan 2.2-14B

### Audiovisual Evaluation (Human Preference)
- Significantly outperforms open-source alternatives such as Ovi
- Comparable to leading proprietary models including Veo 3 and Sora 2

### Temporal Capacity Comparison

| Model | Max Duration | Type |
|-------|-------------|------|
| **LTX-2** | **20 seconds** | Open-source |
| Sora 2 | 16 seconds | Proprietary |
| Veo 3 | 12 seconds | Proprietary |
| Ovi | 10 seconds | Open-source |
| Wan 2.5 | 10 seconds | Open-source |

### Output Specifications

| Feature | Specification |
|---------|--------------|
| Max Resolution | 4K (native, via multi-tile inference) |
| Max Frame Rate | 50 fps |
| Audio Output | Stereo 24 kHz |
| Max Duration | 20 seconds synchronized A/V |

## LTX-2.3 (22B) Performance

### VAE Quality
- +2.5 dB PSNR improvement over LTX-2
- Sharper textures, cleaner hair, stable chrome reflections during camera moves

### Prompt Adherence
- 4x larger text connector enables accurate resolution of complex prompts with multiple subjects, spatial relationships, and stylistic instructions

### Audio Quality
- Upgraded HiFi-GAN vocoder produces cleaner stereo output at 24 kHz

## Third-Party Benchmark References

### T2VWorldBench (World Knowledge)
- LTX-Video average score: ~0.68 (moderate performance on world knowledge evaluation)

### IP-Bench (Image Protection)
- Nearly all protection methods yield lowest VBench degradation rates on LTX and Skyreel, indicating strong base performance

### Taming DiT for Mobile (arXiv:2507.13343)
- LTX-Video (1.9B) used as baseline; mobile-optimized 0.9B model achieves higher VBench total score than LTX-Video while being deployable on iPhone 16 Pro Max

## Key Takeaways

1. **Speed leadership:** LTX models consistently achieve the fastest generation times in their respective parameter classes, from real-time at 2B to 18x faster than competitors at 19B.

2. **Compression efficiency:** The 1:192 VAE compression ratio (4x higher than most competitors) is the primary enabler of speed, with the denoising decoder compensating for potential quality loss.

3. **Scaling effectiveness:** Performance scales well from 2B to 22B, with each generation adding capabilities (higher resolution, audio, longer duration) while maintaining speed advantages.

4. **Open-source competitiveness:** LTX-2 matches or exceeds proprietary systems (Sora 2, Veo 3) on key metrics while being fully open-source.

## Sources

- https://arxiv.org/abs/2501.00103
- https://arxiv.org/abs/2601.03233
- https://vchitect.github.io/VBench-project/
- https://huggingface.co/Lightricks/LTX-Video
- https://huggingface.co/Lightricks/LTX-2
