# Community Project: LTX + Gemini ComfyUI Workflow

**Source:** https://huggingface.co/Lightricks/LTX-Video/discussions/73

---

## Overview

Community member **rubanraja** created a free ComfyUI workflow that integrates Google Gemini API with LTX-Video, designed to simplify video generation for creators of all levels.

## Details

- **Creator:** rubanraja
- **Discussion:** #73 on Lightricks/LTX-Video (March 2025)
- **Comments:** 5
- **Distribution:** Free on Patreon (https://www.patreon.com/posts/ltx-gemini-123897759)

## Integration Components

- **Video Model:** LTX-Video (Lightricks)
- **AI Service:** Google Gemini API
- **Platform:** ComfyUI
- **Purpose:** Unlocking the full potential of LTX-Video by integrating Gemini for enhanced prompt processing

## Community Response

### Security Concern
Community member **Burgstall** identified an exposed API key in the workflow screenshot. The Patreon workflow contained a visible (though partially blurred) Gemini API key.

### Best Practice Recommendation
The community recommended using external config files instead of embedding API keys directly in shared workflows.

### Creator Response
rubanraja acknowledged the security concern but emphasized accessibility, noting:
- The Gemini API is free or inexpensive
- The workflow prioritizes simplicity over custom configuration
- Works in personal setups and online ComfyUI services like ShakerLabs

## Significance

This project demonstrates how the community is combining LTX-Video with other AI services (Gemini) to create more sophisticated video generation pipelines. It also highlights the tension between accessibility and security best practices in shared AI workflows.
