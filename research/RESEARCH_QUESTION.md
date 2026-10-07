# RESOLVE — Research Question

## 1. Primary Research Question

> **Can a dependency-aware urban digital twin improve the prioritization of resilience interventions compared with isolated risk assessment?**

---

## 2. Research Objective

The objective of RESOLVE is to determine whether explicitly modeling dependencies between interconnected urban systems provides measurable value when identifying and prioritizing resilience interventions.

The investigation will compare:

1. an isolated risk-assessment approach; and
2. a dependency-aware approach capable of modeling cascading effects.

The comparison will consider both **resilience outcomes** and **computational cost**.

---

## 3. Secondary Research Questions

### RQ1 — System Representation

Can heterogeneous urban datasets be transformed into a computational representation of infrastructure, services, facilities, population, hazards, and dependencies?

### RQ2 — Cascade Modeling

Can dependency-aware simulation represent cascading disruptions across interconnected urban systems?

### RQ3 — Intervention Evaluation

Can the same computational model represent and compare alternative resilience interventions under defined constraints?

### RQ4 — Intervention Prioritization

Does dependency-aware modeling produce different intervention rankings compared with isolated risk assessment?

### RQ5 — Resilience Outcomes

Does dependency-aware intervention prioritization reduce simulated cascading impact according to predefined resilience metrics?

### RQ6 — Computational Cost

What computational overhead is introduced by dependency-aware modeling and cascade simulation compared with isolated assessment?

### RQ7 — Robustness

How sensitive are intervention rankings and resilience outcomes to uncertainty in dependency relationships, model parameters, and input data?

---

## 4. Research Comparison

The core experimental comparison is:

```text
                    Same Scenario
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
      Isolated Assessment    Dependency-Aware
              │                 Assessment
              │                     │
              ▼                     ▼
       Intervention            Cascade
        Priorities             Simulation
              │                     │
              └──────────┬──────────┘
                         ▼
                  Compare Results
```

Both approaches should use comparable:

* input data
* scenarios
* intervention candidates
* resource constraints
* evaluation metrics

The primary methodological difference should be the treatment of dependencies and cascading effects.

---

## 5. Experimental Unit

The primary experimental unit will be a defined urban disruption scenario.

A scenario may specify:

* geographic area
* hazard type
* hazard intensity
* affected components
* simulation duration
* recovery assumptions
* available interventions
* intervention budget
* model configuration

Each scenario should be reproducible from a stored configuration.

---

## 6. Intervention Prioritization

For each scenario, candidate interventions will be evaluated using predefined objectives.

Potential objectives include:

* reduction in affected population
* reduction in critical-facility impact
* reduction in service downtime
* reduction in cascade size
* reduction in recovery time
* resource efficiency

The final objective function will be defined before the main experiments.

---

## 7. Primary Outcome

The primary outcome should measure whether dependency-aware prioritization provides a measurable improvement over the isolated baseline.

A candidate formulation is:

> **Relative reduction in simulated cascading impact achieved by interventions selected using the dependency-aware approach compared with interventions selected using the isolated baseline.**

The exact mathematical formulation will be finalized in `research/METRICS.md`.

---

## 8. Secondary Outcomes

Secondary outcomes may include:

* intervention-ranking agreement
* population protected
* critical facilities protected
* service downtime reduction
* cascade size reduction
* recovery-time reduction
* resource utilization
* simulation runtime
* memory consumption
* scalability
* sensitivity to dependency uncertainty

---

## 9. Null Hypothesis

The primary null hypothesis is:

> **H₀: Dependency-aware intervention prioritization does not produce a statistically or practically meaningful improvement in resilience outcomes compared with isolated risk assessment under equivalent experimental conditions.**

---

## 10. Alternative Hypothesis

The primary alternative hypothesis is:

> **H₁: Dependency-aware intervention prioritization produces a statistically and practically meaningful improvement in resilience outcomes compared with isolated risk assessment under equivalent experimental conditions.**

The thresholds defining statistical and practical significance will be specified before the main experiments.

---

## 11. Research Boundaries

The research will not attempt to prove that:

* dependency-aware modeling is universally superior;
* the model predicts real-world disasters perfectly;
* simulation outputs represent exact future outcomes;
* one intervention is universally optimal;
* the selected case study represents every city;
* the system can replace professional resilience planning.

The conclusions will be limited to the datasets, scenarios, assumptions, models, and experimental conditions evaluated.

---

## 12. Falsifiability

The research question is considered falsifiable because the proposed approach may:

* fail to improve resilience metrics;
* produce improvements too small to be practically meaningful;
* require excessive computational resources;
* produce unstable intervention rankings;
* depend heavily on uncertain dependency assumptions;
* outperform the baseline only for specific scenarios.

These outcomes will be reported rather than excluded.

---

## 13. Planned Research Flow

```text
Literature Review
       ↓
Define System Boundary
       ↓
Select Datasets
       ↓
Define Baseline
       ↓
Define Dependency Model
       ↓
Define Intervention Model
       ↓
Define Metrics
       ↓
Build Experiments
       ↓
Run Baseline
       ↓
Run Dependency-Aware Model
       ↓
Compare Results
       ↓
Sensitivity Analysis
       ↓
Interpret Findings
```

---

## 14. Final Research Question

The complete research investigation can therefore be stated as:

> **Given the same urban data, disruption scenarios, candidate interventions, and resource constraints, does explicitly modeling dependencies and cascading effects between urban systems lead to better resilience-intervention prioritization than treating those systems independently, and what computational cost and sensitivity to uncertainty does the dependency-aware approach introduce?**
