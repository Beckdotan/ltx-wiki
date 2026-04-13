---
title: LTX-Video 0.9.5
type: source
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/ltx-video-095-release.md
  - raw/ltx-video-early-versions-090-091-095.md
tags:
  - ltx-video
  - release
  - v0.9.5
  - lightricks
  - keyframe-conditioning
  - comfyui
---
# LTX-Video 0.9.5

LTX-Video 0.9.5 was released on **March 5, 2025**, introducing keyframe conditional support as its headline feature. It was the last version before the [[ltx-video-distillation]] milestone in [[ltx-video-096]] and represents the mature state of the original 2B architecture prior to the dev/distilled split.

## Release Details

- **Release date:** March 5, 2025
- **Parameters:** 2B (unchanged)
- **License:** OpenRail-M (new -- allows commercial use, replacing the RAIL-M license from [[ltx-video-090]])
- **Repository:** [Lightricks/LTX-Video-0.9.5](https://huggingface.co/Lightricks/LTX-Video-0.9.5)

## Key Improvements Over 0.9.1

### Keyframe Conditional Support (Major New Feature)
- Condition generation on the first frame, last frame, or any intermediate frame
- Multi-keyframe control for precise video generation
- Greater flexibility for video extension and editing workflows

### Quality Enhancements
- Improved resolution and visual quality over [[ltx-video-091]]
- Reduced artifacts for smoother, more natural visuals
- Better prompt understanding and adherence
- Enhanced detail generation

### Longer Video Generation
- Ability to generate longer video sequences for more complex narrative needs

### Commercial License
- Introduced OpenRail-M license enabling commercial use
- A significant shift from the more restrictive RAIL-M license of prior versions

### ComfyUI Day-1 Support
- Native integration with ComfyUI from launch day
- Seamless integration into existing ComfyUI workflows

## Output Specifications

| Property | Value |
|----------|-------|
| Resolution | Up to 768x512 (real-time), supports up to 720x1280 |
| Resolution constraint | Divisible by 32 |
| Frame rate | 24-30 fps |
| Max frames | 257 (8n+1 pattern) |

## Inference Configuration

- **Inference steps:** 50 (standard)
- **Precision:** torch.bfloat16 recommended
- **Negative prompts:** Supported
- **Padding:** Automatic padding to nearest 32/8+1 divisible values, then cropping

## Availability

- **Hugging Face:** [Lightricks/LTX-Video-0.9.5](https://huggingface.co/Lightricks/LTX-Video-0.9.5)
- **GitHub:** [Lightricks/LTX-Video](https://github.com/Lightricks/LTX-Video)
- **ComfyUI:** Native node support
- **Inference providers:** LTX-Studio, Fal.ai, Replicate
- **Community GGUF:** [city96/LTX-Video-0.9.5-gguf](https://huggingface.co/city96/LTX-Video-0.9.5-gguf) for lower VRAM usage

## Significance

Version 0.9.5 was the bridge between the early iterative refinements ([[ltx-video-090]], [[ltx-video-091]]) and the architectural leap of [[ltx-video-096]] which introduced [[ltx-video-distillation]]. It was also the last version before the major 13B scale-up in v0.9.7. The introduction of keyframe conditioning and the commercial license made it the first version broadly suitable for production use.

## References

- [Hugging Face repository](https://huggingface.co/Lightricks/LTX-Video-0.9.5)
- [ComfyUI blog: Day 1 Support](https://blog.comfy.org/p/ltx-video-095-day-1-support-in-comfyui)
- [Stable Diffusion Art guide](https://stable-diffusion-art.com/ltx-video-0-9-5/)
