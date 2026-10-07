# RESOLVE — Experimental Design

## 1. Purpose

This document defines the experimental framework for evaluating the RESOLVE dependency-aware urban resilience model.

The purpose of the experimental framework is to ensure that research claims are supported by controlled, reproducible, measurable experiments rather than by demonstrations alone.

The experimental design connects:

```text
Research Question
        ↓
Hypotheses
        ↓
Experimental Conditions
        ↓
Scenarios
        ↓
Baseline / Proposed Methods
        ↓
Interventions
        ↓
Simulation
        ↓
Metrics
        ↓
Statistical / Comparative Analysis
        ↓
Evidence
        ↓
Research Conclusions
```

The experimental framework is designed to answer the primary research question:

> Can a dependency-aware urban digital twin improve the prioritization of resilience interventions compared with isolated risk assessment?

The framework must remain valid whether the eventual case study uses Karachi or another city.

---

## 2. Experimental Principles

Experiments MUST follow these principles:

1. **Research before implementation** — experiments exist to answer research questions.
2. **Fair comparison** — baseline and proposed methods must receive equivalent information and constraints wherever possible.
3. **Controlled conditions** — differences between methods must be intentional and documented.
4. **Reproducibility** — experiments must record sufficient configuration and provenance to reproduce results.
5. **Determinism where possible** — deterministic simulations are preferred for controlled evaluation.
6. **Metric freeze** — primary metrics must be defined before the main evaluation.
7. **No post-hoc success criteria** — evaluation criteria must not be changed after observing results.
8. **Negative results are valid** — experiments must be capable of showing no improvement.
9. **Practical significance matters** — statistical significance alone does not establish useful improvement.
10. **Uncertainty must be visible** — uncertain data and assumptions must not be presented as observed facts.
11. **Complexity must be justified** — additional modeling complexity must provide measurable research value.
12. **Evidence must be preserved** — configurations, outputs, logs, and result summaries must be retained.
13. **No unsupported causal claims** — observed associations or simulation outcomes must not be described as real-world causal effects without appropriate evidence.

---

## 3. Experimental Objectives

The experiments should determine whether dependency-aware modeling provides measurable value in intervention prioritization.

Primary objectives:

* Compare isolated risk assessment with dependency-aware assessment.
* Measure the effect of dependency modeling on simulated cascading impact.
* Evaluate whether intervention rankings change when dependencies are represented.
* Determine whether dependency-aware prioritization produces greater resilience benefit under equivalent resource constraints.
* Measure the computational cost introduced by dependency-aware simulation.
* Evaluate robustness to uncertainty and incomplete dependency information.

Secondary objectives:

* Identify cascade pathways that isolated assessment cannot represent.
* Identify infrastructure components whose importance increases because of downstream dependencies.
* Evaluate the stability of intervention rankings.
* Determine which dependency assumptions have the greatest influence on outcomes.
* Establish the minimum data requirements for useful dependency-aware analysis.

---

## 4. Experimental Unit

The primary experimental unit is a **scenario-intervention-budget configuration**.

An experiment MUST identify:

* dataset version;
* geographic study area;
* scenario;
* hazard type;
* hazard intensity;
* affected entities;
* dependency graph version;
* simulation duration;
* recovery assumptions;
* candidate interventions;
* available budget;
* method being evaluated;
* simulation configuration;
* random seed when applicable;
* software version or Git commit;
* experiment identifier.

Conceptually:

```text
Experiment
├── Dataset Version
├── Study Area
├── Scenario
├── Dependency Graph
├── Baseline Method
├── Proposed Method
├── Candidate Interventions
├── Resource Budget
├── Simulation Configuration
├── Metrics
└── Reproducibility Metadata
```

---

## 5. Experimental Conditions

The main evaluation should compare at least two conditions.

### Condition A — Isolated Risk Assessment

The baseline evaluates urban components independently.

The baseline MAY consider:

* hazard exposure;
* vulnerability;
* consequence;
* component-level risk.

The baseline MUST NOT propagate disruption through an explicit dependency graph.

