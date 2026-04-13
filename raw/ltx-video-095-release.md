# LTX-Video 0.9.5 Release (March 2025)

**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.5
**Source:** https://blog.comfy.org/p/ltx-video-095-day-1-support-in-comfyui
**Source:** https://news.aibase.com/news/16004
**Source:** https://stable-diffusion-art.com/ltx-video-0-9-5/

## Release Details

- **Release date:** March 5, 2025
- **Parameters:** 2B
- **License:** OpenRail-M (new -- allows commercial use)

## Key Improvements from 0.9.1

### Keyframe Conditional Support (Major New Feature)
- Ability to generate content conditioned on the video's first frame, last frame, or any intermediate frame
- Greater flexibility for video extension and editing
- Multi-keyframe control for precise video generation

### Quality Enhancements
- Improved resolution and visual quality
- Reduced artifacts for smoother, more natural visuals
- Better prompt understanding and adherence
- Enhanced detail generation

### Longer Video Generation
- Ability to generate longer video sequences
- Caters to more complex narrative needs

### Commercial License
- Introduced OpenRail-M license for commercial use
- Businesses and individual developers can now use the model in commercial projects

### ComfyUI Day-1 Support
- Native integration with ComfyUI from launch
- Seamless integration into existing workflows

## Output Specifications

- **Resolution:** Up to 768x512 (real-time), supports up to 720x1280
- **Frame Rate:** 24-30 FPS
- **Maximum Frames:** 257 (must be divisible by 8+1)
- **Frame Divisibility:** 8n + 1 (e.g., 9, 17, 25, 33, ..., 257)
- **Resolution Divisibility:** Must be divisible by 32

## Inference Parameters

- **Inference steps:** 50 (standard)
- **Precision:** torch.bfloat16 recommended
- **Negative prompts:** Supported ("worst quality, inconsistent motion, blurry, jittery, distorted")
- **Padding:** Automatic padding to nearest 32/8+1 divisible values, then cropping to desired dimensions

## Hardware Requirements

- Python 3.10.5+
- CUDA 12.2+
- PyTorch >= 2.1.2

## Availability

- **Hugging Face:** https://huggingface.co/Lightricks/LTX-Video-0.9.5
- **GitHub:** https://github.com/Lightricks/LTX-Video
- **ComfyUI:** Native node support
- **Inference providers:** LTX-Studio, Fal.ai, Replicate

## Community GGUF Quantization

- **city96/LTX-Video-0.9.5-gguf** on Hugging Face provides GGUF quantized versions for lower VRAM usage
