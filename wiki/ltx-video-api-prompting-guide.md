---
title: LTX Video API Prompting Guide
type: guide
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/api-documentation/prompting-guide
tags:
  - api
  - prompting
  - ltx-video
  - guide
  - best-practices
---

# LTX Video API Prompting Guide

Official guidance from [[lightricks]] on writing effective prompts for the [[ltx-video-api]]. Good prompts are critical for high-quality video output.

## Core Prompt Structure

Write prompts as **a single flowing paragraph** using **present tense verbs**. Aim for **4-8 descriptive sentences**, with detail matched to shot scale.

## Six Essential Elements

### 1. Establish the Shot
Use cinematography terminology aligned with your genre. Specify shot scale or visual characteristics.

### 2. Set the Scene
Incorporate lighting conditions, color schemes, textures, and atmospheric qualities to establish mood.

### 3. Describe the Action
Present the core action as a natural, sequential flow from start to finish.

### 4. Define Characters
Include age, hairstyle, clothing, and distinguishing features. Express emotion through physical cues, not abstract labels.

### 5. Identify Camera Movements
Specify how and when the camera moves. Describe how subjects appear after the movement completes.

### 6. Describe Audio
Detail ambient sound, music, dialogue (in quotation marks), or singing. Specify language and accent when relevant.

## What Works Well

- Cinematic compositions with thoughtful lighting and depth
- Emotive human moments with subtle gestures
- Atmospheric effects (fog, golden hour, rain, reflections)
- Explicit camera language ("slow dolly in", "handheld tracking")
- Stylized aesthetics (painterly, noir, animation)
- Lighting control via backlighting, rim light, color palettes
- Characters speaking or singing in multiple languages

## What to Avoid

- Internal emotional states (substitute visual descriptions instead)
- Readable text and logos
- Complex physics or chaotic motion
- Overloaded scenes with multiple characters or actions
- Conflicting or illogical lighting
- Overcomplicated prompts (layer complexity gradually)

## Technical Vocabulary

### Camera Language
follows, tracks, pans, circles, tilts, pushes in/pulls back, overhead, handheld, over-the-shoulder, establishing shots, static frames

### Pacing Effects
slow motion, time-lapse, rapid cuts, lingering shots, freeze-frames, fades, transitions

### Visual Details
film grain, lens flares, particle systems, motion blur, depth of field

## Camera Motion API Parameter

In addition to describing camera motion in the prompt text, the API offers a `camera_motion` parameter on text-to-video and image-to-video endpoints with these options:

`dolly_in`, `dolly_out`, `dolly_left`, `dolly_right`, `jib_up`, `jib_down`, `static`, `focus_shift`

## Prompt Enhancement

The open-source model supports an `enhance_prompt=True` parameter for automatic prompt improvement. Keep prompts under 200 words for best results.

## Example Prompt

> The turquoise waves crash against the dark, jagged rocks of the shore, sending white foam spraying into the air. The scene is dominated by the stark contrast between the bright blue water and the dark, almost black rocks. The water is a clear, turquoise color, and the waves are capped with white foam. The rocks are dark and jagged, and they are covered in patches of green moss. The shore is lined with lush green vegetation, including trees and bushes. In the background, there are rolling hills covered in dense forest. The sky is cloudy, and the light is dim.

## Related Pages

- [[ltx-video-api]]
- [[ltx-video-api-endpoints]]
- [[ltx-video-huggingface]]
