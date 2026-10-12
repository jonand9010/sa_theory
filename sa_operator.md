# Situation Awareness as a Goal-Conditioned System Operator

*A formal blueprint for designing and analyzing engineered systems*

**Version:** 1.1

## Abstract

This framework defines situation awareness (SA) as a transformation from observation history to a goal-relevant representation. It separates perception, comprehension, and projection so that detection, interpretation, and relevance can be inspected independently.

Comprehension constructs a belief over internal situations. Projection exposes a goal-dependent representation of that belief without changing the underlying evidence. The formulation specifies interfaces and responsibilities; it does not guarantee accurate inference, sufficient abstraction, or safe decisions.

## 1. Scope

The framework concerns engineered systems that observe and interpret dynamic environments. It is a representational architecture, not a control algorithm, planning method, or model of human cognition.

An internal situation may describe latent operating modes, faults, relationships, or future outcomes. It need not reconstruct the full physical state. A state estimator can supply comprehension when its representation serves the intended use. Separating goal-dependent representation is the architectural choice emphasized here.

## 2. Spaces and Assumptions

Let `Y` be the observation space, `Z` the semantic concept record space, `S` the internal situation space, and `G` the goal space. At discrete time `t`, `y_{≤t}` and `z_{≤t}` denote observation and concept histories.

`Prob(S)` denotes probability measures over `S`. The situation belief is `b_t ∈ Prob(S)`. The goal-relevant output is `u_{g,t} ∈ U_g`; `U_g` can contain distributions, scores, margins, categories, or structured records.

The reference architecture assumes that perception and comprehension can be held fixed for the goals being considered. Goal dependence is localized to projection. This supports reuse and comparison, but goal-dependent sensing and inference require an extension.

See [notations.md](notations.md) for the shared conventions.

## 3. Operator Definition and Composition

At time `t`:

```text
P    : Y^{≤t} → Z^{≤t}
C    : Z^{≤t} → Prob(S)
R_g  : Prob(S) → U_g
SA_g : Y^{≤t} → U_g

SA_g(y_{≤t}) = R_g(C(P(y_{≤t})))
```

The output space of each operator matches the input space of the next. SA is not necessarily a distribution: comprehension produces a belief, while projection's output type depends on its declared contract.

The decomposition is functional, not a requirement for three software modules. A fused implementation requires evidence that these responsibilities can be identified; merely writing a factorization does not make intermediate representations inspectable.

## 4. Perception: Detecting Concepts

Perception transforms observation history into semantic record history. Records can include entities, events, attributes, timestamps, confidence information, and missing-data flags.

Perception answers: **What has been observed or detected?** It does not rank situations by consequence. Semantic detection still involves modeling assumptions; goal independence does not imply an assumption-free representation.

Compression can discard information needed later. Goals can share perception only if its outputs retain the information their comprehension and projection require.

## 5. Comprehension: Constructing a Belief

Comprehension integrates concepts into a situation belief:

```text
b_t = C(z_{≤t})
```

A probabilistic model may express this as `b_t(ds) = Pr(s_t ∈ ds | z_{≤t}, M)`, where `M` denotes the model and its assumptions. Writing a distribution does not ensure calibration or correctness.

Comprehension can include explanatory hypotheses and temporal dynamics. Prediction belongs here when future outcomes are explicitly modeled as part of `S`. It does not change probabilities because one outcome is more desirable.

## 6. Projection: Exposing Relevance

Projection produces a representation for a specified goal:

```text
u_{g,t} = R_g(b_t)
```

Two useful forms are:

1. **Distribution over an abstraction.** A mapping `φ_g : S → V_g` groups situations. `R_g` adds the probability mass in each group, producing an element of `Prob(V_g)`.
2. **Summary of a belief.** `R_g` reports event probabilities, expected severity, margin quantiles, or other quantities. The output contract specifies units, assumptions, and interpretation.

Projection can emphasize a severe outcome by exposing its probability and consequence together. It must not present a preference-weighted score as an updated probability. Action thresholds and trade-offs belong to downstream decision logic.

Reduction is a design intention, not a guarantee of adequacy. Whether the output preserves enough information depends on downstream use. See [projection_operator.md](projection_operator.md).

## 7. Worked Example: One Belief, Two Goals

An industrial system detects elevated temperature, active machinery, and smoke indicators. For illustration, comprehension uses three mutually exclusive modeled situations:

| Situation | Belief probability |
| --- | ---: |
| Normal operation | 0.60 |
| Overheating without fire | 0.30 |
| Fire present | 0.10 |

The probabilities sum to one. If overheating and fire overlap in the application, the situation space needs joint states instead of treating those labels as exclusive.

**Safety monitoring:** choose `V_safety = {fire, no_fire}`. Map fire present to `fire` and the other situations to `no_fire`:

```text
Pr(fire) = 0.10
Pr(no_fire) = 0.90
```

**Equipment condition monitoring:** choose `V_condition = {normal, abnormal}`. Map overheating and fire to `abnormal`:

```text
Pr(normal) = 0.60
Pr(abnormal) = 0.40
```

The goals expose different distinctions while preserving the underlying belief. A safety policy might intervene at a fire probability of 0.10 because consequences are severe. That intervention is a downstream choice; it does not make fire more probable.

Neither projection is sufficient for every purpose. The condition output cannot distinguish fire from overheating; a fire-specific response requires a richer output.

## 8. Design and Analysis Implications

The decomposition gives separate review questions:

- Does perception detect necessary concepts and expose missing or unreliable evidence?
- Does comprehension support and calibrate its hypotheses?
- Does projection retain the distinctions its consumers require?
- Do consumers interpret projected quantities according to their declared meaning?

These questions organize evidence. They do not establish accuracy, robustness, or safety by themselves.

## 9. Boundaries and Related Formulations

The framework does not specify a universal situation space, inference algorithm, goal-selection method, or loss function. Those choices remain domain-specific.

Belief construction can use techniques from state estimation or partially observable decision models. The decomposition makes a goal-conditioned representation boundary explicit; it is not a claim of incompatibility with those methods or proof of theoretical novelty.

Here, *projection* means abstraction or summarization for relevance, not automatically forecasting. See [limitations.md](limitations.md).