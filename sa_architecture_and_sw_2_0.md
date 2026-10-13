# End-to-End Learning and Modular Situation Awareness

This document compares architectures with explicit situation-awareness interfaces to architectures that learn an observation-to-action mapping without requiring those interfaces.

The comparison concerns semantic contracts and validation. It does not assume that either architecture is universally superior.

## 1. Two Architectural Choices

An observation-to-action system can be represented schematically as:

```text
F_g : Y^{≤t} → A
```

where `A` is an action space.

The SA framework instead defines an explicit representation pipeline:

```text
P    : Y^{≤t} → Z^{≤t}
C    : Z^{≤t} → Prob(S)
R_g  : Prob(S) → U_g

SA_g = R_g ∘ C ∘ P
```

A simplified downstream policy interface is:

```text
π_g : U_g → A
```

A real policy may require additional inputs or internal state. This schematic interface does not imply that every valid policy is memoryless.

## 2. Learning and Modularity Are Separate Choices

Each SA operator may be implemented with learned, analytical, rule-based, or hybrid methods.

Explicit interfaces do not prevent joint training. Joint training does not prevent explicit interfaces.

The architectural question is whether intermediate representations have declared meanings and independently assessable contracts.

## 3. Explicit Semantic Contracts

An explicit SA design declares:

- which semantic records perception produces
- which situations comprehension represents
- how belief uncertainty is interpreted
- how projection constructs goal-relevant outputs

These contracts allow targeted logging and validation.

Their existence does not guarantee that an implementation satisfies them. A tensor named “belief” is not sufficient evidence that it is a calibrated probability distribution.

## 4. Implicit Representations

A learned observation-to-action model may contain useful internal information without exposing stable semantic interfaces.

Different internal representations can support similar external behavior. Consequently, successful action prediction alone does not establish that a particular internal variable represents a concept, situation belief, or goal-relevant quantity.

Probes and auxiliary objectives may help investigate such representations. Their results require validation, especially after retraining or changes in operating conditions.

## 5. Information Bottlenecks

Explicit interfaces can make information loss visible, but may also constrain performance when their schemas omit useful evidence.

An observation-to-action architecture can avoid some predefined bottlenecks, while making the relevant internal information harder to inspect.

Neither architecture eliminates abstraction. The trade-off concerns where information is compressed, which semantics are declared, and how those choices are evaluated.

## 6. Goal Dependence

This repository's reference architecture holds `P` and `C` fixed across goals and localizes goal dependence in `R_g`.

Changing goals can reuse upstream components when their representations support the new consumer requirements.

An architecture using goal-conditioned sensing or inference requires an explicit extension of that assumption.

Observation-to-action models may also accept goals as inputs. Whether a goal change requires retraining depends on the implementation.

## 7. Uncertainty and Robustness

Explicit SA interfaces can expose uncertainty at selected boundaries.

However:

- detector confidence may not be calibrated
- situation beliefs may omit hypotheses
- projections may discard consequential uncertainty
- consumers may misuse otherwise valid outputs

Robustness requires assessment in either architecture. Modularity, joint training, or model scale alone does not establish it.

## 8. Validation and Assurance

Explicit interfaces support component-level questions:

| Component | Example validation question |
| --- | --- |
| Perception | Are required concepts detected under relevant conditions? |
| Comprehension | Are situation probabilities meaningful and calibrated? |
| Projection | Does the output retain information required by its consumer? |
| Policy | Does the consumer use the output appropriately? |

End-to-end assessment remains necessary because individually acceptable components can interact poorly.

Architectures without explicit SA contracts can still be evaluated through task performance, scenario coverage, runtime monitoring, and other evidence. The available assurance depends on the claims being made and the evidence supporting them.

## 9. Hybrid Designs

Hybrid designs can retain explicit interfaces where inspection, coordination, or uncertainty handling is important while learning other transformations jointly.

Examples include:

- learned perception with probabilistic comprehension
- jointly trained perception and comprehension with a declared belief output
- learned belief summaries with explicit projection contracts

A learned projection must declare whether it approximates an event probability, expected quantity, category mapping, or another summary.

## 10. Summary

The relevant distinction is not neural versus non-neural implementation.

It is whether concept detection, belief construction, and goal-dependent representation are explicit objects of design and validation.

The SA framework supplies contracts for those responsibilities. Architecture selection still depends on information requirements, implementation constraints, and validation evidence.