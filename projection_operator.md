# The Projection Operator in Situation Awareness Systems

Version: 1.3  
Status: Technical specification

## 1. Role

Projection answers:

**Which representation of the current situation belief is relevant for this goal?**

It exposes selected distinctions and summaries for downstream use.

Projection does not update the underlying belief, select actions, or introduce new observational evidence. Here, *projection* means goal-conditioned representation rather than necessarily forecasting.

## 2. Canonical Interface

Using the conventions in [notations.md](notations.md):

```text
R_g : Prob(S) → U_g

u_{g,t} = R_g(b_t)
```

Here:

- `S` is the internal situation space.
- `Prob(S)` is the space of probability measures over `S`.
- `b_t` is the situation belief produced by comprehension.
- `g` specifies the goal or operational context.
- `U_g` is the goal-relevant output space.
- `u_{g,t}` is the projected representation.

The goal determines the output space and the mapping. Outputs may be distributions, event probabilities, risk scores, margins, categories, or structured records. Their types and meanings must be declared.

Perception and comprehension are held fixed across goals in the reference architecture. Goal-dependent sensing or inference requires an extension, not merely a different projection.

## 3. Distribution-Preserving Abstraction

A measurable state abstraction groups situations according to relevance:

```text
φ_g : S → V_g
```

Here, `V_g` is the abstract state space. The corresponding belief projection is the pushforward:

```text
U_g = Prob(V_g)

R_g(b)(A) = b(φ_g⁻¹(A))
```

for measurable `A ⊆ V_g`.

`φ_g⁻¹(A)` is the set of situations mapped into `A`. Its probability under the original belief becomes the probability of `A` under the projected belief.

In a finite situation space:

```text
R_g(b)(v) = ∑_{s: φ_g(s)=v} b(s)
```

For example:

```text
Original belief:
normal                   0.60
overheating_without_fire  0.30
fire_present             0.10

Projected belief:
normal    0.60
abnormal  0.40
```

The projection merges overheating and fire into one category. It preserves their combined probability while discarding the distinction between them.

This is aggregation, not preference-based reweighting. Severe consequences do not increase the probability of the corresponding state.

## 4. Summaries of a Belief

Not every useful output is a distribution over abstract states. Projection can instead summarize the belief.

| Output | Definition or construction | Interpretation |
| --- | --- | --- |
| Event probability | `b(E_g)` for a specified measurable event `E_g ⊆ S` | Probability, in `[0, 1]` |
| Expected severity | `∫ ℓ_g(s) b(ds)` for severity `ℓ_g` integrable under `b` | Score in the units of `ℓ_g` |
| Margin quantile | Quantile of a specified margin under `b` | Margin with a stated quantile level |
| Conservative bound | Extremum over a declared uncertainty set | Bound conditional on that set |
| Display category | A declared mapping of belief quantities | Category with an explicit rule |

A score is not a probability merely because it lies in `[0, 1]`. A category does not convey uncertainty unless its contract includes it.

Conservative summaries must specify their uncertainty set or probability coverage. A worst case over the entire support can be uninformative, especially for distributions with unbounded support.

A predictive summary requires modeled future outcomes from comprehension, including their horizon and assumptions. Projection selects or summarizes those outcomes; changing the goal does not itself introduce a predictive model.

## 5. Projection Contract

For each `R_g`, document:

1. The goal and intended consumer.
2. The input situation space and belief assumptions.
3. The output type, units, and meaning of each field.
4. The mapping, thresholds, quantile levels, and other parameters.
5. The distinctions discarded and uncertainty retained or omitted.
6. Update timing, validity conditions, and behavior for missing or stale inputs.
7. Evidence that the output supports the intended downstream use.

A field such as `fire_probability` states a quantity. A command such as `shut_down_now` selects an action and belongs downstream.

Mode conditions or eligibility can be represented here when their criteria are explicit. Choosing or activating a mode belongs to decision logic.

## 6. Adequacy and Information Loss

A deterministic projection cannot recover distinctions lost upstream. Reduction may also discard information its consumer needs.

A practical check is to find beliefs `b_1` and `b_2` for which:

```text
R_g(b_1) = R_g(b_2)
```

If the intended consumer needs to treat them differently, the projection is inadequate for that use.

For example:

```text
Belief A:
normal      0.60
overheating 0.40
fire        0.00

Belief B:
normal      0.60
overheating 0.00
fire        0.40
```

Both produce:

```text
Pr(abnormal) = 0.40
```

That output cannot support a response that distinguishes fire from overheating.

This check requires an explicit downstream requirement. A goal name alone does not establish sufficiency.

When multiple summaries are exposed, they may still omit dependencies. Marginal probabilities or separate scores do not automatically preserve joint information needed by a consumer.

## 7. Goal Changes and Implementation

Changing goals can reuse upstream components when their representation supports the new projection.

An implementation may use aggregation, rules, learned summaries, or other mechanisms, but it must satisfy the declared contract.

Reusing the same comprehension model requires checking that its situation space and belief retain the distinctions needed by the new goal. Projection cannot compensate for an upstream representation that omits them.

Neither modularity nor an explicit formula guarantees calibration, stability, robustness, or safety. Those properties require validation in the operating domain.

## 8. Relationship to Projection Patterns

This document defines the projection operator's interface, responsibilities, and validation requirements.

[projection_patterns.md](projection_patterns.md) describes recurring output designs, including category distributions, margins, indicators, mode-related information, conservative bounds, and interface summaries.

A pattern is a design option, not evidence that a projection is adequate.

## 9. Summary

Projection transforms a situation belief into a goal-relevant representation:

```text
R_g : Prob(S) → U_g
```

It makes relevance explicit without changing the underlying belief or selecting actions. Its output must declare what it means, which uncertainty it retains, and which downstream uses it can support.