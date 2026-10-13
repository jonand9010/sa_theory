# Situation Awareness Operator Framework

This framework describes how engineered systems transform observations into goal-relevant representations.

```text
SA_g = R_g ∘ C ∘ P
```

- **Perception:** what has been observed or detected?
- **Comprehension:** what situation might be occurring?
- **Projection:** which representation matters for this goal?

## Canonical Interfaces

```text
P    : Y^{≤t} → Z^{≤t}
C    : Z^{≤t} → Prob(S)
R_g  : Prob(S) → U_g
SA_g : Y^{≤t} → U_g
```

Comprehension produces a situation belief. Projection exposes a distribution or another explicitly typed representation for downstream use.

## Main Principles

- Detection, inference, and relevance have separate responsibilities.
- Goals affect projection in the reference architecture.
- Reusing upstream components requires adequate retained information.
- Projection does not preference-reweight the underlying belief.
- Output contracts declare information loss and uncertainty handling.
- Action selection remains downstream.

The decomposition supports inspection and validation. It does not guarantee correctness, robustness, or safety.

## Start Here

- [Overview and repository map](README.md)
- [Framework definition](sa_operator.md)
- [Shared notation](notations.md)
- [System analysis guidelines](analysis_guidelines.md)

## Operator Specifications

- [Perception](perception_operator.md)
- [Comprehension](comprehension_operator.md)
- [Projection](projection_operator.md)

## Design and Scope

- [Projection patterns](projection_patterns.md)
- [Design principles](design_principle.md)
- [End-to-end learning and modular SA](sa_architecture_and_sw_2_0.md)
- [Limitations](limitations.md)

## Terminology

In this repository, *projection* means goal-conditioned abstraction or summarization. It does not necessarily mean prediction.

See [notations.md](notations.md) for the authoritative conventions.