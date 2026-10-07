# RESOLVE — Requirements

## 1. Purpose

This document defines the functional, research, data, engineering, performance, security, observability, and reproducibility requirements for RESOLVE.

RESOLVE is a research-oriented system for modeling urban dependencies, simulating cascading disruptions, evaluating resilience interventions, and comparing dependency-aware prioritization with isolated risk assessment.

Requirements are intentionally separated from implementation details. Specific technologies may change as the system evolves, but the requirements define the behavior and research capabilities the implementation must satisfy.

---

## 2. Requirement Principles

RESOLVE requirements follow these principles:

1. **Research before features** — every major capability must support the research question or a necessary engineering concern.
2. **Evidence before claims** — experimental claims must be connected to measurable metrics.
3. **Reproducibility** — scenarios, configurations, datasets, seeds, and experiment versions must be identifiable and reproducible.
4. **Dependency awareness** — the core research contribution must explicitly represent relationships between urban systems.
5. **Controlled comparison** — baseline and proposed approaches must be evaluated under equivalent conditions.
6. **Uncertainty awareness** — incomplete or uncertain data must not be silently treated as ground truth.
7. **City-agnostic architecture** — Karachi may be used as a case study without hard-coding the system around Karachi.
8. **Incremental complexity** — technologies and models are introduced only when they provide measurable value.

---

# 3. Functional Requirements

## FR-001 — Urban Entity Representation

The system shall represent relevant urban entities as computational objects.

Potential entity categories include:

* geographic areas
* buildings
* roads
* hospitals
* schools
* water facilities
* electricity infrastructure
* emergency facilities
* population groups
* utility components
* hazard zones
* other critical infrastructure

Each entity should support a stable identifier and relevant attributes.

---

## FR-002 — Geographic Representation

The system shall support geographic information required for urban analysis.

The representation should be capable of associating entities with:

* coordinates
* geographic boundaries
* spatial relationships
* administrative areas
* hazard exposure
* proximity relationships

Spatial data shall use an explicitly documented coordinate reference system.

---

## FR-003 — Urban Dependency Representation

The system shall represent dependencies between urban components.

A dependency shall be capable of describing:

* source component
* dependent component
* dependency type
* dependency strength
* direction
* optional geographic relationship
* optional confidence or uncertainty
* optional activation conditions

Example:

```text
Electricity Grid
      ↓
Water Pump
      ↓
Water Availability
      ↓
Hospital Service Capacity
```

The dependency model shall support multiple dependency types rather than assuming every relationship is identical.

---

## FR-004 — Hazard Representation

The system shall represent disruption or hazard scenarios.

A scenario shall be capable of describing:

* hazard type
* intensity
* geographic extent
* affected entities or areas
* start time or simulation step
* duration where applicable
* scenario assumptions
* data source
* uncertainty metadata where available

Initial hazards may include examples such as:

* extreme heat
* flooding
* electricity outage
* water disruption

The final experimental hazard set shall be documented before primary evaluation.

---

## FR-005 — Scenario Definition

The system shall support explicit scenario definitions.

A scenario shall identify, where applicable:

* scenario identifier
* geographic area
* hazard
* affected components
* simulation duration
* recovery assumptions
* dependency configuration
* intervention configuration
* resource budget
* simulation parameters
* random seed

A scenario must be serializable so that the same scenario can be reproduced.

---

## FR-006 — Isolated Risk Assessment Baseline

The system shall implement the primary isolated-risk baseline defined in `research/BASELINES.md`.

The baseline shall evaluate urban components without explicit dependency propagation.

The baseline must use the same relevant:

* data
* scenarios
* interventions
* budgets
* outcome definitions

as the proposed dependency-aware method whenever applicable.

---

## FR-007 — Dependency-Aware Assessment

The system shall implement the proposed dependency-aware RESOLVE model.

The model shall:

1. identify initially affected components,
2. evaluate dependency relationships,
3. propagate disruptions according to defined rules,
4. record affected downstream components,
5. account for intervention effects,
6. calculate defined outcome metrics.

---

## FR-008 — Cascade Simulation

The system shall support simulation of cascading disruption across the urban dependency graph.

The simulator shall support:

* initial failures or disruptions,
* dependency propagation,
* dependency strength,
* activation thresholds or rules where applicable,
* propagation depth,
* propagation breadth,
* recovery behavior,
* simulation termination conditions,
* deterministic execution when a fixed seed/configuration is supplied.

---

