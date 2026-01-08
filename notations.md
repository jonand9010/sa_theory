# Notation and Conventions

This document defines the notation and naming conventions used throughout the Situation Awareness (SA) framework. Its purpose is to ensure consistency and avoid ambiguity across documents.

The notation is intentionally minimal and focuses on operator structure rather than algorithmic detail.

---

## 1. Time and Observations

- t  
  Discrete or continuous time index.

- y_t  
  Raw observation at time t.

- y_{≤t}  
  Observation history up to and including time t.

- Y  
  Space of raw observations.

---

## 2. Situation Awareness Operator

- SA_g  
  Situation awareness operator parameterized by goal or operational context g.

The situation awareness pipeline is defined as:

    SA_g = R_g ∘ C ∘ P

---

## 3. Perception

- P  
  Perception operator.

- Z  
  Space of detected semantic concepts.

The perception operator is defined as:

    P : Y^{≤t} → Z

Perception transforms raw observations into semantic concepts.

---

## 4. Comprehension

- C  
  Comprehension operator.

- S  
  Internal situation space produced by comprehension.

- P(S)  
  Belief or uncertainty distribution over situation states.

The comprehension operator is defined as:

    C : Z^{≤t} → S

or, when uncertainty is represented explicitly:

    C : Z^{≤t} → P(S)

---

## 5. Projection

- R_g  
  Projection operator parameterized by goal g.

- φ_g  
  Goal-conditioned projection function.

- U_g  
  Goal-relevant situation representation space.

U_g is a representation space determined by the goal and is generally not a subset of S. Its elements may represent abstractions such as categories, margins, scores, or modes.

The projection operator is defined as:

    R_g : S → U_g

Equivalently:

    φ_g : S → U_g

with:

    R_g(s) = φ_g(s)

---

## 6. Goals

- g  
  Goal or operational context parameter.

A goal specifies relevance rather than behavior. It determines how the internal situation representation is reduced by the projection operator. A goal may correspond to a safety objective, optimization objective, operational mode, diagnostic intent, or responsibility constraint.

---

## 7. Operator Responsibilities (Summary)

- P (Perception): determines what is detected
- C (Comprehension): determines what situation is believed
- R_g (Projection): determines what aspects of the situation are relevant under goal g

---

## 8. General Conventions

- Uppercase symbols denote spaces or operators.
- Lowercase symbols denote elements of spaces.
- Subscript g denotes parameterization by goal, not subsetting.
- Composition is written using the ∘ symbol.
- No assumptions are made about implementation, algorithms, or control strategies unless explicitly stated.

---

## 9. Scope of Notation

This notation is scoped to the situation awareness framework only. It deliberately avoids symbols related to control, planning, optimization, or learning, which are considered downstream concerns.

---

## 10. Summary

This document defines the minimal notation required to describe the Situation Awareness framework consistently. It serves as a shared reference for all other documents in the repository and is intended to remain stable as the framework evolves.