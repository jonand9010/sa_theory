# Situation Awareness Operator

This repository defines a structured, operator-based framework for **situation awareness (SA)** in engineered systems. The framework separates sensing, interpretation, and relevance into three explicit operators: **Perception**, **Comprehension**, and **Projection**.

The purpose of this repository is to provide a clear, rigorous, and implementation-agnostic definition of situation awareness that supports system design, analysis, and comparison without embedding assumptions about control, planning, or optimization.

---

## 1. Overview

Situation awareness is treated here as a **first-class system capability**, not as a by-product of estimation or decision-making.

The central idea is that a system’s internal understanding of its environment is constructed in stages:

1. Raw observations are transformed into semantic concepts  
2. Concepts are integrated into a coherent situation representation  
3. The situation representation is reduced to a goal-relevant form  

These stages are made explicit through an operator-based formulation.

---

## 2. Situation Awareness Operator

Situation awareness for a given goal or operational context g is defined as the composition:

    SA_g = R_g ∘ C ∘ P

where:

- P is the **Perception operator**
- C is the **Comprehension operator**
- R_g is the **Projection operator**, parameterized by the goal g

At time t, the operator acts on the observation history y_{≤t}. Each operator has a distinct responsibility and a well-defined interface.

---

## 3. Perception Operator

The perception operator is responsible for transforming raw observations into **semantic concepts**.

It answers the question:

What has been detected or observed?

Perception operates directly on sensor data and produces elements such as detected objects, events, signals, attributes, or relations. It does not attempt to explain, interpret, or predict. Any uncertainty or noise in detection is propagated forward.

Perception is **independent of goals or operational context**.

A detailed specification is provided in `perception_operator.md`.

---

## 4. Comprehension Operator

The comprehension operator integrates detected concepts into a **coherent internal situation representation**.

It answers the question:

What situation might be occurring, given what has been observed?

Comprehension introduces structure, consistency, and temporal coherence. It may represent latent modes, causal hypotheses, trends, and uncertainty. Comprehension remains descriptive and does not evaluate relevance or prescribe behavior.

Comprehension is **independent of goals**.

A detailed specification is provided in `comprehension_operator.md`.

---

## 5. Projection Operator

The projection operator reduces the internal situation representation to a **goal-relevant representation**.

It answers the question:

Which aspects of the situation matter under the current goal or responsibility?

Projection is the first stage where intent enters the situation awareness pipeline. It does not perform planning, control, or optimization. Instead, it shapes the form of the situation representation by preserving distinctions that matter for the goal and discarding those that do not.

The goal g materializes entirely in the structure of the projected representation.

A detailed specification is provided in `projection_operator.md`.

---

## 6. Separation of Responsibilities

The three operators serve distinct purposes:

- Perception determines what is detected
- Comprehension determines what situation is believed
- Projection determines what is relevant for the goal

This separation avoids conflating sensing quality, interpretation, and relevance, and enables clearer reasoning about system behavior and failure modes.

---

## 7. Design Intent

This framework is intended to:

- make implicit abstraction steps explicit
- support architectural reasoning and system comparison
- improve transparency and testability
- remain compatible with diverse implementation approaches
- avoid embedding control or optimization assumptions

Situation awareness is treated as a representational process that prepares information for downstream use, not as a decision-making mechanism itself.

---

## 8. Repository Structure

The repository is organized as follows:

- `README.md`  
  High-level introduction to the situation awareness framework

- `perception_operator.md`  
  Formal definition of the perception operator

- `comprehension_operator.md`  
  Formal definition of the comprehension operator

- `projection_operator.md`  
  Formal definition of the projection operator

Additional documents may be added to cover examples, notation, or domain-specific adaptations.

---

## 9. Summary

Situation awareness is defined as a structured transformation from observations to a goal-relevant internal situation representation. By decomposing this transformation into perception, comprehension, and projection operators, the framework provides a clear and rigorous foundation for designing and analyzing situation-aware systems.

This repository serves as a reference specification for that foundation.