# RESOLVE — Research Baselines

## 1. Purpose

This document defines the baseline approaches that will be used to evaluate the RESOLVE dependency-aware methodology.

The purpose of a baseline is to establish a credible comparison point against which the proposed approach can be evaluated.

A baseline must be:

* clearly defined;
* reproducible;
* appropriate for the research question;
* evaluated using the same scenarios and datasets where possible;
* implemented or reproduced without introducing unnecessary methodological advantages.

---

## 2. Primary Baseline

### B0 — Isolated Risk Assessment

The primary baseline evaluates urban components independently without propagating failures through explicit inter-system dependencies.

Conceptually:

```text
             Hazard
                │
       ┌────────┼────────┐
       ▼        ▼        ▼
  Electricity  Water  Healthcare
       │        │        │
       ▼        ▼        ▼
   Risk Score Risk Score Risk Score
       │        │        │
       └────────┼────────┘
                ▼
       Intervention Ranking
```

Each component or system is evaluated according to its direct exposure, vulnerability, consequence, or other predefined risk factors.

Dependencies between components are not used to propagate cascading failures.

---

## 3. Baseline Objective

The isolated baseline should answer:

> **Which interventions would be prioritized if urban systems were assessed independently rather than as an interconnected system?**

This creates the principal comparison against the dependency-aware approach.

---

## 4. Controlled Comparison

The baseline and proposed method should use the same:

* datasets;
* geographic boundary;
* hazard scenarios;
* intervention candidates;
* intervention costs;
* resource constraints;
* evaluation metrics;
* simulation horizon where applicable.

The primary methodological difference should be:

```text
B0:
No explicit dependency propagation

versus

RESOLVE:
Explicit dependency-aware propagation
```

This is necessary to reduce confounding factors.

---

## 5. Baseline B1 — Direct Hazard Exposure

Where data permits, a simpler exposure-based baseline may be implemented.

Each component receives a score based primarily on its direct exposure to the selected hazard.

Conceptually:

```text
Hazard Exposure
      ↓
Component Exposure
      ↓
Exposure Ranking
      ↓
Intervention Selection
```

This baseline does not explicitly model cascading effects.

It provides a simpler reference point than a broader isolated-risk model.

---

## 6. Baseline B2 — Component Risk Score

A second optional baseline may combine direct exposure with component vulnerability and consequence.

A simplified formulation may be:

```text
Risk = Exposure × Vulnerability × Consequence
```

The exact formulation will depend on the selected case study and literature.

This baseline remains component-centric and does not explicitly propagate failures through a dependency graph.

---

## 7. Proposed Approach — P0

The primary proposed approach will be the dependency-aware RESOLVE model.

Conceptually:

```text
                 Hazard
                    │
                    ▼
             Initial Failures
                    │
                    ▼
            Dependency Graph
                    │
                    ▼
          Cascade Propagation
                    │
                    ▼
            System-Level Impact
                    │
                    ▼
          Intervention Evaluation
                    │
                    ▼
           Intervention Ranking
```

The proposed approach may incorporate:

* infrastructure dependencies;
* service dependencies;
* facility dependencies;
* geographic relationships;
* failure thresholds;
* partial degradation;
* recovery;
* intervention effects.

Only mechanisms justified by the research design and available evidence will be included.

---

## 8. Baseline Comparison Matrix

| Approach             | Direct Exposure | Vulnerability | Consequence | Dependencies | Cascade Simulation |
| -------------------- | --------------: | ------------: | ----------: | -----------: | -----------------: |
| B1 — Direct Exposure |               ✓ |             — |           — |            — |                  — |
| B2 — Component Risk  |               ✓ |             ✓ |           ✓ |            — |                  — |
| B0 — Isolated Risk   |               ✓ |             ✓ |           ✓ |            — |                  — |
| P0 — RESOLVE         |               ✓ |             ✓ |           ✓ |            ✓ |                  ✓ |

The exact baseline set may be reduced or expanded after the literature review.

---

## 9. Baseline Selection Criteria

