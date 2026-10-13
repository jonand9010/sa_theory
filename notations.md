# Notation and Conventions

This document defines the shared interfaces of the Situation Awareness (SA) framework.

## 1. Spaces and Elements

| Symbol | Meaning |
| --- | --- |
| `X`, `x_t` | World or plant state space and state at time `t` |
| `Y`, `y_t` | Raw observation space and observation at time `t` |
| `Z`, `z_t` | Semantic concept record space and record at time `t` |
| `S`, `s_t` | Internal situation space and a possible situation at time `t` |
| `Prob(S)`, `b_t` | Probability measures over `S` and the current situation belief |
| `G`, `g` | Goal or operational context space and an active goal |
| `U_g`, `u_{g,t}` | Goal-relevant output space and a projected representation |

`Prob(S)` distinguishes probability measures from the perception operator `P`. Beliefs may be represented by mass functions, densities, or other probability measures as appropriate. Spaces carrying probability measures have suitable measurable structure.

## 2. Time and Histories

The reference formulation uses discrete time: `t = 0, 1, ...`.

- `y_{≤t} = (y_0, ..., y_t)` belongs to `Y^{≤t}`.
- `z_{≤t} = (z_0, ..., z_t)` belongs to `Z^{≤t}`.
- Concept records can contain timestamps, missing-data flags, confidence information, entities, events, and relations.

Continuous-time implementations require corresponding history spaces and are outside this minimal notation.

## 3. Canonical Interfaces

At each time `t`:

```text
P    : Y^{≤t} → Z^{≤t}
C    : Z^{≤t} → Prob(S)
R_g  : Prob(S) → U_g
SA_g : Y^{≤t} → U_g

SA_g = R_g ∘ C ∘ P
b_t = C(P(y_{≤t}))
u_{g,t} = R_g(b_t)
```

`P` is the history-level perception operator. Streaming implementations may emit individual records and retain their history; comprehension may maintain a recursive state instead of recomputing from history. These interfaces impose no full-history storage requirement.

The reference formulation makes comprehension uncertainty explicit. A point estimate can be encoded as a Dirac measure `δ_s`, assigning probability one to `s`. This encoding does not establish that a point estimator is certain or correct.

## 4. State Abstraction and Belief Projection

`R_g` consumes a belief. Its output may be a distribution, scalar, vector, category, or structured record; `U_g` specifies which.

For a measurable state abstraction `φ_g : S → V_g`, a distribution-valued projection is:

```text
U_g = Prob(V_g)
R_g(b)(A) = b(φ_g⁻¹(A))
```

for measurable `A ⊆ V_g`. Here, `φ_g⁻¹(A)` is the set of situations mapped into `A`. This construction is the pushforward of `b`: probabilities of merged states are added, without reweighting according to preference.

A summary such as expected severity instead uses:

```text
U_g ⊆ ℝ
R_g(b) = ∫ ℓ_g(s) b(ds)
```

where `ℓ_g` is a specified severity function integrable under the input belief `b`. This score is not automatically a probability.

## 5. Goals and Responsibilities

The goal `g` specifies relevance and parameterizes `R_g`. In this reference architecture, `P` and `C` are held fixed across goals. Goal locality is a design assumption, not a universal property of sensing and inference.

- `P`: what has been detected?
- `C`: what situation might be occurring?
- `R_g`: which representation matters for this goal?

Action selection remains downstream. Here, *projection* means goal-conditioned representation, not necessarily prediction of future states.