## FR-009 — Intervention Representation

The system shall represent resilience interventions as explicit objects or configurations.

An intervention may define:

* target component or components
* intervention type
* implementation cost
* expected effect
* affected dependency relationships
* implementation assumptions
* recovery or protection effect
* resource requirements

Interventions shall be evaluated consistently across baseline and proposed methods.

---

## FR-010 — Intervention Simulation

The system shall support simulation of scenarios with one or more interventions.

The system shall be able to compare:

```text
Scenario without intervention
        ↓
Baseline impact

Scenario with intervention
        ↓
Intervention impact

Baseline impact vs intervention impact
        ↓
Resilience benefit
```

---

## FR-011 — Intervention Prioritization

The system shall produce an intervention ranking based on defined evaluation criteria.

Possible criteria include:

* cascading impact reduction
* population protected
* critical facilities protected
* service downtime reduction
* recovery-time reduction
* resilience benefit per unit cost

The primary prioritization criterion shall be frozen before the primary experiment.

---

## FR-012 — Budget-Constrained Evaluation

The system shall support evaluation under a defined resource budget.

The budget may represent:

* financial cost
* implementation capacity
* number of interventions
* infrastructure resources
* another explicitly defined constraint

The same budget constraints shall be applied to comparable methods.

---

## FR-013 — Outcome Measurement

The system shall calculate the metrics defined in `research/METRICS.md`.

Potential measurements include:

* affected population
* affected critical facilities
* service downtime
* cascade size
* cascade depth
* cascade breadth
* recovery time
* intervention cost
* resilience benefit
* intervention efficiency
* population protected
* critical facilities protected
* dependency contribution

---

## FR-014 — Baseline Comparison

The system shall support direct comparison between:

* isolated risk assessment
* dependency-aware assessment

The comparison shall use identical experimental conditions wherever possible.

Results shall make clear which method produced each measurement.

---

## FR-015 — Ranking Evaluation

The system shall support quantitative comparison of intervention rankings.

Potential ranking metrics include:

* Spearman rank correlation
* Kendall rank correlation
* top-k overlap
* rank displacement

A difference in ranking shall not automatically be interpreted as an improvement.

---

## FR-016 — Sensitivity Analysis

The system shall support analysis of how outcomes change when uncertain parameters are varied.

Potential parameters include:

* dependency strength
* dependency completeness
* recovery assumptions
* hazard intensity
* intervention effectiveness
* geographic relationships

Sensitivity experiments shall be explicitly separated from the primary evaluation.

---

## FR-017 — Robustness Analysis

The system shall support repeated evaluation under controlled variations in:

* scenario configuration
* random seed
* dependency assumptions
* data completeness
* intervention assumptions

The purpose is to determine whether conclusions remain stable under reasonable uncertainty.

---

## FR-018 — Experiment Execution

The system shall support repeatable experiment execution.

An experiment shall identify:

* experiment identifier
* hypothesis
* baseline/proposed method
* dataset version
* scenario version
* configuration
* random seed where applicable
* software version or commit
* metrics
* output artifacts

---

## FR-019 — Experiment Results

Experiment outputs shall be stored in a structured and machine-readable form.

Results shall support later generation of:

* research tables
* figures
* statistical analysis
* paper results
* reproducibility artifacts

---

## FR-020 — Data Provenance

The system shall preserve provenance information for research datasets.

Where available, provenance shall identify:

* source
* dataset name
* acquisition date
* version
* geographic coverage
* temporal coverage
* preprocessing steps
* transformation steps
* known limitations
* licensing constraints

---

# 4. Data Requirements

## DR-001 — Structured Data Model

Core urban entities and relationships shall use structured schemas.

Schemas shall define:

* identifiers
* required fields
* optional fields
* units
* valid ranges where appropriate
* relationships
* provenance metadata where applicable

---

## DR-002 — Missing Data

Missing values shall be explicitly represented.

The system shall not silently convert missing information into valid measurements without documenting the transformation.

---

## DR-003 — Data Quality

Where practical, data pipelines shall support validation for:

* schema correctness
* missing values
* invalid coordinates
* duplicate entities
* invalid relationships
* inconsistent units
* impossible or out-of-range values

---

## DR-004 — Spatial Consistency

Datasets used together shall have documented spatial reference systems and transformations.

Spatial joins and geographic transformations shall be reproducible.

---

## DR-005 — Temporal Consistency

Datasets with temporal information shall document:

* timestamp or period
* timezone where applicable
* temporal resolution
* temporal coverage

