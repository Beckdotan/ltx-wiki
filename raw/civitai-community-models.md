# Civitai Community Models and Workflows for LTX Video

## Overview
Civitai hosts a growing collection of community-created checkpoints, LoRAs, workflows, and articles for LTX Video models, serving as a major hub for community-driven model sharing and discovery.

## Notable Community Models

### LTX-2 19B Collection
- GGUF quantized versions (Q8_0) for lower-VRAM setups
- All-in-one resource page with links to checkpoints, LoRAs, and workflows
- **URL:** https://civitai.com/models/2291679/ltx-2-19b-all-you-need-is-here

### LTX-2 IC-LoRA Detailer
- Enhances fine visual details and local clarity in LTX-2 generated videos
- Improves textures, edges, and small visual elements
- Does not alter overall composition or motion
- **URL:** https://civitai.com/models/2293339/ltx2-ic-lora-detailer

### SSX LTX2 T2V LoRA
- Text-to-video LoRA trained with the official trainer
- Full weights at 1.5mp dataset resolution
- **URL:** https://civitai.com/models/2301350/ssx-ltx2-t2v

### LTXV 13B (FP8 Checkpoint)
- FP8 quantized version of the 13B v0.9.7 model
- Optimized for lower memory usage
- **URL:** https://civitai.com/models/982559/lightricks-ltxv

## Community Workflows

### LTX-2.3 22B GGUF Workflows (12GB VRAM)
- Optimized for systems with only 12GB VRAM
- Uses GGUF quantized models
- **URL:** https://civitai.com/models/2443867/ltx-23-22b-gguf-workflows-12gb-vram

### LTX-2 GGUF I2V Workflow
- Image-to-video workflow with power LoRA loaders
- Good quality outputs at reduced VRAM
- **URL:** https://civitai.com/models/2313807/ltx-2-gguf-i2v-image-to-video-workflow

### VideoFlow (LTX 2.3 + Wan 2.2/2.1)
- Multi-model workflow supporting both LTX-2.3 and Wan2.2/2.1
- Image-to-video (img2vid) generation
- Multiple versions (v2.0 for LTX 2.3)
- **URL:** https://civitai.com/models/1815300/videoflow-ltx-23-wan-2221-i2v-image-to-video-img2vid-workflow

### LTX-2.3 DEV/DIST with Ollama + RTX VSR
- Image-to-video and text-to-video workflows
- Integrates Ollama for local LLM prompt enhancement
- Uses NVIDIA RTX Video Super Resolution for upscaling
- **URL:** https://civitai.com/models/2318870/ltx-23-devdist-image-to-video-and-text-to-video-with-ollamartx-vsr

### LTX-2 Image Audio to Video
- Audio-to-video generation workflow for LTX-2.3
- **URL:** https://civitai.com/models/2306894

## Community Articles

### Character Consistency Without LoRAs
- Tutorial on achieving character consistency using 360-degree viewers with LTX Video 2.3 in ComfyUI
- No LoRA training required
- **URL:** https://civitai.com/articles/27654/character-consistency-without-loras-free-360-viewers-with-ltx-video-23-in-comfyui

## Community Trends
1. Strong focus on VRAM optimization (GGUF quantization, FP8 variants)
2. Multi-model workflows combining LTX with Wan2.x
3. Integration with local LLMs (Ollama) for prompt enhancement
4. NVIDIA RTX VSR upscaler widely used as a post-processing step
5. IC-LoRA-based detailers and style adapters popular

## Sources
- https://civitai.com (search "LTX Video", "LTX-2", "LTXV")
