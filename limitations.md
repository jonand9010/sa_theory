# Limitations and Scope

This document states the boundaries and assumptions of the Situation Awareness framework.

## 1. Representation, Not Decision-Making

The framework transforms observations into goal-relevant representations:

```text
SA_g = R_g ∘ C ∘ P
```

It does not select actions, perform planning, execute control, or activate operational modes.

Those responsibilities remain downstream.

## 2. Goal Locality Is an Assumption

The reference architecture holds perception and comprehension fixed across goals. Only projection is parameterized by `g`.

This assumption supports reuse but does not cover every system. Active sensing, task-dependent feature extraction, and goal-conditioned inference may require extensions.

Reuse requires checking that upstream representations support the goals being considered.

## 3. No Universal Situation Space

The framework does not define a universal `S`.

Domain-specific choices determine which situations, fault combinations, relationships, and future outcomes can be represented.

An omitted distinction cannot be recovered by projection alone.

## 4. Modeled Uncertainty Is Incomplete

Comprehension produces a belief in `Prob(S)`, but that belief need not capture every source of uncertainty.

Missing hypotheses, model misspecification, parameter uncertainty, and distribution shift require separate assessment.

A point estimate encoded as a Dirac measure does not establish certainty.

## 5. Projection Can Discard Uncertainty

Projection outputs may be distributions, probabilities, scores, margins, bounds, categories, or records.

Some outputs omit uncertainty or dependencies. Their contracts must state what is retained and discarded.

Preference-weighted scores must not be presented as updated probabilities.

## 6. Adequacy Is Consumer-Relative

The framework does not automatically determine whether a projection is sufficient.

Adequacy depends on the downstream task, required distinctions, and operating conditions.

Counterexamples involving beliefs with identical projected outputs can reveal information loss. Passing a finite collection of such checks does not prove universal sufficiency.

## 7. No Automatic Performance Guarantees

The architecture does not guarantee:

- accurate perception
- correct situation models
- calibrated probabilities
- stable or robust projections
- optimal decisions
- safe behavior

These properties require suitable validation evidence.

## 8. Goals and Conflicts Remain External

The framework does not prescribe how goals are selected or how competing objectives are reconciled.

Several projections may expose different information from the same belief. Resolving trade-offs and selecting actions remain downstream responsibilities.

## 9. Projection Is Not Necessarily Forecasting

Here, projection means goal-conditioned abstraction or summarization.

When future outcomes are included, their predictive model belongs to comprehension. Projection selects or summarizes those outcomes and must retain their relevant assumptions.

## 10. Not a Human Cognitive Model

The framework concerns engineered systems.

It does not model human cognitive processes, attention limits, or biases. Its terminology should not be interpreted as a complete account of human situation awareness.

## 11. Relationship to Other Methods

State estimation, probabilistic inference, learned models, and partially observable decision methods can supply components of the architecture.

The decomposition makes a representation boundary explicit. It does not establish incompatibility with those methods or theoretical novelty.

## 12. Intended Use

The framework is intended to support:

- architectural description
- interface specification
- analysis of information loss
- comparison of representations
- organization of validation evidence

It complements domain-specific engineering and assurance methods rather than replacing them.