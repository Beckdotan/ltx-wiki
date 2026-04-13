---
title: LTX Video LoRA Training and IC-LoRA Control in ComfyUI
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://wavespeed.ai/blog/posts/ltx-2-3-lora-training-guide-2026/
  - https://www.runcomfy.com/comfyui-workflows/ltx-2-3-ic-lora-in-comfyui-v2v-motion-track-video-workflow
  - https://docs.ltx.video/open-source-model/usage-guides/lo-ra
  - https://huggingface.co/blog/neph1/ltx-lora
  - https://wavespeed.ai/blog/posts/blog-comfyui-ltxvideo-extension/
  - https://huggingface.co/Lightricks/LTX-Video-Squish-LoRA
  - https://ltx.io/model/model-blog/how-to-use-ic-lora-in-ltx-2
  - https://github.com/Lightricks/LTX-2
  - https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/6.4-ic-lora-workflows
  - https://deepwiki.com/Lightricks/ComfyUI-LTXVideo/6.6-camera-control
  - https://sbcode.net/genai/ltxv-pose/
  - https://github.com/wildminder/awesome-ltx2
tags:
  - comfyui
  - ltx-video
  - lora
  - ic-lora
  - training
  - control
---

# LTX Video LoRA Training and IC-LoRA Control in ComfyUI

[[ltx-video-overview|LTX Video]] has a comprehensive LoRA ecosystem in ComfyUI, supporting custom style LoRAs, motion LoRAs, IC-LoRA structural control, camera control, and identity preservation. The official `ltx-trainer` enables training custom LoRAs, and the [[comfyui-ltxvideo-official-nodes|ComfyUI-LTXVideo extension]] provides nodes for applying them.

## LoRA Types

### Style LoRAs

Teach LTX-2.3 visual aesthetics -- color grading, texture treatment, lighting mood.

**Dataset:** 20-50 images for character/style, 80-120 for highly specific subjects. Diverse in composition but consistent in style.

**Training:** Rank 32 (starting point), learning rate 1e-4. Width/height divisible by 32.

### Motion LoRAs

Teach how things move -- camera pans, object rotation, transformation sequences.

**Dataset:** 15-30 short coherent video clips (not stills). Frame count must be divisible by 8 + 1 (e.g., 17 frames). Consistent framing across clips.

### IC-LoRA (In-Context LoRA)

Separates motion from visual styling. Extracts structure and motion from reference videos and applies them to new generations.

**Control Modes:**

| Mode | Purpose | Best For |
|------|---------|----------|
| **Canny** | Edge preservation | Architectural details, object boundaries, style transfer |
| **Depth** | Camera perspective and spatial geometry | Camera movement preservation, 3D structure |
| **Pose** | Human motion transfer (OpenPose skeleton) | Character animation, motion transfer |

**Union IC-LoRA:** Single LoRA combining all three modes. Advantages: unified control, downsampled latent processing for lower memory, faster inference, one file instead of three.

### Camera Control LoRAs

Apply specific camera movements: dolly (forward/backward), jib (vertical), and other patterns.

**Important constraint:** Camera control LoRAs CANNOT be combined with IC-LoRA controls in a single generation pass. Only one LoRA type per pass.

### ID-LoRA (Identity LoRA)

Identity-preserving generation combining voice and visual appearance.
- Transfers vocal identity from reference audio
- Preserves visual likeness from reference image
- First method to personalize both in a single pass
- Models: CelebVHQ-3K, TalkVid-3K

Natively supported via `LTXVReferenceAudio` node (merged upstream March 2026, PR #13111). See [[comfyui-ltx-community-nodes|ID-LoRA-LTX2.3-ComfyUI]].

## Official LoRA Models

### From Lightricks

| LoRA | Purpose |
|------|---------|
| Union Control | Canny + Depth + Pose combined |
| Motion Track | Sparse point trajectory tracking |
| Detailer | Video enhancement and refinement |
| Pose Control | Human pose-guided generation |
| Cameraman v1 | Camera movement control |
| Dolly variants | Forward/backward camera |
| Jib variants | Vertical camera movement |
| Distilled LoRA | Speed optimization for distilled model |
| Squish LoRA | Aspect ratio manipulation |

All placed in `ComfyUI/models/loras/`.

### Community LoRAs

| LoRA | Creator | Purpose |
|------|---------|---------|
| IC-LoRA-Outpaint | Community | Outpainting extension |
| Audio-Reactive LoRA | Community | Audio-reactive visual effects |
| Transition LoRA | valiantcat | Scene transitions |
| Abliterated Gemma variants | FusionCow | Unrestricted prompt encoding |

See [[comfyui-ltx-community-nodes|awesome-ltx2]] for a curated collection.

## Training Custom LoRAs with ltx-trainer

### Prerequisites

- Official trainer from [Lightricks/LTX-2](https://github.com/Lightricks/LTX-2)
- CUDA GPU (24GB+ recommended for training)
- Python 3.10+

### Training Process

1. **Prepare Dataset**
   - Images: 20-120 depending on specificity
   - Videos: Batch process to correct frame count (divisible by 8 + 1)
   - Resize to dimensions divisible by 32
   - Caption all training data

2. **Configure Training**
   - Rank: start with 32
   - Learning rate: start with 1e-4
   - Base model: LTX-2.3 22B dev or distilled
   - Set training steps and batch size

3. **Train** -- Run ltx-trainer, monitor loss curves, save checkpoints

4. **Test in ComfyUI** -- Place in `models/loras/`, load via LoRA loader, test

### Training Tips

- Start with rank 32 and 1e-4 instead of tweaking every parameter first run
- Motion LoRAs: ensure consistent framing across all clips
- Style LoRAs: diverse compositions, consistent aesthetic
- Frame count = multiples of 8 + 1 (padding issues otherwise)
- Width/height = divisible by 32 (avoid silent resizing)

## Using IC-LoRA in ComfyUI

### Basic IC-LoRA Workflow

1. **Load reference video/image** -- Source for control extraction
2. **Extract control signal** -- Depth map, Canny edges, or pose skeleton
3. **Load IC-LoRA** -- Union control or specific control LoRA
4. **[[comfyui-ltx-node-reference|LTX Add Video IC-LoRA Guide Advanced]]** -- Apply with strength and optional mask
5. **Write style prompt** -- Describe visual style (motion handled by IC-LoRA)
6. **Generate** -- Model follows control structure while applying prompted style

### Key Parameters

| Parameter | Range | Effect |
|-----------|-------|--------|
| `attention_strength` | 0.0-1.0 | How closely to follow control signal |
| `attention_mask` | HxW or TxHxW | Spatial or spatiotemporal control regions |

### IC-LoRA Tips

- Lower strength (0.5-0.7) for softer, more creative results
- Higher strength (0.8-1.0) for precise structure following
- Use spatial masks for region-specific control
- Combine with text prompts for style while IC-LoRA handles structure
- Cannot combine camera LoRAs with IC-LoRA in same pass

## Related Pages

- [[comfyui-ltx-integration-overview]] -- Integration overview
- [[comfyui-ltx-node-reference]] -- Node reference (IC-LoRA nodes, MultimodalGuider)
- [[comfyui-ltx-workflows]] -- Workflow types using LoRA control
- [[comfyui-ltx-community-nodes]] -- Community nodes including ID-LoRA
- [[comfyui-ltxvideo-official-nodes]] -- Official extension
