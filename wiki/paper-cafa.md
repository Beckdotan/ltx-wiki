---
title: "CAFA: A Controllable Automatic Foley Artist"
type: paper
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/paper-cafa-arxiv-2504.06778.md
tags:
  - paper
  - cafa
  - foley
  - video-to-audio
  - lightricks
---

# CAFA: A Controllable Automatic Foley Artist

**Authors:** Roi Benita, Michael Finkelson, Tavi Halperin, Gilad Sterkin (Lightricks); Yossi Adi (Hebrew University of Jerusalem)
**Affiliations:** [[lightricks-research-overview|Lightricks]], Hebrew University of Jerusalem
**Date:** April 2025
**arXiv:** [2504.06778](https://arxiv.org/abs/2504.06778)

## Summary

CAFA addresses the task of controllable automatic foley generation -- producing synchronized audio tracks for video content. The paper presents a controllable approach to video-to-audio (V2A) generation, focusing on temporal dynamics and foley realism. It represents Lightricks' research in dedicated video-to-audio generation before the unified joint approach taken in [[paper-ltx-2|LTX-2]].

## Benchmark Performance

From the [[paper-avcontrol|AVControl]] paper's comparative evaluation (TV2A configuration):

| Metric | CAFA |
|--------|------|
| FAD (PANNS) | 12.60 |
| KL (PANNS) | 2.02 |
| IS (PANNS) | 13.45 |
| IB (ImageBind A/V similarity) | 0.21 |
| Training | 200K steps |

Compared against ReWaS and MMAudio in audio generation benchmarks.

## Significance to LTX Ecosystem

- Represents Lightricks' dedicated research in video-to-audio generation
- Preceded the joint audio-visual approach in [[paper-ltx-2|LTX-2]]
- Key authors (Benita, Finkelson, Halperin) also contributed to LTX-2 and [[paper-avcontrol|AVControl]]
- The audio intensity control LoRA in AVControl addresses the same task as CAFA but via a unified model approach
- An important stepping stone in Lightricks' journey from separate V2A models to the unified audiovisual generation in LTX-2
