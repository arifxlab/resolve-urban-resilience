# RESOLVE — Research Results Framework

## 1. Purpose

This document defines how RESOLVE experimental results are produced, validated, stored, interpreted, and translated into research evidence.

The purpose is to ensure that:

* experimental outputs are traceable;
* metrics are calculated consistently;
* results are not selectively reported;
* negative and inconclusive findings are preserved;
* statistical and practical significance are distinguished;
* uncertainty is visible;
* engineering measurements are not confused with scientific evidence;
* paper claims remain bounded by experimental evidence.

The central research chain is:

```text
Experiment
    ↓
Raw Output
    ↓
Validated Result
    ↓
Metric Calculation
    ↓
Comparison
    ↓
Statistical Analysis
    ↓
Interpretation
    ↓
Evidence
    ↓
Paper Claim
```

A result MUST NOT become a research claim simply because the result exists.

---

## 2. Research Question

The primary research question is:

> Can a dependency-aware urban digital twin improve the prioritization of resilience interventions compared with isolated risk assessment?

Results should therefore focus on whether dependency-aware modeling provides measurable value in intervention prioritization under controlled conditions.

---

## 3. Results Principles

RESOLVE results MUST follow these principles:

1. **Evidence before claims.**
2. **Metrics before interpretation.**
3. **Predefined primary metrics.**
4. **Consistent definitions across methods.**
5. **Fair baseline comparison.**
6. **Complete reporting where practical.**
7. **Negative results are valid.**
8. **Inconclusive results are valid.**
9. **Statistical significance is not practical significance.**
10. **Simulation results are not automatically real-world predictions.**
11. **Uncertainty must be reported.**
12. **Outliers must not be silently removed.**
13. **Unexpected results must be investigated rather than hidden.**
14. **Results must remain reproducible.**
15. **Paper claims must never exceed available evidence.**

---

## 4. Result Categories

RESOLVE distinguishes several result categories.

### 4.1 Scientific Results

Results directly relevant to the research question or hypotheses.

Examples:

* cascading impact reduction;
* intervention ranking improvement;
* population protected;
* critical facilities protected;
* cascade reduction;
* ranking stability.

---

### 4.2 Model Results

Outputs describing model behavior.

Examples:

* cascade pathways;
* dependency activation;
* state transitions;
* graph structure;
* recovery sequence.

These may support scientific results but are not automatically evidence of real-world effectiveness.

---

### 4.3 Engineering Results

Measurements of system implementation.

Examples:

* API latency;
* simulation runtime;
* throughput;
* memory usage;
* database performance;
* test coverage.

Engineering results demonstrate system properties rather than directly answering the research question.

---

### 4.4 Data Quality Results

Measurements describing the research inputs.

Examples:

* dataset completeness;
* missing values;
* spatial coverage;
* temporal coverage;
* dependency confidence;
* validation errors.

Data-quality results are essential for interpreting scientific findings.

---

## 5. Result Hierarchy

Results should be organized into:

```text
Experiment
├── Run
│   ├── Raw Outputs
│   ├── Validation
│   ├── Metrics
│   └── Execution Metadata
│
├── Aggregate Results
│   ├── Summary Statistics
│   ├── Comparisons
│   └── Uncertainty
│
└── Research Interpretation
    ├── Hypothesis Assessment
    ├── Limitations
    └── Evidence Statement
```

This hierarchy prevents summary tables from becoming disconnected from the underlying execution evidence.

---

## 6. Result Identifiers

Results should use stable identifiers.

Recommended format:

```text
RESULT-001
RESULT-002
RESULT-003
```

Run identifiers:

```text
RUN-0001
RUN-0002
RUN-0003
```

Experiment identifiers:

```text
EXP-001
EXP-002
EXP-003
```

The relationship should remain traceable:

```text
RESULT-001
    ↓
RUN-0001
    ↓
EXP-001
```

---

## 7. Raw Results

