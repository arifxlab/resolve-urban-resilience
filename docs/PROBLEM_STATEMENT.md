# RESOLVE — Problem Statement

## 1. Problem Overview

Urban environments are composed of multiple infrastructure and service systems that interact through physical, operational, geographic, and functional dependencies.

These systems include, but are not limited to:

* electricity
* water
* transportation
* healthcare
* communications
* emergency services
* public facilities
* residential areas
* commercial areas

A disruption to one system can therefore create consequences beyond the initially affected component.

For example, an extreme-heat event may increase electricity demand. Increased demand may contribute to grid stress. An electricity outage may then affect water-pumping infrastructure. Reduced water availability may affect hospitals and communities, while transportation and emergency services may experience additional pressure.

This creates a systems-level problem:

> **Urban resilience cannot always be evaluated adequately by analyzing hazards and infrastructure systems independently when those systems are connected through dependencies.**

RESOLVE investigates whether explicitly modeling these dependencies can improve the analysis and prioritization of resilience interventions.

---

## 2. Existing Problem

Many urban risk-assessment approaches focus on individual hazards, individual infrastructure systems, or geographically isolated vulnerabilities.

Such approaches can be useful for understanding localized risks, but they may not fully represent how failures propagate between interconnected systems.

Consider two simplified approaches.

### Isolated Assessment

```text
Hazard
  │
  ├── Electricity Risk
  │
  ├── Water Risk
  │
  ├── Healthcare Risk
  │
  └── Transportation Risk
```

Each system is evaluated independently.

### Dependency-Aware Assessment

```text
                    Hazard
                      │
                      ▼
                Electricity
                      │
                 ┌────┴────┐
                 ▼         ▼
               Water    Communications
                 │         │
                 └────┬────┘
                      ▼
                  Healthcare
                      │
                      ▼
                  Population
```

The second representation introduces the possibility of indirect effects.

The research problem is therefore not simply whether a particular infrastructure component is vulnerable.

It is whether understanding **relationships between components** changes the assessment of system-level risk and the prioritization of interventions.

---

## 3. Core Problem Statement

The core problem addressed by RESOLVE is:

> **Urban resilience planning requires prioritization of interventions under limited resources, but risk assessments that treat infrastructure systems independently may fail to account for cascading consequences created by dependencies between systems.**

A computational framework is therefore needed to investigate whether:

1. urban systems can be represented as dependency-aware models;
2. disruptions can be propagated through those dependencies;
3. cascading consequences can be quantified;
4. resilience interventions can be simulated;
5. intervention strategies can be compared using predefined metrics; and
6. the dependency-aware approach provides measurable improvement over appropriate simpler baselines.

---

## 4. Research Gap to Investigate

RESOLVE does not assume that a gap exists simply because existing systems use different technologies or terminology.

The project will establish its research gap through a structured literature review.

The investigation will examine existing work across areas including:

* urban digital twins
* critical infrastructure interdependency
* cascading failures
* infrastructure resilience
* network-based risk assessment
* urban vulnerability analysis
* resilience optimization
* disaster-risk modeling
* infrastructure intervention prioritization

The literature review will determine:

* what has already been demonstrated;
* which modeling approaches are commonly used;
* which datasets are available;
* how cascading effects are represented;
* how interventions are evaluated;
* which limitations are repeatedly identified;
* which evaluation metrics are used;
* and where a defensible research opportunity remains.

Until that analysis is complete, RESOLVE will avoid claiming that its architecture or methodology is novel.

---

## 5. Research Question

The primary research question is:

> **Can a dependency-aware urban digital twin improve the prioritization of resilience interventions compared with isolated risk assessment?**

This question contains several sub-problems.

### RQ1 — Representation

Can relevant urban infrastructure and service systems be represented as a dependency-aware computational model using available real-world data?

### RQ2 — Cascading Effects

Can the model reproduce or approximate meaningful cascading effects resulting from disruptions to interconnected systems?

### RQ3 — Intervention Evaluation

Can resilience interventions be represented and compared within the same dependency-aware model?

### RQ4 — Comparative Performance

Does dependency-aware intervention prioritization produce measurably different or improved outcomes compared with isolated risk-assessment baselines?

### RQ5 — Computational Feasibility

Can the proposed modeling and simulation approach operate with acceptable computational performance for the selected case study?

---

## 6. Research Scope

RESOLVE will initially focus on a bounded set of urban systems selected according to:

* data availability
* research relevance
* dependency significance
* modeling feasibility
* computational constraints

