# Worked Example: Edge-Device Situation Awareness

This example applies the SA framework to an edge device that collects measurements and sends them to a server.

It demonstrates explicit belief construction, two goal-dependent projections, and a projection-adequacy check.

All probabilities are illustrative assumptions, not measurements from a real device.

## 1. Scenario and Scope

The device's outgoing queue is growing, and messages are arriving late.

Two consumers require different information:

- Operational-condition monitoring requires the probability of a degraded sending regime.
- Fault diagnosis requires separate probabilities for network and sending-worker degradation.

The model uses three mutually exclusive regimes, assumed approximately constant during a one-minute observation window:

```text
S = {
    normal,
    network_degradation,
    worker_degradation
}
```

Network and worker degradation are treated as exclusive in this example. Combined faults and other causes are excluded.

A practical implementation would need to assess these assumptions and extend the situation space where necessary.

## 2. Perception

The raw observations and corresponding concept records are:

| Observation during the window | Perceived concept |
| --- | --- |
| Outgoing queue increased from 20 to 120 messages | `queue_growing = true` |
| Eight of ten messages exceeded a five-second delivery deadline | `delivery_delayed = true` |

Records also carry timestamps and evidence-quality information.

Perception identifies symptoms. It does not conclude that the network or sending worker is faulty.

Let `E` denote the combined evidence:

```text
E = queue_growing AND delivery_delayed
```

For this example, comprehension summarizes the concept history using `E`. This is a deliberate reduction: the illustrative model does not use the exact queue size, delay durations, or timing of individual messages.

## 3. Comprehension Model

Specify prior probabilities and likelihoods:

| Regime `s` | Prior `p(s)` | Joint evidence likelihood `p(E | s)` |
| --- | ---: | ---: |
| Normal | 0.60 | 0.05 |
| Network degradation | 0.30 | 0.40 |
| Worker degradation | 0.10 | 0.80 |

The likelihood is the probability of the combined symptoms given a regime.

It is not the probability of the regime given the symptoms.

The table directly specifies joint evidence likelihoods. It does not assume that queue growth and delayed delivery are conditionally independent.

Bayes' rule gives:

```text
b(s) = p(s | E)
     = p(E | s) p(s) / ∑_{s'} p(E | s') p(s')
```

The normalizing constant is:

```text
p(E) = 0.05 × 0.60
     + 0.40 × 0.30
     + 0.80 × 0.10
     = 0.23
```

The posterior belief is:

| Regime | Unnormalized weight | Posterior probability |
| --- | ---: | ---: |
| Normal | 0.03 | 3/23 ≈ 0.1304 |
| Network degradation | 0.12 | 12/23 ≈ 0.5217 |
| Worker degradation | 0.08 | 8/23 ≈ 0.3478 |

The posterior probabilities sum to one.

Although worker degradation makes the symptoms more likely, network degradation has the higher posterior probability because its prior is larger.

## 4. Operational-Condition Projection

The consumer requirement is:

> Report the probability that the device is in either modeled degraded sending regime.

Define:

```text
R_condition(b) =
    b(network_degradation) + b(worker_degradation)
```

The output space is:

```text
U_condition = [0, 1]
```

For the calculated belief:

```text
probability_degraded_regime = 20/23 ≈ 0.8696
```

This projection discards the distinction between the two degradation causes.

Its output is not automatically the probability that the next message misses its deadline. That quantity would require a predictive model.

## 5. Diagnostic Projection

The consumer requirement is:

> Report separate probabilities for network degradation and worker degradation.

Define:

```text
R_diagnosis(b) = (
    b(network_degradation),
    b(worker_degradation)
)
```

Because the regimes are exclusive:

```text
U_diagnosis = {
    (p_network, p_worker) ∈ [0, 1]² :
    p_network + p_worker ≤ 1
}
```

For the calculated belief:

```text
probability_network_degradation = 12/23 ≈ 0.5217
probability_worker_degradation  = 8/23  ≈ 0.3478
```

In this three-state model, the normal probability can be recovered as:

```text
p_normal = 1 - p_network - p_worker
```

The diagnostic projection therefore retains the full categorical belief, despite exposing only two fields.

This illustrates that a projection need not always discard information.

## 6. Adequacy Counterexample

Consider two possible beliefs:

| Belief | Normal | Network degradation | Worker degradation |
| --- | ---: | ---: | ---: |
| A | 0.13 | 0.80 | 0.07 |
| B | 0.13 | 0.07 | 0.80 |

Both produce:

```text
R_condition(b) = 0.87
```

However:

```text
R_diagnosis(A) = (0.80, 0.07)
R_diagnosis(B) = (0.07, 0.80)
```

No function receiving only `0.87` can recover which diagnostic pair was present.

The condition projection is adequate for reporting total degradation probability, but inadequate for reporting separate cause probabilities.

The diagnostic projection supports both requirements: total degradation probability can be obtained by adding its two outputs.

## 7. Downstream Boundary

Neither projection chooses an action.

A downstream component may use these outputs when deciding whether to investigate connectivity, inspect the sending worker, or perform another intervention.

Such decisions require additional requirements, costs, constraints, or policies. They are outside this example's SA operators.

## 8. Model Limitations

The example does not establish that:

- the specified priors or likelihoods match a real device
- the resulting probabilities are calibrated
- the three regimes cover all relevant causes
- the binary symptom summary retains all useful evidence
- a regime remains constant throughout the window
- either projection supports every operational decision

Real deployment requires evidence for those assumptions.

## 9. Result

The same comprehension belief supports different projections without changing its probabilities.

Projection adequacy depends on the consumer's specified requirement. Retaining total degradation probability is sufficient for one requirement and insufficient for another.

See [projection_adequacy.md](projection_adequacy.md) for the formal criterion.