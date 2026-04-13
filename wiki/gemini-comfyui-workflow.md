---
title: Gemini ComfyUI Workflow
type: community-project
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/community-project-gemini-comfyui-workflow.md
tags:
  - community
  - comfyui
  - gemini
  - workflow
  - prompt-engineering
  - ltx-video
---

# Gemini ComfyUI Workflow

Community member **rubanraja** created a free [[comfyui-ltx-integration-overview|ComfyUI]] workflow that integrates Google Gemini API with [[ltx-video-overview|LTX Video]], designed to simplify video generation by using Gemini for enhanced prompt processing.

## Key Details

- **Creator:** rubanraja
- **Discussion:** #73 on Lightricks/LTX-Video (March 2025)
- **Distribution:** Free on Patreon
- **Components:** LTX-Video + Google Gemini API + ComfyUI

## How It Works

The workflow uses Google Gemini as an intermediary to process and enhance user prompts before passing them to LTX-Video. This approach aims to bridge the gap between casual user input and the detailed, descriptive prompts that LTX-Video performs best with.

## Security Concern

Community member **Burgstall** identified an exposed API key in the workflow screenshot -- the shared Patreon workflow contained a visible (though partially blurred) Gemini API key. The community recommended using external config files instead of embedding API keys directly in shared workflows.

The creator acknowledged the concern but emphasized accessibility, noting that the Gemini API is free or inexpensive and that the workflow prioritizes simplicity for use in personal setups and online ComfyUI services like ShakerLabs.

## Significance

This project demonstrates how the community is combining LTX-Video with external AI services to create more sophisticated generation pipelines. It also highlights a recurring tension between accessibility and security best practices when sharing AI workflows.

## See Also

- [[promptgen]] -- Another community approach to improving LTX-Video prompts
- [[ltx-video-overview]] -- LTX Video base model and prompt guidelines
