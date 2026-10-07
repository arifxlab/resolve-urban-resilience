# RESOLVE — System Architecture

## 1. Purpose

This document defines the high-level software architecture of RESOLVE.

The architecture translates the research requirements and domain model into a maintainable engineering structure while keeping the research core independent from presentation and infrastructure concerns.

RESOLVE is designed as a research-oriented urban resilience platform rather than a conventional dashboard application.

The primary architectural objective is:

> Enable reproducible modeling, simulation, comparison, and measurement of dependency-aware urban resilience interventions.

---

# 2. Architectural Principles

RESOLVE follows these principles:

1. **Research core before interface**
2. **Domain before framework**
3. **Explicit dependencies**
4. **Separation of concerns**
5. **Reproducibility by design**
6. **Measurement before optimization**
7. **Infrastructure only when justified**
8. **Testable core logic**
9. **Observable execution**
10. **City-agnostic design**
11. **Controlled complexity**
12. **Evidence-driven evolution**

---

# 3. Architectural Overview

The system is organized into several major layers:

```text id="8f1q7p"
┌────────────────────────────────────────────────────────────┐
│                     Presentation Layer                     │
│                  React / TypeScript / Maps                 │
└────────────────────────────┬───────────────────────────────┘
                             │
                             ▼
┌────────────────────────────────────────────────────────────┐
│                         API Layer                          │
│                        FastAPI                             │
└────────────────────────────┬───────────────────────────────┘
                             │
                             ▼
┌────────────────────────────────────────────────────────────┐
│                    Application Layer                       │
│       Use Cases / Orchestration / Research Workflows       │
└────────────────────────────┬───────────────────────────────┘
                             │
                             ▼
┌────────────────────────────────────────────────────────────┐
│                       Domain Layer                         │
│ Entities / Dependencies / Scenarios / Interventions       │
└───────────────┬───────────────────────────┬────────────────┘
                │                           │
                ▼                           ▼
┌──────────────────────────┐    ┌───────────────────────────┐
│    Simulation Engine     │    │      Research Engine      │
│ Cascade / Recovery      │    │ Experiments / Metrics     │
└─────────────┬────────────┘    └─────────────┬─────────────┘
              │                               │
              └──────────────┬────────────────┘
                             ▼
┌────────────────────────────────────────────────────────────┐
│                  Infrastructure Layer                      │
│ PostgreSQL/PostGIS / Redis / Files / External Data        │
└────────────────────────────────────────────────────────────┘
```

The frontend is a consumer of the system, not the owner of the research logic.

---

# 4. Backend-First Architecture

The backend is the primary system boundary during initial development.

The backend must be capable of:

* representing the urban model,
* executing scenarios,
* running simulations,
* calculating metrics,
* executing experiments,
* exposing results through APIs.

The frontend will later provide visualization and interaction over these capabilities.

This prevents research logic from becoming coupled to UI implementation.

---

# 5. Backend Package Structure

The intended backend structure is:

```text id="2n6xwz"
backend/
└── app/
    ├── api/
    ├── core/
    ├── domain/
    ├── application/
    ├── infrastructure/
    ├── simulation/
    ├── risk/
    ├── graph/
    ├── data/
    └── ml/
```

Not every package requires substantial implementation immediately.

Packages should become active only when their corresponding responsibility is needed.

---

# 6. API Layer

The API layer provides the external HTTP interface.

Responsibilities include:

* routing
* request parsing
* input validation
* authentication where required
* response serialization
* HTTP error handling
* API documentation
* request-level observability

The API layer must not contain the primary simulation algorithms.

---

# 7. Application Layer

The application layer coordinates domain operations.

Responsibilities include:

* executing use cases,
* coordinating repositories,
* orchestrating simulations,
* starting experiments,
* validating application-level constraints,
* managing workflows.

Example use cases:

```text id="bqv6ue"
Create Scenario
Run Simulation
Evaluate Intervention
Compare Methods
Run Experiment
Retrieve Results
```

Application services should depend on abstractions rather than concrete infrastructure wherever practical.

---

# 8. Domain Layer

The domain layer contains the core concepts and rules.

Potential domain components include:

```text id="nq7n2x"
entities
dependencies
hazards
scenarios
interventions
budgets
simulation states
outcomes
```

The domain layer should remain independent of:

* FastAPI
* PostgreSQL
* Redis
* Celery
* React
* external API clients

This makes core logic easier to test and reuse.

---

# 9. Simulation Layer

The simulation layer implements the computational model of cascading disruption.

Responsibilities include:

* initial impact application,
* dependency traversal,
* cascade propagation,
* state transitions,
* recovery,
* intervention effects,
* simulation termination,
* simulation result generation.

The simulation engine should consume domain representations rather than raw API requests.

---

# 10. Graph Layer

