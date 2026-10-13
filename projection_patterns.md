# Projection Patterns in Situation Awareness Systems

This document describes recurring ways to transform a situation belief into a goal-relevant representation.

The patterns provide a vocabulary for designing, comparing, and evaluating projections. They do not establish that a particular projection is adequate for its intended use.

## 1. Common Interface

Using the conventions in [notations.md](notations.md):

```text
R_g : Prob(S) → U_g

u_{g,t} = R_g(b_t)
```

Here:

- `b_t` is the situation belief produced by comprehension.
- `g` specifies the goal or operational context.
- `U_g` defines the output type and its interpretation.

Two constructions must be distinguished:

1. **State abstraction:** group situations and preserve their aggregated probabilities.
2. **Belief summary:** derive a score, margin, bound, category, or structured record from the belief.

These constructions can be combined, but their meanings must remain explicit.

Here, *projection* means representation according to relevance. It does not necessarily mean prediction.

## 2. Category-Based Projection

### Purpose

Expose a small set of descriptive categories relevant to the goal.

Examples include:

- normal / degraded / unsafe
- fault absent / fault present
- sufficient / insufficient resource margin

### Distribution Over Categories

Define a measurable abstraction:

```text
φ_g : S → V_g

V_g = {c_1, ..., c_k}
U_g = Prob(V_g)
```

The projected probability of a category is:

```text
R_g(b)(c) = ∑_{s: φ_g(s)=c} b(s)
```

for a finite situation space.

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

The projection preserves the probability of abnormal operation while discarding the distinction between overheating and fire.

### Hard Category Output

A hard label instead uses:

```text
U_g = V_g
```

It requires a specified rule, such as selecting the most probable category or applying a threshold to a category probability.

A hard label discards uncertainty unless additional output fields retain it. A most-probable label is not automatically an appropriate basis for a consequential decision.

### Review Considerations

- Are the categories mutually exclusive and exhaustive?
- Which situations are merged?
- Is the output a distribution or a single label?
- How sensitive is the output to category boundaries or thresholds?
- Could merged situations require different downstream treatment?

## 3. Margin-Based Projection

### Purpose

Expose distance to a specified constraint or boundary.

Define a margin function:

```text
m_g : S → ℝ
```

For example, positive values may indicate remaining capacity and negative values a violated constraint. The sign convention and units must be declared.

### Output Options

Projection may expose:

- a distribution over the margin
- expected margin
- a specified margin quantile
- probability that the margin is negative

Examples:

```text
Expected margin:
R_g(b) = ∫ m_g(s) b(ds)

Probability of violation:
R_g(b) = b({s : m_g(s) < 0})
```

These outputs answer different questions. A positive expected margin does not imply a low probability of violation.

### Examples

- temperature headroom below a limit
- remaining energy relative to a requirement
- distance from a capacity constraint
- predicted time margin before a modeled event

A future margin requires a predictive model in comprehension, with a stated horizon and assumptions.

### Review Considerations

- What constraint defines the margin?
- What are the units and sign convention?
- Which aspect of uncertainty is exposed?
- Does the summary hide a consequential tail of the distribution?

## 4. Indicator-Based Projection

### Purpose

Expose compact quantities such as event probabilities, severity scores, or health indicators.

### Typical Structure

```text
U_g ⊆ ℝ^k
```

For a specified event:

```text
R_g(b) = b(E_g)
```

For a specified, integrable severity function:

```text
R_g(b) = ∫ ℓ_g(s) b(ds)
```

The first output is a probability. The second is an expected severity in the units of `ℓ_g`.

### Examples

- probability of a network fault
- expected service disruption
- probability of unavailable capacity
- an explicitly defined health score

### Review Considerations

- Is the indicator a probability, expected quantity, or constructed score?
- What assumptions and scaling define it?
- Does aggregation hide different fault causes or consequences?
- Can consumers interpret it without access to the full belief?

A score is not a probability merely because it lies in `[0, 1]`. Consequence weighting must not be presented as a change in the underlying belief.

## 5. Mode-Relevant Projection

### Purpose

Expose information needed by a downstream policy to assess operational modes.

Projection reports mode-related conditions, feasibility, or eligibility. It does not select or activate a mode.

### Typical Structure

