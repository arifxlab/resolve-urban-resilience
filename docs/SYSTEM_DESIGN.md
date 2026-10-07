# RESOLVE — System Design

## 1. Purpose

This document defines the detailed system design for RESOLVE — Resilient Urban Systems & Vulnerability Observatory.

It translates the high-level architecture into concrete system responsibilities, data flows, execution boundaries, interfaces, and runtime behavior.

The design is intentionally implementation-oriented while preserving research flexibility.

The system must support:

* urban system representation;
* hazards and disruption scenarios;
* dependency-aware cascade simulation;
* isolated baseline assessment;
* intervention modeling;
* intervention prioritization;
* reproducible experiments;
* measurable outcomes;
* research evidence generation.

The system should remain understandable enough that individual components can be tested independently and the complete research pipeline can be reproduced.

---

## 2. Design Principles

### 2.1 Research First

Every major system component must support a research question, hypothesis, requirement, or evaluation objective.

Features without research or engineering justification should not be introduced merely for complexity.

### 2.2 Deterministic Core

The simulation and evaluation core should support deterministic execution through explicit configuration and random seeds where stochastic behavior is introduced.

### 2.3 Explicit Dependencies

Relationships between urban components must be represented explicitly rather than hidden inside application logic.

### 2.4 Separation of Concerns

API handling, application orchestration, domain modeling, simulation, persistence, and research evaluation must remain separate.

### 2.5 Baseline Comparability

The isolated baseline and dependency-aware method must operate on equivalent scenario data, intervention sets, budgets, and evaluation metrics whenever scientifically appropriate.

### 2.6 Observable Execution

Important simulation and experiment executions must produce structured metadata sufficient to understand what was executed and why.

### 2.7 Reproducibility

A completed experiment should be reproducible from:

```text
Dataset Version
+
Scenario Configuration
+
Model Configuration
+
Intervention Set
+
Budget
+
Simulation Parameters
+
Random Seed
+
Software Version
```

### 2.8 Controlled Complexity

Advanced infrastructure such as distributed task queues, ML models, caching, or real-time processing should only be introduced when measurements demonstrate a need.

---

# 3. System Context

At the highest level, RESOLVE receives urban data and research scenarios and produces measurable resilience outcomes.

```text
                 External Data Sources
                         |
                         v
              +-----------------------+
              | Data Ingestion Layer  |
              +-----------------------+
                         |
                         v
              +-----------------------+
              | Canonical Urban Data  |
              +-----------------------+
                         |
                         v
              +-----------------------+
              | Urban System Model    |
              +-----------------------+
                         |
             +-----------+-----------+
             |                       |
             v                       v
      Isolated Baseline      Dependency Graph
             |                       |
             |                       v
             |                Cascade Simulation
             |                       |
             +-----------+-----------+
                         |
                         v
              Intervention Evaluation
                         |
                         v
               Research Evaluation
                         |
                         v
              Experiment Evidence
```

The frontend is not the authoritative research layer.

It presents system state, scenarios, simulations, and results produced by the backend.

---

# 4. Major System Components

## 4.1 API Layer

Technology direction:

* FastAPI
* Pydantic

Responsibilities:

* HTTP request handling;
* request validation;
* authentication boundary when required;
* response serialization;
* API documentation;
* error translation;
* API versioning.

The API layer must not contain core simulation algorithms or research calculations.

---

## 4.2 Application Layer

Responsibilities:

* orchestrating use cases;
* coordinating domain services;
* managing application workflows;
* starting simulations;
* executing experiments;
* evaluating interventions;
* coordinating persistence.

Example application use cases:

```text
Create Scenario
Load Urban Model
Run Baseline Assessment
Run Dependency-Aware Simulation
Evaluate Intervention
Compare Interventions
Run Experiment
Retrieve Experiment Results
```

Application services should coordinate work rather than implement low-level domain rules.

---

## 4.3 Domain Layer

The domain layer represents the concepts of the urban resilience problem.

Core concepts include:

