# Creative Showcase: Official LoRA Effect Demonstrations

**Source:** https://huggingface.co/Lightricks/LTX-Video-Squish-LoRA, https://huggingface.co/Lightricks/LTX-Video-Cakeify-LoRA

---

## Overview

Lightricks released two creative LoRA demonstrations alongside their training datasets, showing the creative potential of LTX-Video fine-tuning.

## Squish LoRA Effect

### Description
A LoRA trained to generate "squish" effects - animations where hands squeeze deformable objects shaped like various subjects.

### Demo Examples
- 3D character being squished
- Cat being squished
- iPhone being squished
- Pikachu being squished

### Prompt Format
```
SQUISH two hands squeezing a squeezable object that is shaped like [your object]
```

### Training Dataset
- Public dataset: [Lightricks/Squish-Dataset](https://huggingface.co/datasets/Lightricks/Squish-Dataset)
- Demonstrates how custom video effects can be trained with relatively small datasets

## Cakeify LoRA Effect

### Description
A LoRA trained to generate "cakeify" effects - scenes where a person cuts a cake that is shaped like various objects.

### Demo Examples
- 3D characters being "cakeified"
- Animals (cats, Pikachu) transformed into cakes
- Consumer products (iPhone) becoming cakes

### Prompt Format
```
CAKEIFY a person using a knife to cut a cake shaped like [your object]
```

### Training Dataset
- Public dataset: [Lightricks/Cakeify-Dataset](https://huggingface.co/datasets/Lightricks/Cakeify-Dataset)

## Significance

These LoRAs demonstrate:
1. **Custom video effects** can be trained with the LTX Video architecture
2. **Trigger word** patterns enable easy activation of trained effects
3. **Public training data** enables community reproduction and variation
4. **Creative applications** beyond standard video generation (viral video effects, social media content)
5. Both LoRAs work with the **image-to-video** pipeline, taking a reference image as input

## ComfyUI Integration
Both LoRAs include ComfyUI workflows (`assets/ltxv-i2v-lora.json`) and example input images for immediate use.