The baseline represents the question:

> Which components appear most important when assessed independently?

---

### Condition B — Dependency-Aware RESOLVE

The proposed method evaluates components while explicitly representing dependencies between urban systems.

The proposed method MAY include:

* dependency relationships;
* dependency strength;
* dependency direction;
* threshold effects;
* geographic relationships;
* disruption propagation;
* recovery behavior;
* intervention effects;
* resource constraints.

The proposed method represents the question:

> Which interventions provide the greatest resilience benefit when dependencies and cascading effects are considered?

---

## 6. Fairness of Comparison

The baseline and proposed method MUST be compared under equivalent conditions.

Where practical, both methods should use:

* the same geographic study area;
* the same source datasets;
* the same hazard scenario;
* the same candidate intervention set;
* the same intervention costs;
* the same resource budget;
* the same evaluation horizon;
* the same outcome definitions;
* the same population and facility data;
* the same experimental repetitions.

The primary methodological difference should be the representation of dependencies and resulting cascade effects.

If additional information is available only to the proposed method, the experiment MUST document this explicitly.

---

## 7. Scenario Definition

A scenario represents a controlled disruption applied to the urban system.

A scenario should define:

```text
Scenario
├── scenario_id
├── name
├── description
├── hazard_type
├── hazard_intensity
├── geographic_extent
├── start_time
├── duration
├── affected_entities
├── initial_states
├── dependency_graph_version
├── recovery_model
└── configuration_version
```

Examples of candidate scenarios include:

* extreme heat;
* flooding;
* electricity disruption;
* water-system disruption;
* transportation disruption;
* compound hazard.

These are experimental candidates rather than commitments.

The final scenario set MUST be selected based on:

* available evidence;
* data quality;
* reproducibility;
* research relevance;
* simulation feasibility;
* meaningful dependency relationships.

---

## 8. Scenario Classes

Experiments should eventually include multiple scenario classes where data permits.

### S1 — Single-System Disruption

A disruption primarily affects one urban system.

Example:

```text
Electricity disruption
        ↓
Grid components affected
```

Purpose:

* establish basic system behavior;
* validate baseline simulation;
* validate dependency propagation.

---

### S2 — Cross-System Cascade

A disruption propagates between multiple systems.

Example:

```text
Electricity disruption
        ↓
Water pumping disruption
        ↓
Water availability reduction
        ↓
Critical facility impact
```

Purpose:

* directly evaluate dependency-aware modeling.

---

### S3 — Spatially Distributed Disruption

A hazard affects geographically distributed components.

Example:

```text
Flooded area
    ├── roads
    ├── substations
    ├── hospitals
    └── water infrastructure
```

Purpose:

* evaluate interaction between geographic exposure and system dependencies.

---

### S4 — Compound or Multi-Hazard Scenario

Multiple hazards or disruptions interact.

This scenario class should only be introduced if the available data and model semantics justify it.

It MUST NOT be introduced merely to increase apparent complexity.

---

## 9. Intervention Definition

An intervention represents an action intended to reduce disruption or improve recovery.

Candidate intervention categories include:

* infrastructure reinforcement;
* redundancy;
* backup power;
* water-system resilience;
* transportation redundancy;
* facility protection;
* dependency reduction;
* recovery acceleration;
* targeted resource allocation.

Each intervention should define:

```text
Intervention
├── intervention_id
├── target_entities
├── intervention_type
├── resource_cost
├── expected_effect
├── implementation_scope
├── implementation_time
├── recovery_effect
└── assumptions
```

Intervention effects must be represented explicitly rather than encoded implicitly inside evaluation logic.

---

## 10. Budget-Constrained Evaluation

Intervention prioritization should be evaluated under constrained resources.

For a budget:

```text
B
```

and candidate interventions:

```text
I1, I2, ..., In
```

the selected intervention set must satisfy:

```text
Σ cost(Ii) ≤ B
```

The exact optimization strategy should be determined during implementation.

Potential strategies include:

* ranked selection;
* greedy selection;
* exhaustive selection for small candidate sets;
* constrained optimization;
* integer programming if justified.

