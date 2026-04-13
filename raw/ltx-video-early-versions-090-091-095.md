# LTX-Video Early Versions: 0.9, 0.9.1, and 0.9.5

**Source:** https://arxiv.org/abs/2501.00103 (original paper, describes v0.9)
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.1
**Source:** https://huggingface.co/Lightricks/LTX-Video-0.9.5

---

## v0.9 (Initial Release)

### Context
- First public release of LTX-Video
- Accompanied by the research paper "LTX-Video: Realtime Video Latent Diffusion" (arxiv: 2501.00103)
- Released in late 2024

### Specifications
- **Model size:** < 2B parameters
- **Architecture:** DiT-based (Diffusion Transformer) with custom Video-VAE
- **VAE compression:** 1:192 (32x32x8 spatiotemporal, 128 channels)
- **Text encoder:** T5-XXL
- **Training:** Rectified-flow with velocity prediction
- **Resolution:** 768x512 at 24 FPS (benchmark), up to ~720p
- **Frame count:** Up to 257 frames (divisible by 8+1)
- **Speed:** 5 seconds of 24 FPS video at 768x512 in 2 seconds on H100
- **Capabilities:** Text-to-video, image-to-video (simultaneous training)
- **Key claim:** "First DiT-based video generation model capable of generating high-quality videos in real-time"

### Evaluation Results (from paper)
Human survey comparison with similar-scale models:
- Outperformed Open-Sora Plan, CogVideoX (2B), and PyramidFlow
- In both text-to-video and image-to-video tasks
- Win rates significantly above competitors despite speed advantage

### Diffusers API
```python
from diffusers import LTXPipeline, LTXImageToVideoPipeline

pipe = LTXPipeline.from_pretrained("Lightricks/LTX-Video", torch_dtype=torch.bfloat16)
pipe.to("cuda")

video = pipe(
    prompt="...",
    negative_prompt="worst quality, inconsistent motion, blurry",
    width=704, height=480,
    num_frames=161,
    num_inference_steps=50,
).frames[0]
```

### Inference Parameters
- **Inference steps:** ~50 recommended
- **Guidance:** CFG (Classifier-Free Guidance) used
- **Negative prompts:** Supported and recommended

---

## v0.9.1

### What Changed
- Incremental improvement over v0.9
- Specific changes not publicly documented in detail
- Quality and stability refinements
- Same 2B parameter architecture

### Hugging Face
- **Repository:** `Lightricks/LTX-Video-0.9.1`
- **Status:** Public access

### API
- Same `LTXPipeline` and `LTXImageToVideoPipeline` as v0.9
- Same inference parameter ranges

### Specifications
- Same as v0.9: 2B parameters, 768x512 native, 24-30 FPS
- Continued to use ~50 inference steps
- T2V and I2V capabilities

---

## v0.9.5

### What Changed
- Further incremental improvement
- Bridge between early versions and the 0.9.6 distillation milestone
- Quality improvements over 0.9.1

### Hugging Face
- **Repository:** `Lightricks/LTX-Video-0.9.5`
- **Status:** Public access

### Specifications
- Still 2B parameters
- Same core architecture
- Real-time generation capability maintained
- Works best under 720x1280 resolution
- Frame count below 257 (divisible by 8+1)

### Significance
- Last version before the distilled variant was introduced (0.9.6)
- Last version before the major 13B scale-up (0.9.7)
- Represents the mature state of the original 2B architecture

---

## Evolution from v0.9 to v0.9.5

The early versions (0.9 -> 0.9.1 -> 0.9.5) represent iterative refinement of the original architecture described in the paper:

1. **Same core architecture** throughout: 2B DiT with custom high-compression Video-VAE
2. **Same capabilities:** T2V and I2V
3. **Same resolution/frame constraints:** Divisible by 32 (res) and 8+1 (frames)
4. **Incremental quality improvements** with each release
5. **No architectural changes** -- improvements likely from training data, hyperparameters, and fine-tuning

The major architectural leaps came later:
- v0.9.6: Distillation (15x speedup)
- v0.9.7: Scale to 13B parameters, FP8, ICLoRA, upscalers
- v0.9.8: Ecosystem maturation with 2B+13B distilled variants
