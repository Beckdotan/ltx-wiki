---
title: IC-LoRA (In-Context LoRA)
type: concept
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://huggingface.co/Lightricks/LTX-Video
  - https://huggingface.co/Lightricks/LTX-Video-0.9.7-dev
  - https://arxiv.org/abs/2603.24793
  - https://arxiv.org/abs/2601.03233
  - raw/community-project-ic-lora-controls.md
  - raw/community-project-lora-ecosystem.md
  - raw/community-project-camera-control-loras.md
  - raw/training-fine-tuning.md
  - raw/tutorial-lora-training-quick-start.md
  - raw/ltx-video-trainer.md
  - raw/social-lora-training-community-ecosystem.md
tags:
  - ltx-video
  - ic-lora
  - lora
  - controlled-generation
  - conditioning
---

# IC-LoRA (In-Context LoRA)

IC-LoRA (In-Context LoRA) is a fine-tuning technique developed by Lightricks that enables conditioning video generation on structural reference signals at inference time. It allows fine-grained video-to-video control on top of the LTX text-to-video base models without requiring full model retraining.

Introduced with [[ltx-video-097|LTX Video v0.9.7]], IC-LoRA adapters provide control over the spatial layout and motion of generated videos through structural inputs like depth maps, pose keypoints, and edge maps.

## Core Mechanism

### Parallel Canvas Conditioning

Unlike spatial concatenation approaches (which fail for structural controls in video), IC-LoRA uses a parallel canvas method:

- Reference signal provided as **additional tokens in self-attention layers**
- The model's per-token timestep mechanism distinguishes reference (clean) from generation (noisy) tokens
- Reference influence can be continuously modulated at inference time

### Reference Downscale Factor

- Reference video can be smaller than output resolution to save tokens
- Example: ref0.5 means reference at 0.5x output resolution (downscale factor 2)
- Reduces computational cost for control modalities that do not need full resolution

### LoRA Training

- Each control modality uses a separate lightweight LoRA adapter
- Trained on frozen LTX backbone (no architectural changes required)
- Default rank: 128
- Convergence: 1,000-3,000 steps for spatial controls
- Total training budget across all 13 modalities: ~55K steps

## Available Adapters

### For LTX Video v0.9.7/0.9.8 (13B base)

| Adapter | Conditioning Type | Input Signal | Use Case |
|---------|------------------|--------------|----------|
| ICLoRA-Depth | Depth maps | Monocular depth estimation (MiDaS, DPT) | Scene layout, 3D structure |
| ICLoRA-Pose | Pose estimation | OpenPose / MediaPipe keypoints | Character animation, motion control |
| ICLoRA-Canny | Edge detection | Canny edge maps | Structural guidance, outlines |
| ICLoRA-Detailer | Quality enhancement | N/A (refinement adapter) | Fine detail and texture improvement |

### For LTX-2 (19B base)

| Model | Control Type |
|-------|-------------|
| LTX-2-19b-IC-LoRA-Union-Control | Depth + Pose + Canny (unified) |
| LTX-2-19b-IC-LoRA-Detailer | Video refinement |
| LTX-2-19b-IC-LoRA-Canny-Control | Edge-guided generation |

### For LTX-2.3 (22B base)

| Model | Control Type |
|-------|-------------|
| LTX-2.3-22b-IC-LoRA-Union-Control | Depth + Pose + Canny (unified) |
| LTX-2.3-22b-IC-LoRA-Motion-Track-Control | Sparse point tracking control |

## Full Modality List (from AVControl Paper)

13 independently trained modalities have been demonstrated:

**Spatial Controls:** Depth, Pose, Canny edge

**Camera Control:** Camera trajectory from image, Camera trajectory from video

**Motion Control:** Sparse point tracking

**Video Editing:** Cut-on-action, Inpainting, Outpainting

**Audio-Visual Controls:** Who-is-talking (speaker-aware), Audio intensity to waveform, and additional audio-visual modalities

## Training Custom IC-LoRAs

IC-LoRA training is supported in both the [[ltx-video-trainer|LTX-Video-Trainer]] (added July/August 2025) and the LTX-2 Trainer.

### Dataset Requirements

Each training sample requires paired data:

| Control Type | Required Pair |
|-------------|---------------|
| Depth Map | Video + depth map |
| Human Pose | Video + pose skeleton |
| Canny Edge | Video + edge map |
| Motion Track | Video + motion track |

### Training Guidelines

- Training can be completed in **less than an hour** for video-to-video transformation workflows
- Default rank: 128 (higher than standard LoRAs)
- Convergence: 1,000-3,000 steps for spatial controls
- Keep prompts focused on **appearance** because IC-LoRA handles motion and structure
- Set IC-LoRA control models to **full strength (1.0)** -- they are designed for it
- Multiple control signals can be used simultaneously
- Avoid mixing multiple IC-LoRA control types in the same generation