```text
UrbanEntity
InfrastructureComponent
CriticalFacility
PopulationGroup
Dependency
Hazard
Scenario
Intervention
ResourceBudget
Simulation
CascadeEvent
Recovery
Outcome
Experiment
Dataset
DataProvenance
```

Domain rules should remain independent from FastAPI, PostgreSQL, Redis, Celery, or frontend technologies.

---

# 5. Urban Model

The urban model represents the state of the system being studied.

At minimum, an urban model contains:

```text
Entities
Geographic Relationships
Functional Dependencies
Attributes
Operational States
Data Provenance
```

Each entity should have a stable identifier.

Example conceptual representation:

```text
UrbanEntity
├── id
├── type
├── name
├── geometry
├── attributes
├── state
├── source
└── confidence
```

The system should distinguish between observed attributes and modeled attributes.

For example:

```text
Observed:
hospital location

Derived:
population within service area

Modeled:
dependency strength

Simulated:
hospital operational state after outage
```

This distinction is important for research transparency.

---

# 6. Dependency Graph

The dependency graph represents functional relationships between urban components.

Conceptually:

```text
Power Plant
    |
    v
Electric Grid
    |
    v
Water Pump
    |
    v
Water Distribution
    |
    v
Hospital
```

A dependency should contain enough information to support analysis and uncertainty representation.

Conceptual structure:

```text
Dependency
├── source_entity_id
├── dependent_entity_id
├── dependency_type
├── direction
├── strength
├── threshold
├── confidence
├── geographic_relation
└── provenance
```

The graph must support:

* directed dependencies;
* dependency strength;
* threshold-based effects;
* confidence;
* dependency types;
* graph traversal;
* dependency contribution analysis.

NetworkX is the initial technology direction for graph analysis unless experiments demonstrate a need for another implementation.

---

# 7. Hazard and Scenario Model

A scenario defines the conditions under which an experiment or simulation is executed.

Conceptually:

```text
Scenario
├── id
├── name
├── hazard
├── geographic_scope
├── intensity
├── start_time
├── duration
├── affected_entities
├── initial_conditions
├── recovery_configuration
└── version
```

A scenario must be immutable once used in a completed experiment.

If a scenario changes, it receives a new version.

This prevents historical experiment results from silently changing.

---

# 8. Baseline Assessment

The primary baseline is the isolated risk assessment.

The baseline evaluates components independently without explicit dependency propagation.

Conceptually:

```text
Hazard
   |
   +--> Component A Risk
   |
   +--> Component B Risk
   |
   +--> Component C Risk
```

No cascade propagation occurs between components.

The baseline must use the same:

* scenario;
* source data;
* intervention candidates;
* budget;
* outcome definitions;

as the proposed dependency-aware method whenever applicable.

This is necessary for fair comparison.

---

# 9. Dependency-Aware Simulation

The proposed method introduces explicit dependency propagation.

Conceptually:

```text
Hazard
   |
   v
Initial Impact
   |
   v
Dependency Evaluation
   |
   v
State Changes
   |
   v
New Failures / Degradation
   |
   v
Further Dependency Evaluation
   |
   v
Cascade
   |
   v
Recovery
   |
   v
Final Outcome
```

The simulation engine should operate through discrete state transitions.

A simplified simulation loop is:

```text
1. Initialize scenario
2. Apply initial hazard effects
3. Evaluate affected entities
4. Evaluate outgoing dependencies
5. Generate cascade events
6. Apply state transitions
7. Apply recovery rules
8. Record state and events
9. Repeat until termination condition
10. Calculate outcomes
```

The exact implementation should be determined after domain rules are sufficiently defined.

---

# 10. Simulation State

A simulation state represents the condition of the urban model at a particular point in the simulation.

Conceptually:

```text
SimulationState
├── simulation_time
├── entity_states
├── active_events
├── service_levels
├── resource_state
└── metadata
```

Possible entity states:

