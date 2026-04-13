---
title: Training Hyperparameters
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/training-fine-tuning.md
  - raw/python-lora-training-guide.md
tags:
  - training
  - hyperparameters
  - configuration
  - lora
---
# Training Hyperparameters

Reference for [[lora-training]] configuration parameters across the official trainers and HuggingFace Diffusers.

## LTX-2 Trainer Configuration (YAML)

```yaml
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
  max_frames: 72        # Must follow 8n+1 rule (actually 73 = 9*8+1)
  caption_dropout: 0.1

output:
  output_dir: /path/to/output
  save_every_n_steps: 500
  log_every_n_steps: 10
```

## Key Parameters

| Parameter | Default | Range | Notes |
|-----------|---------|-------|-------|
| `lora_rank` | 32 | 4-128 | 32 is the recommended default |
| `lora_alpha` | 32 | Usually = rank | Scaling factor for LoRA |
| `learning_rate` | 1e-4 | 1e-5 to 5e-4 | 1e-4 is the standard starting point |
| `batch_size` | 1 | 1-4 | Limited by VRAM |
| `max_steps` | 2000 | 500-5000 | Sample at 500-750 to check for overfitting |
| `gradient_checkpointing` | true | -- | Reduces VRAM usage significantly |
| `caption_dropout` | 0.1 | 0.0-0.3 | Cannot use with cached text embeddings |
| `resolution` | 720 | 480-1080 | Limited by VRAM |
| `max_frames` | 72 | 9-121 | Must follow 8n+1 rule |
| `gradient_accumulation_steps` | 1 | 1-8 | Effective batch multiplier |

## LoRA Rank Selection

| Rank | Memory | Quality | Use Case |
|------|--------|---------|----------|
| 8 | Low | Basic | Simple style changes |
| 16 | Medium | Good | General purpose |
| 32 | Higher | Very good | Detailed styles/concepts (recommended default) |
| 64 | High | Excellent | Complex adaptations |
| 128 | Very high | Maximum | When quality is paramount |

## HuggingFace Diffusers Arguments

| Argument | Description | Recommended Value |
|----------|-------------|-------------------|
| `--pretrained_model_name_or_path` | Base model to fine-tune | `"Lightricks/LTX-Video"` |
| `--train_data_dir` | Path to training videos + captions | Your data path |
| `--output_dir` | Where to save LoRA weights | Output path |
| `--resolution` | Training resolution | 512 or matching your data |
| `--train_batch_size` | Batch size per GPU | 1 (increase if VRAM allows) |
| `--gradient_accumulation_steps` | Effective batch multiplier | 4-8 |
| `--max_train_steps` | Total training steps | 500-5000 |
| `--learning_rate` | Learning rate | 1e-4 to 1e-5 |
| `--lr_scheduler` | LR schedule type | `"cosine"` or `"constant"` |
| `--lr_warmup_steps` | Warmup steps | 100-500 |
| `--rank` | LoRA rank | 16, 32, or 64 |
| `--mixed_precision` | Training precision | `"bf16"` |
| `--num_train_frames` | Frames per training sample | 17-49 |
| `--gradient_checkpointing` | Save memory during training | Enable for low VRAM |
| `--validation_prompt` | Prompt for periodic validation | Your test prompt |
| `--validation_epochs` | Validate every N epochs | 50 |
| `--report_to` | Logging backend | `"wandb"` or `"tensorboard"` |

## ai-toolkit Configuration (Ostris)

```yaml
job: extension
config:
  name: "ltx_video_lora"
  process:
    - type: sd_trainer
      training_folder: "output"
      model:
        name_or_path: "Lightricks/LTX-Video"
        type: ltx_video
      datasets:
        - folder_path: "path/to/training_data"
          caption_ext: ".txt"
      train:
        batch_size: 1
        steps: 2000
        lr: 1e-4
        optimizer: "adamw8bit"
        dtype: bf16
      network:
        type: lora
        rank: 32
        alpha: 16
```

## Accelerate Configuration

Before training with HuggingFace Diffusers, configure accelerate:

**Single GPU:**
```yaml
compute_environment: LOCAL_MACHINE
distributed_type: 'NO'
mixed_precision: bf16
num_machines: 1
num_processes: 1
```

**Multi-GPU:**
```yaml
compute_environment: LOCAL_MACHINE
distributed_type: MULTI_GPU
mixed_precision: bf16
num_machines: 1
num_processes: 2
```

## Monitoring Training

```bash
# TensorBoard (if configured)
tensorboard --logdir /path/to/output/logs

# Check training progress via checkpoints
ls -la /path/to/output/checkpoints/
```

## Tuning Tips

- Start with `lora_rank=32` and `learning_rate=1e-4` -- the "boring but correct" defaults
- Use cosine learning rate schedule for stability
- Sample outputs at checkpoint intervals (every 500 steps) to detect overfitting early
- If overfitting: reduce `max_steps`, increase `caption_dropout`, add more training data
- If underfitting: increase `max_steps`, try higher `learning_rate`, check caption quality
- Common overfitting signs: generated videos look too similar to training data

## References

- [[lora-training]]
- [[training-dataset-preparation]]
- [[ltx-video-trainer]]
