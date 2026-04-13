# ComfyUI LTX Video Workflows: txt2vid, img2vid, vid2vid

## Sources
- [All In One Custom Workflow IMG2Vid and Txt2Vid | OpenArt](https://openart.ai/workflows/jaguar_pesky_18/all-in-one-custom-workflow-vid2vid-and-txt2vid-using-comfyui-ltx-video-model/NE2kbxQX5Z0Of7eA3olt)
- [ltx-video (txt2vid - img2vid) by 77oussam | OpenArt](https://openart.ai/workflows/houssam/ltx-video-txt2vid---img2vid--77oussam/NqiWbONv9zeIpAZB5UBi)
- [All In One Workflow LTXV 0.9.1 | OpenArt](https://openart.ai/workflows/jaguar_pesky_18/all-in-one-workflow-using-the-new-low-vram-ltxv-091-video-model-for-vid2vid-txt2vid-img2vid/RhE223hdGDPmla3L32hH)
- [Smooth Workflow LTX (I2V/T2V) | Civitai](https://civitai.com/models/2524321/smooth-workflow-ltx-i2vt2v)
- [LTX IMAGE to VIDEO with STG workflow | Civitai](https://civitai.com/models/995093/ltx-image-to-video-with-stg-caption-and-clip-extend-workflow)
- [Using ComfyUI with LTX-2 | LTX Documentation](https://docs.ltx.video/open-source-model/integration-tools/comfy-ui)
- [LTX-2.3 ComfyUI Workflow Examples | ComfyUI Docs](https://docs.comfy.org/tutorials/video/ltx/ltx-2-3)
- [MimicPC - LTXVideo: text2Video / Img2Video](https://www.mimicpc.com/workflows/ltxv-video)
- [LTX-2 ComfyUI Workflow | RunComfy](https://www.runcomfy.com/comfyui-workflows/ltx-2-comfyui-workflow-real-time-video-generation-speed)
- [Fast Local video: LTX Video | Stable Diffusion Art](https://stable-diffusion-art.com/ltx-video/)

## Overview

LTX Video supports three primary workflow types in ComfyUI: Text-to-Video (txt2vid), Image-to-Video (img2vid), and Video-to-Video (vid2vid). These workflows can be used individually or combined in all-in-one setups. LTX-2.3 additionally supports audio generation synchronized with video.

---

## Text-to-Video (txt2vid) Workflows

### Basic txt2vid Workflow Structure

1. **Load Model** — LTXVLoader or CheckpointLoaderSimple to load the LTX model
2. **Text Encoding** — Gemma/CLIPTextEncode for positive and negative prompts
3. **Empty Latent Video** — Generate blank latent space for specified resolution and frames
4. **Sampling** — KSampler or LTXVNormalizingSampler for denoising
5. **VAE Decode** — Convert latents back to pixel space
6. **Video Output** — VHS_VideoCombine to merge frames into video file

### Recommended txt2vid Settings

| Parameter | Value |
|-----------|-------|
| Resolution | 768x512 (landscape) or 512x768 (portrait) |
| Frames | 65 (multiples of 8 + 1) |
| FPS | 24-25 |
| Steps | 20-30 |
| CFG | 3.0-7.0 |
| Sampler | euler or euler_ancestral |
| Scheduler | normal or sgm_uniform |
| Denoise | 1.0 |

### Prompting Tips for txt2vid

- Use detailed descriptions including actions, scene transitions, and cinematography terminology
- Include motion descriptions: "camera slowly pans left", "subject walks toward camera"
- Negative prompt suggestion: "worst quality, inconsistent motion, blurry, jittery, distorted, watermarks"
- Consider using the LTXVPromptEnhancer node to automatically enrich basic prompts

---

## Image-to-Video (img2vid) Workflows

### Basic img2vid Workflow Structure

1. **Load Image** — LoadImage node for the reference image
2. **Load Model** — LTXVLoader for the LTX model
3. **Image Preprocessing** — LTXVPreprocess to prepare the image
4. **Add Guide** — LTXVAddGuide to condition generation on the reference image
5. **Text Encoding** — Gemma/CLIPTextEncode for prompts describing desired motion
6. **Sampling** — KSampler with lower denoise for maintaining image fidelity
7. **VAE Decode** — Convert to pixel space
8. **Video Output** — VHS_VideoCombine

### Multi-Frame Image-to-Video

The `Load image → LTXVPreprocess → LTXVAddGuide` nodes can be chained together to set multiple guiding images (first frame, end frame, or other keyframes).

### Recommended img2vid Settings

| Parameter | Value |
|-----------|-------|
| Resolution | Match source image aspect ratio (768x512 or 512x768) |
| Frames | 97 (~4 seconds at 24fps) |
| CFG | 3.0-5.0 (lower to maintain reference consistency) |
| Steps | 15-20 |
| Denoise | 0.7-0.9 (lower preserves more of original image) |

### img2vid Tips

- Images are automatically resized to match the model's base resolutions (768x512 or 512x768)
- Use Florence2 autocaptioning node to generate accurate descriptions of the source image
- Replace terms like "photo" or "image" in auto-generated captions with "video" for better alignment
- Lower CFG values help maintain consistency with the reference image

---

## Video-to-Video (vid2vid) Workflows

### Basic vid2vid Workflow Structure

1. **Load Source Video** — VHS_LoadVideo to load the source video
2. **Load Model** — LTXVLoader for the LTX model
3. **Video Encoding** — VAE Encode the source video frames
4. **Text Encoding** — Prompts describing desired transformation
5. **Sampling** — KSampler with low denoise for style transfer
6. **VAE Decode** — Convert back to pixel space
7. **Video Output** — VHS_VideoCombine

### Recommended vid2vid Settings

| Parameter | Value |
|-----------|-------|
| CFG | 2.0-4.0 |
| Steps | 10-15 |
| Denoise | 0.3-0.7 (lower preserves more source structure) |

### vid2vid with IC-LoRA Control

For more precise control, use IC-LoRA (In-Context LoRA) to separate motion from visual styling:
- **Canny control** — Edge preservation from source video
- **Depth control** — Spatial geometry and camera preservation
- **Pose control** — Human motion transfer from source to new generation

---

## All-in-One Workflows

Several community workflows combine all three modes into a single workflow:

### jaguar_pesky_18's All-in-One (OpenArt)

- Supports vid2vid, txt2vid, and img2vid in one workflow
- Uses LTXV 0.9.1 model with spatio-temporal skip guidance
- Low VRAM consumption design
- Includes preset switches to toggle between generation modes

### Smooth Workflow LTX (Civitai)

- Combined I2V and T2V in single workflow (v1.0)
- Designed for smooth, consistent output
- Community-rated workflow with download support

### LTX IMAGE to VIDEO with STG (Civitai)

- Version 9.5 targeting LTX 0.9.7 Distilled model
- Integrates Florence2 captioning, CLIP extension, and STG guidance
- Full image-to-video pipeline with quality enhancements

---

## LTX-2.3 Official Workflow Examples

### Single-Stage Text/Image-to-Video

Simplest approach — generate directly at target resolution.
- Available for both full (22B dev) and distilled models
- Suitable for quick iteration and testing

### Two-Stage Text/Image-to-Video with Upsampling

1. **Stage 1:** Generate at half resolution for motion coherence
2. **Spatial Upscale:** LTXVLatentUpsampler performs 2x upscale in latent space
3. **Stage 2:** Refinement pass at full resolution
4. **Decode:** Final VAE decode to pixel space

Benefits: Better quality, sharper fine detail, maintained temporal consistency

### IC-LoRA Union Control

Single LoRA combining Canny + Depth + Pose control:
- Depth for camera and spatial geometry
- Canny for edge preservation
- Pose for human motion transfer

### IC-LoRA Motion Tracking

Guide object motion using sparse point trajectories from reference video, then generate new content following those motion paths.

---

## Technical Constraints

- **Resolution:** Must be a multiple of 32
- **Frame count:** Must follow the formula: multiples of 8 + 1 (e.g., 9, 17, 25, 33, 41, 49, 57, 65, 73, 81, 89, 97, 161, 257)
- **Maximum recommended resolution:** 720x1280
- **Maximum recommended frames:** 257
- **Output location:** `ComfyUI/output/`

## Long Video Generation

- Generate segments separately to avoid VRAM issues
- Maintain style consistency through consistent prompts across segments
- Use sequence conditioning to extend from last frame position
- Assemble segments in post-production
- Use temporal upscaler to double frame rates for smoother results