The first implementation should favor the simplest method capable of answering the research question.

---

## 11. Primary Experiment

The primary experiment should compare intervention prioritization under a fixed resource budget.

Conceptual procedure:

```text
1. Load frozen dataset
2. Load frozen scenario
3. Construct urban system model
4. Construct dependency graph
5. Generate candidate interventions
6. Evaluate baseline
7. Evaluate dependency-aware method
8. Apply equivalent budget constraint
9. Simulate outcomes
10. Calculate frozen metrics
11. Compare outcomes
12. Preserve evidence
```

The primary experiment should answer:

> Does dependency-aware prioritization produce greater reduction in simulated cascading impact than isolated risk assessment under equivalent conditions?

---

## 12. Intervention Ranking Experiment

A separate experiment should evaluate whether dependencies materially change intervention rankings.

For each method:

```text
Candidate Interventions
        ↓
Risk / Resilience Evaluation
        ↓
Score
        ↓
Rank
```

The resulting rankings may be compared using:

* Spearman rank correlation;
* Kendall rank correlation;
* top-k overlap;
* rank displacement;
* selected intervention overlap.

A different ranking is not automatically evidence of improvement.

The ranking must be evaluated against downstream resilience outcomes.

---

## 13. Cascade Detection Experiment

The system should evaluate whether dependency-aware modeling identifies meaningful cascade pathways that isolated assessment cannot represent.

For each scenario, record:

* initial disruption;
* affected components;
* dependency edges traversed;
* subsequent state transitions;
* cascade depth;
* cascade breadth;
* terminal affected components;
* recovery sequence.

The experiment should compare:

```text
Isolated Assessment
        vs.
Dependency-Aware Simulation
```

The objective is not merely to produce more events.

The objective is to determine whether dependency modeling reveals structurally meaningful downstream consequences.

---

## 14. Intervention Effectiveness Experiment

For each selected intervention set:

1. Run the scenario without intervention.
2. Run the scenario with intervention.
3. Measure the resulting outcomes.
4. Calculate intervention benefit.
5. Compare against the baseline method.

Candidate outcomes include:

* affected population;
* affected critical facilities;
* service disruption;
* cascade size;
* cascade depth;
* recovery time;
* downtime;
* population protected;
* critical facilities protected.

Primary impact reduction candidate:

```text
Impact Reduction (%) =
    (Baseline Impact - Intervention Impact)
    / Baseline Impact × 100
```

The final impact aggregation must be frozen before the primary evaluation.

---

## 15. Computational Cost Experiment

Dependency-aware modeling introduces additional computational work.

The experiment should measure:

* graph construction time;
* graph traversal time;
* simulation execution time;
* metric calculation time;
* persistence time;
* total experiment runtime;
* memory consumption where practical.

The objective is to quantify the trade-off:

```text
Additional Computational Cost
            vs.
Additional Research Value
```

A method that improves outcomes but introduces excessive computational cost should report both findings.

---

## 16. Scaling Experiments

The system should eventually be tested with increasing model sizes.

Possible dimensions:

* number of entities;
* number of dependencies;
* graph density;
* number of scenarios;
* simulation duration;
* number of interventions;
* number of repeated runs.

Example:

```text
Small
  ↓
Medium
  ↓
Large
```

Scaling experiments are engineering measurements and should not automatically be interpreted as evidence of real-world scalability.

---

## 17. Sensitivity Experiments

Sensitivity experiments evaluate how outcomes change when uncertain assumptions are varied.

Candidate variables:

* dependency strength;
* dependency threshold;
* hazard intensity;
* vulnerability;
* recovery duration;
* intervention effectiveness;
* intervention cost;
* graph completeness;
* missing dependency edges.

For each variable:

```text
Low
Baseline
High
```

or another justified range may be evaluated.

The resulting variation should be recorded for:

* intervention ranking;
* cascading impact;
* population impact;
* critical facility impact;
* recovery time.

---

## 18. Robustness Experiments

Robustness experiments should evaluate whether conclusions remain stable under imperfect information.