```text
U_g = structured mode-condition record
```

Example fields might include:

```text
probability_primary_service_available
probability_fallback_service_available
capacity_margin
evidence_validity
```

A descriptive eligibility label can also be exposed when its criteria and treatment of uncertainty are specified.

### Examples

- conditions relevant to primary versus fallback operation
- resource availability for an operating mode
- confidence that a mode's prerequisites hold

### Review Considerations

- Does the output describe conditions or issue a command?
- Are eligibility criteria and uncertainty semantics explicit?
- What happens when evidence is stale or incomplete?
- Which downstream component selects and activates the mode?

Commands such as `activate_fallback` belong to decision logic.

## 6. Conservative Bound Projection

### Purpose

Expose a bound or conservative summary under declared uncertainty assumptions.

### Uncertainty-Set Construction

For a quantity `q_g : S → ℝ` and a specified nonempty set `K(b) ⊆ S`, a projection may report:

```text
Upper bound:
R_g(b) = sup_{s ∈ K(b)} q_g(s)

Lower bound:
R_g(b) = inf_{s ∈ K(b)} q_g(s)
```

The output is a bound over `K(b)`, not automatically over every possible real-world situation.

The contract must explain how the set is constructed, its intended coverage, and the assumptions supporting that coverage.

### Quantile Construction

A projection may instead report a specified quantile of a quantity under the belief.

A quantile describes probability coverage within the model. It is not an absolute worst-case guarantee.

### Examples

- lower bound on remaining capacity over a declared set
- upper quantile of modeled temperature
- lower quantile of available energy

### Review Considerations

- Is the output an extremum, quantile, or statistical confidence bound?
- Which uncertainty sources are represented?
- Are the bound direction and coverage explicit?
- Is the bound finite and useful?
- How sensitive is it to model assumptions or outliers?

Taking a worst case over the entire support can be uninformative, particularly with unbounded distributions. Conservative construction alone does not establish safety.

## 7. Interface-Oriented Projection

### Purpose

Expose a representation suited to a human or external-system interface.

### Typical Structure

```text
U_g = structured interface record
```

Examples include:

- status summaries with probabilities
- diagnostic reports
- condition indicators with supporting evidence
- compact telemetry messages

Formatting alone does not necessarily constitute a new projection. The projection choice lies in which distinctions and uncertainty are retained, summarized, or omitted.

### Review Considerations

- What does the consumer need to distinguish?
- Are probabilities and scores clearly labeled?
- Are timestamps and validity visible?
- Does presentation hide ambiguity?
- Can users access additional detail when needed?

A concise interface should not imply greater certainty than the underlying belief supports.

## 8. Combining Patterns

A projection can expose several complementary outputs:

```text
U_g = U_g^(1) × ... × U_g^(k)

R_g(b) = (
    R_g^(1)(b),
    ...,
    R_g^(k)(b)
)
```

Examples include:

- category probabilities plus a margin quantile
- event probability plus expected severity
- mode-condition information plus evidence validity
- a health indicator plus diagnostic probabilities

Each component needs an explicit meaning.

Multiple marginal summaries do not necessarily preserve dependencies. If downstream use requires joint probabilities or correlations, those must be retained explicitly.

## 9. Adequacy Check

For any pattern, ask whether two beliefs can produce the same output while requiring different treatment by the intended consumer.

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

That projection cannot support a consumer that needs to distinguish fire from overheating.

Adequacy is therefore relative to a specified downstream requirement. A compact output is useful only when its information loss is acceptable for that use.

## 10. Pattern Selection and Documentation

Select patterns according to the distinctions and quantities the consumer requires.

For each projection, document:

- the goal and intended consumer
- the input belief and its assumptions
- the output type, units, and interpretation
- the mapping and configuration parameters
- uncertainty retained and omitted
- distinctions discarded
- timing and validity behavior
- evidence of adequacy for the intended use

See [projection_operator.md](projection_operator.md) for the complete projection contract and [analysis_guidelines.md](analysis_guidelines.md) for system review guidance.

## 11. Summary

Projection patterns describe recurring output structures for goal-relevant representations.

Their value lies in making abstraction choices explicit: what is retained, what is discarded, how uncertainty is handled, and what downstream use the output can support.