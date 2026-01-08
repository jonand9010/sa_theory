# Projection Patterns in Situation Awareness Systems

This document describes common **projection patterns** used in situation awareness (SA) systems. A projection pattern characterizes how a rich internal situation representation is reduced to a goal-relevant form.

The purpose of this document is not to prescribe correct projections, but to provide a vocabulary for recognizing, comparing, and reasoning about projection designs across systems and domains.

---

## 1. Purpose of Projection Patterns

The projection operator maps an internal situation representation S to a goal-relevant representation space U_g.

    R_g : S → U_g

While the structure of S is often complex and domain-specific, the structure of U_g tends to follow recurring forms. These recurring forms constitute projection patterns.

Identifying projection patterns makes it possible to:
- compare systems with different internal models
- reason about abstraction choices explicitly
- diagnose representational mismatches
- communicate design intent clearly

---

## 2. Classification-Based Projection

### Description

Classification-based projection maps situations to a finite set of discrete categories. Each category represents a qualitatively distinct situation from the perspective of the goal.

### Typical Structure

    U_g = {c_1, c_2, ..., c_k}

Examples include:
- normal / degraded / unsafe
- nominal / warning / critical
- allowed / restricted / forbidden

### Use Cases

- safety monitoring
- supervisory control
- alerting and escalation
- human–machine interfaces

### Characteristics

- highly interpretable
- stable under noise
- limited expressiveness
- may hide fine-grained distinctions

---

## 3. Margin-Based Projection

### Description

Margin-based projection maps situations to scalar or vector-valued quantities representing distance to constraints, limits, or safety boundaries.

### Typical Structure

    U_g ⊆ R^n

Examples include:
- minimum distance to obstacle
- time-to-collision
- energy or resource reserves
- constraint slack variables

### Use Cases

- safety envelopes
- constraint monitoring
- feasibility assessment
- downstream optimization preparation

### Characteristics

- preserves quantitative structure
- sensitive to modeling assumptions
- supports graded responses
- may be harder to interpret directly

---

## 4. Indicator-Based Projection

### Description

Indicator-based projection maps situations to one or more scalar indicators that summarize aspects of relevance without encoding explicit constraints.

### Typical Structure

    U_g ⊆ R^k

Examples include:
- risk scores
- confidence levels
- priority indices
- health indicators

### Use Cases

- ranking or prioritization
- resource allocation
- monitoring dashboards
- decision support

### Characteristics

- compact representation
- flexible aggregation
- may obscure causal structure
- sensitive to calibration

---

## 5. Mode-Based Projection

### Description

Mode-based projection maps situations to discrete operational modes that determine how the system should behave or be interpreted downstream.

### Typical Structure

    U_g = {m_1, m_2, ..., m_k}

Examples include:
- autonomous / assisted / manual
- nominal / fallback / emergency
- exploration / execution / recovery

### Use Cases

- supervisory logic
- fault handling
- system reconfiguration
- responsibility handover

### Characteristics

- strong behavioral implications
- coarse abstraction
- clear system-level meaning
- requires careful transition logic

---

## 6. Conservative (Worst-Case) Projection

### Description

Conservative projection maps situations to worst-case or lower-bound summaries that emphasize safety or robustness.

### Typical Structure

    U_g = worst_case_summary(S)

Examples include:
- maximum estimated risk
- minimum remaining margin
- upper bounds on uncertainty

### Use Cases

- safety-critical systems
- certification contexts
- fail-safe monitoring
- conservative decision-making

### Characteristics

- robust to uncertainty
- may be overly pessimistic
- reduces sensitivity to noise
- can mask benign situations

---

## 7. Interface-Driven Projection

### Description

Interface-driven projection structures the situation representation according to the needs of a human or external system interface.

### Typical Structure

    U_g = interface-aligned representation

Examples include:
- status lights or indicators
- alert messages
- summarized reports
- visual overlays

### Use Cases

- human–machine interaction
- supervisory control
- explainability and transparency
- regulatory reporting

### Characteristics

- optimized for interpretability
- often domain-specific
- may discard internal detail
- tightly coupled to presentation constraints

---

## 8. Combining Projection Patterns

In practice, projection operators often combine multiple patterns.

Examples:
- classification + margin (e.g. risk class with buffer)
- indicator + conservative bound
- mode selection driven by margin thresholds

Such combinations should be treated as intentional design choices rather than ad hoc implementations.

---

## 9. Relationship to Goals

The choice of projection pattern is determined by the goal g.

Different goals applied to the same internal situation representation may result in different projection patterns, even when perception and comprehension remain unchanged.

This highlights the role of projection as the primary mechanism through which goals influence situation awareness.

---

## 10. Summary

Projection patterns describe common structural forms of goal-relevant situation representations. They provide a vocabulary for reasoning about abstraction choices in situation awareness systems without committing to specific algorithms or domains.

By identifying and naming projection patterns, system designers can make relevance explicit, compare architectures systematically, and reason about the consequences of abstraction decisions.