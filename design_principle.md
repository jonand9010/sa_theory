# Design Principles of the Situation Awareness Operator

This document explains the architectural principles behind the separation of situation awareness into perception, comprehension, and projection.

These principles motivate the design. They do not establish correctness, robustness, or safety by themselves.

## 1. Separation of Responsibilities

The framework assigns three distinct responsibilities:

- Perception detects semantic concepts from observations.
- Comprehension constructs a belief over internal situations.
- Projection exposes a representation relevant to a specified goal.

The canonical interfaces are:

```text
P    : Y^{≤t} → Z^{≤t}
C    : Z^{≤t} → Prob(S)
R_g  : Prob(S) → U_g

SA_g = R_g ∘ C ∘ P
```

This separation supports distinct questions about detection quality, situation inference, and information relevance.

It is a functional decomposition, not a requirement for three separate software modules.

## 2. Evidence Before Preference

Perception and comprehension are descriptive. They represent what has been detected and what situations the evidence supports.

Projection introduces goal-dependent relevance by selecting distinctions or constructing summaries.

Consequences may affect which quantities are exposed, but they do not change the underlying belief probabilities. Expected severity and other consequence-weighted quantities must be identified as scores rather than updated probabilities.

## 3. Goal Locality

In the reference architecture, the goal parameter appears only in projection.

Perception and comprehension are held fixed across the goals being considered. This permits several consumers to use different representations of the same situation belief.

Goal locality is a design assumption. Active sensing, goal-dependent attention, and task-specific inference may require an extended architecture.

Reuse is appropriate only when upstream representations retain the information each goal requires.

## 4. Explicit Abstraction

Every reduction of information should have a declared meaning.

For projection, this means specifying:

- the input belief
- the output space
- the mapping or summary rule
- the distinctions discarded
- the uncertainty retained or omitted
- the intended downstream use

An explicit mapping makes abstraction reviewable. It does not automatically make the abstraction adequate.

## 5. Explicit Uncertainty

Comprehension produces a belief in `Prob(S)`.

Projection may preserve a distribution over abstract states or expose a summary such as a probability, score, margin, bound, or category.

These outputs carry different information. A hard label may omit uncertainty; an expected value may hide consequential tails; separate marginal summaries may omit dependencies.

The output contract must explain those limitations.

## 6. Consumer-Relative Adequacy

A projection is adequate only relative to a specified use.

A useful check is to identify beliefs that produce the same projected output. If the consumer needs to distinguish those beliefs, the projection discards necessary information.

Compactness alone is not a measure of projection quality.

## 7. Inspectable Interfaces

Named interfaces allow intermediate representations to be logged, tested, and compared.

Inspectability depends on meaningful schemas and validation evidence. Naming an internal tensor or assigning it an operator symbol does not establish that it represents concepts, beliefs, or relevance.

Fused and jointly learned implementations can use the framework when their claimed interfaces have identifiable meanings.

## 8. Separation from Decision-Making

Situation awareness prepares information for downstream use.

Action selection, planning, control, and operational mode activation remain downstream. Projection may expose mode conditions or eligibility, but it does not choose or activate a mode.

This boundary keeps belief construction, relevance, and behavior distinct.

## 9. Validation over Architectural Guarantees

The decomposition can organize validation, but does not guarantee:

- accurate detection
- calibrated beliefs
- sufficient projection
- stable outputs
- robust operation
- safe decisions

Those properties require evidence appropriate to the model, consumer, and operating domain.

## 10. Summary

The framework makes detection, belief construction, and relevance explicit.

Its architectural value lies in providing reviewable responsibilities and contracts. Its practical value depends on whether those contracts are implemented and validated.