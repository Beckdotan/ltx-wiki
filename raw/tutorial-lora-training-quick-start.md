# Tutorial: LoRA Training Quick Start for LTX Video

**Source:** https://huggingface.co/Lightricks/LTX-Video-Squish-LoRA, https://huggingface.co/Lightricks/LTX-2, https://github.com/Lightricks/ltx-video-trainer

---

## Overview

LTX Video supports LoRA (Low-Rank Adaptation) training for creating custom video effects, styles, and controls. Training times are under 1 hour for most use cases.

## Official Training Repositories

### For LTX-Video (0.9.x)
```bash
# Clone the trainer
git clone https://github.com/Lightricks/ltx-video-trainer.git
cd ltx-video-trainer
```

### For LTX-2 / LTX-2.3
```bash
# Clone the LTX-2 monorepo
git clone https://github.com/Lightricks/LTX-2.git
cd LTX-2
uv sync
source .venv/bin/activate
# Trainer is in packages/ltx-trainer/
```

## Types of LoRAs You Can Train

1. **Motion LoRAs** - Camera movements, action effects (train time: <1 hour)
2. **Style LoRAs** - Visual styles like anime, pixel art (train time: <1 hour)
3. **Likeness LoRAs** - Character appearance and sound (train time: <1 hour)
4. **Effect LoRAs** - Custom visual effects (squish, cakeify, etc.)
5. **IC-LoRA** - In-Context LoRA for video-to-video control

## Using Trained LoRAs in ComfyUI

### Standard LoRAs
1. Place `.safetensors` file in `ComfyUI/models/loras`
2. Use the LoRA Loader node in your workflow
3. Reference the official example workflows

### IC-LoRAs
1. Place LoRA in `models/loras`
2. Use `LTXICLoRALoaderModelOnly` to load and extract downscale factor
3. Use `LTXAddVideoICLoRAGuide` to add reference video as guide
4. Follow official IC-LoRA workflow from ComfyUI-LTXVideo repository

## Trigger Word Patterns

Official LoRAs use trigger words for activation:
- **Squish:** `SQUISH two hands squeezing a squeezable object that is shaped like [object]`
- **Cakeify:** `CAKEIFY a person using a knife to cut a cake shaped like [object]`

## Community Training Tips

- LoRA fine-tuning works on fp8 quantized models (lower VRAM requirement)
- LTX-2.0 trained LoRAs transfer well to LTX-2.3 (often better quality)
- When using 2.0 LoRAs on 2.3, reduce inference strength values
- Public datasets available for experimentation (Squish, Cakeify, Canny-Control)

## Community Training Tool

For image-to-video and text-to-video LoRA training:
```bash
git clone https://github.com/eisneim/ltx_lora_training_i2v_t2v.git
```
