# Worked Example: Edge-Device Situation Awareness

This example applies the SA framework to an edge device that collects measurements and sends them to a server.

It demonstrates belief construction with overlapping faults, goal-dependent projections, and checks for information loss.

All probabilities are illustrative assumptions, not measurements from a real device.

## 1. Scenario and Situation Space

The device's outgoing queue is growing, and messages are arriving late.

Network degradation and sending-worker degradation can occur separately or together.

Represent them using two binary variables:

```text
L = network degradation present
W = worker degradation present
```

The situation space contains their four combinations:

| State | Network degraded? | Worker degraded? |
| --- | --- | --- |
| Neither | No | No |
| Network only | Yes | No |
| Worker only | No | Yes |
| Both | Yes | Yes |

```text
S = {
    neither,
    network_only,
    worker_only,
    both
}
```

These joint states are mutually exclusive. The faults themselves are not.

The model assumes that the fault combination remains approximately constant during a one-minute observation window.

Other degradation mechanisms are outside this model. “Neither” means neither of these two faults is present; it does not establish that every aspect of the device is healthy.

## 2. Perception

The observations and corresponding concepts are:

| Observation during the window | Perceived concept |
| --- | --- |
| Outgoing queue increased from 20 to 120 messages | `queue_growing = true` |
| Eight of ten messages exceeded a five-second delivery deadline | `delivery_delayed = true` |

Records also carry timestamps and evidence-quality information.

Perception detects symptoms. It does not conclude which faults are present.

Let `E` denote the combined evidence:

```text
E = queue_growing AND delivery_delayed
```

For this example, comprehension summarizes the concept history using `E`. It does not use exact queue sizes, individual delay durations, or their temporal ordering.

## 3. Comprehension Model

Specify priors and joint evidence likelihoods:

| State `s` | Prior `p(s)` | Likelihood `p(E | s)` |
| --- | ---: | ---: |
| Neither | 0.60 | 0.05 |
| Network only | 0.20 | 0.40 |
| Worker only | 0.10 | 0.80 |
| Both | 0.10 | 0.90 |

The likelihood is the probability of the combined symptoms given a joint fault state.

The table does not assume that:

- the two faults are independent
- the two symptoms are conditionally independent

The “both” state has its own likelihood. It is not obtained by multiplying the single-fault likelihoods.

Bayes' rule gives:

```text
b(s) = p(s | E)
     = p(E | s) p(s) / ∑_{s'} p(E | s') p(s')
```

The normalizing constant is:

```text
p(E) = 0.05 × 0.60
     + 0.40 × 0.20
     + 0.80 × 0.10
     + 0.90 × 0.10
     = 0.28
```

The posterior belief is:

| State | Unnormalized weight | Posterior probability |
| --- | ---: | ---: |
| Neither | 0.03 | 3/28 ≈ 0.1071 |
| Network only | 0.08 | 8/28 ≈ 0.2857 |
| Worker only | 0.08 | 8/28 ≈ 0.2857 |
| Both | 0.09 | 9/28 ≈ 0.3214 |

The four probabilities sum to one.

## 4. Operational-Condition Projection

The consumer requirement is:

> Report the probability that at least one of the two modeled faults is present.

Define:

```text
R_condition(b) =
    b(network_only) + b(worker_only) + b(both)

Equivalently:
R_condition(b) = 1 - b(neither)
```

The output space is:

```text
U_condition = [0, 1]
```

For the calculated belief:

```text
probability_any_modeled_fault = 25/28 ≈ 0.8929
```

This projection discards fault identity and overlap.

Its output is not automatically the probability that the next message misses its deadline. That requires a predictive model.

## 5. Marginal Diagnostic Projection

The consumer requirement is:

> Report the probability of each fault being present, whether alone or together.

Define:

```text
p_network = b(network_only) + b(both)
p_worker  = b(worker_only) + b(both)

R_marginal(b) = (p_network, p_worker)
```

The output space is:

```text
U_marginal = [0, 1]²
```

For the calculated belief:

```text
probability_network_degradation = 17/28 ≈ 0.6071
probability_worker_degradation  = 17/28 ≈ 0.6071
```

These probabilities need not sum to one because the faults overlap.

The marginal projection does not generally retain the full joint belief.

## 6. Diagnostic Projection with Overlap

A richer consumer requirement includes the probability that both faults are present.

Define:

```text
R_joint(b) = (
    p_network,
    p_worker,
    p_both
)

p_both = b(both)
```

For this example:

```text
probability_network_degradation = 17/28 ≈ 0.6071
probability_worker_degradation  = 17/28 ≈ 0.6071
probability_both                = 9/28  ≈ 0.3214
```

The output space must enforce:

```text
0 ≤ p_network ≤ 1
0 ≤ p_worker ≤ 1

max(0, p_network + p_worker - 1)
    ≤ p_both
    ≤ min(p_network, p_worker)
```

These three values recover the full four-state belief:

```text
b(both)         = p_both
b(network_only) = p_network - p_both
b(worker_only)  = p_worker - p_both
b(neither)      = 1 - p_network - p_worker + p_both
```

They also recover the condition output:

```text
probability_any_modeled_fault =
    p_network + p_worker - p_both
```

Subtracting the overlap avoids counting the “both” state twice.

## 7. Information-Loss Counterexamples

### Total Fault Probability Does Not Identify Causes

Consider:

| Belief | Neither | Network only | Worker only | Both |
| --- | ---: | ---: | ---: | ---: |
| A | 0.13 | 0.80 | 0.07 | 0.00 |
| B | 0.13 | 0.07 | 0.80 | 0.00 |

Both produce:

```text
R_condition(b) = 0.87
```

Their marginal diagnostic outputs differ:

```text
R_marginal(A) = (0.80, 0.07)
R_marginal(B) = (0.07, 0.80)
```

The condition projection cannot recover fault identity.

### Marginal Probabilities Do Not Identify Overlap

Consider:

| Belief | Neither | Network only | Worker only | Both |
| --- | ---: | ---: | ---: | ---: |
| C | 0.00 | 0.50 | 0.50 | 0.00 |
| D | 0.50 | 0.00 | 0.00 | 0.50 |

Both produce:

```text
R_marginal(b) = (0.50, 0.50)
```

However:

```text
R_condition(C) = 1.00
R_condition(D) = 0.50
```

The marginal probabilities alone cannot recover the probability of at least one fault.

Assuming fault independence would supply an overlap value, but would add a modeling assumption. It would not recover information retained by the original joint belief.

## 8. Downstream Boundary

None of these projections chooses an action.

A downstream component may use them to decide whether to investigate connectivity, inspect the sending worker, or perform another intervention.

Such decisions require additional requirements, costs, constraints, or policies.

## 9. Model Limitations

The example does not establish that:

- the priors or likelihoods match a real device
- the posterior probabilities are calibrated
- the two modeled faults cover every relevant cause
- the binary symptom summary retains all useful evidence
- fault states remain constant throughout the window
- any projection supports every operational decision

Real deployment requires evidence for those assumptions.

## 10. Result

The joint belief supports several projections without changing its probabilities.

Total fault probability loses fault identity. Marginal fault probabilities lose overlap information. Marginal probabilities plus overlap recover the full belief in this four-state model.

Projection adequacy therefore depends on the consumer's exact requirement.

See [projection_adequacy.md](projection_adequacy.md) for the formal criterion.