The system shall avoid combining incompatible time periods without explicit documentation.

---

# 5. API Requirements

## API-001 — REST Interface

The backend shall expose a documented HTTP API for supported application functionality.

The API shall provide access to appropriate:

* entities
* scenarios
* dependencies
* simulations
* interventions
* experiment results

Exact endpoint contracts shall be defined separately in `docs/API_CONTRACT.md`.

---

## API-002 — Input Validation

API inputs shall be validated before reaching domain logic.

Invalid requests shall return structured error responses.

---

## API-003 — API Documentation

The API shall provide machine-readable and human-readable documentation where supported by the framework.

---

## API-004 — Separation of Concerns

API routes shall not contain the primary simulation or research logic.

Core domain/application functionality shall remain reusable by:

* REST APIs
* experiments
* tests
* command-line workflows where appropriate

---

# 6. Engineering Requirements

## ER-001 — Layered Architecture

The backend shall maintain clear separation between:

* API/interface
* application services
* domain logic
* infrastructure
* data access
* simulation
* research/experiment execution

---

## ER-002 — Automated Testing

The system shall include automated tests for critical behavior.

Testing shall cover, as appropriate:

* domain rules
* dependency propagation
* cascade simulation
* intervention evaluation
* metric calculations
* API contracts
* data validation
* error handling

---

## ER-003 — Deterministic Testing

Tests for deterministic behavior shall use controlled inputs and fixed random seeds where randomness is involved.

---

## ER-004 — Database Migrations

Database schema changes shall be version-controlled through migrations.

Manual production-schema changes shall not be treated as the normal development workflow.

---

## ER-005 — Configuration Management

Environment-specific configuration shall not be hard-coded into application logic.

Configuration shall support:

* local development
* testing
* experiment execution
* deployment

Sensitive values shall not be committed to source control.

---

## ER-006 — Reproducible Environment

The project shall provide a documented mechanism for recreating the required development and experiment environment.

---

## ER-007 — Version Control

Source code, configuration templates, research documentation, experiment definitions, and reproducibility artifacts shall be version-controlled where licensing and data constraints permit.

---

# 7. Performance Requirements

Performance targets shall be established through measurement rather than arbitrary optimization.

The system shall support measurement of:

* API latency
* p50 latency
* p95 latency
* p99 latency where relevant
* throughput
* database query performance
* simulation runtime
* experiment throughput
* memory consumption

Optimization decisions shall be based on measured bottlenecks.

---

# 8. Scalability Requirements

The architecture should support growth in:

* number of urban entities
* number of dependency edges
* geographic coverage
* scenario count
* simulation count
* experiment repetitions
* concurrent API requests

Scalability claims shall be supported by benchmarks.

---

# 9. Security Requirements

## SEC-001 — Secret Protection

Secrets shall not be committed to the repository.

Examples include:

* database credentials
* API keys
* authentication secrets
* private service credentials

---

## SEC-002 — Input Safety

External inputs shall be validated and constrained before being processed.

---

## SEC-003 — Least Privilege

Application components and external services should receive only the permissions required for their function.

---

## SEC-004 — Dependency Security

Third-party dependencies shall be tracked and kept reasonably current where compatibility permits.

---

## SEC-005 — Research Data Safety

Datasets containing sensitive or restricted information shall be handled according to their applicable licensing, privacy, and access requirements.

RESOLVE shall prefer aggregated, public, synthetic, or appropriately anonymized data for research demonstrations when possible.

---

# 10. Observability Requirements

The system shall provide sufficient observability to diagnose application and simulation behavior.

Where appropriate, observability shall include:

* structured logs
* request identifiers
* simulation identifiers
* experiment identifiers
* execution duration
* error information
* relevant system metrics

Observability must not expose secrets or unnecessarily sensitive data.

---

# 11. Reproducibility Requirements

## REP-001 — Scenario Reproducibility

A recorded scenario configuration shall be sufficient to recreate the scenario under the same dataset and software version.

---

## REP-002 — Seed Reproducibility

Experiments involving stochastic behavior shall record the random seed.

---

## REP-003 — Configuration Reproducibility

Experiment configurations shall be version-controlled or stored as immutable artifacts.

---

## REP-004 — Dataset Versioning

Experiments shall identify the dataset version or snapshot used.

---

## REP-005 — Software Versioning

Primary experiments shall identify the relevant source-code version or Git commit.

---

## REP-006 — Result Traceability

