# RESOLVE — Resilient Urban Systems & Vulnerability Observatory

> A research-oriented platform for modeling cascading urban risks, evaluating infrastructure dependencies, and comparing resilience interventions under constrained real-world data.

## Status

**Research & Engineering — Active Development**

RESOLVE is an experimental research platform designed to investigate how interconnected urban infrastructure systems respond to cascading disruptions.

The project combines urban data, dependency-aware graph modeling, risk analysis, simulation, and intervention evaluation into a reproducible software and research workflow.

The initial implementation will use a real-world urban case study, with Karachi considered as a primary candidate, while keeping the architecture general enough to support other cities and datasets.

---

## Research Motivation

Urban disruptions rarely remain isolated.

A disruption affecting one system can propagate through dependencies between infrastructure, services, and communities.

For example:

```text
Extreme Heat
     ↓
Electricity Demand
     ↓
Grid Stress
     ↓
Power Outage
     ↓
Water Pump Failure
     ↓
Reduced Water Availability
     ↓
Impact on Critical Facilities
     ↓
Emergency-Service Pressure
```

Traditional risk assessments may evaluate individual hazards or infrastructure systems separately.

RESOLVE investigates whether explicitly modeling these dependencies can provide better information for prioritizing resilience interventions.

---

## Research Question

The central research question is:

> **Can a dependency-aware urban digital twin improve the prioritization of resilience interventions compared with isolated risk assessment?**

The project will investigate this question through:

* dependency-aware urban system modeling
* cascading-failure simulation
* risk and vulnerability analysis
* intervention comparison
* quantitative evaluation
* reproducible experiments
* software performance measurement

The project does **not** assume that the proposed approach is superior. Its purpose is to experimentally evaluate that claim.

---

## Objectives

RESOLVE aims to:

1. Model urban infrastructure and services as interconnected systems.
2. Represent dependencies between infrastructure components.
3. Simulate cascading disruptions across those dependencies.
4. Quantify risk and system vulnerability.
5. Compare resilience interventions under constrained resources.
6. Measure the effect of interventions using reproducible metrics.
7. Provide a backend architecture that can support both research experiments and application APIs.
8. Produce evidence suitable for an academic research paper.
9. Maintain reproducible datasets, experiments, configurations, and results.
10. Demonstrate how research-oriented engineering can be implemented as a maintainable software system.

---

## Core Research Areas

RESOLVE sits at the intersection of:

* Urban resilience
* Critical infrastructure modeling
* Cascading-risk analysis
* Graph-based systems modeling
* Urban digital twins
* Geospatial analysis
* Risk assessment
* Simulation
* Resilience intervention optimization
* Machine learning where empirically justified
* Distributed backend systems
* Reproducible computational research

---

## Conceptual Model

The platform represents an urban environment through interacting entities and dependencies.

A simplified model is:

```text
                    ┌───────────────┐
                    │    Hazards    │
                    └───────┬───────┘
                            │
                            ▼
                 ┌────────────────────┐
                 │ Urban System State │
                 └─────────┬──────────┘
                           │
                           ▼
              ┌──────────────────────────┐
              │ Dependency Graph         │
              │                          │
              │ Infrastructure           │
              │ Services                 │
              │ Facilities               │
              │ Population               │
              └────────────┬─────────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ Cascade Simulation  │
                └──────────┬──────────┘
                           │
                           ▼
                 ┌────────────────────┐
                 │ Risk / Impact      │
                 │ Assessment         │
                 └─────────┬──────────┘
                           │
                           ▼
               ┌────────────────────────┐
               │ Intervention Evaluation│
               └────────────┬───────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Resilience Metrics  │
                 └─────────────────────┘
```

This conceptual model will evolve as the research and implementation mature.

---

## What RESOLVE Is Not

RESOLVE is intentionally not positioned as:

* a generic AI chatbot
* an LLM wrapper
* a dashboard-only application
* a simple CRUD system
* a conventional weather application
* a generic GIS viewer
* a prediction model without a systems model
* a claim that urban disasters can be perfectly predicted
* a replacement for professional emergency-management decisions

The frontend will visualize and interact with the research system, but the research contribution is centered on the underlying modeling, simulation, evaluation, and evidence.

---

## System Architecture Direction

The platform is being developed with a backend-first architecture.

The backend will contain the primary domain and research capabilities:

```text
                 ┌───────────────────────┐
                 │       Frontend        │
                 │ React / TypeScript    │
                 └───────────┬───────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │    FastAPI      │
                    │   API Layer     │
                    └────────┬────────┘
                             │
                             ▼
                ┌─────────────────────────┐
                │ Application / Domain    │
                │ Logic                   │
                └────────────┬────────────┘
                             │
          ┌──────────────────┼──────────────────┐
          ▼                  ▼                  ▼
   ┌────────────┐    ┌──────────────┐   ┌──────────────┐
   │ Risk Engine│    │ Graph Engine  │   │ Simulation   │
   └────────────┘    └──────────────┘   │ Engine       │
                                        └──────────────┘
          │                  │                  │
          └──────────────────┼──────────────────┘
                             ▼
                    ┌─────────────────┐
                    │ Data / Storage  │
                    │ PostgreSQL      │
                    │ PostGIS         │
                    │ Redis           │
                    └─────────────────┘
```

The architecture will be refined through documented engineering decisions and experimental requirements.

---

## Technology Direction

