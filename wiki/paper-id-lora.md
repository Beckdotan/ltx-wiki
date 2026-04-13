---
title: "ID-LoRA: Identity-Driven Audio-Video Personalization"
type: paper
created: 2026-04-13
updated: 2026-04-13
sources:
  - raw/paper-id-lora-arxiv-2603.10256.md
tags:
  - paper
  - id-lora
  - personalization
  - identity
  - lora
  - lightricks
  - tel-aviv-university
---

# ID-LoRA: Identity-Driven Audio-Video Personalization with In-Context LoRA

**Authors:** Amit Dahan, Maya Yanuka, Niv Kraicer, Lior Wolf, Raja Giryes
**Affiliations:** Tel Aviv University, [[lightricks-research-overview|Lightricks]] (collaboration)
**Date:** March 2026
**arXiv:** [2603.10256](https://arxiv.org/abs/2603.10256)

## Summary

ID-LoRA extends the [[paper-avcontrol|AVControl]] framework to identity-driven audio-video personalization. It uses the parallel canvas LoRA approach to enable identity preservation in generated audiovisual content -- generating videos that preserve a specific person's appearance and voice characteristics.

## Significance

- Extends the AVControl framework to identity-driven personalization
- Demonstrates that the [[paper-ltx-2|LTX-2]]/AVControl ecosystem can be adapted for person-specific generation
- Addresses one of the noted capability gaps of AVControl (identity preservation, which was listed as a limitation relative to VACE)
- Uses the In-Context LoRA (IC-LoRA) paradigm from the LTX-2 model family

## Relationship to LTX Ecosystem

ID-LoRA is one of three concurrent works (alongside [[paper-just-dub-it|JUST-DUB-IT]] and In-Context Sync-LoRA) that adopted the [[paper-avcontrol|AVControl]] framework. It represents a collaboration between Tel Aviv University and Lightricks, continuing the academic partnership also seen in JUST-DUB-IT. The work builds on the frozen [[paper-ltx-2|LTX-2]] backbone and demonstrates that the modular LoRA approach generalizes to identity-preserving generation tasks.
