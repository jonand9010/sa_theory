# The Comprehension Operator in Situation Awareness Systems

Version: 1.2  
Status: Technical specification  

---

## 1. Introduction

Comprehension is the stage of situation awareness responsible for forming a coherent internal representation of what is happening in the environment, given the detected concepts provided by perception.

This document defines the comprehension operator formally and clarifies how it differs from both perception and projection.

---

## 2. Position Within the Situation Awareness Framework

Situation awareness is defined as the composition of three operators:

    SA_g = R_g ∘ C ∘ P

where:

- P denotes the perception operator
- C denotes the comprehension operator
- R_g denotes the projection operator

The comprehension operator sits between perception and projection. It integrates detected concepts into an internal situation representation that captures structure, coherence, and uncertainty.

---

## 3. Role of the Comprehension Operator

The comprehension operator answers the question:

What situation might be occurring, given what has been observed?

Its role is to transform detected concepts into an internal situation representation that supports explanation, hypothesis management, and temporal consistency.

Comprehension introduces structure but not intent.

---

## 4. Formal Definition

Let Z denote the space of detected concepts produced by perception.

Let S denote the internal situation space.

The comprehension operator is defined as:

    C : Z^{≤t} → S

or, when uncertainty is represented explicitly:

    C : Z^{≤t} → P(S)

where P(S) denotes a belief over situation states.

The situation representation may include:

- latent modes or regimes
- causal or explanatory hypotheses
- temporal trends and dynamics
- uncertainty and ambiguity

---

## 5. Characteristics of the Comprehension Operator

The comprehension operator has the following characteristics:

- It is descriptive, not normative.
- It is independent of goals and preferences.
- It integrates information over time.
- It may represent multiple competing hypotheses.

Comprehension may be implemented using probabilistic models, rule-based reasoning, learned world models, or hybrid approaches.

---

## 6. Boundary of Responsibility

The comprehension operator is not responsible for:

- deciding which aspects of the situation matter
- selecting actions or control inputs
- optimizing outcomes
- enforcing operational priorities

These responsibilities belong to the projection operator and downstream system components.

---

## 7. Relationship to Projection

The comprehension operator produces a rich internal situation representation. This representation is intentionally general and expressive.

The projection operator subsequently reduces this representation according to a specific goal or operational context. Comprehension itself does not perform any reduction based on relevance.

---

## 8. Summary

The comprehension operator constructs an internal situation representation that explains observed concepts and maintains coherence over time. It provides the descriptive foundation upon which goal-dependent projection operates.

By separating comprehension from both perception and projection, the situation awareness framework avoids conflating detection, interpretation, and relevance.