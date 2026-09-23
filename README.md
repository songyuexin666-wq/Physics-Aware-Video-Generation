# Physics-Aware Video Generation

<p align="center">
  <b>State-Conditioned Control and Localized Regeneration for Physically Consistent Video Generation</b>
</p>

This repository documents an ongoing research project on **physics-aware controllable video generation**. The central goal is to move beyond prompt-only generation by introducing explicit physical state representations and intervention signals that can guide a frozen video diffusion model toward more physically consistent transitions.

> **Current research stage:** latent-level control consistency and intervention-direction learning have been implemented and audited. Stable video-level bidirectional intervention mapping and localized regeneration are the next major milestones.

---

## Motivation

Long-form and action-conditioned video generation often suffers from **open-loop drift**:

- object motion gradually deviates from the intended physical evolution;
- control directions become entangled across different actions;
- global regeneration may alter visually unrelated regions;
- physically implausible transitions accumulate over time.

Instead of relying only on text prompts, this project explicitly models:

```text
S_t + A_t → S_{t+1}
```

where:

- `S_t` is the current physical state,
- `A_t` is an intervention or action,
- `S_{t+1}` is the desired next state.

The long-term objective is to use these states to guide **localized, causally targeted video regeneration**.

---

## Core Research Idea

The project is built around two main ideas.

### 1. Physics-State-Guided Intervention

A structured physical state is extracted from the current video and converted into a representation that can control a frozen video generation model.

The intended control path is:

```text
Video / Frame
    │
    ▼
State Extractor
    │
    ▼
Physical State S_t
    │
    + Action A_t
    ▼
State Compiler
    │
    ▼
Control Semantics
    │
    ▼
Hidden-State / Latent Intervention
    │
    ▼
Video Diffusion Model
```

Rather than retraining the whole generator, the current design focuses on learning a **control direction in latent / hidden-state space**.

### 2. Locality-Aware Regeneration

Physical interventions should affect only the causally relevant region.

The target pipeline is:

```text
State transition
    │
    ▼
Causal influence estimation
    │
    ▼
Affected-region mask
    │
    ▼
Localized latent intervention
    │
    ▼
Partial video regeneration
```

This is intended to preserve:

- background consistency,
- object identity,
- unaffected trajectories,
- spatial locality.

---

## System Pipeline

The current research pipeline contains four main components:

### State Extractor

Extracts the physical state from a frame or short video segment.

Typical state variables may include:

- object position,
- velocity,
- motion direction,
- contact / support state,
- relative geometry,
- task-specific physical attributes.

### Drift Checker

Measures whether the generated transition deviates from the desired physical evolution.

A drift-aware decision layer determines whether the current output should be:

- accepted,
- corrected,
- regenerated.

### State Compiler

Converts the physical state and intervention into model-usable control information.

Conceptually:

```text
physical state
      +
intervention
      ↓
continuity semantics
      +
control semantics
      +
optional keyframe constraint
```

### Pilot Runner

Executes the control experiment and records intervention behavior, including control direction, no-op stability, and mapping quality.

---

## PCCA: Physical Control Consistency Alignment

The current implemented research stage focuses on learning whether different interventions correspond to distinguishable latent control directions.

The expected mapping is conceptually:

```text
intervention_i
     ↓
control direction_i
     ↓
hidden-state change_i
```

A good mapping should satisfy:

1. **Control-direction consistency**  
   The intended intervention should produce a predictable latent direction.

2. **No-op stability**  
   A no-op intervention should not introduce unnecessary changes.

3. **Intervention separability**  
   Different interventions should produce distinguishable mappings.

4. **Diagonal ownership**  
   Each intervention should primarily activate its own intended direction rather than another action's direction.

---

## Current Validation Gates

The current implementation uses multiple audit gates rather than a single accuracy number.

Representative checks include:

| Gate | Purpose |
| --- | --- |
| Control-direction gate | Does an intervention move the latent representation in the intended direction? |
| No-op gate | Does the model remain stable when no intervention is applied? |
| Ownership / diagonal test | Does each intervention primarily control its own direction? |
| Row margin | Is the correct mapping preferred over alternatives? |
| Column selectivity | Is each control direction selectively associated with the intended intervention? |
| Locality | Does the intervention avoid changing unrelated regions? |

A typical ownership requirement is:

```text
diagonal fraction >= 0.8
```

Current experiments have already shown that **control direction and no-op behavior can pass while intervention ownership remains insufficient**, which motivates the next stage of research.

---

## Current Research Status

The project is currently between **latent-control validation** and **video-level intervention handoff**.

### Completed / implemented

- physical-state abstraction;
- action-conditioned state transition formulation;
- latent / hidden-state intervention path;
- control-direction validation;
- no-op validation;
- ownership and selectivity diagnostics;
- drift-aware evaluation logic;
- synthetic physical-video data generation strategy.

### In progress

- stable bidirectional intervention mapping;
- causal affected-region estimation;
- video-level localized regeneration;
- trajectory preservation;
- identity preservation;
- locality-aware evaluation.

---

## Data Strategy

The project primarily uses **synthetic physics-controlled videos** generated with tools such as Blender / PyBullet-style simulation pipelines.

Synthetic data is preferred for supervision because the underlying physical state and intervention are explicitly known.

Each sample can conceptually contain:

```text
video
state_before
action
state_after
affected_region
counterfactual / alternative action
```

Real videos are intended mainly for **generalization evaluation** after control learning is stable.

---

## Counterfactual Design

Counterfactual supervision is constructed by changing the action while keeping the initial state as fixed as possible.

Example:

```text
same S_t
 ├── A_1 → S_{t+1}^{(1)}
 ├── A_2 → S_{t+1}^{(2)}
 └── no-op → S_{t+1}^{(0)}
```

This provides supervision for learning whether different interventions create distinct and physically meaningful control directions.

---

## Evaluation

The final video-level evaluation is designed around four groups of metrics:

### Physical Consistency

- state-transition error
- trajectory deviation
- drift accumulation

### Control

- action-to-state consistency
- intervention-direction separability
- bidirectional mapping consistency

### Locality

- changed pixels inside target region
- changed pixels outside target region
- locality ratio

### Visual Preservation

- identity consistency
- background preservation
- temporal continuity

---

## Planned Repository Structure

```text
Physics-Aware-Video-Generation/
├── state_extractor/
├── drift_checker/
├── state_compiler/
├── pcca/
├── regeneration/
├── evaluation/
├── configs/
├── scripts/
├── assets/
└── README.md
```

The code will be organized around the above modules as the public release matures.

---

## Research Goal

The project aims to progress from:

```text
latent-level control consistency
```

to:

```text
stable video-level bidirectional intervention mapping
                +
locality-aware regeneration
```

The final goal is a video generation system in which **physical interventions are controllable, interpretable, and spatially localized**.

---

## Status

**Research in progress.**

This repository currently serves as the project page and will be expanded with code, experiments, visualizations, and checkpoints as the implementation is consolidated.