Reported research results shall be traceable to the experiment configuration and underlying output artifacts.

---

# 12. Research Requirements

## RR-001 — Hypothesis Alignment

Every primary experiment shall map to one or more hypotheses defined in `research/HYPOTHESES.md`.

---

## RR-002 — Baseline Alignment

The primary evaluation shall use the frozen baseline definitions in `research/BASELINES.md`.

---

## RR-003 — Metric Freeze

Primary metrics shall be defined before the main evaluation.

Changing a primary metric after observing results shall require explicit documentation and shall not silently replace the original metric.

---

## RR-004 — Fair Comparison

Baseline and proposed approaches shall use equivalent:

* scenarios
* datasets
* interventions
* budgets
* relevant assumptions
* evaluation metrics

unless a documented methodological reason requires otherwise.

---

## RR-005 — Negative Results

The research workflow shall preserve and report neutral, negative, or inconclusive results.

The system shall not be designed solely to produce positive findings.

---

## RR-006 — Statistical Evaluation

Where appropriate, repeated experiment results shall support statistical analysis.

Potential methods include:

* paired statistical tests
* bootstrap confidence intervals
* permutation tests
* non-parametric tests

The selected statistical method shall depend on the experimental design and data distribution.

---

## RR-007 — Practical Significance

Statistical significance shall not automatically be interpreted as practical significance.

Results shall consider the magnitude and operational meaning of observed differences.

---

# 13. Non-Functional Quality Requirements

RESOLVE should be:

* maintainable
* testable
* observable
* reproducible
* explainable
* extensible
* documented
* measurable

The system should favor understandable engineering decisions over unnecessary architectural complexity.

---

# 14. Out-of-Scope Requirements

The following are not required for the initial research system:

* real-time emergency dispatch
* direct control of physical infrastructure
* autonomous emergency decisions
* guaranteed prediction of real-world disasters
* replacement of government emergency systems
* clinical decision-making
* individual-level surveillance
* unrestricted collection of personal data
* production-scale city-wide operational deployment
* claims of universal applicability across all cities

These may be reconsidered in future work only with appropriate research, safety, governance, and validation.

---

# 15. Requirement Prioritization

Requirements shall be implemented in the following general priority order:

### Priority 1 — Research Core

* urban entity representation
* dependency graph
* scenario definition
* isolated baseline
* dependency-aware model
* cascade simulation
* intervention representation
* intervention comparison
* primary metrics
* reproducible experiments

### Priority 2 — Data and Evaluation

* data provenance
* spatial processing
* sensitivity analysis
* robustness analysis
* ranking evaluation
* statistical analysis
* experiment result storage

### Priority 3 — Engineering Hardening

* API refinement
* observability
* performance benchmarking
* scalability testing
* security hardening
* deployment infrastructure

### Priority 4 — Presentation

* frontend visualization
* interactive maps
* dashboards
* research result exploration

Presentation features shall not take priority over the research core.

---

# 16. Requirement Traceability

Major requirements shall eventually be traceable across the project:

```text
Research Question
      ↓
Hypothesis
      ↓
Requirement
      ↓
Architecture
      ↓
Implementation
      ↓
Test
      ↓
Experiment
      ↓
Metric
      ↓
Evidence
      ↓
Paper Claim
```

A requirement that cannot be connected to a meaningful system, research, or engineering objective should be reconsidered.

---

# 17. Requirement Status

This document defines the initial requirements baseline.

Requirements may evolve during implementation when:

* new research evidence changes the methodology,
* an assumption is shown to be invalid,
* an engineering constraint requires adjustment,
* an experiment exposes a missing capability,
* a requirement is proven unnecessary.

Changes shall be recorded in `docs/DECISION_LOG.md` and reflected in relevant research documentation.

---

## 18. Initial Acceptance Criteria

The initial RESOLVE research prototype will be considered functionally ready for primary experimentation when it can:

1. represent urban entities,
2. represent dependencies between entities,
3. define reproducible disruption scenarios,
4. execute the isolated baseline,
5. execute the dependency-aware model,
6. simulate cascading effects,
7. apply defined interventions,
8. compare intervention outcomes,
9. calculate the frozen primary metric,
10. execute controlled repeated experiments,
11. record experiment configuration and provenance,
12. produce reproducible machine-readable results,
13. pass the required automated tests,
14. provide sufficient evidence to support or reject the stated hypotheses.

Only after these criteria are satisfied should the project move into the primary research evaluation phase.
