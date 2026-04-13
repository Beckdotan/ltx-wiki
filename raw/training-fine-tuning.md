# LTX Video Training and Fine-Tuning

> **Sources:**
> - https://docs.ltx.video/open-source-model/ltx-2-trainer/ltx-2-training
> - https://github.com/Lightricks/LTX-Video-Trainer
> - https://github.com/Lightricks/LTX-2
> - https://apatero.com/blog/ltx-2-lora-training-fine-tuning-complete-guide-2025
> - https://wavespeed.ai/blog/posts/ltx-2-3-lora-training-guide-2026/
> - https://docs.ltx.video/open-source-model/usage-guides/lo-ra
> - https://fal.ai/learn/devs/ltx-2-video-trainer-user-guide
> - https://ghost.oxen.ai/how-to-train-a-ltx-2-character-lora-with-oxen-ai/
> - https://ltx-23.app/blog/ltx-23-lora-training
> - https://runpod.ghost.io/complete-guide-to-training-video-loras/

---

## Overview

LTX-Video supports several fine-tuning approaches:

| Approach | Tool | Description |
|----------|------|-------------|
| LoRA Training | LTX-Video-Trainer / LTX-2 Trainer | Low-rank adaptation, lightweight |
| IC-LoRA Training | LTX-2 Trainer | Identity-consistent control LoRAs |
| Full Fine-Tuning | LTX-2 Trainer | Complete model retraining |
| Diffusers LoRA | HuggingFace Diffusers | Using diffusers training scripts |

---

## Hardware Requirements

### Minimum Requirements for LoRA Training

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| GPU | RTX 4090 (24GB) | H100 (80GB) |
| VRAM | 24 GB | 80+ GB |
| System RAM | 32 GB | 64+ GB |
| Storage | 50 GB free | 200+ GB (for datasets) |
| CUDA | 12.2+ (LTX-Video) / 13+ (LTX-2) | Latest |
| Python | 3.10+ (LTX-Video) / 3.12+ (LTX-2) | 3.12 |
| OS | Linux | Linux (officially supported) |

**Typical Training Times:**
- Style LoRA on RTX 4090: 3-5 hours (mid-sized dataset)
- Character LoRA on H100: 2-4 hours
- Full dataset with 10-30 clips: 2-8 hours on 24GB GPU

---

## LTX-Video-Trainer (Community Trainer)

### Installation

```bash
git clone https://github.com/Lightricks/LTX-Video-Trainer.git
cd LTX-Video-Trainer
pip install -e .
```

### Features

- LoRA (Low-Rank Adaptation) training
- Full fine-tuning
- IC-LoRA (In-Context LoRA) training
- Support for LTXV 13B models
- Pretrained control adapters:
  - Depth map control
  - Human pose skeleton control
  - Canny edge detection control

### Example LoRA Models

- **Cakeify LoRA** -- Cake-texture transformation effects
- **Squish LoRA** -- Playful deformation effects

---

## LTX-2 Official Trainer

### Installation

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync --frozen
source .venv/bin/activate

# Or install the trainer package directly
pip install -e packages/ltx-trainer
```

### Supported Training Modes

1. **Subject LoRAs** -- People, characters, specific objects
2. **Style LoRAs** -- Visual aesthetics, color grading, texture treatment, lighting mood
3. **Concept LoRAs** -- Specific actions, scenarios, motion patterns
4. **Motion LoRAs** -- Movement patterns (requires video clips, not stills)
5. **IC-LoRA** -- Identity-consistent control (depth, pose, edge, motion tracking)

---

## Dataset Preparation

### Critical Rules

1. **Frame Count:** Must follow the **8n+1 rule** (1, 9, 17, 25, 33, ..., 73 frames)
   - If clips are 10 or 15 frames, the trainer will either error or pad internally
   - Example: 17 frames = 2 x 8 + 1

2. **Resolution:** Width and height must be **divisible by 32**
   - Common resolutions: 512x512, 768x512, 512x768, 720x480

3. **Captions:** Every clip needs a detailed caption with your **trigger word**
   - Poor captions result in poor results
   - Include scene description, actions, visual style, camera movement

### Dataset Size Guidelines

| LoRA Type | Minimum | Recommended | Notes |
|-----------|---------|-------------|-------|
| Style | 10-20 images | 20-50 images | Still frames work well |
| Character | 20-30 images | 40-80 images | Various angles/expressions |
| Motion | 10-15 clips | 20-30 clips | Short coherent video clips |
| Complex | 30-50 items | 80-120 items | For highly specific subjects |

### Dataset Directory Structure

```
dataset/
  video_001.mp4
  video_001.txt       # Caption file
  video_002.mp4
  video_002.txt
  image_001.png
  image_001.txt
  ...
