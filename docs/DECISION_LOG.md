# RESOLVE — Architecture & Engineering Decision Log

## 1. Purpose

This document records significant architecture, engineering, data, research-system, and infrastructure decisions made during the development of RESOLVE — Resilient Urban Systems & Vulnerability Observatory.

The purpose is to preserve:

* why a decision was made,
* what alternatives were considered,
* what assumptions were involved,
* what consequences were accepted,
* and whether the decision remains valid.

This document is intended to prevent architectural decisions from becoming dependent on memory or undocumented assumptions.

---

# 2. Decision Principles

RESOLVE decisions follow these principles:

1. Research requirements drive architecture.
2. Domain concepts are defined before framework-specific implementation.
3. Complexity must have a demonstrated purpose.
4. Reproducibility is a first-class requirement.
5. Measurement precedes optimization.
6. Baselines must remain methodologically credible.
7. Data provenance must be preserved.
8. Security and research integrity must be considered together.
9. Decisions should be reversible where practical.
10. Material decisions must remain traceable.

---

# 3. Decision Status

Each decision may have one of the following statuses:

| Status     | Meaning                                                   |
| ---------- | --------------------------------------------------------- |
| Proposed   | Under consideration and not yet accepted                  |
| Accepted   | Current project decision                                  |
| Superseded | Replaced by a later decision                              |
| Rejected   | Considered and explicitly rejected                        |
| Deprecated | No longer recommended but retained for historical context |
| Revisit    | Accepted temporarily and scheduled for reassessment       |

---

# 4. Decision Record Format

Each significant decision should contain:

```text
Decision ID
Title
Date
Status
Context
Problem
Decision
Alternatives Considered
Rationale
Consequences
Research Impact
Engineering Impact
Revisit Conditions
```

Not every minor implementation choice requires a decision record.

---

# 5. ADR-001 — Research Question as Primary Architectural Driver

**Status:** Accepted

## Context

RESOLVE is both a software system and a research implementation.

A conventional application-development workflow could encourage feature development independently from the research question.

That would create a risk of building technically sophisticated features that do not contribute to the intended evaluation.

## Problem

The system requires an architecture that directly supports the research question:

> Can a dependency-aware urban digital twin improve the prioritization of resilience interventions compared with isolated risk assessment?

## Decision

The research question and hypotheses will guide major architectural decisions.

The primary development chain is:

```text
Research Question
→ Hypothesis
→ Requirement
→ Architecture
→ Implementation
→ Measurement
→ Experiment
→ Evidence
→ Paper
```

## Alternatives Considered

### Feature-first development

Rejected because it could produce functionality without research relevance.

### Technology-first development

Rejected because framework selection should follow system requirements.

### UI-first development

Rejected because the analytical and simulation core is the primary research contribution.

## Rationale

The architecture must make the research question executable and measurable.

## Consequences

Positive:

* stronger research alignment,
* reduced feature drift,
* clearer implementation priorities.

Negative:

* some visually attractive features may be postponed,
* engineering decisions require additional research consideration.

## Research Impact

High.

This decision defines the relationship between the research methodology and software architecture.

## Engineering Impact

High.

Architecture and implementation priorities are explicitly tied to research requirements.

## Revisit Conditions

Revisit if the research question materially changes.

---

# 6. ADR-002 — Backend-First Development

**Status:** Accepted

## Context

RESOLVE requires:

* urban system modeling,
* dependency representation,
* simulation,
* intervention evaluation,
* research experiments,
* reproducibility,
* data processing.

The frontend is primarily an interface to these capabilities.

## Problem

Developing frontend functionality before establishing the analytical core could cause the user interface to dictate domain design.

## Decision

RESOLVE will be developed backend-first.

The initial implementation order is:

```text
Domain
→ Persistence
→ Baseline
→ Graph
→ Simulation
→ Intervention Evaluation
→ Research Engine
→ API
→ Frontend
```

## Alternatives Considered

### Frontend-first

Rejected because it prioritizes presentation before research functionality.

