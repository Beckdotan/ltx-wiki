# ComfyUI Integration for LTX Video

## Official ComfyUI-LTXVideo Nodes

### Repository
- **URL:** https://github.com/Lightricks/ComfyUI-LTXVideo
- **Stars:** ~3,400 | **Forks:** ~368
- **Languages:** Python (93.7%), JavaScript (6.3%)

### Description
A collection of powerful custom nodes extending ComfyUI's capabilities for the LTX-2 video generation model. LTX-2 is integrated into ComfyUI core, and this repository provides supplementary nodes and workflows for accessing advanced capabilities.

### Installation
Available via ComfyUI Manager: search "LTXVideo," install, then restart ComfyUI. Nodes appear under the "LTXVideo" category. Required models download on first use.

### System Requirements
- CUDA-compatible GPU (32GB+ VRAM minimum for full models)
- 100GB+ storage for models
- ComfyUI installation

### Node Categories
- Conditioning loaders/savers
- Dynamic conditioning
- Embeddings connectors
- Gemma API text encoding integration
- IC-LoRA attention modules
- Latent normalization
- Mask processing
- Prompt enhancement
- Spatial/temporal upscaling
- Low VRAM optimizations
- Tiled sampling and VAE decode

### Control Methods
- Unified IC-LoRA supporting depth and edge (canny) conditions
- Motion tracking control
- Pose control
- Camera movement controls (dolly, jib movements)
- Detailing workflows

### Workflow Types
- Text-to-Video
- Image-to-Video
- Audio-to-Video
- Frame Interpolation
- Sequence Conditioning (video extension)
- Video-to-Video transformations
- IC-LoRA transformations

### Processing Modes
- Single-stage pipeline (quick prototyping)
- Two-stage pipeline (production quality with 2x upsampling)
- Distilled model variants for faster inference
- Downsampled latent processing for memory reduction

## Community ComfyUI Nodes (Deprecated)

### ComfyUI-LTXTricks by logtd
- **URL:** https://github.com/logtd/ComfyUI-LTXTricks
- **Stars:** ~512 | **Forks:** ~23
- **Status:** DEPRECATED -- all functionality merged into official repo
- **License:** GPL-3.0

Implemented advanced techniques:
- RF-Inversion (rectified stochastic differential equations for semantic inversion/editing)
- RF-Edit/RF-Solver-Edit (taming rectified flow for inversion and editing)
- FlowEdit (inversion-free text-based editing using pre-trained flow models)
- Image+Video to Video (I+V2V)
- Enhance (STG) -- spatiotemporal skip guidance

## OpenArt Community Workflows
Multiple community-created LTX Video workflows are shared on OpenArt:
- LTX-Video Video-to-Video workflows
- ComfyUI LTXVideo Image-to-Video
- LTX Video 8VRAM (optimized for low-VRAM GPUs)
- LTX Basic workflows
- LTX Image-to-Video adapters
- **Source:** https://openart.ai/workflows (search "LTX")

## NVIDIA RTX Integration
NVIDIA features LTX Video in its official RTX AI Video Generation Guide. The workflow runs locally on RTX GPUs using Blender, ComfyUI, and LTX-2.3 with the RTX Video Super Resolution upscaler node.
- Optimize at 1280x720, keep sequences under 257 frames
- Scale to 1920x1080 when ready
- **Source:** https://www.nvidia.com/en-us/geforce/news/rtx-ai-video-generation-guide/
- **Source:** https://blogs.nvidia.com/blog/rtx-ai-garage-ces-2026-open-models-video-generation/

## LTX-2.3 ComfyUI Workflow Guide
Official documentation and examples for the latest LTX-2.3 model in ComfyUI:
- **Source:** https://docs.comfy.org/tutorials/video/ltx/ltx-2-3
- **Source:** https://ltx.io/model/model-blog/comfyui-workflow-guide
- **Source:** https://docs.ltx.video/open-source-model/integration-tools/comfy-ui
