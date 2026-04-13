# LTX-Video Prompt Engineering Guide

**Source:** https://huggingface.co/Lightricks/LTX-Video
**Date:** 2026-04-13

---

## Overview

LTX-Video responds best to detailed, descriptive English prompts. The quality of generated videos is heavily influenced by prompt quality and style. This document covers best practices and examples.

---

## Prompt Format Best Practices

### Do:
- Write detailed, descriptive prompts (3-6 sentences recommended)
- Describe the scene, environment, and atmosphere
- Specify lighting conditions, camera angles, and movement
- Mention colors, textures, and materials
- Describe the motion and action explicitly
- Use present tense ("the camera pans", "waves crash")

### Don't:
- Use very short, vague prompts
- Rely on abstract concepts without visual description
- Expect factual accuracy (model is for creative generation)
- Use overly complex or contradictory instructions

---

## Example Prompts

### Nature Scene
```
"The turquoise waves crash against the dark, jagged rocks of the shore, 
sending white foam spraying into the air. The scene is dominated by the stark 
contrast between the bright blue water and the dark, almost black rocks. 
The water is a clear, turquoise color, and the waves are capped with white foam. 
The rocks are dark and jagged, and they are covered in patches of green moss. 
The shore is lined with lush green vegetation, including trees and bushes. 
In the background, there are rolling hills covered in dense forest. 
The sky is cloudy, and the light is dim."
```

### Character Animation
```
"A cute little penguin takes out a book and starts reading it. The penguin 
sits on a snowy surface, its black and white feathers ruffled by a gentle breeze. 
It reaches into a small red bag and pulls out a miniature book, holding it 
with its flippers. The penguin looks intently at the pages. The background 
shows a soft blue sky with scattered clouds."
```

### Cinematic Shot
```
"A cinematic aerial shot of a medieval castle perched on a hilltop during 
golden hour. The camera slowly orbits around the castle, revealing its stone 
walls, towers, and surrounding moat. Warm sunlight bathes the scene in golden 
tones. Mist rises from the valley below. The surrounding landscape is lush 
and green with dense forest."
```

---

## Negative Prompts

Negative prompts help avoid common artifacts. Recommended negative prompt:

```
"worst quality, inconsistent motion, blurry, jittery, distorted"
```

Additional negative terms that can help:
- `low resolution` - Avoid pixelated output
- `watermark` - Reduce watermark artifacts
- `text overlay` - Reduce unwanted text
- `deformed` - Reduce anatomical issues
- `static` - Encourage more motion
- `flickering` - Reduce temporal inconsistency

---

## Prompt Parameters Interaction

| Parameter | Prompt Impact |
|-----------|---------------|
| `guidance_scale` | Higher values = stronger prompt adherence, lower = more creative freedom |
| `num_inference_steps` | More steps = better prompt following |
| `denoise_strength` | Lower = more faithful to conditioning, higher = more prompt influence |

---

## Tips for Specific Use Cases

### Image-to-Video
- Describe the motion you want added to the still image
- Reference elements visible in the conditioning image
- Focus on what should change/move rather than static elements

### Video-to-Video
- Describe the desired transformation or continuation
- Reference the style and content of the input video
- Specify what should stay the same and what should change

### Multi-Condition
- Describe the narrative arc between conditioning frames
- Specify transitions and movements between keyframes
- Use temporal language ("begins with", "transitions to", "ends with")
