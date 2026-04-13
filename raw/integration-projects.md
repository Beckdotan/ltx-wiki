# LTX Video Integration Projects

## First-Party Integrations

### ComfyUI (Official Nodes)
- **Repo:** https://github.com/Lightricks/ComfyUI-LTXVideo
- Official custom nodes for LTX-2 video generation in ComfyUI
- LTX-2 integrated into ComfyUI core; supplementary repo provides advanced nodes
- 3,400+ stars, actively maintained
- See `comfyui-integration.md` for full details

### Hugging Face Diffusers
- LTX Video integrated into the Diffusers Python library
- Pipelines: LTXPipeline, LTXImageToVideoPipeline, LTXConditionPipeline, LTX2Pipeline
- LoRA loading support via standard Diffusers API
- Single-file checkpoint loading via from_single_file()
- **Docs:** https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
- **LTX-2 Docs:** https://huggingface.co/docs/diffusers/api/pipelines/ltx2

### LTX Desktop
- Open-source desktop app combining video generation with NLE editing
- **Repo:** https://github.com/Lightricks/LTX-Desktop
- See `ltx-desktop.md` for full details

### LTX Studio (Cloud Platform)
- Lightricks' flagship creative development platform
- Full production suite with AI video generation, editing, voiceover
- XML export to Premiere Pro, DaVinci Resolve
- **URL:** https://ltx.studio

### PyTorch Native API
- Direct Python inference without Diffusers abstraction
- Full control over all features via ltx-pipelines package
- Eight specialized pipeline implementations
- **Docs:** https://docs.ltx.video/open-source-model/integration-tools/pytorch-api

## Cloud Platform Integrations

### fal.ai
- LTX-2.3 available in Pro and Fast variants
- Text-to-video, image-to-video, audio-to-video
- Developer guide and API documentation available
- Also provides LoRA training service for LTX models
- **URL:** https://fal.ai/models/fal-ai/ltx-2.3/image-to-video
- **Guide:** https://fal.ai/learn/devs/ltx-video-2-pro-image-to-video-developer-guide

### Replicate
- Hosted inference API for LTX Video models
- CLI, API, and web UI access
- **Source:** Referenced in official LTX documentation

### Modal
- Deploy LTX-Video via CLI, API, and web UI
- Serverless GPU infrastructure
- **Docs:** https://modal.com/docs/examples/image_to_video

### DigitalOcean GPU Droplets
- Tutorial for deploying LTX-2.3 on GPU Droplets
- Recommended: NVIDIA H100 or H200 (also works on A6000)
- **Tutorial:** https://www.digitalocean.com/community/tutorials/generate-videos-ltx-23

### Vast.ai
- LTX Video available in AI Model Library
- Build and deploy on Vast.ai GPU marketplace
- **URL:** https://vast.ai/model/ltx-video

### RunComfy
- LTX 2 API access with unified parameters and sample code
- Browser-based ComfyUI with LTX models pre-loaded
- Also offers browser-based LoRA training
- **URL:** https://www.runcomfy.com/models/ltx/ltx-2/fast/text-to-video/api

## Third-Party Training Services

### WaveSpeedAI
- LTX-2 19B Video LoRA Trainer
- IC-LoRA Trainer for video-to-video transformations
- LTX-2.3 Image-to-Video and Text-to-Video LoRA services
- No cold starts; training jobs start immediately
- Pricing: $0.35-$7.00 per training job (100-2000 steps)
- **URL:** https://wavespeed.ai/models/wavespeed-ai/ltx-2-19b/video-lora-trainer

### Oxen.ai
- Character LoRA training for LTX-2
- Upload data, automated GPU orchestration
- **Tutorial:** https://ghost.oxen.ai/how-to-train-a-ltx-2-character-lora-with-oxen-ai/

## Community Integration Projects

### ComfyUI-LTXTricks (Deprecated)
- Created by GitHub user logtd
- Advanced techniques: RF-Inversion, RF-Edit, FlowEdit, I+V2V, STG
- All functionality now merged into official ComfyUI-LTXVideo
- **Repo:** https://github.com/logtd/ComfyUI-LTXTricks

### Phr00t/LTX2-Rapid-Merges
- Community model merges for LTX-2
- **URL:** https://huggingface.co/Phr00t/LTX2-Rapid-Merges

### a-r-r-o-w/LTX-Video-diffusers
- Community Diffusers-compatible conversion of LTX Video
- **URL:** https://huggingface.co/a-r-r-o-w/LTX-Video-diffusers

### OpenArt Workflows
- Multiple community-created ComfyUI workflows for LTX Video
- Video-to-video, image-to-video, low-VRAM optimized workflows
- **URL:** https://openart.ai/workflows (search "LTX")

### Civitai Community
- GGUF quantized models and workflows for low-VRAM setups
- IC-LoRA Detailer models
- VideoFlow workflow supporting LTX-2.3 + Wan2.2
- Ollama + RTX VSR integration workflows
- **URL:** https://civitai.com (search "LTX")

## Hardware Partner Integrations

### NVIDIA RTX
- Official NVIDIA guide featuring LTX-2.3 in RTX AI Video Generation workflow
- RTX Video Super Resolution upscaler node available in ComfyUI
- Featured at CES 2026 and GDC for game developer workflows
- **Blog:** https://blogs.nvidia.com/blog/rtx-ai-garage-ces-2026-open-models-video-generation/
- **GDC Blog:** https://blogs.nvidia.com/blog/rtx-ai-garage-flux-ltx-video-comfyui-gdc/

## Integration with External AI Services

### Google Gemini
- LTX Desktop supports Gemini for AI prompt suggestions
- Used in upcoming "Bridge Shots" feature for generating transition footage

### fal.ai Image Generation
- LTX Desktop uses fal.ai for Z Image Turbo text-to-image generation

## Sources
- https://docs.ltx.video/open-source-model/integration-tools/
- https://huggingface.co/docs/diffusers/api/pipelines/ltx_video
- https://ltx.io/model/ltx-2
- https://ltx.io/