```text
NORMAL
DEGRADED
DISRUPTED
FAILED
RECOVERING
RECOVERED
```

State transitions must be explicit and testable.

---

# 11. Cascade Events

Cascade events represent changes caused by hazards, dependencies, interventions, or recovery.

Conceptual structure:

```text
CascadeEvent
├── id
├── simulation_id
├── timestamp
├── source_entity
├── affected_entity
├── event_type
├── previous_state
├── new_state
├── cause
├── dependency_id
└── metadata
```

Events provide an audit trail for understanding why the simulated system changed.

They also support research analysis of:

* cascade pathways;
* cascade depth;
* cascade breadth;
* critical dependencies;
* intervention effects.

---

# 12. Recovery Model

Recovery represents the transition of disrupted components toward normal operation.

A recovery configuration may define:

```text
Recovery Rate
Recovery Delay
Repair Priority
Resource Requirement
Maximum Recovery Time
```

Recovery behavior must not be assumed to be universal.

Different infrastructure categories may require different recovery rules.

Where empirical recovery data is unavailable, assumptions must be documented and sensitivity-tested.

---

# 13. Intervention Model

Interventions represent actions intended to reduce disruption or improve recovery.

Examples may include:

```text
Infrastructure Hardening
Backup Power
Redundant Water Supply
Dependency Reduction
Additional Emergency Capacity
Recovery Resource Allocation
```

Each intervention should define:

```text
Intervention
├── id
├── target_entities
├── affected_dependencies
├── implementation_cost
├── resource_requirements
├── expected_effects
├── constraints
└── provenance
```

Interventions must be evaluated under explicit resource constraints.

---

# 14. Intervention Evaluation

The evaluation workflow is:

```text
Scenario
   |
   v
Candidate Intervention
   |
   v
Simulation
   |
   v
Outcome Metrics
   |
   v
Baseline Comparison
   |
   v
Intervention Score
```

The system should support evaluating multiple interventions under the same scenario.

For a budget-constrained experiment:

```text
Candidate Interventions
        |
        v
Feasible Combinations
        |
        v
Simulation / Evaluation
        |
        v
Outcome Measurement
        |
        v
Ranking
```

Optimization algorithms should not be introduced until the evaluation process itself is validated.

An initial implementation may use exhaustive evaluation for small experimental datasets.

---

# 15. Research Experiment Execution

An experiment combines all information required to reproduce an evaluation.

Conceptual structure:

```text
Experiment
├── experiment_id
├── dataset_version
├── scenario_version
├── baseline_method
├── proposed_method
├── interventions
├── budget
├── simulation_config
├── random_seed
├── software_version
├── metrics
└── results
```

Execution flow:

```text
Create Experiment
        |
        v
Validate Inputs
        |
        v
Freeze Configuration
        |
        v
Run Baseline
        |
        v
Run Proposed Method
        |
        v
Evaluate Outcomes
        |
        v
Compare Results
        |
        v
Persist Evidence
```

An experiment should not depend on mutable external state without recording the relevant version or configuration.

---

# 16. Result Model

Experiment results should separate raw simulation outputs from derived research metrics.

Example:

```text
Simulation Output
        |
        +--> State History
        +--> Cascade Events
        +--> Recovery Timeline
        +--> Resource Usage
        |
        v
Metric Calculation
        |
        +--> Impact
        +--> Cascade Size
        +--> Recovery Time
        +--> Population Impact
        +--> Facility Impact
        +--> Intervention Benefit
        |
        v
Research Result
```

This separation makes metric definitions independently testable.

---

# 17. Data Flow

The primary research data flow is:

```text
External Dataset
      |
      v
Raw Storage
      |
      v
Validation
      |
      v
Normalization
      |
      v
Canonical Data Model
      |
      v
Urban Model
      |
      v
Scenario Construction
      |
      v
Simulation
      |
      v
Outcome Calculation
      |
      v
Experiment Result
      |
      v
Research Evidence
```

Every transformation should have a documented purpose.

---

# 18. API-to-Application Flow

