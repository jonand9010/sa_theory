# Situation Awareness Operator

A framework for defining and analyzing goal-relevant representations in engineered systems.

## Core Idea

Situation awareness is defined as:

```text
SA_g = R_g ∘ C ∘ P
```

The operators have distinct responsibilities:

- **Perception `P`:** detects semantic concepts from observations.
- **Comprehension `C`:** constructs a belief over internal situations.
- **Projection `R_g`:** exposes a representation relevant to a specified goal.

The canonical interfaces are:

```text
P    : Y^{≤t} → Z^{≤t}
C    : Z^{≤t} → Prob(S)
R_g  : Prob(S) → U_g
SA_g : Y^{≤t} → U_g
```

Comprehension produces a belief. Projection may expose a distribution, probability, score, margin, category, bound, or structured record.

## Why Separate These Responsibilities?

The separation makes three questions explicit:

1. What was detected?
2. What situations does the evidence support?
3. Which representation does the consumer need?

It helps distinguish detection errors, inference errors, projection inadequacy, and downstream misuse.

The decomposition is functional. It does not require three separate software modules or exclude learned implementations.

## Important Assumptions

In the reference architecture, perception and comprehension are held fixed across goals. Goal dependence is localized to projection.

This is a design assumption. Goal-dependent sensing or inference requires an extension.

Projection does not change underlying belief probabilities because an outcome is severe or desirable. Consequence-weighted quantities are scores, not updated beliefs.

Here, projection means representation according to relevance, not necessarily forecasting.

## Minimal Example

Suppose comprehension produces:

```text
normal                   0.60
overheating_without_fire  0.30
fire_present             0.10
```

A safety projection can expose:

```text
Pr(fire) = 0.10
```

An equipment-condition projection can expose:

```text
Pr(abnormal) = 0.40
```

Both derive from the same belief. They preserve different distinctions for different consumers.

A downstream policy decides what action to take.

## Documents

| Document | Purpose |
| --- | --- |
| [sa_operator.md](sa_operator.md) | Framework definition and worked example |
| [notations.md](notations.md) | Shared spaces, symbols, and interfaces |
| [perception_operator.md](perception_operator.md) | Semantic detection contract |
| [comprehension_operator.md](comprehension_operator.md) | Situation belief contract |
| [projection_operator.md](projection_operator.md) | Goal-relevant representation contract |
| [projection_patterns.md](projection_patterns.md) | Recurring projection designs |
| [design_principle.md](design_principle.md) | Architectural rationale |
| [analysis_guidelines.md](analysis_guidelines.md) | Practical system review |
| [sa_architecture_and_sw_2_0.md](sa_architecture_and_sw_2_0.md) | Explicit interfaces and end-to-end learning |
| [limitations.md](limitations.md) | Assumptions and boundaries |

## Suggested Reading Order

1. [sa_operator.md](sa_operator.md)
2. [notations.md](notations.md)
3. The three operator specifications
4. [projection_patterns.md](projection_patterns.md)
5. [analysis_guidelines.md](analysis_guidelines.md)

## Scope

This is a representational architecture and analysis framework.

It does not select actions, prescribe inference algorithms, guarantee calibration or safety, or model human cognition.

Its practical value depends on explicit contracts and evidence that the resulting representations support their intended consumers.