The graph layer manages dependency-aware computation.

Responsibilities may include:

* graph construction,
* node and edge representation,
* traversal,
* dependency lookup,
* graph metrics,
* subgraph extraction,
* cascade-path analysis.

NetworkX may be used initially if it provides sufficient functionality.

A dedicated graph database should not be introduced unless experiments demonstrate a concrete requirement.

---

# 11. Risk Layer

The risk layer contains risk and impact calculations.

Potential responsibilities:

* exposure calculations,
* vulnerability calculations,
* consequence calculations,
* component risk scoring,
* baseline calculations,
* risk aggregation.

The isolated baseline should use this layer without invoking dependency propagation.

---

# 12. Data Layer

The data layer handles:

* dataset ingestion,
* normalization,
* validation,
* provenance,
* transformation,
* dataset manifests,
* research dataset preparation.

It should separate external source formats from canonical domain representations.

---

# 13. Infrastructure Layer

The infrastructure layer implements technical details required by other layers.

Potential responsibilities:

* database repositories,
* PostgreSQL/PostGIS access,
* Redis integration,
* task queues,
* file storage,
* external data clients,
* telemetry integrations.

Infrastructure components should not define research methodology.

---

# 14. Research Layer

Research functionality may be implemented under application, simulation, or dedicated research modules depending on final package boundaries.

Research workflows should support:

* experiment definitions,
* controlled repetitions,
* random seeds,
* metric calculation,
* statistical analysis,
* baseline comparison,
* sensitivity analysis,
* result export.

Research execution must be able to run without the frontend.

---

# 15. Machine Learning Layer

The `ml/` package is intentionally optional.

Machine learning shall only be introduced when a genuine predictive task exists.

Potential future uses may include:

* hazard prediction,
* infrastructure failure prediction,
* demand estimation,
* vulnerability estimation,
* missing-data estimation.

Machine learning must not be added merely to label RESOLVE as an AI project.

If no validated predictive task is required, the research system should remain primarily simulation- and dependency-model-driven.

---

# 16. Dependency Direction

The intended dependency direction is:

```text id="5g7j11"
API
 │
 ▼
Application
 │
 ▼
Domain
 ▲
 │
Simulation / Risk / Graph
 │
 ▲
Infrastructure
```

A more precise interpretation is:

```text id="x6z1tr"
Presentation
     ↓
API
     ↓
Application
     ↓
Domain
     ↑
Infrastructure
```

Infrastructure implements interfaces required by the application/domain boundaries rather than becoming the source of domain rules.

---

# 17. Research Independence

Research experiments must not require the HTTP API to execute.

Preferred execution paths include:

```text id="s5a4o1"
CLI / Experiment Runner
          │
          ▼
   Application Services
          │
          ▼
     Domain + Engines
```

and:

```text id="4kz5g8"
HTTP Request
      │
      ▼
     API
      │
      ▼
Application Services
      │
      ▼
Domain + Engines
```

Both paths should reach the same core behavior.

---

# 18. Baseline Architecture

The isolated baseline should follow:

```text id="x1uj21"
Scenario
   │
   ▼
Hazard / Exposure
   │
   ▼
Component Risk
   │
   ▼
Independent Outcomes
   │
   ▼
Intervention Evaluation
```

No dependency propagation should occur in the baseline.

This separation is important for experimental validity.

---

# 19. Proposed Architecture

The dependency-aware model follows:

```text id="l8m2x7"
Scenario
   │
   ▼
Hazard / Initial Impact
   │
   ▼
Dependency Graph
   │
   ▼
Cascade Simulation
   │
   ├── Propagation
   ├── State Changes
   └── Recovery
   │
   ▼
System-Level Outcomes
   │
   ▼
Intervention Evaluation
```

---

# 20. Scenario Execution Architecture

A scenario execution should conceptually follow:

```text id="w5h4jp"
Load Scenario
     ↓
Validate Configuration
     ↓
Load Urban Model
     ↓
Load Dependency Graph
     ↓
Apply Hazard
     ↓
Determine Initial Impact
     ↓
Run Simulation
     ↓
Calculate Outcomes
     ↓
Persist / Export Results
```

---

# 21. Intervention Evaluation Architecture

For a candidate intervention:

```text id="7u0i8e"
Base Scenario
      │
      ├───────────────┐
      ▼               ▼
No Intervention   Intervention
      │               │
      ▼               ▼
Simulation A      Simulation B
      │               │
      ▼               ▼
Baseline Impact   Intervention Impact
          \       /
           \     /
            ▼   ▼
       Benefit Metrics
            │
            ▼
       Ranking / Budget
```

This structure supports controlled comparison.

---

# 22. Experiment Architecture

The experiment runner should coordinate repeated simulations.