### Full-stack feature slices from the beginning

Not selected for initial development because the simulation and research core require foundational domain work first.

## Rationale

The research system must remain usable independently of the frontend.

## Consequences

Positive:

* domain logic remains reusable,
* experiments can execute without the UI,
* easier automated testing,
* research scripts can directly use core capabilities.

Negative:

* visible UI progress may initially be slower.

## Research Impact

Positive.

The analytical system becomes the primary artifact rather than the visualization.

## Engineering Impact

Positive.

The architecture encourages separation of concerns.

## Revisit Conditions

Revisit if user studies or interaction requirements demonstrate that earlier frontend integration materially improves research workflow.

---

# 7. ADR-003 — Separate API Layer from Research Core

**Status:** Accepted

## Context

FastAPI is intended for REST API delivery.

However, simulation and research logic must also be callable by experiments and tests.

## Problem

Embedding domain and simulation logic inside HTTP routes would tightly couple research execution to the API.

## Decision

FastAPI will remain an interface layer.

Core logic will reside in domain, application, simulation, graph, risk, and research components.

Conceptually:

```text
REST API
   ↓
Application Services
   ↓
Domain / Simulation / Research
   ↓
Infrastructure
```

## Alternatives Considered

### Route handlers containing business logic

Rejected because it reduces reuse and testability.

### API-only execution model

Rejected because experiments should not require HTTP requests to execute core research logic.

## Consequences

Positive:

* testability,
* API independence,
* reusable simulation execution,
* cleaner architecture.

Negative:

* more explicit layers,
* additional design discipline.

## Revisit Conditions

Revisit only if the architecture demonstrates unnecessary complexity without corresponding value.

---

# 8. ADR-004 — PostgreSQL/PostGIS for Canonical Spatial Data

**Status:** Accepted

## Context

RESOLVE may represent:

* geographic areas,
* infrastructure,
* facilities,
* roads,
* population-related spatial data,
* hazards,
* spatial dependencies.

Spatial relationships are important to the research problem.

## Problem

A storage architecture is required that supports structured relational data and geospatial operations.

## Decision

PostgreSQL with PostGIS is the primary target for canonical structured and spatial data.

## Alternatives Considered

### SQLite

Useful for simple prototypes but insufficient as the intended canonical spatial persistence layer.

### Flat files only

Rejected as the primary store because relational integrity and spatial querying are important.

### NoSQL database

Not selected because the domain has strong relational relationships and transactional requirements.

## Rationale

PostgreSQL provides mature relational capabilities while PostGIS provides spatial functionality required by the domain.

## Consequences

Positive:

* strong relational integrity,
* spatial querying,
* mature ecosystem,
* suitable migration tooling.

Negative:

* more setup complexity than a file-based prototype.

## Research Impact

Positive.

Explicit spatial relationships can be represented and evaluated reproducibly.

## Revisit Conditions

Revisit if measured workload or research requirements demonstrate a different storage architecture is more appropriate.

---

# 9. ADR-005 — SQLAlchemy and Alembic for Persistence

**Status:** Accepted

## Context

The backend requires structured persistence and controlled schema evolution.

## Decision

SQLAlchemy will provide the primary Python database abstraction and Alembic will manage schema migrations.

## Alternatives Considered

### Raw SQL only

Rejected as the sole persistence approach because domain mapping and migration management would become harder to maintain.

### Automatic schema creation

Rejected for research reproducibility and controlled schema evolution.

## Rationale

Explicit migrations provide a versioned database evolution history.

## Consequences

Positive:

* migration history,
* explicit schema changes,
* testable persistence layer.

Negative:

* migration management adds development work.

## Revisit Conditions

Revisit if persistence requirements materially change.

---

# 10. ADR-006 — NetworkX for Initial Dependency Graph Processing

**Status:** Accepted

## Context

Dependency-aware modeling is central to RESOLVE.

The initial research system requires graph operations such as:

* traversal,
* dependency analysis,
* connected-component analysis,
* cascade propagation.

## Decision