Raw results are outputs generated directly by an experiment.

Examples:

* simulation state;
* cascade events;
* intervention selection;
* service states;
* population impact;
* facility impact;
* runtime;
* event logs.

Raw outputs should be preserved where practical.

They SHOULD NOT be manually edited to produce final research tables.

---

## 8. Validated Results

Raw outputs become validated results only after consistency checks.

Validation should verify:

* experiment completed;
* inputs match configuration;
* simulation states are valid;
* dependency references are valid;
* intervention constraints are respected;
* budget is respected;
* metrics can be calculated;
* expected output fields exist;
* no critical execution errors occurred.

A failed validation should prevent the result from being presented as a valid primary result.

---

## 9. Result Status

Each result should have a status.

Recommended states:

```text
GENERATED
VALIDATED
PARTIALLY_VALIDATED
INVALID
SUPERSEDED
BLOCKED
```

### GENERATED

Produced by an experiment but not yet fully validated.

### VALIDATED

Passed required validation checks.

### PARTIALLY_VALIDATED

Some outputs are valid but one or more expected checks remain incomplete.

### INVALID

Cannot be trusted for research interpretation.

### SUPERSEDED

Replaced by a newer valid result while retaining historical traceability.

### BLOCKED

Could not be validated because a required dependency or resource was unavailable.

---

## 10. Primary Metric

The primary candidate metric is:

```text
Cascading Impact Reduction (%)
```

Calculated as:

```text
Impact Reduction (%) =
    (Baseline Impact - Intervention Impact)
    / Baseline Impact × 100
```

The exact definition of `Impact` MUST be frozen before the primary evaluation.

Possible components include:

* affected population;
* critical facility impact;
* service disruption;
* cascade size.

The final aggregation method must be documented and consistently applied.

---

## 11. Secondary Metrics

Secondary metrics may include:

### Population Impact

Number or proportion of population affected.

### Critical Facility Impact

Number or proportion of critical facilities affected.

### Service Downtime

Duration of service disruption.

### Cascade Size

Number of entities affected through the cascade.

### Cascade Depth

Maximum number of dependency transitions in a cascade.

### Cascade Breadth

Number of downstream entities affected.

### Recovery Time

Time required to return to a defined acceptable state.

### Population Protected

Population whose simulated disruption is avoided or reduced by an intervention.

### Critical Facilities Protected

Critical facilities whose simulated disruption is avoided or reduced.

### Intervention Cost

Resources required to implement an intervention.

### Resilience Benefit

Measured improvement produced by an intervention.

### Intervention Efficiency

Conceptually:

```text
Efficiency =
    Resilience Benefit / Intervention Cost
```

The exact benefit definition must be specified before use.

---

## 12. Ranking Metrics

Intervention ranking comparisons may use:

* Spearman rank correlation;
* Kendall rank correlation;
* top-k overlap;
* rank displacement;
* selected intervention overlap.

Example:

```text
Baseline Ranking
1. A
2. B
3. C
4. D

RESOLVE Ranking
1. C
2. A
3. D
4. B
```

A ranking difference is descriptive.

It becomes evidence of improvement only when associated with better downstream resilience outcomes under controlled conditions.

---

## 13. Engineering Metrics

Engineering measurements may include:

* simulation runtime;
* experiment runtime;
* API latency;
* p50 latency;
* p95 latency;
* p99 latency;
* throughput;
* memory usage;
* graph construction time;
* database query performance;
* test coverage.

These should be reported separately from scientific outcome metrics.

---

## 14. Result Comparison

The primary comparison should be:

```text
Baseline
    vs.
Dependency-Aware RESOLVE
```

under equivalent:

* datasets;
* scenarios;
* intervention candidates;
* budgets;
* evaluation horizons;
* outcome definitions.

Comparisons should report both absolute and relative differences where meaningful.

---

## 15. Absolute Difference

For metric `M`:

