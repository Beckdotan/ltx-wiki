# Creative Showcase: User Examples and Settings

**Source:** https://huggingface.co/Lightricks/LTX-2/discussions/6

---

## Overview

A pinned discussion on the LTX-2 model page serves as a community showcase where users share their video generation examples, complete with prompts, settings, and hardware configurations.

---

## Example 1: tlennon-ie (RTX 5090)

### Hardware
- RTX 5090
- 128GB DDR5 RAM
- ComfyUI Latest version

### Settings
- **Model:** ltx-2-19b-dev-fp4.safetensors / BF16 version
- **Text Encoder:** gemma_3_12B_it.safetensors
- **Resolution:** 1024x720 (FP4) / 1280x720 (BF16)
- **Frames:** 121
- **Generation Time:** 80-95 seconds
- **Speed:** 1.6-4.4 s/it

### Prompts Shared
1. "A close-up of a cheerful boy puppet with curly auburn yarn hair..."
2. "A smartphone video of a blonde man standing in a vintage-style workshop..."
3. "Create a high-resolution, photorealistic video in the style of French choreographer and artist Yoann Bourgeois..."

---

## Example 2: Sikaworld1990 (FP8 Model)

### Settings
- **Model:** FP8 version
- **Text Encoder:** unsloth/gemma-3-12b-it-bnb-4bit
- **Resolution:** 1280x720
- **Scheduler:** res2s (noted as slower than Euler A)
- Included embedded ComfyUI workflow for replication

---

## Example 3: drakexp (GGUF Quantized)

### Settings
- **Model:** Q4_K_M GGUF LTX model
- **Text Encoder:** FP8 scaled Gemma
- **Resolution:** 1280x720
- **CFG:** 4
- **Steps:** 20 (first sampling)
- **LoRA:** rank175_fp8 distilled lora
- **Prompt:** "A smartphone video of a blonde man standing in a vintage-style workshop..."

---

## Key Observations from Community Showcases

### Common Settings Patterns
- **Resolution:** 1280x720 is the community standard for quality/speed balance
- **Frames:** 121 frames (5 seconds at 24fps) is the most common output length
- **Text Encoder:** Gemma 3 12B is universally used for LTX-2/2.3
- **Generation Time:** 80-95 seconds for 121 frames on RTX 5090

### Prompt Style
Community examples consistently use:
- Detailed, cinematic descriptions
- Specific camera direction language ("smartphone video", "close-up", "photorealistic")
- Material and texture descriptions
- Mood and lighting specifications

### Model Variant Comparison
- FP4 vs BF16: Similar quality, FP4 slightly slower
- FP8 vs Full-Dev: "Minimal and negligible" quality difference
- GGUF Q4_K_M: Viable for lower-VRAM setups with acceptable quality
