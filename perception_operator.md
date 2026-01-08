# The Perception Operator in Situation Awareness Systems

Version: 1.2  
Status: Technical specification  

---

## 1. Introduction

Perception is the first stage in the situation awareness (SA) pipeline. Its role is to transform raw observations into semantically meaningful elements that can be reasoned about by higher-level system components.

This document defines the perception operator formally and clarifies its scope, responsibilities, and relationship to the rest of the situation awareness framework.

---

## 2. Position Within the Situation Awareness Framework

Situation awareness is defined as the composition of three operators:

    SA_g = R_g ∘ C ∘ P

where:

- P denotes the perception operator
- C denotes the comprehension operator
- R_g denotes the projection operator

The perception operator is the entry point of the situation awareness pipeline. It operates directly on raw observations and is independent of goals or operational context.

---

## 3. Role of the Perception Operator

The perception operator answers the question:

What has been observed or detected?

Its purpose is to extract semantic concepts from raw sensor data. These concepts serve as the basic informational units for subsequent reasoning but do not yet constitute an understanding of the situation.

Perception does not explain, predict, or evaluate. It detects.

---

## 4. Formal Definition

Let Y denote the space of raw observations, and let Z denote the space of detected concepts.

The perception operator is defined as:

    P : Y^{≤t} → Z

where Y^{≤t} represents the observation history up to time t.

The output Z may include:

- detected objects or entities
- events or signals
- attributes or measurements
- relations between detected elements

Uncertainty, noise, and ambiguity may be present in the output and are propagated forward.

---

## 5. Characteristics of the Perception Operator

The perception operator has the following characteristics:

- It is goal-independent.
- It is descriptive rather than interpretive.
- It does not impose causal or temporal structure.
- It does not aggregate information into higher-level hypotheses.

Both classical signal processing pipelines and learned detection models fit naturally within this operator.

---

## 6. Boundary of Responsibility

The perception operator is not responsible for:

- determining what the detected elements mean
- assessing relevance or importance
- resolving ambiguity between competing explanations
- predicting future developments

These responsibilities belong to the comprehension operator.

---

## 7. Summary

The perception operator transforms raw observations into semantic concepts.