The initial case study may focus on Karachi, subject to dataset availability and research feasibility.

The project will not attempt to model every component of a city.

Instead, the system boundary will be explicitly defined.

A simplified boundary may include:

```text id="kq0z0p"
                External Hazards
                       │
                       ▼
        ┌────────────────────────────┐
        │       RESOLVE Model        │
        │                            │
        │ Infrastructure             │
        │ Services                   │
        │ Critical Facilities        │
        │ Population                 │
        │ Dependencies               │
        │ Recovery / Interventions   │
        └──────────────┬─────────────┘
                       │
                       ▼
                Model Outputs
```

---

## 7. Out of Scope

The initial project will not attempt to:

* reproduce an entire city at building-level physical fidelity;
* provide guaranteed real-time disaster predictions;
* replace professional emergency-management systems;
* provide operational control over infrastructure;
* make autonomous public-safety decisions;
* claim causal certainty from observational datasets;
* simulate every possible hazard;
* model every infrastructure dependency;
* guarantee that simulation results represent future real-world events;
* optimize interventions without explicitly defined constraints and objectives.

These boundaries are intended to keep the research experimentally tractable.

---

## 8. Key Technical Challenges

### 8.1 Heterogeneous Data

Urban information comes from different sources with different:

* formats
* spatial resolutions
* temporal resolutions
* coordinate systems
* update frequencies
* data quality
* licensing conditions

Combining these datasets into a coherent model is itself a significant engineering challenge.

---

### 8.2 Dependency Representation

Dependencies between systems are not always directly available in datasets.

Some relationships may be:

* explicit
* inferred
* geographically derived
* expert-defined
* statistically estimated
* uncertain

The project must distinguish between observed relationships and modeled assumptions.

---

### 8.3 Cascading-Failure Modeling

A cascade is not simply a list of failures.

The model may need to represent:

* triggering conditions
* dependency thresholds
* propagation
* partial degradation
* recovery
* redundancy
* intervention effects
* uncertainty

The simulation therefore needs a clearly defined state-transition model.

---

### 8.4 Intervention Comparison

An intervention may reduce one risk while creating another cost or dependency.

For example:

```text
Intervention
     │
     ├── Cost
     ├── Direct Benefit
     ├── Indirect Benefit
     ├── Dependency Effects
     └── Recovery Effects
```

Interventions therefore need to be evaluated against predefined objectives rather than a single arbitrary score.

---

### 8.5 Incomplete Information

Real-world infrastructure information may be incomplete or unavailable.

The system must therefore distinguish:

```text
Observed Data
     +
Derived Information
     +
Model Assumptions
     +
Uncertainty
```

These categories should not be silently mixed.

---

### 8.6 Validation

A simulation can produce internally consistent results while still representing the real world poorly.

Validation will therefore be treated as a central research concern.

Where possible, the project should compare model behavior against:

* historical incidents
* known infrastructure relationships
* external datasets
* documented events
* expert-supported assumptions
* established baseline methods

---

## 9. Proposed Problem Decomposition

The overall problem can be decomposed into six major stages.

```text id="yrk5i7"
1. Data Acquisition
        ↓
2. Urban System Modeling
        ↓
3. Dependency Graph Construction
        ↓
4. Cascade Simulation
        ↓
5. Intervention Simulation
        ↓
6. Quantitative Evaluation
```

### Stage 1 — Data Acquisition

Collect and document relevant urban, environmental, infrastructure, and demographic datasets.

### Stage 2 — Urban System Modeling

Transform raw data into structured domain entities.

### Stage 3 — Dependency Graph Construction

Represent relationships between entities and systems.

### Stage 4 — Cascade Simulation

Apply disruption scenarios and propagate effects through the dependency model.

### Stage 5 — Intervention Simulation

Modify the system according to candidate resilience interventions and rerun comparable scenarios.

### Stage 6 — Quantitative Evaluation

Compare outcomes against predefined baselines and metrics.

---

## 10. Expected Inputs

Depending on the selected case study, possible inputs include:

### Hazard Inputs

* temperature
* rainfall
* flooding
* storms
* other selected hazards

### Infrastructure Inputs

* power infrastructure
* water infrastructure
* transportation networks
* communication infrastructure

### Facility Inputs

* hospitals
* schools
* emergency facilities
* public services

### Population Inputs

* population density
* demographic characteristics where appropriate
* geographic distribution

### Geographic Inputs

* administrative boundaries
* roads
* land-use information
* geographic coordinates
* spatial relationships