```text
Absolute Difference =
    M_RESOLVE - M_Baseline
```

The interpretation depends on the metric.

For example, lower cascade size is generally favorable, while higher population protected is favorable.

Metric direction MUST be explicitly documented.

---

## 16. Relative Difference

Where appropriate:

```text
Relative Difference (%) =
    (M_RESOLVE - M_Baseline)
    / |M_Baseline| × 100
```

Relative differences should not be used blindly when the baseline approaches zero.

In such cases, an alternative measure should be selected.

---

## 17. Statistical Analysis

Statistical analysis must reflect the actual experimental design.

Potential methods include:

* paired tests;
* non-parametric paired tests;
* bootstrap confidence intervals;
* permutation tests;
* effect-size analysis.

The selected method should be documented before interpreting the primary results.

The project should avoid selecting statistical tests solely because they produce favorable outcomes.

---

## 18. Statistical vs Practical Significance

Results must distinguish:

```text
Statistical Significance
        vs.
Practical Significance
```

A result may be statistically significant but practically negligible.

A result may also be practically meaningful but statistically uncertain because of:

* small sample size;
* limited scenarios;
* high variance;
* incomplete data.

Both dimensions should be reported.

---

## 19. Confidence and Uncertainty

Where repeated observations permit, report uncertainty using appropriate methods.

Possible approaches:

* confidence intervals;
* bootstrap intervals;
* empirical distributions;
* sensitivity ranges;
* scenario ranges.

The choice depends on the experimental design.

Uncertainty should not be hidden by reporting only a single average.

---

## 20. Distribution Reporting

When multiple runs or scenarios are available, avoid relying exclusively on means.

Potential statistics include:

* mean;
* median;
* standard deviation;
* interquartile range;
* minimum;
* maximum;
* confidence interval.

The selected summary should match the distribution and research question.

---

## 21. Outlier Handling

Outliers MUST NOT be removed automatically.

If an observation appears anomalous:

1. identify it;
2. investigate the cause;
3. determine whether it represents valid system behavior;
4. document the decision;
5. retain the original observation where practical.

Possible causes include:

* data errors;
* simulation bugs;
* unusual but valid scenarios;
* hardware anomalies;
* configuration mistakes.

Any exclusion from statistical analysis must be documented.

---

## 22. Missing Results

Missing results should remain visible.

Recommended states:

```text
AVAILABLE
MISSING
INVALID
NOT_APPLICABLE
BLOCKED
```

A missing experiment should not silently disappear from a results table.

Reasons for missingness should be recorded where known.

---

## 23. Negative Results

Negative findings MUST be preserved.

Examples:

* RESOLVE performs similarly to the baseline;
* dependency modeling provides no measurable improvement;
* rankings remain unchanged;
* computational cost exceeds benefit;
* results are highly sensitive to dependency assumptions.

Negative results may be scientifically valuable because they identify conditions under which the proposed method does not provide additional value.

---

## 24. Inconclusive Results

An experiment may be inconclusive because:

* insufficient data;
* high uncertainty;
* inadequate sample size;
* unstable dependencies;
* conflicting outcomes;
* implementation limitations.

An inconclusive result should not be converted into a positive or negative claim without justification.

---

## 25. Hypothesis Evaluation

Each hypothesis should eventually receive an evidence status.

Recommended statuses:

```text
SUPPORTED
PARTIALLY_SUPPORTED
NOT_SUPPORTED
INCONCLUSIVE
NOT_TESTED
```

Example:

```text
H1
Dependency-aware prioritization reduces simulated cascading impact.

Evidence:
EXP-001
EXP-002
EXP-003

Status:
SUPPORTED / PARTIALLY_SUPPORTED /
NOT_SUPPORTED / INCONCLUSIVE
```

The status must be based on predefined evaluation criteria.

---

## 26. Hypothesis Evidence Matrix

A final research report should contain a matrix similar to:

| Hypothesis | Experiments | Primary Evidence      | Result | Status |
| ---------- | ----------- | --------------------- | ------ | ------ |
| H1         | EXP-001–003 | Impact reduction      | TBD    | TBD    |
| H2         | EXP-002     | Cascade pathways      | TBD    | TBD    |
| H3         | EXP-004     | Ranking displacement  | TBD    | TBD    |
| H4         | EXP-002–003 | Downstream impact     | TBD    | TBD    |
| H5         | EXP-001–003 | Benefit/cost          | TBD    | TBD    |
| H6         | EXP-002     | Cascade reduction     | TBD    | TBD    |
| H7         | EXP-007     | Runtime               | TBD    | TBD    |
| H8         | EXP-005     | Sensitivity           | TBD    | TBD    |
| H9         | EXP-006     | Robustness            | TBD    | TBD    |
| H10        | EXP-001–003 | Controlled comparison | TBD    | TBD    |

`TBD` values are placeholders until experiments are actually executed.

---

## 27. Result Tables

Research result tables should clearly distinguish:

* baseline;
* proposed method;
* scenario;
* metric;
* units;
* aggregation;
* uncertainty.

Example:

| Scenario | Method | Impact | Impact Reduction | Runtime |
| -------- | ------ | -----: | ---------------: | ------: |
| SCN-001  | B0     |    TBD |                — |     TBD |
| SCN-001  | P0     |    TBD |              TBD |     TBD |

No placeholder should be replaced with invented values.

---

## 28. Result Visualization

Visualizations may be used to communicate:

* intervention rankings;
* cascade pathways;
* impact distributions;
* population impact;
* facility impact;
* recovery curves;
* sensitivity;
* runtime scaling;
* spatial outcomes.

Visualization design must not exaggerate differences.

Axes, scales, units, uncertainty, and sample sizes should be visible where relevant.

---

## 29. Spatial Results

If spatial outputs are generated, record:

* geographic extent;
* CRS;
* spatial resolution;
* dataset version;
* scenario;
* metric;
* aggregation method.

Maps should distinguish between:

* observed data;
* derived data;
* modeled data;
* simulated results.

A simulated impact map must not be presented as an observed disaster map.

---

## 30. Cascade Results

Cascade results should preserve the causal structure represented by the simulation.

A cascade record should identify:

```text
Initial Event
    ↓
Affected Component
    ↓
Dependency
    ↓
Downstream Effect
    ↓
Subsequent State
    ↓
Recovery
```

The word "causal" should be used carefully.

Within the simulation, an event may causally follow another event according to model rules.

This does not establish that the same relationship has been empirically proven in the real world.

---

## 31. Intervention Selection Results

For budget-constrained experiments, record:

* candidate interventions;
* selected interventions;
* total cost;
* unused budget;
* method ranking;
* achieved outcome;
* alternative feasible selections where relevant.

Budget violations invalidate the corresponding result unless explicitly identified as a failed execution.

---

## 32. Sensitivity Results

Sensitivity analysis should report how key outcomes change when assumptions vary.

Potential outputs:

* impact range;
* ranking stability;
* ranking displacement;
* cascade variation;
* population impact variation;
* facility impact variation.

The objective is to determine whether conclusions are robust or highly dependent on assumptions.

---

## 33. Robustness Results

Robustness analysis should evaluate whether the primary conclusion remains under reasonable perturbations.

Examples:

```text
Complete Graph
      ↓
Reduced Graph
      ↓
Incomplete Graph
```

or:

```text
Low Hazard
Baseline Hazard
High Hazard
```

If the conclusion changes substantially, this should be reported rather than hidden.

---

## 34. Ablation Results

Ablation results should explain what changes when a model component is removed.

Example:

| Configuration               | Impact Reduction | Runtime | Ranking Stability |
| --------------------------- | ---------------: | ------: | ----------------: |
| Full P0                     |              TBD |     TBD |               TBD |
| Without dependency strength |              TBD |     TBD |               TBD |
| Without recovery            |              TBD |     TBD |               TBD |
| Reduced graph               |              TBD |     TBD |               TBD |