NetworkX will be used initially for graph representation and analysis where appropriate.

## Alternatives Considered

### Custom graph implementation

Rejected initially because it would introduce unnecessary implementation risk.

### Graph database

Not selected initially because the research workload has not yet demonstrated a need for a dedicated graph database.

### Specialized distributed graph framework

Rejected as premature complexity.

## Rationale

NetworkX provides mature graph primitives suitable for an initial research prototype.

## Consequences

Positive:

* rapid implementation,
* established algorithms,
* strong Python integration.

Negative:

* memory limitations for very large graphs,
* potentially limited scalability for large production workloads.

## Research Impact

Allows dependency-aware experiments to be implemented without prematurely committing to specialized infrastructure.

## Revisit Conditions

Revisit if graph size, memory usage, or benchmark results demonstrate that NetworkX materially limits required experiments.

---

# 11. ADR-007 — Isolated Risk Assessment as Primary Baseline

**Status:** Accepted

## Context

The research question compares dependency-aware intervention prioritization with isolated risk assessment.

## Decision

The primary baseline will be an isolated risk assessment that evaluates components without explicit dependency propagation.

The baseline will use equivalent relevant inputs and outcome metrics where methodologically appropriate.

## Alternatives Considered

### Direct hazard exposure only

Retained as an optional lower-complexity baseline.

### Component risk scoring only

May be included as an additional baseline.

### No baseline

Rejected because the research question requires comparative evaluation.

## Rationale

A credible baseline is necessary to determine whether dependency modeling provides measurable benefit.

## Consequences

Positive:

* direct experimental comparison,
* clearer causal interpretation of dependency modeling.

Negative:

* baseline implementation requires methodological care.

## Research Impact

Very high.

The baseline is central to hypothesis testing.

## Revisit Conditions

Revisit if literature review or experimental design establishes a more appropriate primary comparison method.

---

# 12. ADR-008 — Dependency-Aware Simulation as Proposed Method

**Status:** Accepted

## Context

The central research hypothesis concerns whether explicit dependencies improve resilience intervention prioritization.

## Decision

The proposed RESOLVE method will represent dependencies explicitly and simulate cascading effects under defined scenarios.

Interventions will be evaluated using common outcome metrics.

## Alternatives Considered

### Static risk scoring

Insufficient to represent dynamic cascading behavior.

### Independent component simulation

Does not capture cross-system dependency propagation.

### Full city-scale real-time digital twin

Rejected as disproportionate to the research scope.

## Rationale

The proposed method must directly operationalize the research question without claiming to model every aspect of a real city.

## Consequences

Positive:

* enables cascade-aware analysis,
* supports intervention comparison.

Negative:

* requires explicit assumptions about dependencies and state transitions,
* increases computational complexity.

## Research Impact

Very high.

This is the primary methodological component under evaluation.

## Revisit Conditions

Revisit if experiments demonstrate that the selected simulation abstraction cannot meaningfully distinguish dependency-aware behavior.

---

# 13. ADR-009 — Simulation as a Deterministic Core Where Possible

**Status:** Accepted

## Context

Research reproducibility requires repeated execution under equivalent conditions to be comparable.

## Decision

Simulation behavior should be deterministic where the research design permits.

If stochastic behavior is intentionally introduced, random seeds and relevant configuration must be recorded.

## Alternatives Considered

### Uncontrolled randomness

Rejected because it weakens reproducibility.

### Fully deterministic modeling

Preferred where scientifically appropriate, but not required when stochastic processes are meaningful.

## Consequences

Positive:

* easier debugging,
* reproducible experiments,
* stronger regression testing.

Negative:

* may require explicit handling of random-state management.

## Revisit Conditions

Revisit if a research experiment requires stochastic modeling.

---

# 14. ADR-010 — Separate Observed, Derived, Modeled, and Simulated Data

**Status:** Accepted

## Context

RESOLVE combines external datasets with transformations and simulation outputs.

## Problem

Without explicit distinctions, simulated outputs could be incorrectly interpreted as observations.

## Decision