```text id="qv3x6g"
Experiment Definition
        │
        ├── Dataset
        ├── Scenario
        ├── Baseline
        ├── Proposed Method
        ├── Metrics
        └── Seeds
               │
               ▼
        Experiment Runner
               │
        ┌──────┼──────┐
        ▼      ▼      ▼
      Run 1  Run 2  Run N
        │      │      │
        └──────┼──────┘
               ▼
       Aggregated Results
               │
               ▼
        Statistical Analysis
               │
               ▼
          Evidence Artifacts
```

---

# 23. Storage Architecture

The initial storage architecture is:

```text id="4m5p7u"
                 PostgreSQL/PostGIS
                         │
       ┌─────────────────┼──────────────────┐
       ▼                 ▼                  ▼
Urban Entities      Dependencies       Experiment Metadata
       │                 │                  │
       └─────────────────┼──────────────────┘
                         ▼
                     Scenarios

Redis
  │
  └── Temporary state / caching / task coordination

File Storage
  │
  ├── Raw datasets
  ├── Large derived datasets
  ├── Experiment artifacts
  ├── Figures
  └── Exported results
```

Redis should not become the authoritative store for research data.

---

# 24. Background Processing

Long-running simulations or experiments should not block ordinary API requests.

A background execution mechanism may be introduced using:

* Celery
* Redis
* another measured and justified task system

The initial implementation should avoid distributed task infrastructure until simulation workloads demonstrate the need.

---

# 25. Caching

Caching may be introduced for expensive, deterministic operations such as:

* repeated spatial queries,
* repeated graph calculations,
* immutable dataset access,
* previously computed simulation results.

Caching must never compromise research reproducibility.

A cached result must be distinguishable from authoritative experiment artifacts.

---

# 26. Configuration Architecture

Configuration should be separated into:

```text id="2y1p3e"
Application Configuration
        │
        ├── Environment
        ├── Database
        ├── External Services
        └── Observability

Research Configuration
        │
        ├── Scenario
        ├── Dataset
        ├── Seeds
        ├── Simulation Parameters
        └── Metrics
```

Research configuration must be versioned or preserved as an experiment artifact.

---

# 27. Error Handling

Errors should be classified according to their origin.

Potential categories:

```text id="sjq3bx"
Validation Error
Domain Rule Error
Data Error
Simulation Error
Infrastructure Error
External Service Error
Configuration Error
```

Errors exposed through the API should use structured responses.

Internal diagnostic information should be available through logs without exposing sensitive details.

---

# 28. Observability Architecture

Observability should span:

```text id="yq6y1f"
HTTP Request
     ↓
Application Use Case
     ↓
Simulation
     ↓
Database / External Services
```

Where appropriate, the system should associate:

* request ID
* simulation ID
* experiment ID
* execution duration
* error information

with relevant operations.

---

# 29. Testing Architecture

Testing should exist at multiple levels.

```text id="x4c5nt"
Unit Tests
    ↓
Domain / Metrics / Graph Rules

Integration Tests
    ↓
Database / API / Infrastructure

Simulation Tests
    ↓
Cascade / Recovery / Intervention Behavior

Experiment Tests
    ↓
Reproducibility / Metric Pipelines

End-to-End Tests
    ↓
Critical Application Workflows
```

The majority of core research logic should be testable without external services.

---

# 30. Security Architecture

Security boundaries include:

```text id="1t3v2x"
Client
  ↓
API Boundary
  ↓
Authentication / Authorization
  ↓
Application
  ↓
Domain
  ↓
Infrastructure
```

Security requirements are defined separately in:

* `docs/SECURITY_MODEL.md`
* `docs/THREAT_MODEL.md`

Security mechanisms should not unnecessarily contaminate domain logic.

---

# 31. Frontend Architecture

The frontend will be implemented after the backend research core is sufficiently stable.

Potential technologies include:

* React
* TypeScript
* MapLibre
* data visualization libraries

Frontend responsibilities include:

* scenario exploration,
* geographic visualization,
* dependency visualization,
* intervention comparison,
* result presentation,
* experiment/result exploration.

The frontend must not become the source of research calculations.

---

# 32. Geographic Visualization

The frontend may visualize:

* urban entities
* hazard areas
* dependency links
* affected regions
* cascade paths
* intervention locations
* simulation states

Visualization should be treated as an interpretation layer over backend results.

---

# 33. API and Frontend Boundary

The frontend should communicate with the backend through documented API contracts.

The frontend should not directly access:

* database credentials,
* internal repositories,
* simulation implementation details,
* private infrastructure services.

---

# 34. Deployment Architecture

The initial deployment target may use a containerized architecture.

Conceptually:

