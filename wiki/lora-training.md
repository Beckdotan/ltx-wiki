---
title: LoRA Training for LTX Video
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/training-fine-tuning.md
  - raw/python-lora-training-guide.md
  - raw/tutorial-lora-training-quick-start.md
  - raw/ltx-video-trainer.md
tags:
  - lora
  - training
  - fine-tuning
  - guide
---
# LoRA Training for LTX Video

LoRA (Low-Rank Adaptation) is the primary method for customizing [[ltx-video-overview|LTX-Video]], [[ltx-2]], and [[ltx-2-3]] models. It produces lightweight adapter weights (typically a few hundred MBs) that can be applied at inference time to achieve custom styles, characters, motions, or effects.

## Fine-Tuning Approaches

| Approach | Tool | Description |
|----------|------|-------------|
| LoRA Training | [[ltx-video-trainer]] | Low-rank adaptation, lightweight |
| [[ic-lora]] Training | LTX-2 Trainer | Identity-consistent control LoRAs |
| Full Fine-Tuning | LTX-2 Trainer | Complete model retraining |
| Diffusers LoRA | HuggingFace Diffusers | Using diffusers training scripts |

## Types of LoRAs

1. **Subject LoRAs** -- People, characters, specific objects
2. **Style LoRAs** -- Visual aesthetics, color grading, texture treatment, lighting mood
3. **Concept LoRAs** -- Specific actions, scenarios, motion patterns
4. **Motion LoRAs** -- Movement patterns (requires video clips, not stills)
5. **Effect LoRAs** -- Custom visual effects (e.g., Squish, Cakeify)
6. **[[ic-lora|IC-LoRA]]** -- Identity-consistent control (depth, pose, edge, motion tracking)

## Hardware Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| GPU | RTX 4090 (24GB) | H100 (80GB) |
| VRAM | 24 GB | 80+ GB |
| System RAM | 32 GB | 64+ GB |
| Storage | 50 GB free | 200+ GB (for datasets) |
| CUDA | 12.2+ (LTX-Video) / 13+ (LTX-2) | Latest |
| Python | 3.10+ (LTX-Video) / 3.12+ (LTX-2) | 3.12 |
| OS | Linux | Linux (officially supported) |

Training on HuggingFace Diffusers has a lower floor: minimum 16GB VRAM for the 2B model with optimizations (gradient checkpointing, reduced resolution).

**Typical training times:**
- Style LoRA on RTX 4090: 3-5 hours (mid-sized dataset)
- Character LoRA on H100: 2-4 hours
- Motion/style/likeness LoRA with LTX-2 Trainer: under 1 hour

## Quick Start

### Using the LTX-2 Trainer (Official)

```bash
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync --frozen
source .venv/bin/activate

python -m ltx_trainer.train --config training_config.yaml
```

### Using the LTX-Video-Trainer (Community)

```bash
git clone https://github.com/Lightricks/LTX-Video-Trainer.git
cd LTX-Video-Trainer
pip install -e .

python train.py --config configs/lora_training.yaml
```

### Using HuggingFace Diffusers

```bash
pip install -U git+https://github.com/huggingface/diffusers
pip install -U transformers accelerate peft torch torchvision

accelerate launch train_ltx_video_lora.py \
  --pretrained_model_name_or_path="Lightricks/LTX-Video" \
  --train_data_dir="path/to/training_data" \
  --output_dir="output/ltx-video-lora" \
  --resolution=512 \
  --train_batch_size=1 \
  --gradient_accumulation_steps=4 \
  --max_train_steps=1000 \
  --learning_rate=1e-4 \
  --rank=32 \
  --mixed_precision="bf16"
```

See [[training-hyperparameters]] for the full configuration reference and [[training-dataset-preparation]] for dataset requirements.

## Running Training

### LTX-2 Trainer

```bash
# Using the CLI
python -m ltx_trainer.train --config training_config.yaml

# Or with uv
uv run python -m ltx_trainer.train --config training_config.yaml
```

### LTX-Video-Trainer