The technology stack will be introduced incrementally rather than installing every possible dependency at the beginning.

### Backend

Potential core technologies include:

* Python
* FastAPI
* Pydantic
* SQLAlchemy
* Alembic
* PostgreSQL
* PostGIS
* Redis
* Celery

### Research & Simulation

Potential research technologies include:

* NetworkX
* NumPy
* pandas
* scikit-learn
* geospatial Python libraries
* statistical and optimization tooling where justified

### Frontend

The planned frontend direction includes:

* React
* TypeScript
* MapLibre
* data visualization technologies
* WebGL where justified by visualization requirements

### Engineering & Infrastructure

The project may use:

* Docker
* Pytest
* GitHub Actions
* OpenTelemetry
* Prometheus
* structured logging
* automated testing and reproducibility tooling

Specific technologies will only become project dependencies when they provide a demonstrated engineering or research benefit.

---

## Data Sources

RESOLVE is intended to operate using real-world datasets where legally and technically appropriate.

Potential data categories include:

* weather and climate observations
* satellite and remote-sensing data
* population information
* road networks
* buildings
* hospitals
* schools
* water infrastructure
* electricity infrastructure where available
* emergency facilities
* historical incidents
* geographic boundaries
* other publicly available urban datasets

Because real-world urban data is incomplete and heterogeneous, uncertainty, missing data, assumptions, and dataset limitations will be explicitly documented.

---

## Research Evaluation

The system will be evaluated using measurable criteria rather than visual demonstrations alone.

Potential evaluation categories include:

### Predictive Performance

Where predictive models are used:

* MAE
* RMSE
* precision
* recall
* F1 score
* calibration

### Simulation Performance

Potential measures include:

* cascade detection accuracy
* propagation accuracy
* simulation execution time
* scenario throughput
* graph-processing performance

### Intervention Effectiveness

Potential measures include:

* population protected
* critical facilities protected
* expected downtime reduction
* response-time reduction
* resource utilization
* affected-area reduction

### Software Performance

Potential engineering measurements include:

* API latency
* throughput
* database query performance
* simulation execution time
* memory consumption
* scalability
* automated test coverage

Final metrics will be defined in the research documentation before the corresponding experiments are conducted.

---

## Research Baselines

The evaluation will compare the proposed dependency-aware approach against appropriate baselines.

Possible baselines include:

* isolated risk assessment
* hazard-only prioritization
* infrastructure-only prioritization
* non-dependency-aware scoring
* simpler graph models
* alternative intervention-ranking strategies

The final baselines will be documented before experimentation to reduce the risk of selecting comparisons after seeing the results.

---

## Reproducibility

Research reproducibility is a core project requirement.

Experiments should record, where applicable:

* dataset versions
* preprocessing steps
* configuration
* model parameters
* simulation parameters
* random seeds
* software versions
* environment information
* experiment identifiers
* generated metrics
* result artifacts

The goal is to make significant experiments repeatable by another researcher using the documented project environment.

---

## Repository Structure

```text
RESOLVE/
│
├── backend/             # Backend application and domain services
├── frontend/            # Frontend application
├── simulation/          # Simulation and scenario logic
├── data/                # Dataset-related resources
├── ml/                  # Machine-learning components
├── infrastructure/     # Infrastructure and deployment resources
├── tests/               # Cross-component tests
├── scripts/             # Development and research utilities
│
├── docs/                # Engineering documentation
├── research/            # Research methodology and experiments
├── paper/               # IEEE manuscript and paper assets
├── evidence/            # Sprint and experiment evidence
│
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── SECURITY.md
├── .gitignore
└── .env.example
```

---

## Development Philosophy

RESOLVE follows a research-engineering workflow:

```text
Research Question
        ↓
Hypothesis
        ↓
Requirements
        ↓
Architecture
        ↓
Implementation
        ↓
Testing
        ↓
Experiment
        ↓
Measurement
        ↓
Evidence
        ↓
Research Interpretation
        ↓
Paper
```

Implementation decisions should be traceable to either:

* a research requirement,
* an engineering requirement,
* an experimental requirement,
* or a documented decision.

Features will not be added merely because they appear technically interesting.

---

## Current Research Status

The project is currently in the **research and architecture foundation phase**.

Completed:

* repository structure
* initial documentation structure
* research-oriented project direction
* IEEE conference manuscript workspace
* official IEEE template preservation
* working manuscript structure

In progress:

* research question formalization
* hypotheses
* problem definition
* system requirements
* architecture
* research baselines
* evaluation metrics
* dataset strategy

Planned:

* backend foundation
* data ingestion
* urban domain model
* dependency graph
* risk engine
* cascade simulation
* intervention evaluation
* experiments
* frontend visualization
* reproducibility pipeline
* IEEE manuscript development

---

## Academic Scope

RESOLVE is being developed with the intention of producing a research paper suitable for submission to an appropriate IEEE venue.

The project does not claim publication, acceptance, or scientific novelty in advance.

Any contribution claims will be based on the results of the literature review, implementation, experiments, and quantitative evaluation.

---

## License

See [`LICENSE`](LICENSE).

---

## Author

**Arif Khan**

Software Engineering
Karachi, Pakistan

GitHub: `arifxlab`
LinkedIn: `arif-khan-086a5a405`

---

## Project Principle

> **Build the system. Measure the system. Challenge the assumptions. Report what the evidence actually shows.**
