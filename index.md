# Situation Awareness Operator Framework

Welcome to the Situation Awareness (SA) framework repository.

## What is this?

This repository defines **situation awareness as a structured, goal-conditioned operator** for engineered systems.

Instead of treating SA as an informal concept, we decompose it into three explicit, auditable functions:

```
SA_g = R_g ∘ C ∘ P
```

- **P (Perception)** — transforms raw observations into semantic concepts
- **C (Comprehension)** — integrates concepts into an internal situation model
- **R_g (Projection)** — reduces that model to what matters for the goal

This separation makes SA explicit, inspectable, and comparable across systems.

## Why does this matter?

Most systems treat SA implicitly. When things go wrong, it is hard to diagnose whether the failure was in sensing, interpretation, or relevance. This framework makes all three visible.

By isolating these three functions, you can:

- reason clearly about system behavior
- compare alternative designs
- diagnose failures by layer
- reuse perception and comprehension across goals
- make abstraction and relevance decisions explicit

## Getting started

**New to this framework?**
Start with the [README](README.md).

**Want the formal definition?**
Read [sa_operator.md](sa_operator.md).

**Want to apply this to a real system?**
Read [analysis_guidelines.md](analysis_guidelines.md).

## Core documents

- [README.md](README.md) — overview and repo map
- [sa_operator.md](sa_operator.md) — formal operator definition
- [design_principle.md](design_principle.md) — rationale and design principles
- [analysis_guidelines.md](analysis_guidelines.md) — how to analyze a system with this framework
- [perception_operator.md](perception_operator.md) — perception operator specification
- [comprehension_operator.md](comprehension_operator.md) — comprehension operator specification
- [projection_operator.md](projection_operator.md) — projection operator specification
- [notations.md](notations.md) — symbols and conventions
- [limitations.md](limitations.md) — scope and intentional boundaries

## Key ideas

**Separation of concerns**
Perception, comprehension, and projection have distinct responsibilities.

**Goal locality**
Goals affect SA only through projection. Perception and comprehension are reusable and goal-agnostic.

**Explicit abstraction**
All systems abstract information. This framework makes that abstraction visible and analyzable.

**Representational, not behavioral**
SA prepares information for downstream use; it does not directly decide or control.

## Suggested reading order

1. [README.md](README.md)
2. [sa_operator.md](sa_operator.md)
3. [design_principle.md](design_principle.md)
4. [analysis_guidelines.md](analysis_guidelines.md)
5. [projection_operator.md](projection_operator.md)

## What this is not

- not a control algorithm
- not a planning method
- not a model of human cognition
- not a substitute for domain-specific safety analysis

---

Version: 1.0