Potential perturbations include:

* removing selected dependency edges;
* reducing dependency confidence;
* introducing missing values;
* varying hazard intensity;
* varying recovery assumptions;
* reducing dataset completeness;
* perturbing intervention effectiveness.

The goal is to determine whether the research conclusion depends on one fragile assumption.

---

## 19. Ablation Experiments

Optional ablation experiments may isolate individual model components.

Candidate ablations:

### A1 — Remove Dependency Strength

Use dependency existence without explicit strength.

Purpose:

* determine whether dependency strength contributes measurable value.

### A2 — Remove Recovery Modeling

Disable recovery behavior.

Purpose:

* evaluate whether recovery assumptions materially influence intervention rankings.

### A3 — Remove Geographic Relationships

Disable geographic relationships while retaining functional dependencies.

Purpose:

* evaluate the contribution of spatial context.

### A4 — Reduced Dependency Graph

Use a deliberately incomplete dependency graph.

Purpose:

* evaluate sensitivity to graph completeness.

### A5 — Simplified Cascade Rules

Use simplified propagation rules.

Purpose:

* determine whether detailed cascade semantics materially affect results.

Ablations should only be implemented when they answer a meaningful research question.

---

## 20. Repeated Runs

If any part of the simulation is stochastic, experiments MUST use controlled random seeds.

Repeated runs should be performed where stochasticity exists.

Each run should record:

* run identifier;
* experiment identifier;
* random seed;
* software version;
* configuration;
* dataset version;
* scenario version;
* dependency graph version;
* intervention configuration;
* output metrics.

For deterministic simulations, repeated runs may instead be used to verify deterministic behavior and performance consistency.

---

## 21. Statistical Analysis

Statistical analysis should be selected according to the actual experimental data.

Potential methods include:

* paired statistical tests;
* non-parametric paired tests;
* bootstrap confidence intervals;
* permutation tests;
* effect-size estimation.

The selected statistical method MUST be documented before the primary result is interpreted.

The analysis should distinguish:

```text
Statistical Significance
        ≠
Practical Significance
```

A small numerical improvement may be statistically significant but operationally unimportant.

Conversely, a meaningful practical improvement may not reach statistical significance if the experiment has limited observations.

---

## 22. Experimental Factors

Important experimental factors should be explicitly recorded.

Candidate factors include:

| Factor                  | Example Values             |
| ----------------------- | -------------------------- |
| Hazard type             | heat, flood, disruption    |
| Hazard intensity        | low, medium, high          |
| Study area              | defined geographic region  |
| Dependency completeness | low, medium, high          |
| Recovery model          | disabled, simple, extended |
| Budget                  | fixed budget levels        |
| Intervention set        | predefined candidate set   |
| Graph density           | sparse, medium, dense      |
| Dataset version         | immutable version          |
| Random seed             | controlled integer         |
| Simulation duration     | defined horizon            |

The final factor set should be based on the research question and available evidence.

---

## 23. Experimental Matrix

The eventual primary experiment should be represented using a matrix similar to:

| Experiment | Scenario             | Baseline | Proposed | Budget | Repeats | Primary Outcome             |
| ---------- | -------------------- | -------- | -------- | -----: | ------: | --------------------------- |
| E01        | Single-system        | B0       | P0       |  Fixed | Defined | Impact reduction            |
| E02        | Cross-system cascade | B0       | P0       |  Fixed | Defined | Impact reduction            |
| E03        | Spatial disruption   | B0       | P0       |  Fixed | Defined | Impact reduction            |
| E04        | Ranking              | B0       | P0       |  Fixed | Defined | Rank agreement/displacement |
| E05        | Sensitivity          | B0       | P0       |  Fixed | Defined | Ranking stability           |
| E06        | Robustness           | B0       | P0       |  Fixed | Defined | Outcome stability           |
| E07        | Scaling              | N/A      | P0       |    N/A | Defined | Runtime / throughput        |

This table is a planning structure.

The final experiment matrix must be frozen before the main evaluation.

---

## 24. Experiment Identifiers