```

### Caption Format Example

```
MYTRIGGER A woman with flowing red hair walks through a sunlit meadow,
her dress billowing in the gentle breeze. The camera slowly pans from
left to right, following her movement. Warm golden light filters through
the trees, creating dappled shadows on the ground. The scene has a
dreamy, ethereal quality with soft focus in the background.
```

---

## Training Configuration

### Default Configuration Parameters

```yaml
# training_config.yaml
model:
  checkpoint_path: /path/to/ltx-2.3-22b-dev.safetensors
  gemma_root: /path/to/gemma

training:
  lora_rank: 32
  lora_alpha: 32
  learning_rate: 1.0e-4
  batch_size: 1
  max_steps: 2000
  gradient_accumulation_steps: 1
  gradient_checkpointing: true

data:
  dataset_path: /path/to/dataset
  resolution: 720
  fps: 24
  max_frames: 72    # Must follow 8n+1 rule (actually 73 = 9*8+1)
  caption_dropout: 0.1

output:
  output_dir: /path/to/output
  save_every_n_steps: 500
  log_every_n_steps: 10
```

### Key Hyperparameters

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| `lora_rank` | 32 | 4-128 | 32 is the recommended default |
| `lora_alpha` | 32 | Usually = rank | Scaling factor for LoRA |
| `learning_rate` | 1e-4 | 1e-5 to 5e-4 | 1e-4 is the "boring but correct" answer |
| `batch_size` | 1 | 1-4 | Limited by VRAM |
| `max_steps` | 2000 | 500-5000 | Sample at 500-750 to check for overfitting |
| `gradient_checkpointing` | true | -- | Reduces VRAM usage significantly |
| `caption_dropout` | 0.1 | 0.0-0.3 | Cannot use with cached text embeddings |
| `resolution` | 720 | 480-1080 | Limited by VRAM |
| `max_frames` | 72 | 9-121 | Must follow 8n+1 rule |

---

## Running Training

### LoRA Training (LTX-Video-Trainer)

```bash
# Basic LoRA training
python train.py --config configs/lora_training.yaml

# With custom parameters
python train.py \
    --config configs/lora_training.yaml \
    --learning_rate 1e-4 \
    --max_steps 2000 \
    --lora_rank 32
```

### LoRA Training (LTX-2 Trainer)

```bash
# Using the CLI
python -m ltx_trainer.train \
    --config training_config.yaml

# Or with uv
uv run python -m ltx_trainer.train --config training_config.yaml
```

---

## IC-LoRA Training

IC-LoRA enables conditioning video generation on reference signals (depth maps, poses, edges) at inference time.

### Supported Control Types

| Control Type | Description | Dataset Requirement |
|-------------|-------------|---------------------|
| Depth Map | Conditions on depth information | Video + depth map pairs |
| Human Pose | Conditions on pose skeletons | Video + pose skeleton pairs |
| Canny Edge | Conditions on edge detection | Video + edge map pairs |
| Motion Track | Conditions on motion tracking | Video + motion track pairs |

### IC-LoRA Notes

- Keep prompts focused on appearance because IC-LoRA handles motion and structure
- Set IC-LoRA control models to full strength (designed for 1.0)
- Multiple control signals can be used simultaneously
- Avoid mixing multiple IC-LoRA control types

---

## Validation and Testing

### Testing Approach

Validate LoRAs across three scenarios:

1. **Identical prompt** with and without LoRA -- Verify LoRA has an effect
2. **Varied prompts** -- Test generalization beyond training data
3. **Edge cases** -- Prompts/styles absent from training data

### Sample at Checkpoints

```bash
# Generate test videos at different training steps
python inference.py \
    --checkpoint /path/to/checkpoint_step_500.safetensors \
    --prompt "TRIGGER a beautiful landscape" \
    --output_path test_500.mp4

