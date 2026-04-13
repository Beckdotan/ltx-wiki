# LTX Video LoRA Ecosystem

**Source:** https://huggingface.co/models?search=ltx+video+lora, https://huggingface.co/Lightricks/LTX-Video-Squish-LoRA, https://huggingface.co/Lightricks/LTX-Video-Cakeify-LoRA, https://huggingface.co/Lightricks/LTX-Video-ICLoRA-detailer-13b-0.9.8

---

## Official LoRAs by Lightricks

### LTX-Video-Squish-LoRA
- **Purpose:** Creates "squish"-style video effects where objects are squeezed and deformed
- **Base Model:** LTX Video v0.9.5
- **Likes:** 15
- **Trigger prompt:** `SQUISH two hands squeezing a squeezable object that is shaped like [your object]`
- **Training data:** [Lightricks/Squish-Dataset](https://huggingface.co/datasets/Lightricks/Squish-Dataset) (public)
- **Usage:** ComfyUI workflow provided at `assets/ltxv-i2v-lora.json`
- **Demo examples:** 3D character, cat, iPhone, Pikachu squishing

### LTX-Video-Cakeify-LoRA
- **Purpose:** Creates "cakeify" effects where objects transform into cake-cutting scenes
- **Base Model:** LTX Video v0.9.5
- **Likes:** 19
- **Trigger prompt:** `CAKEIFY a person using a knife to cut a cake shaped like [your object]`
- **Training data:** [Lightricks/Cakeify-Dataset](https://huggingface.co/datasets/Lightricks/Cakeify-Dataset) (public)
- **Usage:** ComfyUI workflow provided

### IC-LoRA Detailer (0.9.8)
- **Purpose:** Video detailer for enhancing fine details and textures
- **Base Model:** LTXV_13B_098_DEV
- **Downloads:** 29,812/month
- **Likes:** 29
- **Training:** In-Context LoRA (IC LoRA) methodology
- **Available formats:** ComfyUI and Diffusers safetensors
- **Trainer:** [LTX-Video-Trainer](https://github.com/Lightricks/ltx-video-trainer)

### IC-LoRA Canny Control (0.9.7)
- **Downloads:** 405/month
- **Likes:** 13
- **Purpose:** Edge-based video control

### IC-LoRA Depth Control (0.9.7)
- **Downloads:** 362/month
- **Likes:** 13
- **Purpose:** Depth-based video control

### LTX-2 IC-LoRA Detailer
- **Base Model:** LTX-2-19b
- **Purpose:** Video detail refinement for LTX-2 outputs
- **Likes:** 62
- **Used in:** 29+ HuggingFace Spaces

### LTX-2.3 Union Control IC-LoRA
- **Base Model:** LTX-2.3-22b
- **Likes:** 43
- **Purpose:** Multiple control signals (canny + depth + pose)
- **Reference downscale:** 2x (0.5x output resolution)

### LTX-2.3 Motion Track Control IC-LoRA
- **Base Model:** LTX-2.3-22b
- **Likes:** 32
- **Purpose:** Motion guidance via sparse point trajectories and spline overlays

---

## Community-Created LoRAs

| Creator | LoRA Name | Purpose | Likes |
|---------|-----------|---------|-------|
| Burgstall | amgery-lora-for-ltxv-13b-0.9.7 | Video expressions | 15 |
| Burgstall | nhl-smile-lora-for-ltxv-13b-0.9.7 | Smile effects | 1 |
| Burgstall | sorpresa-lora-for-ltxv-13b | Surprise expressions | 0 |
| Burgstall | headbanger-lora-for-ltxv-13b-0.9.7 | Head movement | 0 |
| eisneim | LTX-video_lora_bullet_time | Bullet time effect | 0 |
| svjack | ltx_video_anime_landscape_early_lora | Anime landscapes | 1 |
| svjack | ltx_video_pixel_early_lora | Pixel art style | 0 |
| Pierre-Jean | ltx-video-trajectory-lora | Camera trajectory | 0 |
| Sameric934 | ltx2-video-lora | General video | 0 |
| xuaxu | ltxvideo_lora_mine | Custom LoRA | 0 |

## LoRA Training Tools

### Official Trainer
- **Repository:** https://github.com/Lightricks/ltx-video-trainer (for LTX-Video)
- **LTX-2 Trainer:** https://github.com/Lightricks/LTX-2/blob/main/packages/ltx-trainer/README.md
- Training time: Under 1 hour for motion, style, or likeness LoRAs

### Community Training
- **eisneim** published a community training repository: https://github.com/eisneim/ltx_lora_training_i2v_t2v
- Discussion #92 confirmed LoRA fine-tuning works on fp8 models with community experimentation

## In-Context LoRA (IC-LoRA) Technology

IC-LoRA is a key innovation in the LTX ecosystem:
- Enables video context injection into generation
- Provides video-to-video control on text-to-video models
- Works by conditioning on reference video frames during inference
- Supports initial images for image-to-video generation
- Reference downscale factor allows efficiency with smaller reference inputs
- Paper: "AVControl: Efficient Framework for Training Audio-Visual Controls" (arXiv: 2603.24793)
