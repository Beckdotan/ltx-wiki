# Reddit r/comfyui - LTX Video Workflow Discussions

## Source
- **Platform**: Reddit r/comfyui and ComfyUI community forums
- **Date range**: 2024 - 2026
- **URLs**:
  - https://github.com/Lightricks/ComfyUI-LTXVideo (Official ComfyUI-LTXVideo nodes)
  - https://github.com/logtd/ComfyUI-LTXTricks (Community LTXTricks nodes)
  - https://docs.comfy.org/tutorials/video/ltx/ltx-2-3 (Official LTX-2.3 workflow examples)
  - https://blog.comfy.org/p/ltx-video-095-day-1-support-in-comfyui (Day-1 ComfyUI support blog post)
  - https://comfyui-wiki.com/en/tutorial/advanced/ltx-video-workflow-step-by-step-guide (Step-by-step guide)
  - https://www.facebook.com/groups/comfyui/posts/880881668017868/ (LTX-2 workflows discussion)
  - https://www.facebook.com/groups/comfyui/posts/917484794357555/ (GGUF workflow discussion)
  - https://www.facebook.com/groups/comfyui/posts/919303280842373/ (Achieving quality results)

## Key Discussion Points

### Official ComfyUI Integration
- LTX Video nodes appear under the "LTXVideo" category in ComfyUI's node menu
- Day-1 support was provided for LTX-Video 0.9.5 by ComfyUI team
- Recommended workflows available in ComfyUI Manager
- LTX-2.3 is natively supported in ComfyUI (latest version)

### Essential Custom Nodes
1. **ComfyUI-LTXVideo** - Core custom node suite by Lightricks (official)
2. **ComfyUI-LTXTricks** - Community nodes by logtd for additional control
3. **ComfyUI-GGUF** - Crucial for running quantized models on <24GB VRAM
4. **ComfyUI-VideoHelperSuite (VHS)** - Standard for handling video file I/O and MP4 outputs

### Two-Stage Sampling Workflow (Most Popular)
- Widely validated by top creators on X as the optimal balance between coherence and sharpness
- **Stage 1**: Generate video at half target resolution, focusing on motion structure and scene coherence using MultiModalGuider node
- **Stage 2**: Use LTXVLatentUpsampler for 2x spatial upscale
- This is the recommended "premium-quality" approach

### Three-Stage Workflow (Community Fix)
- Developed to solve issues with the two-stage approach:
  - Stiff motion
  - Broken background audio
  - Standard two-stage crashes
  - Rigid movements that ruin fine details
- Alternative workflow adds an intermediate refinement pass

### Community Preferences for Settings
- Reddit discussions heavily favor using the "Dev" model paired with a distilled LoRA
- Recommended settings: CFG around 4.0, 20 steps
- This gives "high-fidelity stability" while accelerating render time

### Low-VRAM Optimization
- Full uncompressed LTX 2.3 setup demands over 40GB VRAM
- Community created variations running on consumer hardware (12GB GPUs like RTX 3060)
- GGUF quantized models are the primary solution for limited VRAM
- Kijai's quantized FP8 variants on HuggingFace are widely used

### Hardware Discussion
- NVIDIA GPU with at least 12GB VRAM recommended
- Can "scrape by on 8GB with extreme optimizations and low frame counts"
- RTX 4090 is the gold standard for local LTX video generation
- Turbo 24GB or Ultra 48GB machines recommended on ThinkDiffusion cloud

### Quality Improvements in LTX 2.3
- Vastly improved latent space and larger text connector
- Better preservation of fine details: skin textures, fabric movements, hair
- Stronger adherence to complex, multi-sentence prompts
- Community noted significant quality jump from previous versions

## Sentiment Analysis
- **Overall**: Very positive - ComfyUI is the primary way community uses LTX Video
- **Strengths**: Easy integration, active workflow sharing, official support
- **Weaknesses**: VRAM demands for full quality, complexity of multi-stage workflows
- **Community**: Highly collaborative, lots of workflow sharing and optimization tips
