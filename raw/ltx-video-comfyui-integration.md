# LTX-Video ComfyUI Integration and Workflows

**Source:** https://github.com/Lightricks/ComfyUI-LTXVideo
**Source:** https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
**Source:** https://blog.comfy.org/p/ltx-video-095-day-1-support-in-comfyui
**Source:** https://comfyui-wiki.com/en/tutorial/advanced/ltx-video-workflow-step-by-step-guide

## Official ComfyUI Support

LTX-Video has first-class ComfyUI support through the official **ComfyUI-LTXVideo** repository maintained by Lightricks.

- **Repository:** https://github.com/Lightricks/ComfyUI-LTXVideo
- **Installation:** Via ComfyUI Manager or manual clone
- **Node category:** "LTXVideo" in node menu

## Integration Timeline

- **v0.9.5 (Mar 2025):** Day-1 ComfyUI support announced
- **v0.9.7 (May 2025):** New simplified flows and nodes
- **v0.9.8 (Jul 2025):** IC-LoRA and multi-condition workflow support

## Available Nodes

The ComfyUI-LTXVideo package provides specialized nodes for:

### Core Generation
- Text-to-video generation
- Image-to-video conversion
- Video-to-video transformation

### Advanced Features
- Frame interpolation for smooth transitions
- Sequence conditioning for video extension
- Multi-keyframe conditioning
- IC-LoRA control (depth, pose, canny)

### Specialized Nodes
- **LTXVLatentUpsampler:** Spatial upscaling (2x)
  - Parameters: upscale_factor: 2.0, denoise_strength: 0.35-0.55, steps: 4-8, cfg: 1.0
- **MultimodalGuider:** Independent per-modality guidance control
  - Separate control for audio and video
  - Independent prompt adherence settings
  - Artifact reduction per modality

## Workflow Types

### Simplified Workflows (v0.9.7+)
1. **Simplified image-to-video**
2. **Image-to-video with extension**
3. **Image-to-video with keyframes**

### Advanced Workflows
- Multi-condition generation
- Spatial + temporal upscaling pipeline
- IC-LoRA controlled generation
- Detail enhancement via detailer IC-LoRA

## Example Workflows Location

```
ComfyUI/custom_nodes/ComfyUI-LTXVideo/example_workflows/
```

## GGUF Integration

GGUF quantized models work in ComfyUI via the ComfyUI-GGUF custom node by city96:
- Model files placed in: `ComfyUI/models/unet/`
- Uses built-in LTXVideo nodes from ComfyUI Manager
- Supports quantized models from city96/LTX-Video-gguf and unsloth/LTX-2-GGUF

## Standard Pipeline in ComfyUI

1. **Stage 1:** Generate base video at target resolution
2. **Stage 2:** Spatial upscale 2x via LTXVLatentUpsampler
3. **Stage 3:** Temporal upscale (optional, doubles frame rate)
4. **Stage 4:** Refinement/denoising pass (optional)