A baseline should satisfy at least one of the following:

1. represent a commonly used risk-assessment strategy;
2. represent a meaningful simplified version of the proposed methodology;
3. provide a strong methodological comparison;
4. appear in relevant prior research;
5. isolate the value of a specific RESOLVE component.

A baseline will not be included merely because it is easy to implement.

---

## 10. Ablation Baselines

In addition to external or conventional baselines, RESOLVE may use ablation experiments to determine which parts of the proposed architecture contribute to performance.

Potential ablations include:

### A1 — No Dependency Strength

Treat all dependency relationships equally.

### A2 — No Recovery Model

Evaluate cascading impact without recovery dynamics.

### A3 — No Geographic Relationship

Remove spatial relationships while retaining other dependencies.

### A4 — Reduced Dependency Graph

Remove selected dependency categories.

### A5 — Simplified Cascade Rules

Use simplified propagation thresholds.

These experiments can help determine whether improvements arise from the overall architecture or from specific mechanisms.

---

## 11. Fairness Requirements

Baseline comparisons must avoid giving one method an unfair advantage.

Where possible:

* use identical input datasets;
* use identical scenario definitions;
* use identical intervention candidates;
* use identical intervention budgets;
* use identical output metrics;
* use identical evaluation periods;
* use equivalent computational environments.

Any unavoidable difference must be documented.

---

## 12. Intervention Selection

Each baseline should produce an intervention ranking or selection according to its own methodology.

The selected interventions should then be evaluated using a common outcome-evaluation framework.

This prevents a method from being considered successful merely because it uses a favorable scoring function.

Conceptually:

```text
             Method
                │
                ▼
       Intervention Ranking
                │
                ▼
       Selected Interventions
                │
                ▼
        Common Evaluation
                │
                ▼
        Comparable Outcomes
```

---

## 13. Ranking Evaluation

Baseline and RESOLVE rankings may be compared using:

* Spearman rank correlation;
* Kendall rank correlation;
* top-k overlap;
* rank displacement;
* intervention-selection agreement.

A different ranking is not automatically considered better.

The ranking must be evaluated against downstream resilience outcomes.

---

## 14. Outcome Evaluation

All methods should ultimately be evaluated using common outcome metrics.

Potential measures include:

* affected population;
* affected critical facilities;
* service downtime;
* cascade size;
* cascade depth;
* recovery time;
* intervention cost;
* resilience benefit;
* benefit per unit cost.

The final metric definitions will be specified in `research/METRICS.md`.

---

## 15. Baseline Limitations

The baselines may have limitations.

For example:

* isolated risk assessment may ignore indirect impacts;
* exposure-only scoring may ignore vulnerability;
* simple risk scores may require subjective weighting;
* some baselines may not represent temporal dynamics;
* simplified methods may be computationally cheaper but less expressive.

These limitations are not reasons to exclude a baseline.

They are part of the comparison being investigated.

---

## 16. Baseline Reproducibility

Every implemented baseline should have:

* documented mathematical formulation;
* input specification;
* configuration;
* parameter definitions;
* implementation version;
* experiment identifier;
* output format;
* evaluation procedure.

Where an established published method is reproduced, the relevant publication and implementation differences will be documented in `research/REFERENCES.md` or the appropriate research documentation.

---

## 17. Final Baseline Strategy

The initial experimental hierarchy is:

```text
B1 — Direct Hazard Exposure
          │
          ▼
B2 — Component Risk Score
          │
          ▼
B0 — Isolated Risk Assessment
          │
          ▼
P0 — Dependency-Aware RESOLVE
```

The final baseline set will be frozen before the primary evaluation experiments.

Any subsequent changes will be recorded with:

* reason for change;
* date;
* affected experiments;
* expected impact;
* whether previously completed experiments require rerunning.

---

## 18. Baseline Principle

> **The proposed method should earn its advantage through evidence, not through a weak comparison.**

A strong baseline is therefore considered a requirement for a credible RESOLVE evaluation.