```bash
python train.py --config configs/lora_training.yaml

# With custom overrides
python train.py \
    --config configs/lora_training.yaml \
    --learning_rate 1e-4 \
    --max_steps 2000 \
    --lora_rank 32
```

### HuggingFace Diffusers (Memory-Optimized)

For limited VRAM (16-24GB):

```bash
accelerate launch train_ltx_video_lora.py \
  --pretrained_model_name_or_path="Lightricks/LTX-Video" \
  --train_data_dir="path/to/training_data" \
  --output_dir="output/ltx-video-lora" \
  --resolution=384 \
  --train_batch_size=1 \
  --gradient_accumulation_steps=8 \
  --gradient_checkpointing \
  --max_train_steps=2000 \
  --learning_rate=1e-4 \
  --rank=16 \
  --mixed_precision="bf16" \
  --num_train_frames=17
```

## Validation and Testing

Validate LoRAs across three scenarios:

1. **Identical prompt** with and without LoRA -- verify LoRA has an effect
2. **Varied prompts** -- test generalization beyond training data
3. **Edge cases** -- prompts/styles absent from training data

Sample at checkpoints (every 500 steps recommended) to detect overfitting early.

## Using Trained LoRAs

### In Diffusers

```python
import torch
from diffusers import LTXPipeline

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)
pipeline.load_lora_weights("output/ltx-video-lora", adapter_name="my_lora")
pipeline.set_adapters("my_lora")
pipeline.to("cuda")

video = pipeline(
    prompt="MYTRIGGER a person walking in a park",
    width=704, height=480, num_frames=161, num_inference_steps=50,
).frames[0]
```

### In ComfyUI

1. Place `.safetensors` file in `ComfyUI/models/loras`
2. Use the LoRA Loader node in your workflow
3. For [[ic-lora|IC-LoRAs]], use `LTXICLoRALoaderModelOnly` and `LTXAddVideoICLoRAGuide` nodes

### LoRA Strength Tuning

| Strength | Effect | When to Use |
|----------|--------|-------------|
| 0.0 | No effect | Disabled |
| 0.9-1.1 | Subtle | Preserving base model traits |
| 1.2-1.4 | Balanced | Most applications |
| 1.5-1.6 | Strong | Maximum style transfer |

**Rules:**
- Keep combined total strength under 2.0 for multiple LoRAs
- [[ic-lora|IC-LoRA]] control models should be at 1.0 (designed for full strength)
- LoRAs add minimal computational overhead (typically under 5%)
- LoRA fine-tuning works on FP8 quantized models with reduced VRAM demands

## Version Compatibility

**LTX-2 to LTX-2.3:** LoRAs trained on [[ltx-2]] do **not** transfer to [[ltx-2-3]] due to VAE redesign and text connector architecture changes. You must retrain LoRAs when upgrading.

**Cross-version exception:** Community reports that LTX-2.0 LoRAs actually work *significantly better* on LTX-2.3 than on 2.0 itself. When using 2.0 LoRAs on 2.3, reduce inference strength values.

**Scheduler changes (2.0 to 2.3):**
- Recommended step counts shifted from ~28-32 to ~22-26 steps
- Different sampler defaults and sweet spots between model versions

## Troubleshooting

| Issue | Solution |
|-------|----------|
| OOM during training | Enable gradient checkpointing, reduce batch size, lower resolution, reduce max_frames |
| Training loss not decreasing | Check learning rate (try 5e-5), verify dataset captions, ensure trigger word is consistent |
| Overfitting | Reduce max_steps, increase caption dropout, add more diverse training data |
| Poor motion quality | Use video clips instead of stills for motion LoRAs |
| Caption dropout + cached text embeddings conflict | Keep Cache Text Embeddings OFF if using caption dropout |
| Frame count errors | Ensure all video clips follow the 8n+1 frame rule |
| Resolution errors | Width and height must be divisible by 32 |

## References

- [[training-dataset-preparation]]
- [[training-hyperparameters]]
- [[ic-lora]]
- [[ltx-video-trainer]]
- [[third-party-training-services]]
- [[lora-community-ecosystem]]