python inference.py \
    --checkpoint /path/to/checkpoint_step_1000.safetensors \
    --prompt "TRIGGER a beautiful landscape" \
    --output_path test_1000.mp4
```

---

## Using Trained LoRAs

### In Diffusers

```python
import torch
from diffusers import LTXConditionPipeline
from diffusers.utils import export_to_video

pipeline = LTXConditionPipeline.from_pretrained(
    "Lightricks/LTX-Video-0.9.5", torch_dtype=torch.bfloat16
)

# Load your custom LoRA
pipeline.load_lora_weights("/path/to/my_custom_lora", adapter_name="my_lora")
pipeline.set_adapters("my_lora")

video = pipeline(
    prompt="MYTRIGGER a person walking in a park",
    width=704,
    height=480,
    num_frames=161,
    num_inference_steps=50,
).frames[0]
export_to_video(video, "output.mp4", fps=24)
```

### In LTX-2 Pipelines

```python
from ltx_core.loader import LTXV_LORA_COMFY_RENAMING_MAP, LoraPathStrengthAndSDOps
from ltx_pipelines.ti2vid_two_stages import TI2VidTwoStagesPipeline

custom_loras = [
    LoraPathStrengthAndSDOps(
        "/path/to/my_custom_lora.safetensors",
        1.0,  # Strength
        LTXV_LORA_COMFY_RENAMING_MAP
    ),
]

pipeline = TI2VidTwoStagesPipeline(
    checkpoint_path="/path/to/checkpoint.safetensors",
    distilled_lora=distilled_lora,
    spatial_upsampler_path="/path/to/upsampler.safetensors",
    gemma_root="/path/to/gemma",
    loras=custom_loras,
)
```

---

## LoRA Strength Tuning

| Strength | Effect | When to Use |
|----------|--------|-------------|
| 0.0 | No effect | Disabled |
| 0.9 - 1.1 | Subtle | Preserving base model traits |
| 1.2 - 1.4 | Balanced | Most applications |
| 1.5 - 1.6 | Strong | Maximum style transfer |

**Rules:**
- Keep combined total strength under 2.0 for multiple LoRAs
- IC-LoRA control models should be at 1.0 (designed for full strength)
- Distilled models remain compatible with LoRAs trained on dev versions
- FP8 quantized models support LoRAs with reduced VRAM demands
- LoRAs add minimal computational overhead (typically under 5%)

---

## Compatibility Notes

### LTX-2 to LTX-2.3 Migration

**LoRAs trained on LTX-2 do NOT transfer to LTX-2.3** due to:
- VAE redesign
- Text connector architecture changes

You must retrain LoRAs when upgrading from LTX-2 to LTX-2.3.

### Scheduler Changes (2.0 to 2.3)

- Recommended step counts shifted from ~28-32 steps to ~22-26 steps
- Different sampler defaults between versions
- Sweet spots for step counts differ between model versions

---

## Cloud Training Options

### fal.ai

The fal.ai platform provides a hosted LTX-2 Video Trainer:

```
https://fal.ai/learn/devs/ltx-2-video-trainer-user-guide
```

### RunPod

Complete guide available for training video LoRAs on RunPod infrastructure:

```
https://runpod.ghost.io/complete-guide-to-training-video-loras/
```

### Oxen.ai

Character LoRA training with Oxen.ai:

```
https://ghost.oxen.ai/how-to-train-a-ltx-2-character-lora-with-oxen-ai/
```

---

## Troubleshooting Training

### Common Issues

1. **OOM during training**: Enable gradient checkpointing, reduce batch size, lower resolution, reduce max_frames
2. **Training loss not decreasing**: Check learning rate (try 5e-5), verify dataset captions, ensure trigger word is consistent
3. **Overfitting**: Reduce max_steps, increase caption dropout, add more diverse training data
4. **Poor motion quality**: Use video clips instead of still images for motion LoRAs, ensure clips are coherent
5. **Caption dropout + cached text embeddings conflict**: If using caption dropout, keep Cache Text Embeddings OFF
6. **Frame count errors**: Ensure all video clips follow the 8n+1 frame rule (1, 9, 17, 25, 33, ...)
7. **Resolution errors**: Width and height must be divisible by 32

### Monitoring Training

```bash
# Monitor with TensorBoard (if configured)
tensorboard --logdir /path/to/output/logs

# Check training progress
ls -la /path/to/output/checkpoints/
```