```text id="kz8v6y"
                 Internet
                    │
                    ▼
               Reverse Proxy
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
       Frontend             API
                                │
                 ┌──────────────┼──────────────┐
                 ▼              ▼              ▼
             PostgreSQL       Redis        Worker
                                                │
                                                ▼
                                            Simulation
```

Exact deployment topology will be selected after local functionality and workload requirements are established.

---

# 35. Local Development Architecture

Development should initially minimize infrastructure complexity.

Preferred progression:

```text id="9h7n4x"
Phase 1
FastAPI
PostgreSQL/PostGIS
Local Files
    ↓
Phase 2
Simulation
Research Runner
    ↓
Phase 3
Redis / Background Jobs
    ↓
Phase 4
Observability
    ↓
Phase 5
Containerized Deployment
```

Components should be introduced only when needed.

---

# 36. Technology Selection Principles

Technology decisions shall be based on:

* research requirement
* functional requirement
* measured performance
* maintainability
* reproducibility
* ecosystem maturity
* licensing
* operational complexity

Technology should not be selected solely because it is fashionable or commonly associated with AI systems.

---

# 37. Initial Technology Direction

The current architectural direction is:

| Concern               | Initial Direction                       |
| --------------------- | --------------------------------------- |
| Backend               | Python + FastAPI                        |
| Validation            | Pydantic                                |
| ORM / Database Access | SQLAlchemy                              |
| Migrations            | Alembic                                 |
| Database              | PostgreSQL                              |
| Spatial Database      | PostGIS                                 |
| Graph Processing      | NetworkX                                |
| Simulation            | Python domain engine                    |
| Testing               | Pytest                                  |
| Async Jobs            | Celery + Redis if justified             |
| ML                    | scikit-learn if justified               |
| Frontend              | React + TypeScript                      |
| Mapping               | MapLibre where justified                |
| Containers            | Docker                                  |
| Observability         | OpenTelemetry + Prometheus if justified |

These are architectural directions, not irreversible commitments.

---

# 38. Architecture Evolution

Architecture changes must be driven by evidence.

Examples:

```text id="8b8f2f"
Observed Bottleneck
      ↓
Measurement
      ↓
Architecture Decision
      ↓
Implementation
      ↓
Benchmark
      ↓
Evidence
```

A technology should not be introduced merely because a theoretical future requirement might exist.

---

# 39. Architectural Constraints

The architecture must preserve:

### Constraint 1 — Research Reproducibility

Experiments must be executable independently of the frontend.

### Constraint 2 — Baseline Integrity

The isolated baseline must remain distinct from dependency-aware simulation logic.

### Constraint 3 — Domain Independence

Core domain concepts must not depend directly on framework-specific implementations.

### Constraint 4 — Data Traceability

Research results must be traceable to datasets and configurations.

### Constraint 5 — City Agnosticism

Karachi-specific data must enter through datasets/configuration rather than hard-coded domain rules.

---

# 40. Architecture Decision Criteria

A proposed architectural change should answer:

1. What requirement does it satisfy?
2. What problem does it solve?
3. What evidence supports the need?
4. What complexity does it introduce?
5. How will the benefit be measured?
6. Does it affect reproducibility?
7. Does it affect research validity?
8. Can it be removed later if unnecessary?

Decisions should be recorded in `docs/DECISION_LOG.md`.

---

# 41. Architecture and Research Validity

Software architecture can affect research validity.

Examples:

* nondeterministic execution may affect repeated experiments;
* hidden caching may affect measured runtime;
* inconsistent data loading may invalidate comparisons;
* changing dependency rules may alter hypotheses;
* untracked preprocessing may make results irreproducible.

Therefore architecture decisions affecting experiment execution must be treated as research methodology decisions where appropriate.

---

# 42. Initial Architecture Acceptance Criteria

The architecture will be considered ready for implementation when:

1. domain responsibilities are clearly separated;
2. API responsibilities are separated from simulation logic;
3. research experiments can run independently of the frontend;
4. baseline and proposed methods remain distinguishable;
5. data provenance can be preserved;
6. simulation state and outcomes can be represented;
7. core logic can be unit tested;
8. infrastructure can be replaced or changed without rewriting domain rules;
9. experiment configurations can be reproduced;
10. major architectural decisions can be documented.

---

# 43. Guiding Architecture

The intended architectural philosophy can be summarized as:

```text id="r7u8w4"
                 RESEARCH
                    │
                    ▼
              DOMAIN MODEL
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
      SIMULATION            BASELINE
          │                   │
          └─────────┬─────────┘
                    ▼
                 METRICS
                    │
                    ▼
                EVIDENCE
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
        API                EXPERIMENTS
          │                   │
          ▼                   ▼
      FRONTEND            RESEARCH PAPER
```

The system exists to produce trustworthy, measurable resilience analysis.

The UI, API, infrastructure, and deployment environment exist to support that objective rather than replace it.
