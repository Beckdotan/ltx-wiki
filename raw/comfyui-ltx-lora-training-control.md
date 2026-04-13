# LTX Video LoRA Training and IC-LoRA Control in ComfyUI

## Sources
- [LTX-2.3 LoRA Training Guide: Style, Motion & IC-LoRA Control (2026) | WaveSpeedAI](https://wavespeed.ai/blog/posts/ltx-2-3-lora-training-guide-2026/)
- [LTX 2.3 IC-LoRA in ComfyUI | V2V Motion Track Video Workflow | RunComfy](https://www.runcomfy.com/comfyui-workflows/ltx-2-3-ic-lora-in-comfyui-v2v-motion-track-video-workflow)
- [LTX-2 LoRAs | LTX Documentation](https://docs.ltx.video/open-source-model/usage-guides/lo-ra)
- [LTX-Video LoRA training study (Single image/style training) | HuggingFace Blog](https://huggingface.co/blog/neph1/ltx-lora)
- [ComfyUI-LTXVideo Extension: LoRA Support, Workflows | WaveSpeedAI](https://wavespeed.ai/blog/posts/blog-comfyui-ltxvideo-extension/)
- [LTX-Video-Squish-LoRA | HuggingFace](https://huggingface.co/Lightricks/LTX-Video-Squish-LoRA)
- [Learn How to Use IC-LoRA in LTX-2 | LTX Blog](https://ltx.io/model/model-blog/how-to-use-ic-lora-in-ltx-2)
- [LTX-2 19B Video-to-Video LoRA | RunComfy](https://www.runcomfy.com/models/ltx/ltx-2-19b/video-to-video/lora)
- [GitHub - Lightricks/LTX-2 (Official trainer)](https://github.com/Lightricks/LTX-2)
- [IC-LoRA Workflows | DeepWiki](https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/6.4-ic-lora-workflows)
- [Camera Control | DeepWiki](https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/6.6-camera-control)
- [LTXV IC Pose | GenAI with ComfyUI](https://sbcode.net/genai/ltxv-pose/)
- [LTX-2.3 Transition LoRA | HuggingFace](https://huggingface.co/valiantcat/LTX-2.3-Transition-LORA)
- [LTX-2 ControlNet Depth-Controlled Video | RunComfy](https://www.runcomfy.com/comfyui-workflows/ltx-2-controlnet-in-comfyui-depth-controlled-video-workflow)
- [awesome-ltx2 | GitHub](https://github.com/wildminder/awesome-ltx2)

## Overview

LTX Video has a comprehensive LoRA ecosystem in ComfyUI, supporting custom style LoRAs, motion LoRAs, IC-LoRA structural control, camera control, and identity preservation. The official `ltx-trainer` enables training custom LoRAs, and the ComfyUI-LTXVideo extension provides nodes for applying them.

---

## LoRA Types

### Style LoRAs

**Purpose:** Teach LTX-2.3 visual aesthetics — color grading, texture treatment, lighting mood.

**Dataset Requirements:**
- 20-50 images for character or style LoRAs
- 80-120 images for highly specific subjects
- Images should be diverse in composition but consistent in style

**Training Settings:**
- Rank: 32 (recommended starting point)
- Learning rate: 1e-4
- Width and height must be divisible by 32

---

### Motion LoRAs

**Purpose:** Teach how things move rather than how they look — camera pans, object rotation, transformation sequences.

**Dataset Requirements:**
- Short coherent video clips (not stills)
- Frame count must be divisible by 8 + 1 (e.g., 17 frames)
- Consistent framing across clips
- Example: 15-second clips at consistent framing for dolly-in motion
- 15-30 clips recommended for reliable motion learning

**Training Notes:**
- If clips are wrong frame count, trainer will error or pad internally
- Resize to dimensions divisible by 32 before uploading
- Batch-process to 17 frames (2 x 8 + 1) for smooth training

---

### IC-LoRA (In-Context LoRA)

**Purpose:** Separates motion from visual styling. Extracts structure and motion directly from reference videos and applies them to new generations.

**Control Modes:**

#### Canny (Edge Control)
- Preserves edge structure from reference
- Best for maintaining architectural details, object boundaries
- Ideal for style transfer while keeping structure

#### Depth (Spatial Control)
- Preserves camera perspective and spatial geometry
- Best for camera movement preservation
- Maintains 3D structure of scenes

#### Pose (Human Motion)
- Transfers human motion from reference
- OpenPose-based skeleton extraction
- Best for character animation and motion transfer

**Union IC-LoRA:**
- Single LoRA combining Canny + Depth + Pose
- Advantages:
  - Unified control supporting multiple simultaneous conditions
  - "Downsampled Latent Processing" reduces memory usage
  - Accelerates inference while maintaining quality
  - One LoRA file instead of three

---

### Camera Control LoRAs

**Purpose:** Apply specific camera movements to generated video.

**Available Variants:**
- Dolly (forward/backward camera movement)
- Jib (vertical camera movement)
- Additional camera movement patterns

**Important Constraint:** Camera control LoRAs CANNOT be combined with IC-LoRA controls (Canny, Depth, Pose) in a single generation. Only one LoRA type per pass.

---

### ID-LoRA (Identity LoRA)

**Purpose:** Identity-preserving generation combining voice and visual appearance.

**Capabilities:**
- Transfers speaker's vocal identity from reference audio
- Preserves visual likeness from reference image
- First method to personalize both visual appearance and voice in single pass
- Operates in unified latent space

**Models Available:**
- CelebVHQ-3K
- TalkVid-3K

**ComfyUI Integration:** Now natively supported via `LTXVReferenceAudio` node (merged March 2026, PR #13111).

---

## Official LoRA Models

### From Lightricks

| LoRA | Purpose | File Location |
|------|---------|---------------|
| Union Control | Canny + Depth + Pose combined | `models/loras/` |
| Motion Track | Sparse point trajectory tracking | `models/loras/` |
| Detailer | Video enhancement and refinement | `models/loras/` |
| Pose Control | Human pose-guided generation | `models/loras/` |
| Cameraman v1 | Camera movement control | `models/loras/` |
| Dolly variants | Forward/backward camera | `models/loras/` |
| Jib variants | Vertical camera movement | `models/loras/` |
| Distilled LoRA | Speed optimization for distilled model | `models/loras/` |
| Squish LoRA | Aspect ratio manipulation | `models/loras/` |

### Community LoRAs

| LoRA | Creator | Purpose |
|------|---------|---------|
| IC-LoRA-Ungrade | Community | Enhanced upgrade effects |
| IC-LoRA-Outpaint | Community | Outpainting extension |
| Audio-Reactive LoRA | Community | Audio-reactive visual effects |
| VBVR-lora-I2V | Community | Complex reasoning tasks |
| Transition LoRA | valiantcat | Scene transitions |
| Abliterated Gemma variants | FusionCow | Unrestricted prompt encoding |

---

## LoRA Training with ltx-trainer

### Prerequisites

- Official trainer from [Lightricks/LTX-2](https://github.com/Lightricks/LTX-2)
- CUDA-compatible GPU (24GB+ recommended for training)
- Python 3.10+
- Training dataset (images for style, video clips for motion)

### Training Process

1. **Prepare Dataset**
   - Images: 20-120 depending on specificity
   - Videos: Batch process to correct frame count (divisible by 8 + 1)
   - Resize to dimensions divisible by 32
   - Caption all training data

2. **Configure Training**
   - Set rank (start with 32)
   - Set learning rate (start with 1e-4)
   - Select model base (LTX-2.3 22B dev or distilled)
   - Configure training steps and batch size

3. **Train**
   - Run the ltx-trainer
   - Monitor loss curves
   - Save checkpoints at intervals

4. **Test in ComfyUI**
   - Place trained LoRA in `ComfyUI/models/loras/`
   - Load via LoRA loader node
   - Test with various prompts and settings

### Training Tips

- Start with rank 32 and 1e-4 learning rate instead of tweaking every parameter on first run
- For motion LoRAs, ensure all clips have consistent framing
- For style LoRAs, include diverse compositions with consistent aesthetic
- Frame count = multiples of 8 + 1 (padding issues otherwise)
- Width/height = divisible by 32 (avoid silent resizing)

---

## Using IC-LoRA in ComfyUI Workflows

### Basic IC-LoRA Workflow

1. **Load Reference Video/Image** — Source material for control extraction
2. **Extract Control Signal** — Depth map, Canny edges, or pose skeleton
3. **Load IC-LoRA** — Union control or specific control LoRA
4. **LTX Add Video IC-LoRA Guide Advanced** — Apply with strength and optional mask
5. **Write Style Prompt** — Describe desired visual style (motion handled by IC-LoRA)
6. **Generate** — Model follows control structure while applying prompted style

### Multi-Control Workflow

The RunComfy LTX 2.3 IC-LoRA workflow:
- Handles depth, pose, and edge extraction automatically
- Load a reference clip
- Choose a control mode
- Write a style-focused prompt
- Model handles motion separately from styling

### Key Parameters

| Parameter | Range | Effect |
|-----------|-------|--------|
| `attention_strength` | 0.0-1.0 | How closely to follow the control signal |
| `attention_mask` | HxW or TxHxW | Spatial or spatiotemporal regions for control |

### IC-LoRA Tips

- Lower `attention_strength` (0.5-0.7) for softer, more creative results
- Higher `attention_strength` (0.8-1.0) for precise structure following
- Use spatial masks to apply control only to specific regions
- Combine with text prompts for style control while IC-LoRA handles structure
- Cannot combine camera LoRAs with IC-LoRA in same generation