Experiments should use stable identifiers.

Recommended format:

```text
EXP-001
EXP-002
EXP-003
...
```

Scenario identifiers:

```text
SCN-001
SCN-002
...
```

Intervention identifiers:

```text
INT-001
INT-002
...
```

Run identifiers:

```text
RUN-0001
RUN-0002
...
```

Identifiers must remain stable once results are published internally as evidence.

---

## 25. Experiment Configuration

Each experiment should have a machine-readable configuration.

Conceptual structure:

```yaml
experiment_id: EXP-001
scenario_id: SCN-001
dataset_version: DATASET-V001
baseline_method: B0
proposed_method: P0
budget:
  value: ...
  unit: ...
simulation:
  duration: ...
  seed: ...
metrics:
  primary:
    - cascading_impact_reduction
  secondary:
    - population_impact
    - critical_facility_impact
    - cascade_size
```

The actual configuration format may evolve during implementation.

Configuration files MUST be version controlled when they do not contain secrets or restricted data.

---

## 26. Experiment Execution Lifecycle

Each experiment should follow:

```text
Prepare
  ↓
Validate Inputs
  ↓
Freeze Configuration
  ↓
Execute Baseline
  ↓
Execute Proposed Method
  ↓
Validate Outputs
  ↓
Calculate Metrics
  ↓
Compare Results
  ↓
Generate Evidence
  ↓
Review
  ↓
Store Results
```

An experiment MUST NOT silently continue after a critical input or simulation failure.

Failure states should be recorded explicitly.

---

## 27. Input Validation

Before execution, the system should validate:

* dataset availability;
* dataset version;
* schema compatibility;
* geographic validity;
* scenario validity;
* dependency graph validity;
* intervention validity;
* budget constraints;
* simulation parameters;
* metric configuration;
* reproducibility metadata.

Invalid experiments should fail clearly rather than producing apparently valid results.

---

## 28. Output Validation

After execution, the system should validate:

* simulation completion;
* expected entity coverage;
* state transition validity;
* cascade event consistency;
* metric completeness;
* intervention selection validity;
* budget compliance;
* absence of impossible states;
* reproducibility metadata.

Output validation is part of research integrity.

---

## 29. Evidence Artifacts

Each completed experiment should preserve appropriate evidence.

Possible artifacts include:

```text
Experiment Configuration
Scenario Configuration
Dataset Manifest
Dependency Graph Metadata
Simulation Summary
Baseline Results
Proposed Results
Metric Results
Ranking Results
Sensitivity Results
Performance Results
Execution Logs
Environment Metadata
Research Notes
```

Large raw outputs should not automatically be committed to Git.

Storage strategy should depend on size, reproducibility requirements, licensing, and project policy.

---

## 30. Experiment Result Record

A result record should conceptually contain:

```text
ExperimentResult
├── experiment_id
├── run_id
├── method
├── dataset_version
├── scenario_id
├── intervention_set
├── budget
├── metrics
├── runtime
├── status
├── configuration_hash
├── software_version
├── timestamp
└── evidence_location
```

The implementation may evolve this structure as the research engine is developed.

---

## 31. Research Integrity Controls

The experiment system should prevent or expose:

* missing dataset versions;
* missing scenario versions;
* undocumented configuration changes;
* overwritten result files;
* inconsistent metric definitions;
* mismatched baseline/proposed conditions;
* budget violations;
* invalid intervention sets;
* missing random seeds for stochastic experiments;
* missing provenance;
* accidental reuse of stale results.

Research results should be traceable back to the exact experiment configuration that produced them.

---

## 32. Primary Experiment Freeze

Before the primary evaluation begins, the following SHOULD be frozen:

* research question;
* primary hypothesis;
* baseline definition;
* proposed method definition;
* primary metric;
* dataset version;
* scenario set;
* intervention set;
* budget definition;
* simulation horizon;
* major model assumptions;
* statistical analysis plan;
* experiment configuration.

Any subsequent change MUST be documented and justified.

If the change materially affects the experiment, the experiment should receive a new version or identifier.

---