Data will be classified as:

```text
Observed
Derived
Modeled
Simulated
Experimental
```

Provenance should be preserved across transformations.

## Rationale

This distinction is necessary for research integrity.

## Consequences

Positive:

* clearer scientific interpretation,
* stronger provenance,
* easier reproducibility.

Negative:

* additional metadata requirements.

## Revisit Conditions

Revisit if the data architecture evolves substantially.

---

# 15. ADR-011 — Research Evidence as a First-Class Artifact

**Status:** Accepted

## Context

Research claims require evidence that can be inspected and reproduced.

## Decision

Experiment evidence will be stored separately from ordinary application logs.

Evidence should include sufficient metadata to reconstruct the computational conditions of the experiment.

## Alternatives Considered

### Rely only on database results

Rejected because execution context and experiment artifacts may not be fully represented.

### Rely only on logs

Rejected because logs are not an appropriate primary research-result format.

## Consequences

Positive:

* stronger reproducibility,
* easier paper preparation,
* auditable experiment history.

Negative:

* additional artifact-management work.

## Research Impact

Very high.

## Revisit Conditions

Revisit if a more robust research-artifact system is introduced.

---

# 16. ADR-012 — IEEE Conference LaTeX Template as Immutable Reference

**Status:** Accepted

## Context

The project intends to prepare an IEEE-format research paper.

The official IEEE conference LaTeX template has been downloaded into:

```text
paper/ieee-template/
```

## Decision

The official template will remain unchanged.

The working manuscript will be developed separately under:

```text
paper/manuscript/
```

The official template files will serve as the formatting reference.

## Alternatives Considered

### Modify the downloaded official template directly

Rejected because it makes it harder to distinguish reference material from project manuscript content.

### Use a custom LaTeX class

Rejected because IEEE formatting should follow the official template.

## Consequences

Positive:

* preserves official reference,
* reduces accidental template corruption,
* separates reference material from manuscript development.

Negative:

* template updates must be manually reflected in the working manuscript when necessary.

## Research Impact

Supports consistent paper preparation.

## Revisit Conditions

Revisit if a target venue provides a different official template.

---

# 17. ADR-013 — Paper Developed Alongside Implementation

**Status:** Accepted

## Context

Writing the paper only after implementation could create gaps between actual evidence and documented methodology.

## Decision

The paper will be developed incrementally alongside implementation.

The intended workflow is:

```text
Implementation
→ Measurement
→ Evidence
→ Research Documentation
→ Paper
```

## Consequences

Positive:

* fewer documentation gaps,
* easier traceability,
* claims remain connected to implementation.

Negative:

* requires discipline to update research writing throughout development.

## Revisit Conditions

Revisit if the target publication format or research workflow changes.

---

# 18. ADR-014 — No Machine Learning Without a Genuine Research Task

**Status:** Accepted

## Context

Machine learning is technically attractive but is not automatically necessary for the research question.

## Decision

Machine learning will be introduced only if a clearly defined predictive or analytical task requires it.

If introduced, the task must have:

* target variable,
* dataset,
* training procedure,
* validation strategy,
* baseline,
* evaluation metrics,
* reproducibility requirements.

## Alternatives Considered

### Add ML because the project is an AI project

Rejected.

### Add an LLM component for demonstration

Rejected unless a justified research or engineering use case emerges.

## Rationale

The project should not use AI as decoration.

## Consequences

Positive:

* reduces unnecessary complexity,
* protects research focus,
* avoids unsupported AI claims.

Negative:

* fewer immediately marketable AI features.

## Revisit Conditions

Revisit if literature review or experimental requirements identify a genuine ML task.

---

# 19. ADR-015 — Optional Background Processing

**Status:** Accepted

## Context

Simulation and experiment workloads may eventually become too expensive for synchronous API execution.

## Decision

Background processing using Celery and Redis may be introduced when measurements demonstrate a need.

It is not a mandatory part of the earliest backend implementation.

## Alternatives Considered

### Always run simulations synchronously

Simple but potentially unsuitable for long-running workloads.

