# Community Project: LoRA Training Tools and Ecosystem

**Source:** https://github.com/eisneim/ltx_lora_training_i2v_t2v, https://github.com/Lightricks/ltx-video-trainer, https://huggingface.co/Lightricks/LTX-2

---

## Official Training Tools

### LTX-Video Trainer
- **Repository:** https://github.com/Lightricks/ltx-video-trainer
- Official tool for training LoRAs on LTX-Video 0.9.x models
- Produced the Squish and Cakeify LoRAs with public datasets
- IC-LoRA training for detailer, canny, and depth controls

### LTX-2 Trainer (Monorepo Package)
- **Repository:** https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md
- Part of the LTX-2 monorepo alongside ltx-core and ltx-pipelines
- **Training time claims:** Under 1 hour for motion, style, or likeness LoRAs
- Supports audio-video joint training

## Community Training Tools

### eisneim - LTX LoRA Training (i2v & t2v)
- **Repository:** https://github.com/eisneim/ltx_lora_training_i2v_t2v
- Community-created training code supporting both image-to-video and text-to-video LoRA training
- Produced the bullet_time LoRA as a proof of concept
- MIT license

### finetrainers
- **Model:** finetrainers/dummy-ltxvideo on HuggingFace
- Test/dummy model for validating fine-tuning pipelines
- Indicates community infrastructure being built for LTX training

## Training Insights from Community

### Cross-Version LoRA Transfer
From HuggingFace Discussion #4 on LTX-2.3:
- LTX-2.0 trained LoRAs work **significantly better** on LTX-2.3 than on 2.0
- LoRAs that were unusable for further training on 2.0 become usable on 2.3
- Inference requires reduced strength values when applying 2.0 LoRAs to 2.3

### Training on Quantized Models
From HuggingFace Discussion #92 on LTX-Video:
- LoRA fine-tuning confirmed to work on fp8 quantized models
- Enables training on lower-VRAM hardware

### Community LoRA Training Results
Active community members creating LoRAs:
- **Burgstall** - Multiple expression LoRAs (amgery, smile, surprise, headbanger)
- **svjack** - Style LoRAs (anime landscape, pixel art)
- **Pierre-Jean** - Camera trajectory LoRA
- **Sameric934** - General LTX-2 video LoRA

## Public Training Datasets

Lightricks provides three public datasets for reproducible training:

| Dataset | Purpose | URL |
|---------|---------|-----|
| Squish-Dataset | Object deformation effects | https://huggingface.co/datasets/Lightricks/Squish-Dataset |
| Cakeify-Dataset | Object-to-cake transformation | https://huggingface.co/datasets/Lightricks/Cakeify-Dataset |
| Canny-Control-Dataset | Edge-based control training | https://huggingface.co/datasets/Lightricks/Canny-Control-Dataset |
