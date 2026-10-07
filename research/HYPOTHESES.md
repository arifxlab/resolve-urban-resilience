# RESOLVE — Research Hypotheses

## 1. Purpose

This document defines the hypotheses that will be evaluated through the RESOLVE experiments.

The hypotheses are stated before the main experiments to reduce post-hoc interpretation and selective reporting.

The hypotheses may be refined if the literature review or pilot experiments demonstrate that an assumption is technically invalid. Any such change must be recorded in the decision log.

---

## 2. Primary Hypothesis

### H1 — Dependency-Aware Prioritization

> **Dependency-aware intervention prioritization will reduce simulated cascading urban impact more effectively than isolated risk assessment under equivalent scenarios, intervention candidates, and resource constraints.**

The primary comparison will use predefined resilience metrics.

The exact primary metric and significance criteria will be specified in `research/METRICS.md`.

### H0 — Null Hypothesis

> **Dependency-aware intervention prioritization will not produce a statistically or practically meaningful improvement in simulated resilience outcomes compared with isolated risk assessment under equivalent experimental conditions.**

---

## 3. Secondary Hypotheses

### H2 — Cascade Identification

> **Dependency-aware modeling will identify cascading failure pathways that are not represented by isolated risk assessment.**

Evaluation may compare:

* detected cascade paths
* affected components
* secondary failures
* tertiary failures
* critical dependency relationships

---

### H3 — Intervention Ranking

> **Dependency-aware modeling will produce materially different intervention rankings from isolated risk assessment in scenarios where significant system dependencies exist.**

Evaluation may use:

* rank correlation
* top-k overlap
* ranking changes
* intervention-selection agreement

A difference in ranking alone will not be interpreted as an improvement.

The resulting interventions must also be evaluated using outcome-based metrics.

---

### H4 — Critical Component Identification

> **Dependency-aware modeling will identify system components whose failure has greater downstream impact than would be indicated by isolated component-level risk.**

Potential evaluation measures include:

* downstream affected nodes
* cascade size
* affected population
* affected critical facilities
* service disruption
* graph centrality compared with observed cascade impact

---

### H5 — Intervention Efficiency

> **Under a fixed intervention budget, dependency-aware prioritization will achieve greater resilience benefit per unit of resource expenditure than isolated prioritization.**

A candidate formulation is:

```text
Intervention Efficiency =
Resilience Benefit / Intervention Cost
```

The final formulation will be defined before the main experiment.

---

### H6 — Cascade Reduction

> **Interventions selected using dependency-aware analysis will reduce cascade propagation more effectively than interventions selected using isolated assessment when evaluated under the same disruption scenarios.**

Potential measurements include:

* cascade size
* number of failed components
* propagation depth
* propagation duration
* affected population
* affected critical facilities

---

### H7 — Computational Overhead

> **Dependency-aware simulation will require greater computational resources than isolated risk assessment.**

This hypothesis is intentionally non-directional with respect to the practical acceptability of the overhead.

Measurements may include:

* execution time
* CPU utilization
* memory consumption
* graph size
* scenario throughput

The research will investigate whether the additional computational cost is justified by the resulting resilience benefit.

---

## 4. Sensitivity Hypothesis

### H8 — Dependency Uncertainty

> **Intervention rankings and resilience outcomes will be sensitive to uncertainty in dependency relationships and model parameters.**

Sensitivity analysis will vary selected parameters and measure changes in:

* intervention rankings
* cascade size
* population impact
* critical-facility impact
* resilience metrics

This hypothesis is important because dependency relationships may not be directly observable in available datasets.

---

## 5. Data Quality Hypothesis

### H9 — Data Completeness

> **Reduced completeness or increased uncertainty in urban input data will reduce the stability and reliability of dependency-aware intervention prioritization.**

Where feasible, experiments will introduce controlled data degradation or uncertainty and measure resulting changes.

Potential factors include:

* missing infrastructure records
* incomplete dependency edges
* uncertain component attributes
* spatial-data gaps
* parameter uncertainty

---

## 6. Baseline Comparison Hypothesis

### H10 — Added Value of Dependencies

> **The primary performance difference between the proposed approach and the isolated baseline will be attributable to explicit dependency and cascade modeling rather than unrelated differences in data, scenarios, or intervention budgets.**

To evaluate this, the experiments should maintain equivalent:

* input datasets
* hazard scenarios
* intervention candidates
* resource constraints
* evaluation metrics

The dependency representation should be the principal methodological difference.

---

## 7. Hypothesis Evaluation Matrix

| ID  | Hypothesis                                                      | Primary Evidence         |
| --- | --------------------------------------------------------------- | ------------------------ |
| H1  | Dependency-aware prioritization improves resilience outcomes    | Resilience metrics       |
| H2  | Dependency-aware modeling identifies additional cascades        | Cascade detection        |
| H3  | Dependency modeling changes intervention rankings               | Ranking metrics          |
| H4  | Dependency-aware modeling identifies critical components        | Downstream impact        |
| H5  | Dependency-aware interventions provide greater benefit per cost | Benefit/cost metrics     |
| H6  | Dependency-aware interventions reduce cascade propagation       | Cascade metrics          |
| H7  | Dependency-aware simulation introduces computational overhead   | Runtime/resource metrics |
| H8  | Results are sensitive to dependency uncertainty                 | Sensitivity analysis     |
| H9  | Data quality affects prioritization stability                   | Robustness analysis      |
| H10 | Improvements are attributable to dependency modeling            | Controlled comparison    |

---

## 8. Statistical Testing

Statistical testing will be selected after the experimental design and data distributions are understood.

Potential methods may include:

* paired statistical tests
* bootstrap confidence intervals
* permutation tests
* effect-size analysis
* non-parametric tests where appropriate

The project will avoid selecting statistical tests solely because they produce favorable results.

The final analysis plan will document:

* statistical test
* assumptions
* significance threshold
* confidence interval
* effect-size measure
* multiple-comparison handling where required

---

## 9. Practical Significance

Statistical significance alone will not determine whether a hypothesis is considered supported.

The project will distinguish between:

```text
Statistical Significance
          +
Practical Significance
          =
Meaningful Improvement
```

For example, a very small improvement may become statistically significant with many experiments while remaining operationally unimportant.

Practical thresholds will therefore be defined before the final evaluation where feasible.

---

## 10. Hypothesis Outcomes

Each hypothesis will ultimately be classified as one of:

* **Supported**
* **Partially Supported**
* **Not Supported**
* **Inconclusive**

The classification will be based on predefined evidence and reported with the corresponding limitations.

The project will not treat an unsupported hypothesis as a failed project.

An unsupported hypothesis can provide useful evidence about the limits of dependency-aware urban modeling.

---

## 11. Reporting Rule

No hypothesis will be marked as supported solely because the system produced the expected output.

Support must come from the predefined evaluation methodology and measured experimental results.

Results that contradict the hypotheses must be preserved and reported.