### Introduce Celery immediately

Rejected as premature infrastructure.

### Custom task system

Not preferred unless requirements demonstrate a need.

## Rationale

The project should first establish correct synchronous execution and measure actual workloads.

## Consequences

Positive:

* controlled complexity,
* evidence-driven infrastructure decisions.

Negative:

* background execution may require later architectural work.

## Revisit Conditions

Introduce when:

* simulations exceed practical API timeouts,
* experiment workloads become long-running,
* parallel execution becomes necessary,
* queue management provides measurable benefit.

---

# 20. ADR-016 — Observability Introduced Incrementally

**Status:** Accepted

## Context

Observability is necessary for debugging, performance measurement, and research reproducibility.

However, a complete production monitoring stack would add unnecessary complexity during early development.

## Decision

Observability will evolve incrementally:

```text
Structured Logs
→ Execution Metadata
→ Performance Measurements
→ Metrics
→ Tracing
→ Centralized Monitoring
```

## Rationale

The level of observability should correspond to actual system complexity.

## Consequences

Positive:

* lower early complexity,
* easier development,
* evidence-driven tooling decisions.

Negative:

* advanced monitoring may need to be introduced later.

## Revisit Conditions

Revisit when distributed execution or deployment complexity increases.

---

# 21. ADR-017 — Security and Research Integrity Are Joint Concerns

**Status:** Accepted

## Context

A research system can produce technically valid-looking results that are compromised by:

* dataset tampering,
* scenario modification,
* result overwriting,
* provenance loss,
* unauthorized changes.

## Decision

Security controls will protect not only system availability and confidentiality but also research integrity.

## Consequences

Security considerations will include:

* dataset integrity,
* experiment configuration,
* result provenance,
* access control,
* auditability,
* secret handling.

## Research Impact

High.

Research conclusions depend on trustworthy computational artifacts.

## Revisit Conditions

Revisit when deployment exposure or research collaboration changes.

---

# 22. ADR-018 — City-Agnostic Domain Model

**Status:** Accepted

## Context

Karachi may serve as a case study, but the research question concerns urban resilience more broadly.

## Decision

The core domain model will remain city-agnostic.

Karachi-specific assumptions, datasets, and scenarios will be represented as case-study configuration or data rather than embedded into core domain logic wherever practical.

## Alternatives Considered

### Karachi-specific implementation

Rejected because it would unnecessarily restrict the research architecture.

### Generic architecture without any concrete case study

Rejected because an actual case study is required to make the research evaluable.

## Consequences

Positive:

* better generalization potential,
* cleaner domain model,
* easier future case studies.

Negative:

* additional abstraction may be required.

## Research Impact

Important.

The architecture can support a concrete case study without claiming universal city applicability.

## Revisit Conditions

Revisit if the research scope is explicitly narrowed to a single-city system.

---

# 23. ADR-019 — Synthetic Data Permitted for Development

**Status:** Accepted

## Context

Real urban datasets may be difficult to acquire, license, clean, or integrate during early implementation.

## Decision

Synthetic data may be used for:

* unit tests,
* integration tests,
* development,
* deterministic simulation testing,
* architecture validation.

Synthetic data must be explicitly labeled and must not be presented as real-world evidence.

## Consequences

Positive:

* faster development,
* deterministic test scenarios,
* reduced privacy and licensing concerns.

Negative:

* synthetic data may not represent real urban complexity.

## Research Impact

Synthetic data may support engineering validation but should not automatically support real-world research claims.

## Revisit Conditions

Revisit data strategy when primary empirical evaluation begins.

---

# 24. ADR-020 — Primary Metric Defined Before Main Evaluation

**Status:** Accepted

## Context

Selecting metrics after observing results can introduce evaluation bias.

## Decision

The primary evaluation metric will be defined before the main experiment.

The current candidate is cascading impact reduction:

```text
(Baseline Impact - Intervention Impact)
-------------------------------------- × 100
           Baseline Impact
```

The final definition of aggregate impact must be frozen before primary evaluation.

