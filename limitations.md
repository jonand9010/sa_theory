# Limitations and Scope

This document clarifies the scope and limitations of the Situation Awareness (SA) framework defined in this repository. Its purpose is to prevent misinterpretation of the framework’s intent and to set appropriate expectations for its use.

The limitations described here are intentional design boundaries rather than shortcomings.

---

## 1. Situation Awareness Is Not Decision-Making

The framework defines situation awareness as a representational process that prepares information for downstream use.

It does not:
- select actions
- perform planning
- execute control
- guarantee correct or safe behavior

Decisions and actions are assumed to be handled by separate system components.

---

## 2. No Guarantees of Correctness or Optimality

The framework does not guarantee that:
- perceptions are accurate
- situation models are correct
- projections preserve all relevant distinctions
- downstream decisions are optimal

Correctness and performance depend on implementation choices, assumptions, and operating conditions.

---

## 3. Goals Define Relevance, Not Behavior

Goals in this framework parameterize relevance through the projection operator.

The framework does not:
- define how goals are chosen
- resolve conflicts between goals
- optimize behavior with respect to goals

Goals may be constraints, preferences, or optimization objectives, but their interpretation lies outside the SA operators.

---

## 4. No Domain-Specific Semantics Are Built In

The framework is domain-agnostic.

It does not define:
- what constitutes a “good” projection in a specific domain
- which distinctions are safety-critical
- how relevance should be encoded

These aspects must be supplied by domain knowledge and system requirements.

---

## 5. Projection Quality Is Not Solved Automatically

The framework makes projection explicit and inspectable, but it does not provide a method for automatically determining whether a projection is appropriate.

Designing and validating projection operators remains an engineering task.

---

## 6. Not a Model of Human Cognition

Although inspired by the concept of situation awareness, this framework does not model human cognitive processes, limitations, or biases.

It is intended for engineered systems and architectural reasoning.

---

## 7. Intended Use

This framework is intended to:
- structure system architectures
- clarify responsibilities between components
- support analysis and comparison
- improve transparency of abstraction and relevance

It is not intended to replace control theory, planning algorithms, or safety analysis methods.

---

## 8. Summary

The Situation Awareness framework defines a clear and explicit structure for transforming observations into goal-relevant representations. Its limitations reflect a deliberate choice to focus on representation and architecture rather than behavior or optimization.

Understanding these boundaries is essential for correct application of the framework.