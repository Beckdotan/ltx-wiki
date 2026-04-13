# CAFA: A Controllable Automatic Foley Artist

## Metadata
- **Title:** CAFA: A Controllable Automatic Foley Artist
- **Authors:** Roi Benita, Michael Finkelson, Tavi Halperin, Gilad Sterkin, Yossi Adi
- **Affiliation:** Lightricks (Benita, Finkelson, Halperin, Sterkin); Hebrew University of Jerusalem (Adi)
- **Date:** April 2025 (arXiv: 2504.06778)
- **Source URL:** https://arxiv.org/abs/2504.06778

## Abstract (Based on References)

CAFA addresses the task of controllable automatic foley generation -- generating synchronized audio tracks for video content. The paper presents a controllable approach to video-to-audio generation, focusing on temporal dynamics and foley realism.

## Key Details (from AVControl paper context)

- CAFA is referenced as a video-to-audio (V2A) system in the "TV2A configuration"
- Benchmarked on metrics: FAD (Frechet Audio Distance), KL divergence, Inception Score (IS), and ImageBind audio-visual similarity (IB)
- CAFA performance benchmarks (from AVControl Table):
  - FAD (PANNS): 12.60
  - KL (PANNS): 2.02
  - IS (PANNS): 13.45
  - IB: 0.21
  - Training: 200K steps
- Compared against ReWaS and MMAudio in audio generation benchmarks

## Significance to LTX Ecosystem

- Represents Lightricks' research in video-to-audio generation
- Preceded the joint audio-visual approach in LTX-2
- Key authors (Benita, Finkelson, Halperin) also contributed to LTX-2 and AVControl
- The audio intensity control LoRA in AVControl addresses the same task as CAFA but via a unified model approach

## Notes

This paper was identified through its citation in the AVControl paper (reference [4]). Full abstract text was not directly accessible during research. The paper represents an important stepping stone in Lightricks' journey from separate V2A models to the unified audiovisual generation in LTX-2.