## Consequences

Positive:

* clearer evaluation methodology,
* reduced metric-selection bias.

Negative:

* requires committing to measurement definitions before all implementation details are known.

## Revisit Conditions

The metric may be revised during methodology development if justified and documented before the primary evaluation is finalized.

---

# 25. ADR-021 — Baseline and Proposed Method Must Share Evaluation Conditions

**Status:** Accepted

## Context

Comparisons can become invalid if methods receive different data, scenarios, budgets, or outcome definitions.

## Decision

Where methodologically appropriate, baseline and proposed methods will use equivalent:

* datasets,
* scenarios,
* intervention sets,
* budgets,
* evaluation metrics,
* execution conditions.

## Rationale

Differences in outcomes should be attributable as much as practical to the methodological difference under investigation.

## Consequences

Positive:

* stronger experimental validity.

Negative:

* requires careful experiment design.

## Revisit Conditions

Revisit if a legitimate methodological reason requires different inputs.

Any such difference must be documented.

---

# 26. ADR-022 — Complexity Must Be Evidence-Driven

**Status:** Accepted

## Context

RESOLVE could potentially incorporate:

* Redis,
* Celery,
* graph databases,
* machine learning,
* distributed execution,
* advanced observability,
* real-time streaming,
* complex frontend rendering.

Adding all of these technologies would increase implementation complexity.

## Decision

A technology will be introduced only when at least one of the following is demonstrated:

1. A research requirement requires it.
2. An engineering requirement requires it.
3. A measured bottleneck requires it.
4. A reproducibility requirement requires it.
5. A security requirement requires it.

## Consequences

The initial architecture remains intentionally conservative.

Advanced infrastructure may be introduced later based on evidence.

---

# 27. ADR-023 — API Versioning from the Beginning

**Status:** Accepted

## Context

The system may evolve while experiments and frontend clients depend on API behavior.

## Decision

The API will use explicit versioning beginning with:

```text
/api/v1
```

## Rationale

Explicit versioning provides a stable contract boundary between API consumers and evolving implementation.

## Consequences

Positive:

* clearer API evolution,
* easier compatibility management.

Negative:

* future breaking changes may require additional versions.

## Revisit Conditions

Revisit if the project remains entirely internal and versioning demonstrates unnecessary overhead.

---

# 28. ADR-024 — Frontend Is a Research Interface, Not the Research Core

**Status:** Accepted

## Context

A map-based interface can make RESOLVE easier to understand and demonstrate.

However, the research contribution is the dependency-aware analytical system.

## Decision

The frontend will visualize and interact with capabilities provided by the backend and research core.

It will not contain the authoritative implementation of:

* cascade rules,
* intervention scoring,
* research metrics,
* baseline methodology.

## Consequences

Positive:

* consistent results across API, experiments, and UI,
* easier testing,
* lower risk of duplicated logic.

Negative:

* frontend development depends on stable backend contracts.

---

# 29. ADR-025 — No Real-Time Emergency Dispatch Scope

**Status:** Accepted

## Context

Urban resilience software could potentially evolve into operational emergency management.

That would introduce significant safety, reliability, regulatory, and operational requirements.

## Decision

RESOLVE will not initially provide:

* real-time emergency dispatch,
* autonomous emergency decisions,
* direct physical infrastructure control,
* guaranteed disaster prediction.

The project remains a research and analytical system.

## Rationale

The research question does not require operational emergency-control functionality.

## Consequences

The scope remains manageable and reduces safety risk.

## Revisit Conditions

Only revisit under a fundamentally different project scope and appropriate validation requirements.

---

# 30. ADR-026 — Research Claims Must Be Evidence-Bounded

**Status:** Accepted

## Context

A research prototype can easily make claims broader than its experiments support.

## Decision

Claims in:

* README,
* documentation,
* research files,
* presentations,
* demonstrations,
* and the IEEE paper

must not exceed the evidence available.

Claims must distinguish between:

* implemented capability,
* measured result,
* hypothesis,
* interpretation,
* limitation,
* future work.

