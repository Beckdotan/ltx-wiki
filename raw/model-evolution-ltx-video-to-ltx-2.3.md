# LTX Video Model Evolution: From LTX-Video to LTX-2.3

**Source:** https://huggingface.co/Lightricks/LTX-Video, https://huggingface.co/Lightricks/LTX-2, https://huggingface.co/Lightricks/LTX-2.3

---

## Version Timeline

### LTX-Video 0.9.0 (Late 2024)
- **Parameters:** 2B
- **Capability:** Text-to-video, image-to-video
- **Key Innovation:** First DiT-based real-time video generation model
- **Resolution:** 768x512 at 24fps, faster-than-real-time
- **VAE Compression:** 1:192 (32x32x8 spatiotemporal downscaling)
- **Text Encoder:** T5

### LTX-Video 0.9.1 (Late 2024)
- Improved quality over 0.9.0
- Community GGUF quantizations released

### LTX-Video 0.9.5 (Early 2025)
- LoRA support added
- Training datasets released (Squish, Cakeify)
- Base for official creative LoRAs

### LTX-Video 0.9.6 (Mid 2025)
- **15x faster** than previous versions
- Real-time capable on consumer hardware
- 2B distilled version

### LTX-Video 0.9.7 (Mid 2025)
- **13B parameter** version introduced (major scale-up from 2B)
- Distilled and Dev variants
- IC-LoRA support (canny, depth)

### LTX-Video 0.9.8 (Late 2025)
- **13B parameter** with improved quality
- IC-LoRA Detailer (29.8K downloads/month)
- Spatial and temporal upscalers
- Resolution: 1216x704 at 30fps

### LTX-2 (January 2026)
- **19B parameters** (14B video + 5B audio)
- **Joint audio-video generation** - first of its kind in open source
- Gemma 3 12B text encoder (replacing T5)
- Bidirectional audio-video cross-attention
- 20 seconds of continuous output
- 18x faster than Wan 2.2
- Downloads: 907K/month

### LTX-2.3 (March 2026)
- **22B parameters**
- Improved audio quality
- Improved visual quality
- Enhanced prompt adherence
- Distilled model (8-step inference)
- Multiple spatial upscalers (x2, x1.5)
- Temporal upscaler (x2 FPS)
- Downloads: 1.66M/month (highest of any version)

## Key Evolution Milestones

| Milestone | Version | Significance |
|-----------|---------|--------------|
| Real-time generation | 0.9.0 | First DiT model to achieve real-time |
| LoRA ecosystem | 0.9.5 | Custom effects and styles |
| Scale-up to 13B | 0.9.7 | Major quality improvement |
| Audio-video fusion | LTX-2 | First open-source joint A/V model |
| 22B with controls | LTX-2.3 | Union control, motion tracking |

## Architecture Changes

### LTX-Video -> LTX-2
- Added 5B audio stream alongside video stream
- Replaced T5 with Gemma 3 12B text encoder
- Added bidirectional cross-attention between audio/video
- Added thinking tokens for better prompt adherence
- Added modality-aware CFG

### LTX-2 -> LTX-2.3
- Increased from 19B to 22B parameters
- Improved audio quality
- Enhanced prompt adherence
- New upscalers (x1.5 spatial option added)
- Better LoRA compatibility (2.0 LoRAs work better on 2.3)

## Quantization/Variant Ecosystem Growth

| Version | Quantizations | Adapters | Finetunes |
|---------|--------------|----------|-----------|
| LTX-Video | 16 | 24 | 25 |
| LTX-2 | 8 | 49 | 54 |
| LTX-2.3 | 14 | 20 | 27 |
