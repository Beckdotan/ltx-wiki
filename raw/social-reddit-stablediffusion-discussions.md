# Reddit r/StableDiffusion - LTX Video Discussions

## Source
- **Platform**: Reddit r/StableDiffusion
- **Date range**: November 2024 - 2026
- **Key threads**: Multiple discussions about LTX Video quality, speed, and ComfyUI integration
- **URLs**:
  - https://www.reddit.com/r/StableDiffusion/comments/1hiapt6/ltx_video_91_or_cogvideox_15_ltxv_is_faster_but/ (LTX Video 0.91 vs CogVideoX 1.5)
  - https://daslikes.wordpress.com/2024/12/20/ltx-i2v-is-incredible-for-unblurring-photos-via-r-stablediffusion/ (LTX I2V for unblurring photos)
  - https://daslikes.wordpress.com/2025/01/04/ltxv-animating-images-that-typically-dont-yield-good-results-via-r-stablediffusion/ (LTXV animating difficult images)

## Key Discussion Points

### Speed vs Quality Trade-off (Core Theme)
- Community consensus: LTX Video is the fastest open-source video generation model, but not the highest quality
- Notable quote: "The quality is not as high as Hunyuan, SkyReels or Wan2.1 but the efficiency of this model is beyond anything out there"
- Another perspective: "The quality is surprisingly good for its size"
- General advice: "While it's super fast, you should expect the quality to be lower than other models"

### LTX Video 0.91 vs CogVideoX 1.5 Thread
- Users compared LTX Video 0.91 against CogVideoX 1.5
- LTXV praised for being faster but CogVideoX noted for higher quality in some scenarios
- Speed difference was significant enough that many users preferred LTX for rapid iteration

### Image-to-Video Use Cases
- Users discovered LTX I2V is excellent for "unblurring" photos by giving it an initial image and prompting it to pull focus
- Creative use case: animating images that typically don't work well with other models
- Community tip: Images that are "a bit blurry work best" because the model was trained on video data with natural motion blur

### Motion Issues and Workarounds
- Common complaint: Default image-to-video workflows produce videos that lack motion
- Reddit community identified that the model was trained with videos containing motion, so static sharp images confuse it
- Workarounds shared:
  1. Compress the image to a single frame video to reduce quality (preferred method)
  2. Slightly blur the input image before feeding it to the model
- Community consensus: first method works better

### VRAM and Performance
- LTX Video generates a 4-second video in ~20 seconds on RTX 4090
- Requires only 6 GB VRAM (remarkably low for video generation)
- 3-second videos in just 10 seconds on L40S GPU

### 13B Model Reception
- "LTXV 13B model is a huge step from the 2B model with noticeably higher quality"
- Community excitement about running 13B variant on consumer hardware
- Q8 kernels with diffusers enable efficient inference on consumer GPUs
- Trained on licensed data from Getty Images and Shutterstock (noted positively for ethical training)

### TeaCache Acceleration
- Community adopted TeaCache, a training-free caching approach
- Leverages timestep differences across model outputs
- Accelerates LTX-Video inference by up to 2x without significant visual quality degradation
- Popular optimization shared across r/StableDiffusion

## Sentiment Analysis
- **Overall**: Cautiously positive - valued for speed and accessibility
- **Strengths praised**: Speed, low VRAM, open source, rapid iteration
- **Weaknesses noted**: Quality gap vs Hunyuan/Wan, motion issues with I2V
- **Trend**: Sentiment improved significantly with each version update (0.91 -> 0.95 -> 0.96 -> 0.97 -> 13B)
