# PCCA — Physics-Conditioned Counterfactual Video Repair

**Research prototype | Event-grounded counterfactual dynamics + persistent Wan Flow state anchoring**

> **Status (2026-10-18):** The physics-prediction-to-video-execution mechanism has positive *metadata-assisted* research evidence. The production RigidBench/Wan2.2 code, full dataset, and checkpoints have **not yet** been publicly released. The earlier PCCA learned-adapter handoff did not pass. This README is a research status and reproducibility index, not a claim that the public repository alone reproduces the GPU study.

## Problem

Given a video containing a local physical failure and a physical intervention, predict a physically consistent alternative trajectory and regenerate **only the causally affected spacetime region**, preserving background, first frame, identity and unaffected interactions.

```
source video + permitted intervention
  └─ source-grounded state / contact geometry
       └─ K3-SG event-based counterfactual rollout
            └─ predicted state / proxy video / causal support
                 └─ P-CSA persistent state anchoring in Wan Flow
                      └─ R1 reference outside causal support
                           └─ repaired local physical video
```

## Current evidence: four held-out bouncing cases

Four distinct cases, seeds 42/43/44, three conditions, 36 runs without execution errors were reported in the latest internal validation.

| Paired P-CSA advantage over old D1 | Mean direction | Case means |
| --- | ---: | --- |
| Physics recovery | **+0.7782** | 4/4 improved |
| Trajectory RMSE improvement | +0.2300 | 4/4 |
| Velocity RMSE improvement | +0.2455 | 4/4 |
| Acceleration RMSE improvement | +0.2964 | 4/4 |
| Target mask IoU | **+0.4713** | 4/4 |
| Canonical shape IoU | +0.1429 | 4/4 |
| Target appearance cosine | +0.0457 | 4/4 |
| Background MAE | **0.000422 lower** | 4/4 |

**Critical limitations:** Only **four independent physical cases**; multiple generation seeds are nested repeats. All four positive directions imply a two-sided exact sign-test p=0.125, not a statistically significant generalization claim. K3-SG used permitted **source-scene geometry / physical metadata**, not source-video-only estimated geometry; its artifact was marked `research_prediction_diagnostic=true`, `deployment_eligible=false`. The numbers are **not** official VideoPhysEdit / PCVE PES.

The earlier fixed F1 state-anchor capability ceiling achieved 15/15 success under oracle target-state conditions. It is **not** a deployable input setting. The newer evidence supports persistent denoising anchoring over simply transplanting absolute VAE latents; source/proxy initialization alone did not provide a meaningful increment under fixed anchoring.

## Research boundaries

- `K1-A`: linear observed-prefix baseline, window-8 selected by separate development data; it cannot model contact and bounce events.
- `K1-B`: screen-space gravitational acceleration performed worse and is preserved as a negative ablation.
- `K2`: multi-window observability gate found 1 SINGLE / 6 MULTI / 8 ABSTAIN in readout15; the only SINGLE still had poor long-horizon accuracy.
- `K3-SG`: contact/support-grounded dynamics — current promising research path, but source-scene geometry provenance must be explicit.
- `P-CSA`: fixed denoising-step state anchoring + causal support and R1 protection — positive conditional generation evidence.
- `PCCA learned adapter`: historical mapping and video handoff gates **not passed**; do not claim the adapter is trained and validated.

## Next experiments

1. Freeze K3-SG, P-CSA and SAM2 evaluation revisions. Audit **absolute** success counts on all case/seed combinations, not just directional improvement.
2. Run matched `masked_regen`, no-edit and D1 controls on the same cases and seeds. Preserve full failures.
3. Evaluate unseen free-fall and rolling families; track second-contact event error and identity persistence.
4. Build **video-only** support-geometry and state estimates; measure loss relative to source-metadata-assisted K3-SG.
5. Evaluate frozen K3 outputs with frozen P-CSA on independent scene families.
6. Port full-length output to [PCVE-RigidBench](https://github.com/Hammour-steak/PCVE-RigidBench) and run its **official** evaluator. SET-subset results cannot replace the whole 129-case benchmark.
7. Release a **separate** CausalStateBench dataset/evaluator with Observation-Only, Predicted-State and Oracle-State tracks (never mixed into a single leaderboard).

For experiment details see [Evidence Ledger](docs/EXPERIMENT_STATUS_20261018.md).

## Related work

- [VideoPhysEdit](https://github.com/Hammour-steak/VideoPhysEdit) — reconstruct, simulate and edit rigid-body scenes.
- [PCVE-RigidBench](https://github.com/Hammour-steak/PCVE-RigidBench) — paired edits, official physics/video metrics.
- [PhysMind](https://github.com/ccyydd/PhysMind) — video to executable world reasoning.
- [Wan-Move](https://github.com/ali-vilab/Wan-Move) — latent trajectory guidance.
- [DiffSynth-Studio](https://github.com/modelscope/DiffSynth-Studio) — video generation runtime.

## Code & data release note

The historical GitHub README described an earlier latent-only stage and is now superseded by this dated status. The actual private RigidBench runtime and video data are **not** being represented as present in this repository. Source-only K3 CPU examples, benchmark tooling, trained weights and licenses require separate verified releases. Please do not use internal oracle diagnostic results as comparable external benchmark scores.