## 33. Development Experiments vs Research Experiments

Not every execution is a research experiment.

### Development Execution

Used for:

* debugging;
* unit testing;
* integration testing;
* performance profiling;
* feature development;
* validating schemas;
* checking simulation behavior.

Development executions do not automatically constitute research evidence.

### Research Experiment

Must have:

* defined research objective;
* controlled configuration;
* documented inputs;
* frozen or versioned datasets;
* defined metrics;
* reproducible execution;
* preserved results;
* evidence artifact.

This distinction prevents development runs from being mistaken for scientific results.

---

## 34. Experiment Reproducibility

A research experiment should be reproducible from:

```text
Code Version
+
Dataset Version
+
Scenario Version
+
Dependency Graph Version
+
Intervention Configuration
+
Experiment Configuration
+
Environment
+
Random Seed
```

The exact reproducibility requirements are defined further in:

```text
research/REPRODUCIBILITY.md
```

---

## 35. Experiment Limitations

The experimental framework cannot by itself establish real-world effectiveness.

Simulation results may be limited by:

* incomplete datasets;
* uncertain dependencies;
* simplified hazard models;
* simplified recovery models;
* assumptions about intervention effectiveness;
* synthetic data;
* limited historical validation;
* limited geographic coverage;
* imperfect infrastructure representations.

These limitations MUST be reported with the results.

---

## 36. Expected Research Outcomes

The experiment framework permits several possible conclusions.

### Outcome A — Supported

Dependency-aware prioritization produces a meaningful and reproducible improvement over the isolated baseline.

### Outcome B — Partially Supported

Improvements occur only for particular scenario classes, budgets, or dependency conditions.

### Outcome C — Inconclusive

Evidence is insufficient to determine whether dependency-aware modeling provides meaningful improvement.

### Outcome D — Not Supported

The dependency-aware method does not outperform the baseline under the evaluated conditions.

### Outcome E — Trade-Off

Dependency-aware modeling improves resilience outcomes but introduces significant computational or data requirements.

All outcomes are valid research findings.

---

## 37. Experiment-to-Paper Traceability

Every primary paper claim should map to one or more experiments.

Recommended chain:

```text
Paper Claim
    ↓
Research Question / Hypothesis
    ↓
Experiment ID
    ↓
Scenario ID
    ↓
Dataset Version
    ↓
Method Version
    ↓
Metric
    ↓
Result Artifact
```

A result should not appear in the paper unless the underlying evidence can be traced.

---

## 38. Experiment Evolution

The experiment framework is expected to evolve during implementation.

Changes may occur because of:

* unavailable datasets;
* invalid assumptions;
* discovered model limitations;
* implementation constraints;
* computational limitations;
* improved evaluation methodology;
* reviewer feedback;
* new evidence.

Changes MUST be documented rather than silently replacing earlier experimental decisions.

---

## 39. Definition of Done

The experimental framework is considered sufficiently implemented for the primary evaluation when:

* [ ] baseline method is implemented;
* [ ] dependency-aware method is implemented;
* [ ] scenarios are versioned;
* [ ] datasets are versioned;
* [ ] interventions are defined;
* [ ] budget constraints are enforced;
* [ ] primary metric is frozen;
* [ ] experiment configuration is reproducible;
* [ ] deterministic behavior is verified where expected;
* [ ] stochastic behavior uses controlled seeds where required;
* [ ] baseline and proposed conditions are comparable;
* [ ] results are validated;
* [ ] evidence artifacts are preserved;
* [ ] performance is measured;
* [ ] limitations are documented;
* [ ] primary experiment is traceable to the paper.

---

## 40. Guiding Principle

> An experiment is not a demonstration. It is a controlled, reproducible test designed to distinguish between competing explanations or methods.

For RESOLVE, the experimental system must make it possible to determine whether dependency-aware urban modeling provides measurable value beyond isolated risk assessment.

The goal is not to prove that the proposed architecture is useful.

The goal is to construct an evaluation in which the evidence can honestly show whether it is useful, under what conditions it is useful, what it costs, and where it fails.
