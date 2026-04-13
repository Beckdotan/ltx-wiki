---
title: Prompt Engineering
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/tutorial-prompting-guide.md
  - raw/dev-prompt-engineering.md
tags:
  - prompting
  - guide
  - best-practices
  - negative-prompts
  - ltx-video
  - ltx2
---

# Prompt Engineering

Prompt quality heavily influences the output of all [[ltx-video-overview|LTX Video]] models. This page covers best practices, templates, negative prompts, and model-specific guidance for writing effective prompts.

## General Rules

- Prompts must be in **English**
- Elaborate, detailed prompts work best (3-6 sentences recommended)
- Use present tense ("the camera pans", "waves crash")
- Describe visual contrast, color specificity, texture, and spatial composition
- Include camera movement and lighting descriptions
- Avoid very short or vague prompts
- Do not rely on abstract concepts without visual description

## Official Prompt Template

```
[Scene description with visual details]. [Subject description]. [Action/motion description].
[Camera movement]. [Lighting and atmosphere]. [Style/medium specification].
```

### What to Include

- **Scene and environment:** Setting, atmosphere, weather, background elements
- **Subject:** Appearance, clothing, position, materials
- **Motion and action:** Explicit description of what should move and how
- **Camera:** Angle, movement (pan, orbit, dolly, tracking), distance
- **Lighting:** Time of day, color temperature, shadows, direction
- **Style:** "cinematic", "photorealistic", "animated", "stop-motion", etc.
- **Colors and textures:** Specific materials, surface details, color palette

### What to Avoid

- Overly complex or contradictory instructions
- Expecting factual accuracy (the model is for creative generation)
- Relying on emotions without grounding them in physical actions

## Example Prompts

**Nature scene:**
> "The turquoise waves crash against the dark, jagged rocks of the shore, sending white foam spraying into the air. The scene is dominated by the stark contrast between the bright blue water and the dark, almost black rocks. The water is a clear, turquoise color, and the waves are capped with white foam. The rocks are dark and jagged, and they are covered in patches of green moss. The shore is lined with lush green vegetation, including trees and bushes. In the background, there are rolling hills covered in dense forest. The sky is cloudy, and the light is dim."

**Cinematic shot:**
> "A cinematic aerial shot of a medieval castle perched on a hilltop during golden hour. The camera slowly orbits around the castle, revealing its stone walls, towers, and surrounding moat. Warm sunlight bathes the scene in golden tones. Mist rises from the valley below. The surrounding landscape is lush and green with dense forest."

**Character animation:**
> "A cute little penguin takes out a book and starts reading it. The penguin sits on a snowy surface, its black and white feathers ruffled by a gentle breeze. It reaches into a small red bag and pulls out a miniature book, holding it with its flippers. The penguin looks intently at the pages. The background shows a soft blue sky with scattered clouds."

## Negative Prompts

Negative prompts help suppress common artifacts. Recommended default:

```
worst quality, inconsistent motion, blurry, jittery, distorted
```

Additional negative terms:

| Term | Effect |
|------|--------|
| `low resolution` | Reduces pixelated output |
| `watermark` | Reduces watermark artifacts |
| `text overlay` | Reduces unwanted text |
| `deformed` | Reduces anatomical issues |
| `static` | Encourages more motion |
| `flickering` | Reduces temporal inconsistency |

**Note:** Distilled models running at CFG=1.0 cannot use negative prompts.

## Parameter Interactions

| Parameter | Prompt Impact |
|-----------|---------------|
| `guidance_scale` | Higher values = stronger prompt adherence; lower = more creative freedom |
| `num_inference_steps` | More steps = better prompt following |
| `denoise_strength` | Lower = more faithful to conditioning; higher = more prompt influence |

## Model-Specific Prompting

### LTX-Video (0.9.x)

- Uses T5 text encoder
- Simpler prompt structures work adequately
- Negative prompts supported on dev models

### LTX-2 / LTX-2.3

- Uses Gemma 3 12B text encoder with multi-layer feature extraction
- "Thinking tokens" improve prompt adherence
- More complex, detailed prompts recommended
- Multilingual text encoder supports broader prompt understanding
- Prompts should also describe audio elements:
  - Sound effects and ambient audio
  - Speech content (for lip-sync)
  - Music or background audio
  - Language and accent specifications

See the official prompting guide at https://ltx.video/blog/how-to-prompt-for-ltx-2

## Tips for Specific Workflows

### Image-to-Video

- Describe the motion you want added to the still image
- Reference elements visible in the conditioning image
- Focus on what should change or move rather than static elements

### Video-to-Video

- Describe the desired transformation or continuation
- Reference the style and content of the input video
- Specify what should stay the same and what should change

### Multi-Condition

- Describe the narrative arc between conditioning frames
- Specify transitions and movements between keyframes
- Use temporal language ("begins with", "transitions to", "ends with")

## Technical Constraints

- Width and height must be divisible by 32
- Frame count must follow the 8n+1 pattern (9, 17, 25, ..., 257)
- Optimal range: below 720x1280 resolution, below 257 frames
- Character [[lora-training-and-finetuning|LoRAs]] may reduce motion when action prompts conflict

## Community Resources

### MushroomFleet LTXV PromptGen

- 110 prompts across 11 categories using the ACE-HOLOFS technique
- Repository: https://github.com/MushroomFleet/LLM-Base-Prompts/tree/main/LTXV-PromptGen
- Adopted by Lightricks in their official Playground Space

### Full Prompt Collection

- https://github.com/MushroomFleet/DJZ-Nodes/blob/main/prompts/LTXV-video-full.txt

## See Also

- [[installation-quickstart]]
- [[tutorials-and-community-guides]]
- [[audio-video-generation]]
- [[lora-training-and-finetuning]]
