# Analysis Guidelines for Situation Awareness Systems

This document provides practical guidance for analyzing situation awareness (SA) in existing or proposed systems using the Situation Awareness framework defined in this repository. The emphasis is on concrete system artifacts and representations rather than abstract qualities, performance metrics, or behavioral outcomes.

The guidelines are intended to support architectural reasoning, design reviews, and post-incident analysis by making representational choices explicit.

---

## 1. Purpose and Scope of Analysis

Analysis using the SA framework aims to understand how a system represents its environment internally, how that representation is reduced, and how the resulting information is made available to downstream components.

The focus is not on whether the system behaved optimally or correctly, but on how information was structured, transformed, and exposed. This type of analysis can be carried out without access to ground truth and without assuming that a single “correct” behavior exists.

---

## 2. Identifying the Situation Awareness Pipeline

The first step in analysis is to identify the components that together form the system’s situation awareness pipeline. These components correspond conceptually to perception, comprehension, and projection, even if they are not labeled as such in the implementation.

At this stage, the analyst should identify where raw data enters the system, where it is transformed into semantic entities or signals, where these are combined into an internal situation representation, and where that representation is reduced or summarized for downstream use.

*Example:*  
In an industrial process monitoring system, raw sensor measurements such as temperatures, pressures, and flow rates are first validated and filtered (perception). These signals are then integrated into an internal process model that estimates operating regimes and latent fault hypotheses (comprehension). Finally, selected aspects of this internal model are reduced to alarm states and health indicators presented to operators or supervisory systems (projection).

---

## 3. Analyzing Perception: What Is Detected

Analysis of perception focuses on what the system is capable of detecting and representing as basic semantic elements.

This involves identifying the concrete outputs of perception, such as named signals, events, or measurements, together with any associated attributes like validity flags, confidence measures, or timestamps. These outputs are often visible as variables, message fields, or logged data streams.

Equally important is identifying what the system cannot detect by design. Any limitation at this stage constrains all downstream reasoning and cannot be compensated for later.

*Example:*  
A refinery monitoring system may measure temperature and pressure at multiple points but lack direct measurement of material composition inside a reactor. This means certain internal process deviations are fundamentally unobservable and must remain hypothetical.

---

## 4. Analyzing Comprehension: What Is Explained

Comprehension analysis focuses on how detected elements are transformed into an internal explanation of what may be occurring in the system.

This typically involves identifying internal state variables, models, or hypotheses that encode causal or structural understanding. These representations aim to explain *why* observed signals look the way they do, rather than to summarize them for use.

The analyst should examine how temporal information is incorporated, whether multiple competing explanations are maintained, and what assumptions are embedded in the explanatory model.

*Example:*  
In a power plant monitoring system, sensor readings may be interpreted through a thermodynamic model that infers latent operating states such as “stable load,” “thermal stress accumulation,” or “incipient fouling.” These inferred states are not alarms or outputs, but internal explanations that organize observations over time.

At this stage, the representation is intentionally rich and may include uncertainty, redundancy, or alternative hypotheses.

---

## 5. Identifying the Projection Boundary

A critical step in SA analysis is identifying where and how the internal explanatory representation is reduced.

This projection boundary is typically where the system stops explaining the situation in detail and instead selects or constructs a representation suitable for downstream use. The projection boundary is not defined by modeling complexity, but by **intentional reduction**.

The analyst should identify the exact outputs that leave the situation awareness subsystem, along with any thresholds, mappings, or configuration parameters that control which distinctions are preserved.

*Example:*  
An internal model may track several latent degradation mechanisms simultaneously, but projection may expose only whether any mechanism exceeds a predefined safety threshold.

---

## 6. Describing the Projected Representation

The projected representation should be described concretely and precisely.

Rather than referring to it abstractly, the analyst should be able to write down the exact form of the projected output: the set of possible values, their meanings, update rates, and intended interpretation.

*Example:*  
A projected representation may consist of a single operational status variable taking values such as “normal,” “restricted,” or “shutdown required,” derived from a complex internal state estimate.

If the projected representation cannot be described precisely, this is a strong indication that projection is implicit and should be clarified.

---

## 7. Inferring the Goal from the Projection

In this framework, the goal is inferred from the structure of the projected representation itself.

By examining which internal distinctions are preserved and which are collapsed, the analyst can infer what the system considers relevant at the point of projection.

*Example:*  
If multiple internal degradation mechanisms are all collapsed into a single “unsafe” state, the implicit goal is rapid risk signaling rather than diagnostic differentiation.

---

## 8. Assessing Alignment with Downstream Use

Once the projected representation is understood, it should be compared to how downstream components actually use it.

Misalignment occurs when downstream logic assumes distinctions that are no longer present, or when projection preserves distinctions that downstream components ignore.

*Example:*  
A maintenance planner may assume that different fault causes are distinguished, while the projection exposes only a generic “maintenance required” flag.

---

## 9. Comparative Analysis of Alternative Designs

The SA framework supports comparison between different system designs by focusing on representational structure rather than performance metrics.

When comparing two designs, the analyst should examine how perception outputs differ, how explanatory situation representations are constructed, and how projection differs.

*Example:*  
Two systems may share the same internal degradation model, but differ in that one projects a single binary alarm while the other projects a graded health margin. Differences in operational behavior follow directly from this projection choice.

---

## 10. Application to Failure and Incident Analysis

In post-incident analysis, the framework can be used to trace failures through the SA pipeline.

Rather than attributing failure directly to incorrect actions, the analyst should examine what was detected, how it was explained internally, and what aspects of that explanation were ultimately projected.

*Example:*  
An unexpected equipment failure may be traced to a projection that suppressed early internal indicators of stress in favor of a simplified operational status.

---

## 11. Summary

These guidelines provide a concrete method for applying the Situation Awareness framework to real systems. By clearly separating internal explanation from goal-driven reduction, the framework enables precise reasoning about abstraction, relevance, and system behavior without conflating modeling with decision support.

The embedded examples demonstrate how comprehension and projection play distinct and non-overlapping roles in industrial monitoring contexts.