## Rationale

Research credibility depends on disciplined claims.

## Consequences

The project may appear less ambitious in promotional material, but the resulting claims will be more defensible.

---

# 31. ADR-027 — Architecture Must Remain Experiment-Friendly

**Status:** Accepted

## Context

Research experiments may require configurations that differ from normal API usage.

If the architecture only supports production-style API requests, experimentation becomes unnecessarily difficult.

## Decision

Core domain, simulation, and research components must remain callable independently of HTTP.

Experiment execution should be able to reuse the same authoritative implementation as the API.

## Consequences

Positive:

* less duplication,
* stronger consistency,
* easier reproducibility.

Negative:

* application boundaries must be designed carefully.

---

# 32. ADR-028 — Documentation Is Part of the Implementation

**Status:** Accepted

## Context

RESOLVE contains substantial research methodology and architectural assumptions.

Code alone cannot fully communicate those decisions.

## Decision

Documentation is treated as a project artifact and part of the implementation lifecycle.

Material changes must update relevant documentation.

## Consequences

Positive:

* improved maintainability,
* stronger research traceability,
* easier paper preparation.

Negative:

* additional maintenance effort.

---

# 33. ADR-029 — Research Prototype Before Production System

**Status:** Accepted

## Context

The initial objective is to answer a research question and establish measurable evidence.

Building a production-grade city-scale platform before validating the research hypothesis would consume resources without necessarily improving scientific validity.

## Decision

Development will prioritize a research-capable prototype with strong engineering foundations before production-scale deployment capabilities.

## Rejected Alternative

### Production-first platform

Rejected because operational scale is not necessary to answer the initial research question.

## Consequences

The system should be:

* modular,
* testable,
* reproducible,
* measurable,
* demonstrable,

without pretending to be a production emergency-management platform.

---

# 34. Decision Review Rules

A decision should be revisited when:

* the research question changes,
* a major requirement changes,
* empirical measurements contradict an assumption,
* scalability limits are demonstrated,
* security requirements change,
* new evidence materially changes the trade-off,
* a selected technology becomes unsuitable,
* a target publication imposes new methodological requirements.

A decision should not be reversed merely because another technology is newer or more popular.

---

# 35. Decision Numbering

Decision IDs use the format:

```text
ADR-XXX
```

where `XXX` is a sequential number.

Once assigned, an ID should not be reused.

Superseded decisions remain in this document for historical traceability.

---

# 36. Decision Quality Criteria

A strong architectural decision should answer:

1. What problem were we solving?
2. What alternatives existed?
3. Why was this option selected?
4. What trade-offs were accepted?
5. What research impact does it have?
6. What engineering impact does it have?
7. Under what conditions should we revisit it?

If these questions cannot be answered, the decision may not be sufficiently documented.

---

# 37. Current Decision Summary

The current architectural direction can be summarized as:

```text
Research-driven architecture
        ↓
Backend-first
        ↓
Domain-centered core
        ↓
API as interface
        ↓
PostgreSQL/PostGIS persistence
        ↓
Dependency graph
        ↓
Dependency-aware simulation
        ↓
Intervention evaluation
        ↓
Reproducible experiments
        ↓
Evidence
        ↓
IEEE research paper
```

Optional technologies remain conditional rather than mandatory.

---

# 38. Future Decision Records

Future decisions may cover:

* exact database schema,
* spatial indexing strategy,
* dependency-strength representation,
* cascade propagation algorithm,
* recovery model,
* intervention scoring methodology,
* experiment execution architecture,
* dataset selection,
* scenario generation,
* graph storage optimization,
* background processing,
* caching,
* observability tooling,
* deployment architecture,
* frontend mapping architecture,
* statistical analysis methodology.

These should be added only when the corresponding design problem actually arises.

---

# 39. Guiding Principle

The decision log exists to preserve one simple rule:

> **We should be able to explain not only what RESOLVE does, but why it was designed that way, what alternatives were rejected, and what evidence would cause us to change our minds.**
