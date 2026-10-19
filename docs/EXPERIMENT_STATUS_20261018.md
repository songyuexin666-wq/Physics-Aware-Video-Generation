# Experiment status — 2026-10-18

These results were reported in the researcher's latest experiment summary. They are not independently reproduced by the contents of this public repository.

## Current latest K3-SG + P-CSA result

- 4 independent unseen bouncing cases × 3 generation seeds (42/43/44).
- Three compared conditions, 36 technical runs, zero reported run errors.
- Relative to legacy D1: physics recovery +0.7782; trajectory RMSE improvement +0.2300; velocity improvement +0.2455; acceleration improvement +0.2964.
- Relative gains in target-mask IoU +0.4713, canonical-shape IoU +0.1429, target appearance cosine +0.0457, and background MAE 0.000422 lower.
- 4/4 case mean improvements; exact two-sided sign test p=0.125 at independent case level, **not significant** by conventional cutoffs.
- Contact timing remains imperfect (00008 second predicted contact 16 vs reference 13).

## Input contract limitation

The K3-SG predictive artifact used source-scene physical/geometry metadata, not geometry inferred solely from source pixels. It is `research_prediction_diagnostic=true`, `trajectory_inference_eligible=false`, `deployment_eligible=false`. No target GT video/trajectory used for trajectory construction according to the supplied audit.

**Do not label this result as video-only deployment or official PCVE-RigidBench.**

## Earlier experiments

| Experiment | Outcome | Meaning |
|---|---|---|
| R1 vs R0/R2 | Outside decoded MAE ~0.0009 vs ~0.117 | Reference projection helps preserve non-causal regions |
| D1 Oracle occupancy transport | Low physics fidelity despite target coverage | Absolute latent relocation is not sufficient |
| U1 GT latent / C2 proxy ceiling | Strong video control | Oracle-capability ceiling |
| F1 fixed persistent anchor | 15/15 success across 5 cases×3 seeds, recovery ~0.9503 | Persistent guidance useful when physical targets are correct, oracle diagnostic |
| K0 | ADE 0.7200 / FDE 1.1194 | Polynomial rollout fails after events |
| K1-A | ADE 0.2911 / FDE 0.7136 in internal repair | Longer CV fit not enough |
| K1-B | Worse screen-gravity prediction | Negative ablation |
| K2 observability | 1 SINGLE / 6 MULTI / 8 ABSTAIN; single case long-term drift | Model set and contact physics inadequate |
| K3-SG+P-CSA | Four unseen bounce cases show consistent improvement | Metadata-assisted event dynamics evidence |

## Required next evidence

1. Absolute success counts and full per-seed report, including appearance and mask thresholds.
2. Full no-edit, masked regen, and D1 matched controls under one tracker revision.
3. Independent non-bouncing families and video-only state estimator.
4. Output full-length videos and score using the official PCVE evaluator.
5. Scene-family-level uncertainty intervals once more than four independent scenes are available.

This document is an **evidence ledger**, not a trained model release.