The final dataset list will be defined in `research/DATASETS.md`.

---

## 11. Expected Outputs

The platform should eventually produce structured outputs such as:

* affected components
* failed dependencies
* cascade paths
* affected population estimates
* affected facilities
* service availability
* recovery characteristics
* intervention costs
* intervention benefits
* risk-reduction metrics
* simulation performance metrics

Example conceptual output:

```text id="g6dr3t"
Scenario: Extreme Heat

Baseline:
    Population affected: X
    Critical facilities affected: Y
    Estimated service disruption: Z

Intervention A:
    Population affected: X₁
    Critical facilities affected: Y₁
    Estimated service disruption: Z₁

Intervention B:
    Population affected: X₂
    Critical facilities affected: Y₂
    Estimated service disruption: Z₂
```

The actual metrics and units will be defined before experiments are conducted.

---

## 12. Evaluation Problem

The central evaluation challenge is determining whether dependency-aware modeling provides meaningful value.

The project will therefore compare at least two conceptual approaches:

### Baseline

Risk or intervention prioritization without explicit dependency propagation.

### Proposed Approach

Risk or intervention prioritization using dependency-aware cascading simulation.

The comparison should answer:

* Does the ranking of interventions change?
* Does the proposed approach identify critical components missed by the baseline?
* Does it reduce simulated cascading impact?
* Does it improve protection of critical facilities or population?
* What computational cost does the dependency-aware approach introduce?

---

## 13. Constraints

The project operates under several realistic constraints.

### Data Constraints

* incomplete datasets
* inconsistent data quality
* uncertain dependency information
* restricted or unavailable infrastructure data

### Computational Constraints

* potentially large graphs
* expensive simulations
* geospatial processing requirements
* repeated experimental runs

### Research Constraints

* limited time
* limited ground-truth data
* difficulty validating complex urban systems
* potential uncertainty in causal relationships

### Engineering Constraints

* reproducibility
* maintainability
* API performance
* storage requirements
* deployment complexity

These constraints will be documented rather than hidden from the research process.

---

## 14. Assumptions

All important assumptions should be explicitly documented.

Potential assumptions include:

* certain infrastructure relationships can be approximated as graph dependencies;
* selected infrastructure components can be represented at an appropriate abstraction level;
* available datasets provide sufficient information for the chosen case study;
* simulation parameters can be estimated or justified;
* intervention effects can be represented using defined model rules;
* the selected metrics provide useful approximations of resilience outcomes.

These assumptions are provisional until supported by literature, data, or experiments.

---

## 15. Research Risk

There is a possibility that the proposed dependency-aware approach will not demonstrate a significant improvement over simpler methods.

Other possible outcomes include:

* dependency-aware modeling improves some metrics but not others;
* improvements occur at a significant computational cost;
* results are highly sensitive to dependency assumptions;
* available data is insufficient for reliable validation;
* intervention rankings change without producing better outcomes;
* the approach performs well for some hazards but poorly for others.

These are valid research outcomes.

The project is therefore designed to evaluate the hypothesis rather than guarantee a positive result.

---

## 16. Definition of Success

For the problem to be considered meaningfully addressed, RESOLVE should demonstrate that it can:

1. Construct a documented urban system model.
2. Represent explicit dependencies.
3. Execute reproducible disruption scenarios.
4. Propagate failures through the dependency structure.
5. Represent candidate resilience interventions.
6. Compare intervention outcomes against a defined baseline.
7. Produce quantitative evaluation metrics.
8. Measure computational cost.
9. Document uncertainty and limitations.
10. Reproduce the principal experimental results.

A successful research result does not necessarily mean that the proposed method wins every metric.

A scientifically useful result may instead demonstrate:

* where dependency-aware modeling helps;
* where it does not help;
* under what assumptions it helps;
* and what additional computational or data requirements it introduces.

---

## 17. Problem Statement Summary

The problem addressed by RESOLVE can be summarized as follows:

> **Cities contain interconnected systems whose dependencies can transform localized disruptions into cascading consequences. Existing risk assessments may evaluate systems independently, potentially overlooking these interactions when prioritizing resilience interventions. RESOLVE investigates whether a dependency-aware urban digital twin can represent these relationships, simulate cascading effects, and improve intervention prioritization compared with isolated risk assessment, while remaining computationally feasible and reproducible.**

The project will answer this problem through documented modeling assumptions, controlled experiments, quantitative metrics, appropriate baselines, and reproducible evidence.
