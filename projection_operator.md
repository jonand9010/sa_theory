# The Projection Operator in Situation Awareness Systems

Version: 1.3
Status: Technical specification

## 1. Role

Projection answers: **Which representation of the current situation belief is relevant for this goal?** It exposes selected distinctions and summaries for downstream use.

Projection does not update the underlying belief, select actions, or introduce new observational evidence. Here, *projection* means goal-conditioned representation rather than necessarily forecasting.

## 2. Canonical Interface

Using the conventions in [notations.md](notations.md):

```text
R_g : Prob(S) → U_g
u_{g,t} = R_g(b_t)
```

The goal `g` determines the output space `U_g` and the mapping. Outputs may be distributions, event probabilities, risk scores, margins, categories, or structured records. Their types and meanings must be declared.

Perception and comprehension are held fixed across goals in the reference architecture. Goal-dependent sensing or inference requires an extension, not merely a different projection.

## 3. Distribution-Preserving Abstraction

A measurable state abstraction `φ_g : S → V_g` groups situations according to relevance. Lift it to beliefs by taking the pushforward:

```text
U_g = Prob(V_g)
R_g(b)(A) = b(φ_g⁻¹(A))
```

for measurable `A ⊆ V_g`. In a finite situation space:

```text
R_g(b)(v) = ∑_{s: φ_g(s)=v} b(s)
```

Thus a belief of 0.60 normal, 0.30 overheating without fire, and 0.10 fire maps to 0.60 normal and 0.40 abnormal when both fault states are grouped together.

This is aggregation, not preference-based reweighting. Severe consequences do not increase the probability of the corresponding state.

## 4. Summaries of a Belief

Not every useful output is a distribution over abstract states. Examples include:

| Output | Definition or construction | Interpretation |
| --- | --- | --- |
| Event probability | `b(E_g)` for a specified event `E_g ⊆ S` | Probability, in `[0, 1]` |
| Expected severity | `∫ ℓ_g(s) b(ds)` for integrable severity `ℓ_g` | Score in the units of `ℓ_g` |
| Margin quantile | Quantile of a specified margin under `b` | Margin with a stated quantile level |
| Conservative bound | Extremum over a declared uncertainty set | Bound conditional on that set |
| Display category | A declared mapping of belief quantities | Category with an explicit rule |

A score is not a probability merely because it lies in `[0, 1]`. A bound is not automatically calibrated, and a category does not convey uncertainty unless its contract includes it.

Conservative summaries must specify their uncertainty set or probability coverage. A worst case over the entire support can be uninformative, especially for distributions with unbounded support.

## 5. Projection Contract

For each `R_g`, document:

1. The goal and intended consumer.
2. The input situation space and belief assumptions.
3. The output type, units, and meaning of each field.
4. The mapping, thresholds, quantile levels, and other parameters.
5. The distinctions discarded and uncertainty retained or omitted.
6. Update timing, validity conditions, and behavior for missing or stale inputs.
7. Evidence that the output supports the intended downstream use.

A label such as `fire_probability` states a quantity. A command such as `shut_down_now` selects an action and belongs downstream. Mode eligibility can be represented here; choosing or activating a mode belongs to decision logic.

## 6. Adequacy and Information Loss

A deterministic projection cannot recover distinctions lost upstream. Reduction may also discard information its consumer needs.

A practical check is to find beliefs `b_1` and `b_2` for which `R_g(b_1) = R_g(b_2)`. If the intended consumer needs to treat them differently, the projection is inadequate for that use. This check needs an explicit downstream requirement; a goal name alone does not establish sufficiency.

For example, beliefs `(normal=0.6, overheating=0.4, fire=0)` and `(normal=0.6, overheating=0, fire=0.4)` both produce `Pr(abnormal)=0.4`. That output cannot support a response that distinguishes fire from overheating.

## 7. Goal Changes and Implementation

Changing goals can reuse upstream components when their representation supports the new projection. An implementation may use aggregation, rules, learned summaries, or other mechanisms, but it must satisfy the declared contract.

Neither modularity nor an explicit formula guarantees calibration, stability, robustness, or safety. Those properties require validation in the operating domain.