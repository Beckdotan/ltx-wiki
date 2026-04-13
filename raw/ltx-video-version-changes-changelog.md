# LTX-Video Version Changes and Changelog

**Source:** https://github.com/Lightricks/LTX-Video
**Source:** https://huggingface.co/Lightricks/LTX-Video
**Source:** https://ltx.studio/release-notes
**Source:** https://x.com/LTXStudio/status/1870099463416803594
**Source:** https://x.com/LTXStudio/status/1912894622927298809

## Version 0.9.0 -> 0.9.1 (Nov 2024 -> Dec 2024)

### Improvements
- **Smoother motion:** More natural and fluid video movement
- **Improved physics:** Better physical plausibility in generated scenes
- **Cleaner visuals:** Reduced artifacts and visual noise
- **Enhanced image-to-video quality:** Significant boost when conditioning on input images
- **Training-free video enhancement:** First capability to improve generated video without retraining

### Unchanged
- Same 2B parameter architecture
- Same resolution/fps capabilities
- Same inference requirements

---

## Version 0.9.1 -> 0.9.5 (Dec 2024 -> Mar 5, 2025)

### New Features
- **Multi-keyframe conditional support:** Generate from first frame, last frame, or any intermediate frame
- **Video extension:** Greater flexibility for extending videos forward and backward
- **Commercial license:** OpenRail-M license introduced (vs RAIL-M for 0.9.0/0.9.1)
- **ComfyUI native integration:** Day-1 support in ComfyUI

### Improvements
- **Resolution and quality enhancements:** Reduced artifacts, smoother visuals
- **Longer video generation:** Support for longer sequences
- **Better prompt understanding and adherence**

### Unchanged
- Same 2B parameter architecture
- 1216x704 native resolution, 30 FPS

---

## Version 0.9.5 -> 0.9.6 (Mar 2025 -> ~Apr 2025)

### Major Changes
- **First dev/distilled split:** Two separate model variants introduced
  - Dev: Full-quality for final outputs
  - Distilled: For rapid iteration and lightweight deployment
- **Distillation technique:** 15x faster inference, 8 diffusion steps, no CFG/STG needed

### Improvements
- **Enhanced prompt adherence**
- **Smoother motion**
- **Finer details:** More realistic and coherent video outputs
- **Ultra-fast inference:** 8 diffusion steps for distilled model

### New Default Settings
- Resolution: 1216 x 704 pixels
- FPS: 30

### Unchanged
- 2B parameters
- Same base architecture

---

## Version 0.9.6 -> 0.9.7 (Apr 2025 -> May 6, 2025)

### Major Changes
- **13B parameter model:** Massive scale-up from 2B to 13B (6.5x increase)
- **Multiscale rendering:** New breakthrough rendering approach
  - Drafts at lower detail first for coarse motion
  - Progressively adds structure, lighting, micro-motion
  - For 1080p: renders at 960x540, then upscales 2x
  - 30x faster than comparable-sized models
- **Spatial upscaler:** 2x resolution increase capability
- **FP8 quantization:** Official quantized models provided
- **LoRA support:** LoRA fine-tuning and distilled-lora128 variant

### New Model Variants
| Variant | New |
|---------|-----|
| ltxv-13b-0.9.7-dev | Yes |
| ltxv-13b-0.9.7-distilled | Yes |
| ltxv-13b-0.9.7-dev-fp8 | Yes |
| ltxv-13b-0.9.7-distilled-fp8 | Yes |
| ltxv-13b-0.9.7-distilled-lora128 | Yes |

### Improvements
- **Breakthrough prompt adherence and physical understanding**
- **Enhanced video length:** Up to 60 seconds
- **Simplified ComfyUI flows:** New simplified nodes for img2vid, img2vid+extension, img2vid+keyframes

### 2B Models
- 2B models from v0.9.6 continued to be available alongside new 13B models

---

## Version 0.9.7 -> 0.9.8 (May 2025 -> Mid-July 2025)

### New Features
- **IC-LoRA control models:** Depth, pose, canny edge control
- **Detailer model:** LTX-Video-ICLoRA-detailer-13B-0.9.8 for enhancing fine details
- **New 2B distilled checkpoints:** Both 2B and 13B distilled models added
- **Video-to-video generation:** Conditioning on video segments
- **Multi-condition generation:** Condition on multiple images/videos at different frame indices
- **IC-LoRA compatibility:** Works with official IC-LoRAs

### Improvements
- **Better prompt understanding and detail generation**
- **Slightly faster than 0.9.7**
- **Extended video generation:** Confirmed 60-second capability

### New Model Variants
| Variant | New |
|---------|-----|
| ltxv-13b-0.9.8-dev | Yes |
| ltxv-13b-0.9.8-distilled | Yes |
| ltxv-13b-0.9.8-dev-fp8 | Yes |
| ltxv-13b-0.9.8-distilled-fp8 | Yes |
| ltxv-2b-0.9.8-distilled | Yes |
| ltxv-2b-0.9.8-distilled-fp8 | Yes |

### Pipeline Change
- New pipeline class: LTXConditionPipeline (enables multi-condition support)

### Known Issues
- Some users reported plastic-looking skin in certain generations (GitHub Issue #230)

---

## Summary Timeline

```
Nov 2024  v0.9.0  First release, 2B, real-time generation
Dec 2024  v0.9.1  Motion/physics/visual quality improvements
Mar 2025  v0.9.5  Keyframe conditioning, commercial license, ComfyUI
Apr 2025  v0.9.6  Dev/distilled split, 15x faster distilled
May 2025  v0.9.7  13B model, multiscale rendering, spatial upscaler
Jul 2025  v0.9.8  IC-LoRA, detailer, 60s videos, multi-condition
```