Ablations should be interpreted only in relation to the component being tested.

---

## 35. Performance Results

Performance results should be reported separately from scientific outcomes.

Example:

| Workload   | Size | p50 | p95 | p99 | Throughput |
| ---------- | ---: | --: | --: | --: | ---------: |
| API        |  TBD | TBD | TBD | TBD |        TBD |
| Graph      |  TBD | TBD | TBD | TBD |        TBD |
| Simulation |  TBD | TBD | TBD | TBD |        TBD |

Performance measurements should include relevant environment metadata.

---

## 36. Result Quality Checks

Before publication or major reporting, results should pass:

### Data Check

* correct dataset;
* correct version;
* expected coverage.

### Configuration Check

* correct scenario;
* correct budget;
* correct interventions;
* correct simulation parameters.

### Execution Check

* completed successfully;
* no unresolved critical errors.

### Metric Check

* frozen definitions;
* correct units;
* expected direction;
* no accidental denominator errors.

### Comparison Check

* fair baseline;
* same evaluation conditions.

### Reproducibility Check

* experiment traceable;
* configuration preserved;
* code version recorded.

---

## 37. Result Provenance

Each important result should be traceable through:

```text
Result
  ↓
Run
  ↓
Experiment
  ↓
Configuration
  ↓
Scenario
  ↓
Dataset
  ↓
Code
```

A result without sufficient provenance should be classified accordingly.

---

## 38. Result Storage

Results should be stored using a layered strategy.

### Raw

Original experiment output.

### Processed

Validated and normalized result data.

### Analytical

Metric calculations and statistical summaries.

### Presentation

Tables, charts, figures, and paper-ready outputs.

Conceptually:

```text
Raw
 ↓
Processed
 ↓
Analytical
 ↓
Presentation
```

Presentation artifacts should be regenerable from analytical results where practical.

---

## 39. Result Immutability

Completed primary experiment results should not be silently modified.

Corrections should produce:

* a new result version;
* an explanation;
* updated provenance;
* preserved original evidence where practical.

Example:

```text
RESULT-001-v1
       ↓
correction
       ↓
RESULT-001-v2
```

---

## 40. Reproducibility Link

Every primary result should connect to the reproducibility framework defined in:

```text
research/REPRODUCIBILITY.md
```

At minimum, the result should identify:

* code version;
* dataset version;
* scenario version;
* dependency graph version;
* configuration;
* environment;
* seed;
* run identifier.

---

## 41. Result Interpretation Rules

When interpreting results:

### Rule 1

Do not claim improvement without comparison.

### Rule 2

Do not claim robustness from one scenario.

### Rule 3

Do not claim generalization from one city or dataset.

### Rule 4

Do not claim real-world prediction from simulation alone.

### Rule 5

Do not claim causal relationships beyond the model's supported semantics.

### Rule 6

Do not hide unfavorable experiments.

### Rule 7

Do not change the primary metric after seeing the results.

### Rule 8

Do not treat ranking differences as improvement without outcome evidence.

### Rule 9

Do not ignore computational cost.

### Rule 10

Do not present assumptions as observations.

---

## 42. Research Claim Levels

Claims should be categorized according to evidence strength.

### Level C1 — Implementation Claim

Example:

> RESOLVE implements dependency-aware cascade simulation.

Supported by:

* source code;
* tests;
* architecture documentation.

---

### Level C2 — Experimental Observation

Example:

> Under the evaluated scenarios, RESOLVE produced lower simulated cascading impact than the isolated baseline.

Supported by:

* controlled experiments;
* result metrics;
* reproducibility evidence.

---

### Level C3 — Conditional Research Finding

Example:

> Under the evaluated scenario and data conditions, dependency-aware modeling improved intervention prioritization.

