# LTX-2 in ComfyUI: Nodes, Workflows, and Integration

## Overview

LTX-2 is natively supported in ComfyUI through the official ComfyUI-LTXVideo node pack maintained by Lightricks. The nodes address specific workflow pain points: prompt iteration speed, quality stability, advanced guidance control, and IC-LoRA strength tuning.

## Repository
- **GitHub:** https://github.com/Lightricks/ComfyUI-LTXVideo
- **Documentation:** https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
- **Node Reference:** https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes
- All nodes appear under the "Lightricks" category in ComfyUI

## Installation

Models are automatically downloaded on first use. Users can load a workflow and click "Queue Prompt" to begin.

Workflow files are located at:
```
ComfyUI/custom_nodes/ComfyUI-LTXVideo/example_workflows/
```

## Key LTX-2 Specific Nodes

### Text Conditioning Nodes

#### Problem Addressed
Gemma 3-12B (the text encoder) has a large memory footprint, requiring frequent VRAM load/unload cycles that slow prompt iteration on consumer GPUs.

#### Solution Nodes
1. **Text Encode (API)** -- Offloads text encoding to a free API endpoint
2. **Save Text Conditioning** -- Saves computed text conditioning to disk as .safetensors for reuse
3. **Load Text Conditioning** -- Loads previously saved conditioning from disk

These nodes remove Gemma from the critical path, enabling faster prompt iteration without repeatedly loading the 12B text encoder.

### Advanced Sampler (LTXVAdvancedSampler)

#### Purpose
Drop-in replacement for standard samplers that applies statistical normalization to latents during generation.

#### Key Features
- Prevents overbaking and audio clipping
- Addresses latent value drift into problematic ranges during denoising
- Especially effective with high guidance values
- Compatible with all guider nodes (including MultimodalGuider)
- Compatible with text/image conditioning, LoRA, and IC-LoRA workflows

### Multimodal Guider (MultimodalGuider)

#### Problem Addressed
Standard guidance treats audio and video as a single unit. Increasing guidance for video quality affects audio synchronization and vice versa.

#### Solution
Decouples audio and video guidance controls:
- **Video guidance** -- tune independently for visual quality
- **Audio guidance** -- tune independently for audio quality
- **Cross-modal alignment** -- control sync without affecting individual quality

Enables dialing up motion fluidity without overfitting to the prompt.

### IC-LoRA Control (LTXVAddVideoICLoRAGuideAdvanced)

#### Features
- Granular IC-LoRA strength control
- Global strength adjustment
- Optional spatial/spatiotemporal masking
- Three control modes: Canny (edge preservation), Depth (camera/spatial geometry), Pose (human motion transfer)

### Tiled Sampler (LTXVTiledSampler)
- For generating larger videos by tiling
- Manages VRAM for high-resolution generation

### Looping Sampler
- Creates seamlessly looping video clips
- Documentation at: ComfyUI-LTXVideo/looping_sampler.md

## Pre-configured Workflows

### LTX-2 Workflows (6 included)
The LTX-2 release includes six pre-configured workflows demonstrating different generation modes:
1. Text-to-video (basic)
2. Text-to-video with upsampling (two-stage)
3. Image-to-video
4. Audio-to-video
5. IC-LoRA control
6. Keyframe interpolation

### LTX-2.3 Workflows
- Updated workflows for the 2.3 model
- Documentation: https://docs.comfy.org/tutorials/video/ltx/ltx-2-3
- Official blog guide: https://ltx.io/model/model-blog/comfyui-workflow-guide

### Community Workflows
- **RuneXX/LTX-2-Workflows** -- Community workflow collection on Hugging Face
- **Civitai** -- Multiple community workflows
- **kombitz.com** -- Workflow resources and community references

## NVIDIA Quantization Support in ComfyUI

### NVFP8
- Available for RTX 30/40/50 Series
- 2x faster, 40% less VRAM
- Checkpoints available directly in ComfyUI

### NVFP4
- Available for RTX 50 Series only
- 3x faster, 60% less VRAM
- Announced at CES 2026

## GGUF Support in ComfyUI

- Community GGUF quantizations work through ComfyUI-GGUF nodes
- Gemma 3-12B GGUF support tracked in: https://github.com/city96/ComfyUI-GGUF/issues/398
- Enables running LTX-2 on GPUs with as little as 8-10GB VRAM

## Recommended Workflow (Community Consensus)

Reddit and community discussions heavily favor:
1. Using the "Dev" model paired with a distilled LoRA
2. Gives high-fidelity stability while accelerating render time
3. Two-stage generation: base resolution + latent upscaling

## Awesome-LTX2 Community Resource
- **GitHub:** https://github.com/wildminder/awesome-ltx2
- Curated list of all available LTX-2 models, encoders, workflows, LoRAs for ComfyUI

## Sources

- [LTX-2 ComfyUI Nodes Documentation](https://docs.ltx.video/open-source-model/integration-tools/ltx-2-comfy-ui-nodes)
- [Using ComfyUI with LTX-2](https://docs.ltx.video/open-source-model/integration-tools/comfy-ui)
- [ComfyUI-LTXVideo GitHub](https://github.com/Lightricks/ComfyUI-LTXVideo/)
- [LTX-2.3 ComfyUI Workflow Examples](https://docs.comfy.org/tutorials/video/ltx/ltx-2-3)
- [LTX-2 ComfyUI Blog -- Comfy.org](https://blog.comfy.org/p/ltx-2-open-source-audio-video-ai)
- [ComfyUI Workflow Guide -- LTX Blog](https://ltx.io/model/model-blog/comfyui-workflow-guide)
- [LTX-2 in ComfyUI -- kombitz](https://www.kombitz.com/2026/01/11/ltx-2-in-comfyui-workflows-resources-community-references/)
- [How to Configure LTX-2 in ComfyUI -- DEV Community](https://dev.to/gary_yan_86eb77d35e0070f5/how-to-configure-ltx-2-in-comfyui-complete-2026-guide-for-ai-video-generation-32lk)
- [awesome-ltx2 GitHub](https://github.com/wildminder/awesome-ltx2)
