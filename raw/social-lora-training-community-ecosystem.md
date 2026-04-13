# LTX Video LoRA Training and Community Ecosystem

## Source
- **Platform**: GitHub, HuggingFace, community forums
- **Date range**: 2024 - 2026
- **URLs**:
  - https://github.com/Lightricks/LTX-Video-Trainer (Official community trainer)
  - https://github.com/Lightricks/LTX-2 (Official LTX-2 with trainer package)
  - https://wavespeed.ai/blog/posts/ltx-2-3-lora-training-guide-2026/ (LTX-2.3 LoRA training guide)
  - https://docs.ltx.video/open-source-model/ltx-2-trainer/ltx-2-training (Official training docs)
  - https://wavespeed.ai/blog/posts/introducing-wavespeed-ai-ltx-2-19b-video-lora-trainer-on-wavespeedai/ (WaveSpeedAI trainer)
  - https://fal.ai/learn/devs/ltx-2-video-trainer-user-guide (fal.ai trainer guide)
  - https://github.com/huggingface/finetrainers (HuggingFace finetrainers)
  - https://github.com/wildminder/awesome-ltx2 (Community resource collection)
  - https://wavespeed.ai/blog/posts/ltx-2-to-ltx-2-3-upgrade-guide-2026/ (LTX-2 to 2.3 migration)

## Official Training Tools

### LTX-Video-Trainer (Original)
- Community trainer for Lightricks' LTX Video model
- Supports LoRA training, full fine-tuning, video-to-video transformation workflows
- Works with custom datasets

### LTX-2 Trainer Package
- Built into the LTX-2 repository (packages/ltx-trainer/)
- Supports:
  - **LoRA training** - lightweight fine-tuning for specific styles
  - **Full fine-tuning** - complete model retraining
  - **IC-LoRA** - video-to-video transformation conditioning
  - **Control LoRAs** - custom control models (depth, pose, canny)
  - **Effect LoRAs** - specialized effects and transformations

### IC-LoRA (Inference-Conditioned LoRA)
- Unique to LTX ecosystem
- Enables conditioning video generation on reference video frames at inference time
- Allows fine-grained video-to-video control on top of text-to-video base model
- Enables teaching the model specific visual styles and motion patterns

## Community Training Services

### WaveSpeedAI
- LTX 2 19B Video-LoRA Trainer
- Cloud-based training service
- "High-performance custom model training service built on Lightricks' LTX-2 foundation model"
- 19 billion parameters: 14B for video, 5B for audio

### fal.ai
- LTX-2 Video Trainer user guide available
- Cloud GPU infrastructure for training
- "Enables developers to create custom video generation models without managing GPU infrastructure"

### HuggingFace finetrainers
- Added T2V LoRA fine-tuning support for LTX Video in December 2024
- Community extension: eisneim/ltx_lora_training_i2v_t2v (image-to-video training support)
- Scalable and memory-optimized training

## Community Ecosystem

### awesome-ltx2 (GitHub: wildminder/awesome-ltx2)
- Community-maintained collection of all available resources
- LTX-2 models, encoders, workflows, LoRAs for ComfyUI
- Central resource hub for the community

### Civitai
- Community LoRAs for various styles being shared
- Custom models and fine-tunes available for download
- Active community uploading LTX-compatible content

### HuggingFace
- Model weights and community fine-tunes hosted
- Discussion forums active with troubleshooting and sharing
- Kijai's quantized variants widely used (Kijai/LTXV2_comfy, Kijai/LTX2.3_comfy)

## Migration Challenges (LTX-2 to LTX-2.3)
- LoRA compatibility breaks between major versions documented
- WaveSpeedAI published migration guide
- Community sharing workarounds for LoRA adaptation

## Sentiment
- **Very positive** about training ecosystem maturity
- Community values the official trainer being open source
- Multiple cloud training options reduce barrier to entry
- IC-LoRA seen as innovative and unique capability
- Concern about LoRA compatibility across versions
