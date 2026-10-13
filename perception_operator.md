# The Perception Operator in Situation Awareness Systems

Version: 1.3  
Status: Technical specification

## 1. Role

Perception answers:

**What has been observed or detected?**

It transforms raw observations into semantic records that comprehension can interpret. These records describe detected entities, events, attributes, or relations; they do not yet constitute an explanation of the overall situation.

In the reference architecture, perception is held fixed across goals. It does not rank situations by consequence, select actions, or construct competing explanations of the overall situation.

Detection still depends on models, ontologies, and sensor assumptions. Goal independence does not mean that perception is assumption-free.

## 2. Formal Interface

Using the conventions in [notations.md](notations.md):

```text
P : Y^{≤t} → Z^{≤t}

z_{≤t} = P(y_{≤t})
```

Here:

- `Y` is the raw observation space.
- `y_{≤t}` is the observation history through time `t`.
- `Z` is the semantic concept record space.
- `z_{≤t}` is the concept record history available at time `t`.

The history-level interface ensures that perception's output matches comprehension's input:

```text
C : Z^{≤t} → Prob(S)
```

This is an interface definition, not a requirement to store or recompute the entire history.

## 3. Semantic Records

A concept record may contain:

- detected entities or objects
- detected events
- attributes or measurements
- relations between detected elements
- observation timestamps
- confidence information
- validity and missing-data flags
- source or provenance information

The record schema is domain-specific and must define the meaning of each field.

For example, a monitoring system might produce:

```text
Concept: elevated_temperature
Observed at: t
Measured value: 92 °C
Detection rule: temperature exceeds 85 °C
Sensor validity: valid
```

This record states what was detected. It does not establish why the temperature is elevated, whether a fault exists, or what action should be taken.

Those questions belong to comprehension and downstream components.

## 4. Temporal Processing

Perception may use temporal information to produce semantic records. Examples include filtering, tracking, event detection, and estimating attributes from multiple observations.

The use of memory does not by itself make an operation comprehension. The boundary is determined by the meaning of its output:

- Perception produces detected concepts.
- Comprehension produces a belief about the overall situation.

A streaming implementation may emit individual records and maintain the information needed for subsequent processing.

A causal implementation must not use observations that were unavailable when an output was produced. If later evidence revises an earlier detection, the record should distinguish the observation time from the revision time.

## 5. Uncertainty and Missing Evidence

Perception should expose relevant ambiguity, measurement uncertainty, and missing evidence through its output contract.

A detector confidence score is not automatically a calibrated probability. Its meaning and limitations must be documented.

Likewise, the absence of a detection is not necessarily evidence that the corresponding entity or event is absent. It may reflect:

- insufficient sensor coverage
- missing or invalid observations
- detection thresholds
- limited detector sensitivity

Comprehension must be able to distinguish these cases when they affect situation inference.

## 6. Information Loss and Goal Reuse

Perception often compresses raw observations. Information discarded at this stage may constrain subsequent inference.

A property that is not directly measured may still be inferred from indirect evidence and model assumptions. Missing direct measurement alone does not establish unobservability.

Holding perception fixed across goals is appropriate only when its outputs retain the information those goals require. A new goal may require additional concepts, greater resolution, or different uncertainty information.

Such changes extend the upstream representation rather than merely changing projection.

## 7. Boundary of Responsibility

Perception is responsible for detecting and describing concepts.

It is not responsible for:

- constructing competing explanations of the overall situation
- assessing consequences under a goal
- determining which situation distinctions matter downstream
- selecting actions or operational modes

Comprehension integrates the concept history into a situation belief. Projection exposes a goal-relevant representation of that belief.

## 8. Review Questions

When specifying or evaluating perception, ask:

- Which concepts can be detected, and which are outside the output schema?
- How are timestamps, validity, uncertainty, and revisions represented?
- Can missing evidence be distinguished from a negative detection?
- What information is discarded?
- Do downstream goals require any discarded information?
- Can detector outputs be validated separately from situation inference?

## 9. Summary

Perception transforms observation history into semantic record history:

```text
P : Y^{≤t} → Z^{≤t}
```

Its output provides the detected concepts and evidence-quality information needed by comprehension. It remains descriptive and is held fixed across goals in the reference architecture.