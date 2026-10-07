# RESOLVE — Research Metrics

## 1. Purpose

This document defines the metrics that will be used to evaluate RESOLVE and compare it with the selected baselines.

Metrics are divided into:

1. resilience outcomes;
2. intervention effectiveness;
3. cascade behavior;
4. ranking quality;
5. robustness and sensitivity;
6. computational performance;
7. software performance.

Final primary metrics will be frozen before the main evaluation experiments.

---

## 2. Primary Metric

### 2.1 Cascading Impact Reduction

The primary outcome metric will measure the reduction in simulated cascading impact produced by the intervention strategy.

A general formulation is:

```text
Impact Reduction (%) =
    (Baseline Impact - Intervention Impact)
    / Baseline Impact
    × 100
```

The exact definition of `Impact` will depend on the selected case study and may combine predefined measures such as:

* affected population;
* affected critical facilities;
* service disruption;
* cascade size.

The final aggregation method must be documented before the primary experiment.

---

## 3. Population Impact

### Definition

Number or proportion of the modeled population affected by a disruption.

Possible formulation:

```text
Population Impact (%) =
    Affected Population
    / Total Modeled Population
    × 100
```

### Use

This metric evaluates whether an intervention reduces human exposure to modeled service disruption.

---

## 4. Critical Facility Impact

### Definition

Proportion of modeled critical facilities affected by the scenario.

Examples include:

* hospitals;
* emergency facilities;
* other facilities explicitly classified as critical.

```text
Facility Impact (%) =
    Affected Critical Facilities
    / Total Critical Facilities
    × 100
```

---

## 5. Service Downtime

### Definition

Duration for which a modeled service remains below its defined operational threshold.

Possible unit:

```text
hours
```

Possible aggregate:

```text
Total Service Downtime =
    Σ downtime across affected services
```

The exact aggregation will depend on the modeled service structure.

---

## 6. Cascade Size

### Definition

Number of components affected through the simulated cascade.

```text
Cascade Size =
    Number of affected nodes
```

This may be reported as:

* absolute number;
* percentage of modeled nodes.

---

## 7. Cascade Depth

### Definition

Maximum propagation distance from the initiating failure.

```text
Cascade Depth =
    Maximum dependency-path length
```

This measures how far a disruption propagates through the dependency graph.

---

## 8. Cascade Breadth

### Definition

Number of affected components at each propagation level.

Example:

```text
Level 0 → Initial failures
Level 1 → Directly dependent failures
Level 2 → Secondary failures
Level 3 → Tertiary failures
...
```

This allows the shape of a cascade to be analyzed rather than only its final size.

---

## 9. Recovery Time

### Definition

Time required for the modeled system or selected service to return to a predefined operational threshold.

Possible units:

* hours;
* days;
* simulation time steps.

Interventions that reduce recovery time may be considered more resilient even when their immediate impact is similar.

---

## 10. Intervention Cost

Each intervention should have a documented cost or resource requirement.

Depending on the case study, cost may represent:

* financial cost;
* deployment capacity;
* personnel;
* infrastructure capacity;
* computational or operational resources.

Where monetary cost is unavailable, a clearly defined normalized resource unit may be used.

---

## 11. Resilience Benefit

A general intervention benefit can be expressed as:

```text
Resilience Benefit =
    Baseline Impact - Intervention Impact
```

The specific impact measure must be defined before the corresponding experiment.

---

## 12. Intervention Efficiency

Potential formulation:

```text
Intervention Efficiency =
    Resilience Benefit
    / Intervention Cost
```

This metric evaluates how much modeled resilience improvement is achieved per unit of resource expenditure.

It is particularly relevant when interventions are subject to limited budgets.

---

## 13. Intervention Ranking

The system will generate an ordered list of candidate interventions.

Ranking quality may be evaluated using:

### Spearman Rank Correlation

Measures monotonic agreement between two rankings.

### Kendall Rank Correlation

Measures pairwise ranking agreement.

### Top-K Overlap

Measures how many of the highest-ranked interventions are shared between approaches.

```text
Top-K Overlap =
    |A ∩ B|
    / K
```

A ranking difference does not automatically imply an improvement.

Ranking metrics must be interpreted together with outcome metrics.

---

## 14. Population Protected

### Definition

Population that remains unaffected or becomes less affected because of an intervention.

Possible formulation:

```text
Population Protected =
    Population Impact without Intervention
    - Population Impact with Intervention
```

This may be reported as an absolute count or percentage.

---

## 15. Critical Facilities Protected

### Definition

Number or proportion of critical facilities whose modeled disruption is prevented or reduced by an intervention.

This can be reported alongside population-level outcomes.

---

## 16. Dependency Contribution

Where possible, the system should estimate the contribution of individual dependency relationships to cascade outcomes.

Potential measurements include:

* downstream affected nodes;
* additional cascade size;
* incremental impact;
* propagation frequency;
* intervention sensitivity.

This metric can help identify high-leverage dependencies.

---

## 17. Ranking Stability

Ranking stability measures whether intervention priorities remain consistent under controlled changes to:

* input data;
* dependency strengths;
* model parameters;
* hazard intensity;
* random seeds.

Potential measures include:

* rank correlation;
* top-k overlap;
* rank displacement.

