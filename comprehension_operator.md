# The Comprehension Operator in Situation Awareness Systems

Version: 1.3  
Status: Technical specification

## 1. Role

Comprehension answers:

**What situation might be occurring, given what has been detected?**

It integrates semantic records into a belief over internal situations. These situations may describe operating conditions, latent faults, relationships, or explanatory hypotheses.

In the reference architecture, comprehension is descriptive and held fixed across goals. It does not rank hypotheses according to preference, determine their operational importance, or select actions.

A severe situation may be unlikely, and a desirable situation may be unlikely. Consequences and preferences do not determine belief probabilities.

## 2. Formal Interface

Using the conventions in [notations.md](notations.md):

```text
C : Z^{≤t} → Prob(S)

b_t = C(z_{≤t})
```

Here:

- `Z` is the semantic concept record space.
- `z_{≤t}` is the concept history available at time `t`.
- `S` is the internal situation space.
- `Prob(S)` is the space of probability measures over `S`.
- `b_t` is the current situation belief.

The output matches the input of projection:

```text
R_g : Prob(S) → U_g
```

Comprehension therefore supplies a belief, while projection determines how that belief is represented for a particular goal.

## 3. The Internal Situation Space

The situation space specifies the alternatives and distinctions the model can represent.

Its elements may include:

- operating modes or regimes
- combinations of fault conditions
- latent physical quantities
- explanatory hypotheses
- relationships between entities
- modeled future outcomes

The situation space need not reproduce the complete physical state. It must retain the distinctions needed for the intended downstream uses.

For a finite categorical situation space, states must be mutually exclusive and collectively exhaustive within the modeled domain.

For example:

```text
S = {
    normal_operation,
    overheating_without_fire,
    fire_present
}
```

These labels define alternatives. If multiple faults can coexist, the model must represent their combinations or use another suitable structure.

Marginal probabilities of overlapping faults are not a categorical distribution and need not sum to one.

## 4. Belief Construction

A probabilistic model may express comprehension as:

```text
b_t(A) = Pr(s_t ∈ A | z_{≤t}, M)
```

where:

- `A` is a measurable set of situations.
- `M` denotes the model and its assumptions.
- `b_t(A)` is the probability assigned to those situations.

Comprehension may use probabilistic models, learned inference, rules, or hybrid methods. Whatever the implementation, the reference interface requires a valid probability measure with a documented interpretation.

An arbitrary collection of confidence scores is not automatically a situation belief. Any conversion from scores to probabilities requires justification and validation.

A point estimate can be encoded as a Dirac measure:

```text
b_t = δ_ŝ_t
```

This assigns probability one to the estimated situation `ŝ_t`. It makes the interface compatible but does not establish that the estimate is certain or that uncertainty has been adequately represented.

## 5. Temporal Integration

Comprehension may use concept history to distinguish persistent conditions, transient events, and changing hypotheses.

A recursive implementation can retain an internal state:

```text
h_t = F(h_{t-1}, z_t)
b_t = H(h_t)
```

Here, `h_t` contains the information retained for subsequent inference. It may be the belief itself or a richer representation.

The history-level interface does not require storing every record. However, the retained state must support the inference method and its assumptions.

The implementation should specify how it handles:

- delayed or out-of-order records
- missing observations
- revisions to earlier detections
- conflicting evidence
- changes in operating conditions

## 6. Explanation and Prediction

Comprehension can organize observations through explanatory hypotheses. However, a situation belief does not automatically establish causality.

A model that distinguishes operating regimes may provide useful comprehension without identifying why a regime occurred. Causal interpretations require additional assumptions and evidence.

Prediction belongs to comprehension when future outcomes are explicitly represented in `S`. Predictive outputs must specify their horizon and relevant assumptions, including assumed future inputs or actions.

Projection then selects or summarizes those modeled outcomes according to relevance. It does not introduce new predictive evidence or a dynamics model merely by changing the goal.

## 7. Uncertainty and Model Limits

A belief represents uncertainty within a specified model. It does not necessarily capture every source of uncertainty.

Separate assessment is needed for:

- probability calibration
- omitted situations or hypotheses
- model misspecification
- unreliable perception outputs
- distribution shift
- uncertainty in model parameters

Writing a probability distribution does not ensure that its probabilities are accurate.

Likewise, uncertainty cannot always be resolved by collecting more observations. Different situations may remain indistinguishable under the available evidence.

## 8. Relationship to Projection

Comprehension produces the belief:

```text
b_t = C(z_{≤t})
```

Projection exposes a goal-relevant representation:

```text
u_{g,t} = R_g(b_t)
```

Different goals can use the same belief without changing its probabilities.

For example:

```text
Pr(normal_operation) = 0.60
Pr(overheating_without_fire) = 0.30
Pr(fire_present) = 0.10
```

A safety projection may expose the fire probability. An equipment-condition projection may expose the probability of abnormal operation:

```text
Pr(fire_present) = 0.10
Pr(abnormal_operation) = 0.40
```

Both outputs derive from the same belief. Their interpretation and downstream use differ.

Reusing comprehension for a new goal requires checking that the situation space and belief retain the necessary distinctions. Projection cannot recover distinctions the model does not represent.

## 9. Boundary of Responsibility

Comprehension is responsible for constructing and maintaining a situation belief from detected concepts.

It is not responsible for:

- changing belief probabilities according to desirability
- deciding which distinctions matter for a goal
- selecting actions or operational modes
- resolving trade-offs between objectives

These responsibilities belong to projection or downstream decision-making, as appropriate.

## 10. Review Questions

When specifying or evaluating comprehension, ask:

- What does each situation or hypothesis mean?
- Which alternatives and combinations are excluded?
- What assumptions connect detected concepts to situations?
- How are temporal information and conflicting evidence handled?
- What supports the interpretation and calibration of the probabilities?
- Are explanatory claims descriptive, predictive, or causal?
- Which downstream distinctions can the belief support?

## 11. Summary

Comprehension transforms concept history into a situation belief:

```text
C : Z^{≤t} → Prob(S)
```

It supplies the descriptive basis for goal-dependent projection. Its explicit belief interface supports inspection of uncertainty, but correctness, calibration, and adequacy require separate validation.