Supported by:

* multiple controlled experiments;
* statistical/practical analysis;
* documented limitations.

---

### Level C4 — Generalized Claim

Example:

> Dependency-aware urban digital twins improve resilience intervention prioritization across cities.

This requires substantially stronger evidence than a single case study.

RESOLVE MUST NOT make C4 claims without appropriate evidence.

---

## 43. Claim Traceability

Every major paper claim should map to:

```text
Claim
 ↓
Hypothesis
 ↓
Experiment
 ↓
Result
 ↓
Metric
 ↓
Evidence Artifact
```

If this chain cannot be constructed, the claim should be weakened or removed.

---

## 44. Paper Synchronization

The paper should be updated only from validated research results.

Recommended process:

```text
Experiment
    ↓
Validation
    ↓
Result Freeze
    ↓
Analysis
    ↓
Evidence Review
    ↓
Paper Table/Figure
    ↓
Paper Text
```

Paper values should not be manually typed from memory.

Where practical, paper tables and figures should be generated from structured result artifacts.

---

## 45. Result Freeze

Before final paper submission, the project should freeze:

* primary results;
* result identifiers;
* dataset versions;
* experiment versions;
* metric definitions;
* statistical analysis;
* tables;
* figures;
* claim mapping.

Any post-freeze change must be documented.

---

## 46. Unexpected Results

Unexpected results should trigger investigation.

Potential questions:

* Is the result valid?
* Is the implementation correct?
* Is the dataset correct?
* Is the baseline correctly implemented?
* Is the dependency graph incomplete?
* Is the intervention effect realistic?
* Is the metric appropriate?
* Is the result caused by an implementation artifact?

The correct response is investigation, not automatic rejection.

---

## 47. Result Review

Before a result becomes a paper claim, it should be reviewed for:

* correctness;
* provenance;
* reproducibility;
* metric correctness;
* baseline fairness;
* statistical interpretation;
* practical significance;
* uncertainty;
* limitations;
* wording strength.

This review may be performed as part of the project's research evidence workflow.

---

## 48. Research Result Ledger

The project should eventually maintain a result ledger.

Conceptual structure:

| Result ID  | Experiment | Metric           | Value | Status | Evidence | Paper Use |
| ---------- | ---------- | ---------------- | ----- | ------ | -------- | --------- |
| RESULT-001 | EXP-001    | Impact reduction | TBD   | TBD    | TBD      | TBD       |
| RESULT-002 | EXP-002    | Cascade size     | TBD   | TBD    | TBD      | TBD       |
| RESULT-003 | EXP-004    | Rank correlation | TBD   | TBD    | TBD      | TBD       |

This ledger should become the authoritative index of research results.

---

## 49. Definition of Done

The results framework is considered sufficiently implemented when:

* [ ] raw results are preserved;
* [ ] result validation exists;
* [ ] result identifiers are stable;
* [ ] primary metric is frozen;
* [ ] secondary metrics are defined;
* [ ] baseline/proposed comparisons are traceable;
* [ ] statistical analysis is documented;
* [ ] uncertainty is reported where appropriate;
* [ ] negative results are preserved;
* [ ] missing results are visible;
* [ ] hypothesis statuses can be assigned;
* [ ] result provenance is recorded;
* [ ] paper claims can be traced to evidence;
* [ ] result artifacts are reproducible;
* [ ] primary results can be frozen before paper submission.

---

## 50. Guiding Principle

> Results are evidence, not decoration.

For RESOLVE, the purpose of the results system is not to make the project look successful.

Its purpose is to make the project's conclusions **measurable, traceable, reproducible, falsifiable, and honest about uncertainty**.

If the dependency-aware approach improves outcomes, the evidence should demonstrate it.

If it does not, the system should demonstrate that instead.

If the evidence is insufficient, the correct result is uncertainty.

That standard is essential for turning RESOLVE from a software project into a credible research system.
