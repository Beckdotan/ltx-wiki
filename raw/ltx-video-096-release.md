# LTX-Video 0.9.6 Release (~April 2025)

**Source:** https://x.com/LTXStudio/status/1912894622927298809
**Source:** https://www.patreon.com/posts/ltx-video-v0-9-6-127046557
**Source:** https://huggingface.co/Lightricks/LTX-Video

## Release Details

- **Release date:** ~April 2025 (less than three weeks before the 0.9.7 release on May 6, 2025)
- **Parameters:** 2B
- **Major change:** Introduction of two distinct model variants

## Two Model Variants Introduced

### ltxv-2b-0.9.6-dev (Full-Quality)
- **File size:** ~6.34 GB
- **Purpose:** Final outputs requiring highest quality
- **Inference:** Standard diffusion steps

### ltxv-2b-0.9.6-distilled (Distilled)
- **File size:** ~6.34 GB
- **Purpose:** Rapid iteration and lightweight deployment
- **Speed:** 15x faster inference than non-distilled
- **Steps:** 8 diffusion steps recommended (or fewer)
- **No CFG/STG required:** Does not need classifier-free guidance or spatio-temporal guidance
- **Real-time capable** on H100

## Key Improvements from 0.9.5

### Quality
- Enhanced prompt adherence
- Smoother motion
- Finer details for more realistic and coherent video outputs
- More stable generation

### Performance
- Ultra-fast inference: up to 15x faster with distilled variant
- High-quality videos in seconds with just 8 diffusion steps
- Real-time generation on H100 with distilled model

### New Default Settings
- Default resolution: 1216 x 704 pixels
- Default FPS: 30 FPS

## Significance

Version 0.9.6 was a critical stepping stone between the 2B-only era and the 13B model release. It introduced the distillation approach that would carry forward into the 13B models, proving that high-quality video could be generated with dramatically fewer diffusion steps. The dev/distilled split became the standard pattern for all subsequent releases.

## Output Specifications

- **Resolution:** 1216x704 (default), works best under 720x1280
- **Frame Rate:** 30 FPS
- **Maximum Frames:** 257 (8n+1 pattern)
- **Efficient sampling:** 8 steps recommended for distilled model

## Availability

- **Hugging Face:** https://huggingface.co/Lightricks/LTX-Video (within main repo)
- **Config files:** ltxv-2b-0.9.6-dev.yaml, ltxv-2b-0.9.6-distilled.yaml
