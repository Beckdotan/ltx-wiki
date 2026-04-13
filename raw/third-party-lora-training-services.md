# Third-Party LoRA Training Services for LTX Video

## WaveSpeedAI

### Overview
WaveSpeedAI offers managed LoRA training services built on the LTX-2 foundation model, enabling users to create custom adapters without managing GPU infrastructure.

### Services Offered

#### LTX-2 19B Video LoRA Trainer
- Train lightweight LoRA adapters from video clips
- Captures motion patterns, visual styles, and character appearances
- Audio-video sync learning during training
- **URL:** https://wavespeed.ai/models/wavespeed-ai/ltx-2-19b/video-lora-trainer

#### LTX-2 IC-LoRA Trainer
- Custom video-to-video transformation training
- Advanced control adapters for structural guidance
- **URL:** https://wavespeed.ai/models/wavespeed-ai/ltx-2-19b/ic-lora-trainer

#### LTX-2.3 Text-to-Video LoRA
- Custom style LoRAs for text-to-video generation
- **URL:** https://wavespeed.ai/models/wavespeed-ai/ltx-2.3/text-to-video-lora

#### LTX-2.3 Image-to-Video LoRA
- Custom style LoRAs for image-to-video animation
- **URL:** https://wavespeed.ai/models/wavespeed-ai/ltx-2.3/image-to-video-lora

### Pricing
Training costs scale linearly:
| Steps | Cost |
|-------|------|
| 100 | $0.35 |
| 500 | $1.75 |
| 1,000 | $3.50 |
| 2,000 | $7.00 |

### Key Features
- No cold starts; training jobs begin immediately
- Video-first training (video clips, not static images)
- Motion-aware learning for temporal coherence
- Customizable parameters (steps, learning rate, LoRA rank)

### Use Cases
1. Motion style transfer (dance, action sequences)
2. Character animation with consistent movement
3. Brand video production
4. Artistic animation style replication
5. Visual effects and transitions

### Technical Details
- LTX-2 model: 19 billion parameters (14B video + 5B audio)
- Adjustable training hyperparameters

## Oxen.ai

### Overview
Oxen.ai provides a managed platform for training LTX-2 Character LoRAs with automated GPU orchestration.

### Process
1. Upload training data
2. Oxen.ai orchestrates GPUs and training
3. Download trained LoRA weights

### Key Selling Point
"Model training shouldn't be intimidating" -- emphasizes accessibility for non-ML-engineers.

### Tutorial
- "How to Train a LTX-2 Character LoRA with Oxen.ai"
- **URL:** https://ghost.oxen.ai/how-to-train-a-ltx-2-character-lora-with-oxen-ai/

## RunComfy

### Overview
Browser-based LoRA training for LTX models without managing GPU infrastructure.
- Upload dataset, configure parameters, download LoRA when complete
- Integrated with ComfyUI workflows
- **URL:** https://www.runcomfy.com/

## Community-Shared LoRAs

### Civitai
Multiple community-trained LoRAs shared on Civitai:
- **LTX-2 IC-LoRA Detailer** -- enhances fine visual details and clarity
  - URL: https://civitai.com/models/2293339/ltx2-ic-lora-detailer
- **SSX LTX2 T2V** -- text-to-video LoRA trained with official trainer
  - URL: https://civitai.com/models/2301350/ssx-ltx2-t2v
- Various NSFW-focused community LoRAs
- Image-to-video adapter LoRAs

### Hugging Face
- 24 adapter models for LTX-Video
- 49 adapter models for LTX-2
- 20 adapter models for LTX-2.3
- Community fine-tunes: 25 (LTXV) + 54 (LTX-2) + 27 (LTX-2.3)
- **Phr00t/LTX2-Rapid-Merges** -- community model merges
  - URL: https://huggingface.co/Phr00t/LTX2-Rapid-Merges

## Sources
- https://wavespeed.ai/blog/posts/introducing-wavespeed-ai-ltx-2-19b-video-lora-trainer-on-wavespeedai/
- https://wavespeed.ai/blog/posts/ltx-2-3-lora-training-guide-2026/
- https://ghost.oxen.ai/how-to-train-a-ltx-2-character-lora-with-oxen-ai/
- https://civitai.com (search "LTX")
