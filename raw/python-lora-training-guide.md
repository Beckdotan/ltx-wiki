# LTX Video - LoRA Training and Fine-Tuning Guide

> **Source:** https://huggingface.co/docs/diffusers/training/ltx_video (referenced but not yet published as of v0.37.1)
> **Source:** https://github.com/huggingface/diffusers/tree/main/examples/ltx_video
> **Source:** https://huggingface.co/docs/diffusers/tutorials/using_peft_for_inference

## Overview

LTX Video supports LoRA (Low-Rank Adaptation) training through the Hugging Face Diffusers training examples. LoRA training allows you to fine-tune the model on custom data with minimal compute resources, producing lightweight adapter weights (typically a few hundred MBs).

Note: As of Diffusers v0.37.1, the LTX Video training documentation is available on the `main` branch but not yet in the stable release. Install from source for the latest training scripts.

## Prerequisites

### Installation for Training

```bash
# Install diffusers from source (for latest training scripts)
pip install -U git+https://github.com/huggingface/diffusers

# Additional training dependencies
pip install -U transformers accelerate peft torch torchvision

# For dataset handling
pip install datasets pillow imageio imageio-ffmpeg

# For logging (optional)
pip install wandb tensorboard
```

### Hardware Requirements

- **Minimum:** NVIDIA GPU with 16GB VRAM (2B model with optimizations)
- **Recommended:** NVIDIA GPU with 24-48GB VRAM (for comfortable training)
- For the 13B model, multi-GPU or significant memory optimization is required
- CPU RAM: At least 32GB recommended

## Dataset Preparation

### Video Dataset Format

Prepare your training data as a directory of video files with corresponding caption text files:

```
training_data/
  video_001.mp4
  video_001.txt
  video_002.mp4
  video_002.txt
  ...
```

Each `.txt` file should contain the text description/caption for the corresponding video.

### Dataset Requirements

- **Resolution:** Videos should ideally match the target generation resolution (e.g., 704x480, 768x512)
- **Frame count:** Should follow the 8n+1 pattern (e.g., 9, 17, 25, ..., 161)
- **FPS:** 24 or 25 FPS recommended
- **Format:** MP4 or other common video formats
- **Captions:** Detailed English descriptions work best

### Using HuggingFace Datasets

You can also use a HuggingFace Dataset format:

```python
from datasets import Dataset

# Create dataset from local files
dataset = Dataset.from_dict({
    "video": ["path/to/video1.mp4", "path/to/video2.mp4"],
    "text": ["Caption for video 1", "Caption for video 2"]
})
```

## Accelerate Configuration

Before training, configure accelerate:

```bash
accelerate config
```

Example configuration for single GPU:

```yaml
compute_environment: LOCAL_MACHINE
distributed_type: 'NO'
mixed_precision: bf16
num_machines: 1
num_processes: 1
```

Example configuration for multi-GPU:

```yaml
compute_environment: LOCAL_MACHINE
distributed_type: MULTI_GPU
mixed_precision: bf16
num_machines: 1
num_processes: 2  # Number of GPUs
```

## LoRA Training Script

### Basic Training Command

```bash
accelerate launch train_ltx_video_lora.py \
  --pretrained_model_name_or_path="Lightricks/LTX-Video" \
  --train_data_dir="path/to/training_data" \
  --output_dir="output/ltx-video-lora" \
  --resolution=512 \
  --train_batch_size=1 \
  --gradient_accumulation_steps=4 \
  --max_train_steps=1000 \
  --learning_rate=1e-4 \
  --lr_scheduler="cosine" \
  --lr_warmup_steps=100 \
  --rank=32 \
  --mixed_precision="bf16" \
  --seed=42 \
  --validation_prompt="A test prompt for validation" \
  --validation_epochs=50 \
  --report_to="wandb"
```

### Key Training Arguments

| Argument | Description | Recommended Value |
|----------|-------------|-------------------|
| `--pretrained_model_name_or_path` | Base model to fine-tune | `"Lightricks/LTX-Video"` |
| `--train_data_dir` | Path to training videos + captions | Your data path |
| `--output_dir` | Where to save LoRA weights | Output path |
| `--resolution` | Training resolution | 512 or matching your data |
| `--train_batch_size` | Batch size per GPU | 1 (increase if VRAM allows) |
| `--gradient_accumulation_steps` | Effective batch multiplier | 4-8 |
| `--max_train_steps` | Total training steps | 500-5000 depending on dataset |
| `--learning_rate` | Learning rate | 1e-4 to 1e-5 |
| `--lr_scheduler` | LR schedule type | "cosine" or "constant" |
| `--lr_warmup_steps` | Warmup steps | 100-500 |
| `--rank` | LoRA rank | 16, 32, or 64 |
| `--mixed_precision` | Training precision | "bf16" |
| `--seed` | Random seed | Any integer |
| `--num_train_frames` | Frames per training sample | 17-49 |
| `--gradient_checkpointing` | Save memory during training | Enable for low VRAM |

### Memory-Optimized Training

For limited VRAM, use gradient checkpointing and lower precision:

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
  --lr_scheduler="cosine" \
  --lr_warmup_steps=200 \
  --rank=16 \
  --mixed_precision="bf16" \
  --num_train_frames=17 \
  --seed=42
```

## Using Trained LoRA

### Loading Your LoRA

```python
import torch
from diffusers import LTXPipeline

pipeline = LTXPipeline.from_pretrained(
    "Lightricks/LTX-Video", torch_dtype=torch.bfloat16
)

# Load your trained LoRA
pipeline.load_lora_weights(
    "output/ltx-video-lora",
    weight_name="pytorch_lora_weights.safetensors",
    adapter_name="my_lora"
)
pipeline.set_adapters("my_lora")
pipeline.to("cuda")

video = pipeline(
    prompt="Your trigger word and description here",
    negative_prompt="worst quality, blurry",
    width=704,
    height=480,
    num_frames=161,
    num_inference_steps=50,
).frames[0]
```

### Uploading LoRA to Hub

```python
from huggingface_hub import HfApi

api = HfApi()
api.upload_folder(
    folder_path="output/ltx-video-lora",
    repo_id="your-username/my-ltx-video-lora",
    repo_type="model",
)
```

## Community Training Tools

### ai-toolkit

The [ai-toolkit](https://github.com/ostris/ai-toolkit) by Ostris supports LoRA training for various video models including LTX Video:

```bash
git clone https://github.com/ostris/ai-toolkit
cd ai-toolkit
pip install -r requirements.txt
```

Configuration example:

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

## Tips for Effective LoRA Training

### Data Quality
- Use high-quality, consistent video data
- Ensure captions accurately describe the video content
- Use a diverse but focused dataset (50-500 videos for a specific style)
- Aim for consistent resolution and frame rate

### Training Configuration
- Start with a low rank (16-32) and increase if needed
- Use cosine learning rate schedule for stability
- Monitor validation outputs to detect overfitting
- Common overfitting signs: generated videos look too similar to training data

### Trigger Words
- Consider adding a unique trigger word to all captions during training
- This allows you to activate the LoRA style on demand
- Example: prefix all captions with "MYSTYLE" during training

### LoRA Rank Selection
| Rank | Memory | Quality | Use Case |
|------|--------|---------|----------|
| 8 | Low | Basic | Simple style changes |
| 16 | Medium | Good | General purpose |
| 32 | Higher | Very good | Detailed styles/concepts |
| 64 | High | Excellent | Complex adaptations |
| 128 | Very high | Maximum | When quality is paramount |