A typical simulation request follows:

```text
HTTP Request
    |
    v
FastAPI Router
    |
    v
Request Validation
    |
    v
Application Service
    |
    v
Domain Model
    |
    v
Simulation Engine
    |
    v
Result / Persistence
    |
    v
Response Schema
    |
    v
HTTP Response
```

The router should not directly execute database queries, manipulate graph structures, or implement simulation rules.

---

# 19. Persistence Design

PostgreSQL/PostGIS is the primary persistence technology.

Conceptual storage groups:

```text
Urban Data
├── urban_entities
├── infrastructure_components
├── critical_facilities
├── population_groups
└── geographic_data

Dependency Data
├── dependencies
└── dependency_metadata

Scenario Data
├── hazards
├── scenarios
└── scenario_versions

Experiment Data
├── experiments
├── simulations
├── cascade_events
├── outcomes
└── experiment_results
```

Exact database schema should be derived from the domain model and refined during implementation.

Database migrations must be managed through Alembic.

---

# 20. Redis and Background Processing

Redis and Celery are optional infrastructure components.

They should be introduced only when simulation or experiment execution demonstrates a meaningful need for:

* long-running background tasks;
* task queues;
* distributed execution;
* temporary caching;
* workload isolation.

Initial development should prefer synchronous execution when simulations are sufficiently small.

This keeps early development simple and makes debugging easier.

---

# 21. Configuration

Configuration must be externalized from application code.

Important configuration categories include:

```text
Database
API
Simulation
Experiment
Logging
External Data
Security
Task Processing
```

Environment-specific configuration should not alter research semantics without being explicitly recorded.

For research execution, the effective configuration should be persisted with experiment metadata.

---

# 22. Error Handling

Errors should be categorized.

### Validation Errors

Invalid input, missing fields, invalid ranges, or inconsistent identifiers.

### Domain Errors

Invalid state transitions, invalid dependencies, invalid interventions, or violated domain invariants.

### Infrastructure Errors

Database, storage, queue, or external service failures.

### Simulation Errors

Invalid simulation configuration, unsupported scenario state, or execution failure.

### Research Errors

Invalid experiment configuration, incompatible datasets, missing metrics, or reproducibility violations.

Errors returned through the API should use structured response formats.

Internal errors must be logged without exposing sensitive implementation details.

---

# 23. Observability

The system should produce structured logs for important operations.

Important events include:

```text
Scenario Created
Scenario Loaded
Simulation Started
Simulation Completed
Simulation Failed
Experiment Started
Experiment Completed
Experiment Failed
Dataset Loaded
Intervention Evaluated
```

Important metadata may include:

```text
request_id
experiment_id
simulation_id
scenario_id
dataset_version
duration
status
error_type
```

Metrics should be introduced incrementally.

Initial observability should prioritize correctness and reproducibility over operational complexity.

---

# 24. Testing Strategy

Testing occurs at multiple levels.

### Unit Tests

Test:

* domain rules;
* dependency behavior;
* state transitions;
* risk calculations;
* intervention effects;
* metric calculations.

### Integration Tests

Test:

* database persistence;
* repository behavior;
* API/application integration;
* scenario loading;
* simulation persistence.

### Simulation Tests

Use deterministic scenarios with known expected outcomes.

Example:

```text
A -> B -> C

A fails

Expected:
A = FAILED
B = affected
C = affected through B
```

### Research Tests

Verify:

* metric calculations;
* baseline/proposed comparability;
* experiment configuration;
* deterministic execution;
* result reproducibility.

### API Tests

Verify:

* request validation;
* response schemas;
* error responses;
* endpoint behavior.

---

# 25. Reproducibility Design

A reproducible execution should record:

```text
Dataset Version
Scenario Version
Model Version
Experiment Configuration
Simulation Configuration
Random Seed
Software Commit
Dependency Environment
Execution Timestamp
```

Where practical, the Git commit SHA should identify the software state used for the experiment.

