# Analysis Guidelines for Situation Awareness Systems

This document describes how to analyze an existing or proposed system using the Situation Awareness framework.

The analysis identifies concrete representations, transformations, assumptions, and consumers.

Structural analysis can be performed without ground truth. Claims about accuracy, calibration, or performance require additional evidence.

## 1. Establish the Scope

Identify:

- the system and operating domain
- the observations available
- the goals under consideration
- the downstream consumers
- the tasks or decisions those consumers must support

Describe goals through requirements rather than names alone. “Safety” or “diagnostics” does not specify which information a consumer needs.

## 2. Identify the Functional Pipeline

Locate the responsibilities corresponding to:

```text
P    : Y^{≤t} → Z^{≤t}
C    : Z^{≤t} → Prob(S)
R_g  : Prob(S) → U_g
```

Do not assume that each operator corresponds to one software component.

For each responsibility, identify its inputs, outputs, configuration, and available evidence. If an intermediate representation is implicit, record that limitation rather than assuming its meaning.

## 3. Analyze Perception

Describe the concept records produced from observations.

Record:

- detectable entities, events, attributes, and relations
- timestamps and source information
- confidence semantics
- validity and missing-data behavior
- temporal filtering and revisions
- information discarded

Distinguish missing evidence from a negative detection.

Missing direct measurements do not by themselves establish unobservability. Assess whether indirect evidence and model assumptions support inference of the quantities of interest.

## 4. Analyze Comprehension

Describe the situation space `S` and the belief `b_t`.

Record:

- the meaning of each state or hypothesis
- excluded alternatives
- whether faults can coexist
- inference assumptions
- temporal state and update rules
- handling of conflicting or delayed evidence
- calibration evidence and model limits

For categorical states, verify that alternatives are mutually exclusive and exhaustive within the modeled domain.

Do not treat arbitrary confidence scores as probabilities without justification.

If future outcomes are modeled, record their horizon and assumptions, including assumed future inputs or actions.

## 5. Identify the Projection Boundary

Locate where the situation belief is transformed into information for a particular consumer.

The boundary is defined by goal-dependent representation, not by algorithmic complexity.

For example, comprehension may maintain probabilities of several fault combinations while projection exposes only the probability that service is unavailable.

Record the exact mapping and its configuration.

## 6. Describe the Output Contract

For each projected output, specify:

- its type and possible values
- units and interpretation
- mapping, thresholds, and quantile levels
- uncertainty retained or omitted
- distinctions discarded
- update timing and validity
- behavior for missing or stale inputs

Explicitly distinguish probabilities, scores, descriptive categories, and commands.

A `fire_probability` field describes a quantity. A `shut_down_now` command selects an action and belongs downstream.

## 7. Verify the Goal and Consumer

Projection reveals which distinctions are exposed, but does not uniquely identify the goal.

Check the intended goal against requirements and actual consumer behavior.

The same representation may support several goals. Conversely, a consumer may require distinctions that the current projection omits.

## 8. Check Adequacy

Construct beliefs with the same projected output:

```text
R_g(b_1) = R_g(b_2)
```

Ask whether the intended consumer needs to treat them differently.

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

Both produce `Pr(abnormal) = 0.40`.

That output cannot support a consumer requiring different treatment of fire and overheating.

This is a way to find inadequacy, not proof that every necessary distinction has been checked.

## 9. Compare Alternative Designs

Compare designs through their contracts:

| Layer | Comparison questions |
| --- | --- |
| Perception | Which concepts and evidence-quality information are exposed? |
| Comprehension | Which situations and dependencies are represented? |
| Projection | Which distinctions and uncertainty are retained? |
| Consumer | Which tasks can the available output support? |

Differences in representation affect the information available downstream. Resulting behavior also depends on the consuming policy.

## 10. Analyze Incidents

Trace an incident through the pipeline:

1. What observations were available at the relevant time?
2. What concepts were detected?
3. What belief was constructed?
4. What information was projected?
5. How did the consumer interpret and use it?

Separate detection errors, inference errors, projection inadequacy, and downstream misuse.

Failures may involve several layers; the decomposition does not guarantee a unique cause.

## 11. Review Record

For each goal and consumer, retain a concise review record:

| Item | Required detail |
| --- | --- |
| Goal | Required distinctions and intended use |
| Perception | Concept schema, timing, validity, confidence semantics |
| Comprehension | Situation space, belief representation, model assumptions |
| Projection | Mapping, output type, units, uncertainty semantics |
| Information loss | Merged distinctions and omitted uncertainty |
| Consumer | Actual use of the output |
| Validation | Evidence for accuracy, calibration, adequacy, and timing |

## 12. Summary

Analysis using this framework makes representational choices explicit.

It connects observations, concepts, beliefs, and goal-relevant outputs to their actual consumers, while keeping architectural description separate from validation claims.