### Cloud Training

IC-LoRA training is available through [[third-party-training-services|third-party services]], notably WaveSpeedAI's dedicated LTX-2 IC-LoRA Trainer.

See [[lora-training]] for the full training guide and [[training-dataset-preparation]] for general dataset preparation.

## How It Works (Workflow)

1. **Extract conditioning signal** from input image/video (e.g., run depth estimation)
2. **Load the IC-LoRA adapter** on top of the base LTX Video model
3. **Pass conditioning signal** alongside the text prompt
4. **Generate video** that follows both the structural conditioning and text description

The adapter modifies attention layers in the DiT transformer to attend to the structural conditioning signal while maintaining the generative quality of the base model.

## Usage with Diffusers

```python
import torch
from diffusers import LTXConditionPipeline
from diffusers.pipelines.ltx.pipeline_ltx_condition import LTXVideoCondition

pipe = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.8-dev",
    torch_dtype=torch.bfloat16
).to("cuda")

pipe.load_lora_weights(
    "Lightricks/LTX-Video",
    subfolder="iclora-depth",
    adapter_name="depth_adapter"
)

depth_condition = LTXVideoCondition(video=depth_map_frames, frame_index=0)

result = pipe(
    conditions=[depth_condition],
    prompt="A person walking through a garden",
    negative_prompt="worst quality, blurry",
    width=768, height=512,
    num_frames=97,
    num_inference_steps=30,
).frames[0]
```

## Usage with ComfyUI

1. Copy LoRA weights to `models/loras/`
2. Load base model with `LTXVLoader`
3. Load IC-LoRA adapter with `LTXVICLoRA` / `LTXICLoRALoaderModelOnly` node
4. Add reference latent as guide with `LTXAddVideoICLoRAGuide`
5. Provide conditioning signal (depth/pose/canny map)
6. Connect to `LTXVSampler` and generate

## Generating Conditioning Signals

**Depth maps:** Use `Intel/dpt-large` or MiDaS via the Transformers `depth-estimation` pipeline.

**Pose estimation:** Use OpenPose or MediaPipe to extract keypoints from reference images/videos.

**Canny edge detection:** Use OpenCV `cv2.Canny()` with thresholds (e.g., 100, 200).

## Comparison to Other Approaches

| Approach | Mechanism | Focus |
|----------|-----------|-------|
| IC-LoRA | Lightweight LoRA adapters on attention layers | Structural video control |
| ControlNet | Separate parallel network | Image structural control |
| IP-Adapter | Image embedding injection | Style/content transfer |
| T2I-Adapter | Lightweight adapter | Image structural control |

IC-LoRA is specifically designed for video and uses a more lightweight mechanism than ControlNet while supporting a wider range of modalities.

## Applications

- **Character re-animation** -- apply new motions using pose conditioning
- **Scene reconstruction** -- generate new views using depth conditioning
- **Style transfer with structure** -- maintain structural layout while changing visual style
- **Storyboard-to-video** -- convert sketches/outlines to full video using edge conditioning
- **Quality enhancement** -- use the detailer adapter for finer output quality

## Key Papers

- **LTX-2:** arXiv:2601.03233 (foundation model)
- **AVControl:** arXiv:2603.24793 (control framework methodology)

## Performance

- VACE Benchmark: outperforms all baselines on depth, pose, inpainting, outpainting
- VBench depth scores: 81.6 with rank-128 LoRA at 3K steps

## Community Adoption

The IC-LoRA Detailer for LTX-2 is the most widely adopted IC-LoRA model, used in 29+ HuggingFace Spaces. The Union Control model for LTX-2.3 is used in 21+ Spaces.

Community member **prithivMLmods** created a dedicated Space for [[camera-control-loras|camera control]] using IC-LoRA dolly effects (58 likes). The [[jetson-thor-deployment|Jetson Thor deployment]] by Divhanthegray also integrates camera control LoRAs for dolly, jib, and static movements.

See [[lora-ecosystem]] for the full inventory of official and community LoRA adapters, and [[camera-control-loras]] for camera-specific controls.

## See Also

- [[ltx-video-097]] -- release that introduced IC-LoRA adapters
- [[ltx-video-098]] -- updated IC-LoRA adapters
- [[ltxv-13b]] -- the base model IC-LoRA operates on
- [[ltxv-model-variants]] -- compatible model variants
- [[lora-ecosystem]] -- Full LoRA inventory and training tools
- [[camera-control-loras]] -- Camera movement and motion tracking controls
- [[lora-training]] -- General LoRA training guide
- [[training-dataset-preparation]] -- Dataset preparation for training
- [[ltx-video-trainer]] -- Official training tools
- [[third-party-training-services]] -- Cloud-based training options