Experiment outputs should be stored separately from source code while retaining a clear link between them.

---

# 26. Security Design Considerations

Although RESOLVE is primarily a research system, security must be designed into the architecture.

Minimum requirements include:

* environment-based secret management;
* no credentials in source control;
* input validation;
* parameterized database operations;
* controlled file handling;
* dependency management;
* least-privilege database access;
* structured error handling;
* logging without sensitive information.

The system must not require individual-level personal data for its core research function.

---

# 27. Scalability Strategy

Scalability should be measured rather than assumed.

The initial implementation should prioritize:

```text
Correctness
Reproducibility
Testability
Research Validity
```

before distributed scalability.

If experiments become computationally expensive, scaling options include:

```text
Parallel Scenario Execution
Background Task Queues
Simulation Workers
Caching
Batch Processing
Distributed Experiment Execution
```

Each optimization should be supported by measured performance evidence.

---

# 28. Initial Runtime Sequence

A representative research execution is:

```text
1. Load dataset
2. Validate dataset
3. Build urban model
4. Construct dependency graph
5. Load scenario
6. Validate scenario
7. Create experiment
8. Freeze experiment configuration
9. Run isolated baseline
10. Run dependency-aware simulation
11. Apply intervention
12. Run intervention simulation
13. Calculate metrics
14. Compare methods
15. Store results
16. Generate evidence
17. Record software/configuration versions
```

The exact sequence may evolve as implementation and experiments expose new requirements.

---

# 29. Research-to-System Traceability

The system must preserve traceability between research concepts and implementation.

```text
Research Question
      |
      v
Hypothesis
      |
      v
Requirement
      |
      v
Domain Concept
      |
      v
Implementation
      |
      v
Test
      |
      v
Experiment
      |
      v
Metric
      |
      v
Evidence
      |
      v
Paper Result
```

This chain is a core design requirement rather than documentation decoration.

---

# 30. Initial Implementation Order

Implementation should proceed in the following order:

### Phase 1 — Domain Foundation

* domain entities;
* identifiers;
* enums;
* validation rules;
* dependency representation;
* scenario representation.

### Phase 2 — Persistence

* PostgreSQL configuration;
* SQLAlchemy models;
* Alembic migrations;
* repositories.

### Phase 3 — Baseline

* isolated assessment;
* baseline metrics;
* deterministic tests.

### Phase 4 — Dependency Graph

* graph construction;
* dependency traversal;
* dependency validation;
* graph tests.

### Phase 5 — Simulation

* state transitions;
* cascade propagation;
* recovery;
* deterministic simulation scenarios.

### Phase 6 — Interventions

* intervention representation;
* budget constraints;
* intervention evaluation;
* ranking.

### Phase 7 — Research Engine

* experiment configuration;
* metric calculation;
* baseline comparison;
* reproducibility metadata;
* evidence generation.

### Phase 8 — API

* REST endpoints;
* schemas;
* application services;
* API integration tests.

### Phase 9 — Frontend

* urban model visualization;
* scenario configuration;
* simulation execution;
* intervention comparison;
* result visualization.

Infrastructure complexity should be introduced only when justified by the implementation or experimental workload.

---

# 31. Design Acceptance Criteria

The system design is considered sufficiently defined when:

* domain responsibilities are separated from API concerns;
* application workflows are explicit;
* baseline and proposed methods can be compared fairly;
* dependencies can be represented explicitly;
* simulations can be deterministic;
* interventions can be evaluated under constraints;
* experiments can record reproducibility metadata;
* results can be connected to defined metrics;
* persistence boundaries are clear;
* testing boundaries are defined;
* observability requirements are documented;
* security responsibilities are identified;
* research traceability exists from question to evidence.

---

# 32. Guiding System Design

The central system design principle is:

> **Represent the urban system explicitly, model dependencies transparently, simulate consequences reproducibly, evaluate interventions quantitatively, and preserve enough evidence to support scientific comparison.**

RESOLVE should remain a research system first and a visualization product second.
