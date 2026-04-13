# LTX Video Version History and ComfyUI Support Timeline

## Sources
- [GitHub - Lightricks/ComfyUI-LTXVideo](https://github.com/Lightricks/ComfyUI-LTXVideo/)
- [NEW VIDEO MODEL: LTXV day-1 Native Support in ComfyUI](https://blog.comfy.org/p/ltxv-day-1-comfyui)
- [LTX-Video 0.9.5 Day-1 Support in ComfyUI](https://blog.comfy.org/p/ltx-video-095-day-1-support-in-comfyui)
- [LTX-Video | Stable Diffusion Art](https://stable-diffusion-art.com/ltx-video/)
- [How to run LTX Video 13B on ComfyUI | Stable Diffusion Art](https://stable-diffusion-art.com/ltxv-13b/)
- [LTX-2.3: Introducing LTX's Latest AI Video Model | LTX](https://ltx.io/model/ltx-2-3)
- [LTX-0.9.5 ComfyUI Workflow Examples | ComfyUI Docs](https://docs.comfy.org/tutorials/video/ltxv)
- [LTX-2.3 ComfyUI Workflow Examples | ComfyUI Docs](https://docs.comfy.org/tutorials/video/ltx/ltx-2-3)
- [LTXV Video Generation Supported in ComfyUI | Civitai](https://civitai.com/articles/9115/ltxv-video-generation-supported-in-comfyui-another-day-one-support-release-2025-update)
- [LTX-Video: A Groundbreaking Real-Time Video Generation Model | MarkTechPost](https://www.marktechpost.com/2024/11/23/ltx-video-a-groundbreaking-real-time-video-generation-open-source-model-with-day-one-native-support-in-comfyui-empowering-innovators-to-transform-content-creation/)
- [GitHub - Lightricks/LTX-Video (Original repo)](https://github.com/Lightricks/LTX-Video)
- [GitHub - Lightricks/LTX-2 (Current repo)](https://github.com/Lightricks/LTX-2)

## Version Timeline

### LTX Video 2B v0.9 (November 2024)

**Parameters:** 2 billion
**Architecture:** DiT (Diffusion Transformer)
**Resolution:** 768x512 @ 24fps
**Speed:** 5 seconds of video in ~4 seconds on RTX 4090
**VRAM:** ~6GB minimum

**Key Features:**
- First release of LTX Video
- Day-1 native ComfyUI support (announced on blog.comfy.org)
- Text-to-video generation
- Image-to-video generation
- Pioneering DiT-based video generation model
- Real-time generation capability

**ComfyUI Integration:**
- Official custom nodes branded as "LTXVideo"
- Available through ComfyUI Manager from day one
- Custom nodes by Lightricks team

**Model File:** `ltx-video-2b-v0.9.safetensors`
**Text Encoder:** PixArt / T5-XXL

---

### LTX Video 2B v0.9.1 (Late 2024)

**Parameters:** 2 billion
**VRAM:** Improved low-VRAM support

**Key Improvements:**
- Spatio-temporal skip guidance for more consistent video
- Low VRAM consumption improvements
- Better video quality and consistency
- All-in-one workflows for vid2vid, txt2vid, and img2vid

**Model File:** `ltx-video-2b-v0.9.1.safetensors`

---

### LTX Video 0.9.5 (Early 2025)

**Key Improvements:**
- Multiple keyframe control
- Improved quality
- Longer video support
- Day-1 ComfyUI support (announced on blog.comfy.org)

**ComfyUI Integration:**
- Updated example workflows
- Official documentation on docs.comfy.org
- Enhanced node functionality

---

### LTX Video 13B v0.9.7 (Mid 2025)

**Parameters:** 13 billion (6x increase over 2B)
**Resolution:** 768x512 (landscape) or 512x768 (portrait) — fixed base resolutions
**Default Frames:** 97 (~4 seconds at 24fps)

**Key Improvements:**
- Dramatically better details and prompt adherence
- More coherent video output
- Start and end frame animation capability
- Multi-frame image-to-video (first frame, end frame, other keyframes)
- `Load image -> LTXVPreprocess -> LTXVAddGuide` chain for multiple guiding images

**ComfyUI Integration:**
- Florence2 autocaptioning integration
- Text node for replacing "photo/image" with "video" in captions
- Updated workflow examples

**Model File:** LTXV-13B checkpoint

---

### LTX-2 (Late 2025)

**Parameters:** 19 billion
**Architecture:** Major architecture update

**Key Features:**
- Native ComfyUI core integration (built into ComfyUI, not just custom nodes)
- IC-LoRA control system (depth, pose, edge)
- Gemma text encoder support
- Video-to-video detailer
- Improved generation quality

**ComfyUI Integration:**
- Built-in nodes in ComfyUI core
- ComfyUI-LTXVideo extension for advanced features
- Day-0 native support

**Repository:** Moved from Lightricks/LTX-Video to Lightricks/LTX-2

---

### LTX-2.3 (Early 2026)

**Parameters:** 22 billion
**Architecture:** DiT-based audio-video foundation model

**Major New Features:**
- **Audio generation** — First open-source model with synchronized audio-video output
- **Gemma 3 12B text encoder** — Upgraded from previous encoders
- **Spatial upscalers** — 2x and 1.5x latent-space upscaling
- **Temporal upscaler** — 2x frame rate doubling
- **MultimodalGuider** — Independent audio/video guidance control
- **LTXVNormalizingSampler** — Statistical normalization for quality
- **GemmaAPITextEncode** — Free API-based text encoding
- **Conditioning save/load** — Reuse encodings across sessions
- **ID-LoRA support** — Identity-preserving audio-video generation
- **Union IC-LoRA** — Single LoRA combining Canny + Depth + Pose
- **Camera control LoRAs** — Dolly, jib movements
- **Motion tracking** — Sparse point trajectory tracking

**Quality Improvements:**
- Sharper fine detailing
- Better prompt adherence
- Cleaner audio quality
- Stable image-to-video generation
- Portrait video support (vertical format for social media)

**ComfyUI Integration:**
- Full workflow examples on docs.comfy.org
- One-stage and two-stage pipeline support
- Desktop auto-installer utility available
- Upstream integration of ID-LoRA (PR #13111, March 2026)

**Model Files:**
- `ltx-2.3-22b-dev.safetensors` (full model)
- `ltx-2.3-22b-distilled.safetensors` (fast model, 8-step generation)
- Spatial and temporal upscaler models
- Distilled LoRA
- Multiple IC-LoRA and control LoRAs

---

## ComfyUI-LTXTricks Timeline

**Created:** Community node pack by logtd providing RF-Inversion, RF-Edit, FlowEdit, I+V2V, STG
**Peak:** 512 stars on GitHub
**Deprecated:** Functionality migrated to official ComfyUI-LTXVideo repository
**Replacement Nodes:** `LTXVKeyFrameConditioning` and `LTXVSequenceConditioning` in core ComfyUI

---

## Ecosystem Growth

| Date | Event |
|------|-------|
| Nov 2024 | LTX Video 0.9 launches with day-1 ComfyUI support |
| Late 2024 | ComfyUI-LTXTricks community nodes created |
| Early 2025 | LTX Video 0.9.5 with multi-keyframe control |
| Mid 2025 | LTX Video 13B (0.9.7) — 6x parameter increase |
| Late 2025 | LTX-2 with native ComfyUI core integration |
| Early 2026 | LTX-2.3 with audio-video generation |
| March 2026 | ID-LoRA merged into upstream ComfyUI |
| March 2026 | ComfyUI-LTXTricks deprecated, migrated to official repo |
