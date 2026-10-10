# Situation Awareness Operator

A structured framework for defining and analyzing situation awareness in engineered systems.

## Quick start

This repository defines situation awareness as a goal-conditioned transformation from observations to a goal-relevant internal representation:

SA_g = R_g ∘ C ∘ P

Where:

- P = Perception: transforms raw observations into semantic concepts
- C = Comprehension: integrates concepts into an internal situation model
- R_g = Projection: reduces that model to a goal-relevant representation

The framework is intended to make implicit abstraction steps explicit, so systems can be compared, analyzed, and audited more clearly.

## Why this repository exists

Situation awareness is often discussed informally, but it is rarely defined with enough precision to support system design and analysis. This repository addresses that gap by separating three distinct functions:

1. What is detected?
2. What situation is believed to exist?
3. What matters given the goal?

This separation helps distinguish sensing failures, explanation failures, and relevance failures.

## What this framework is

- a representational architecture for SA
- a conceptual tool for system analysis
- a way to reason about goals, abstraction, and relevance
- a basis for comparing design alternatives

## What this framework is not

- not a control algorithm
- not a planning method
- not a model of human cognition
- not a substitute for domain-specific safety analysis

## Core documents

This repo is organized around a small set of documents:

- README.md — high-level overview and navigation
- sa_operator.md — formal operator definition
- design_principle.md — rationale and design principles
- analysis_guidelines.md — how to analyze a real system with this framework
- notations.md — notation and naming conventions
- limitations.md — scope and intentional boundaries
- perception_operator.md — perception operator specification
- comprehension_operator.md — comprehension operator specification
- projection_operator.md — projection operator specification
- projection_patterns.md — common projection patterns
- sa_architecture_and_sw_2_0.md — broader architectural framing

## Repository map

- Overview and theory
  - README.md
  - sa_operator.md
  - design_principle.md
- Formal specification
  - perception_operator.md
  - comprehension_operator.md
  - projection_operator.md
  - notations.md
- Analysis and use
  - analysis_guidelines.md
  - projection_patterns.md
  - sa_architecture_and_sw_2_0.md
- Boundaries and scope
  - limitations.md

## Typical analysis flow

When applying this framework to a system, the usual steps are:

1. Identify the observation inputs
2. Identify the perception outputs
3. Identify the internal situation model
4. Identify the projection boundary
5. Describe the goal-conditioned representation
6. Check whether downstream components are using the projected representation correctly

## Suggested next reading order

For a first-time reader:

1. README.md
2. sa_operator.md
3. design_principle.md
4. analysis_guidelines.md
5. projection_operator.md

## Summary

This repository is a conceptual specification for situation awareness as a structured system capability. Its main value is in making the representation pipeline explicit, inspectable, and reusable across system designs.