---

## 18. Sensitivity

For a parameter `x`, a basic sensitivity measure may be represented as:

```text
Sensitivity =
    Change in Outcome
    / Change in Parameter
```

More appropriate methods may be used depending on the experimental design.

Sensitivity analysis should focus on parameters that are uncertain or have strong influence on model behavior.

---

## 19. Robustness

An intervention strategy is considered more robust if its performance remains acceptable across reasonable variations in:

* hazard intensity;
* dependency strength;
* missing data;
* model parameters;
* intervention effectiveness.

Robustness should be evaluated using predefined perturbation scenarios.

---

## 20. Simulation Runtime

### Definition

Time required to execute a complete simulation scenario.

Unit:

```text
seconds
```

Measurements should distinguish between:

* cold-start runtime;
* warm runtime;
* repeated-run runtime where relevant.

---

## 21. Scenario Throughput

### Definition

Number of scenarios processed per unit of time.

```text
Throughput =
    Completed Scenarios
    / Execution Time
```

This metric is useful when performing large experimental sweeps.

---

## 22. Memory Consumption

Measure peak memory usage during representative experiments.

Possible reporting units:

* MB;
* GB.

Measurements should identify the scenario size and configuration.

---

## 23. Graph Processing Performance

Potential measurements include:

* graph construction time;
* graph serialization time;
* graph traversal time;
* dependency propagation time;
* graph size;
* number of nodes;
* number of edges.

These metrics help identify scalability limits.

---

## 24. API Performance

For backend endpoints where relevant:

### Latency

Measure:

* median;
* p95;
* p99.

### Throughput

Measure:

```text
Requests per second
```

### Error Rate

Measure:

```text
Failed Requests
/
Total Requests
× 100
```

API benchmarks must document:

* endpoint;
* payload;
* environment;
* concurrency;
* dataset size;
* software version.

---

## 25. Database Performance

Potential metrics include:

* query latency;
* p95 query latency;
* spatial-query execution time;
* index effectiveness;
* database CPU;
* database memory;
* query-plan characteristics.

Only queries relevant to actual RESOLVE workloads should be benchmarked.

---

## 26. Prediction Metrics

If machine-learning or predictive components are introduced, appropriate metrics may include:

### Regression

* MAE;
* RMSE;
* R² where appropriate.

### Classification

* precision;
* recall;
* F1;
* ROC-AUC where appropriate;
* PR-AUC where appropriate.

### Probabilistic Predictions

* calibration;
* Brier score.

Prediction metrics will only be used where a genuine predictive task exists.

---

## 27. Statistical Reporting

Where repeated experiments produce comparable observations, results should report:

* mean;
* median;
* standard deviation;
* interquartile range where appropriate;
* confidence intervals;
* effect size;
* sample size.

The statistical method must match the experimental design and distributional assumptions.

---

## 28. Repeated Runs

Stochastic experiments should use multiple runs where appropriate.

Each run should record:

* random seed;
* configuration;
* dataset version;
* model version;
* execution environment.

Deterministic experiments should be reproducible from the same inputs and configuration whenever technically possible.

---

## 29. Primary Evaluation Table

The main experiment should ultimately support a table similar to:

| Metric                   | B1 | B2 | B0 | RESOLVE |
| ------------------------ | -: | -: | -: | ------: |
| Population Impact        |  — |  — |  — |       — |
| Critical Facility Impact |  — |  — |  — |       — |
| Cascade Size             |  — |  — |  — |       — |
| Cascade Depth            |  — |  — |  — |       — |
| Service Downtime         |  — |  — |  — |       — |
| Recovery Time            |  — |  — |  — |       — |
| Intervention Cost        |  — |  — |  — |       — |
| Resilience Benefit       |  — |  — |  — |       — |
| Intervention Efficiency  |  — |  — |  — |       — |
| Simulation Runtime       |  — |  — |  — |       — |

Values will be populated only after experiments are executed.

---

## 30. Metric Selection Rules

Metrics must satisfy the following principles:

1. Define the metric before the primary experiment.
2. State the unit of measurement.
3. Document the calculation.
4. Use the same definition across compared methods.
5. Avoid changing the primary metric after observing results.
6. Report negative or neutral results.
7. Distinguish statistical significance from practical significance.
8. Record missing or unavailable measurements.
9. Avoid metrics that cannot be reliably measured from available data.

---

## 31. Primary vs Secondary Metrics

### Primary

* cascading impact reduction

### Secondary

* population impact;
* critical facility impact;
* cascade size;
* cascade depth;
* service downtime;
* recovery time;
* intervention efficiency;
* intervention ranking;
* robustness;
* sensitivity.

### Engineering

* simulation runtime;
* scenario throughput;
* memory consumption;
* graph-processing time;
* API latency;
* API throughput;
* database performance.

The final primary metric may be refined after the literature review and pilot experiment.

---

## 32. Metric Freeze

Before the main evaluation:

* primary metric definitions will be frozen;
* baseline definitions will be frozen;
* principal experiment configurations will be frozen;
* intervention candidates will be frozen where practical.

Any subsequent changes must be documented with a reason and impact assessment.

---

## 33. Measurement Principle

> **If a claim cannot be connected to a defined measurement, it should not be presented as an experimental result.**
