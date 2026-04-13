# Tutorial: LTX Video Prompting Guide

**Source:** https://huggingface.co/Lightricks/LTX-Video, https://huggingface.co/Lightricks/LTX-2, https://huggingface.co/Lightricks/LTX-Video/discussions/7

---

## Official Prompting Guidelines

### General Rules
- Prompts must be in **English**
- Elaborate, detailed prompts work best
- Describe visual contrast, color specificity, texture, and spatial composition
- Include camera movement and lighting descriptions
- Negative prompts supported: "worst quality, inconsistent motion, blurry, jittery, distorted"

### Official Prompt Template

```
[Scene description with visual details]. [Subject description]. [Action/motion description]. 
[Camera movement]. [Lighting and atmosphere]. [Style/medium specification].
```

### Example Official Prompts

**Nature Scene:**
> "The turquoise waves crash against the dark, jagged rocks of the shore, sending white foam spraying into the air. The scene is dominated by the stark contrast between the bright blue water and the dark, almost black rocks. The water is a deep turquoise color, and the waves are capped with white foam as they crash against the rocks..."

**Character Scene:**
> "A close-up of a cheerful boy puppet with curly auburn yarn hair..."

**Cinematic Scene:**
> "A smartphone video of a blonde man standing in a vintage-style workshop..."

**Artistic Reference:**
> "Create a high-resolution, photorealistic video in the style of French choreographer and artist Yoann Bourgeois..."

## LTX-2/2.3 Specific Prompting

### Official Prompting Guide
- Available at: https://ltx.video/blog/how-to-prompt-for-ltx-2
- Detailed guidance for the audio-video model

### Audio-Video Prompts
For LTX-2/2.3, prompts should also describe:
- Sound effects and ambient audio
- Speech content (for lip-sync)
- Music or background audio
- Language and accent specifications (for speech)

## Community Prompting Resources

### MushroomFleet LTXV PromptGen
- **Repository:** https://github.com/MushroomFleet/LLM-Base-Prompts/tree/main/LTXV-PromptGen
- **110 prompts across 11 categories**
- Uses ACE-HOLOFS technique
- YouTube tutorial: https://www.youtube.com/watch?v=s5c0IX8F0os
- **Adopted by Lightricks** in their official Playground Space

### Prompt Collection
- **Full official prompts:** https://github.com/MushroomFleet/DJZ-Nodes/blob/main/prompts/LTXV-video-full.txt

## Technical Prompt Constraints

### Resolution Requirements
- Width and height must be divisible by 32
- Frame count must be divisible by 8 + 1 (e.g., 9, 17, 25, ..., 257)
- Optimal range: less than 720x1280 resolution, less than 257 frames

### Model-Specific Notes

**LTX-Video (0.9.x):**
- T5 text encoder used
- Simpler prompt structure works

**LTX-2/2.3:**
- Gemma 3 12B text encoder
- Multi-layer feature extraction captures phonetic and semantic information
- "Thinking tokens" improve prompt adherence
- More complex, detailed prompts recommended
- Multilingual text encoder supports broader prompt understanding

## Common Prompt Issues

- Prompt adherence varies based on prompting style
- Complex prompts may not be perfectly matched
- Distilled models (CFG=1.0) cannot use negative prompts
- Character LoRAs may reduce motion when action prompts conflict
