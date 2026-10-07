# RESOLVE — Domain Model

## 1. Purpose

This document defines the conceptual domain model for RESOLVE.

The domain model describes the urban entities, relationships, scenarios, hazards, interventions, simulations, outcomes, and research experiments that form the core of the system.

The model is intentionally independent of specific database tables, API endpoints, frameworks, or frontend components.

The implementation should preserve these domain concepts even if the underlying technologies change.

---

# 2. Domain Overview

RESOLVE models an urban environment as a collection of interconnected systems.

At a high level:

```text
Urban Environment
       │
       ├── Urban Entities
       │      │
       │      ├── Infrastructure
       │      ├── Facilities
       │      ├── Population
       │      └── Geographic Areas
       │
       ├── Dependencies
       │
       ├── Hazards
       │
       ├── Scenarios
       │
       ├── Interventions
       │
       └── Simulations
              │
              └── Outcomes
```

The central research object is the **dependency-aware urban system**.

---

# 3. Core Domain Concepts

The initial domain consists of the following concepts:

1. Urban Entity
2. Geographic Area
3. Infrastructure Component
4. Critical Facility
5. Population Group
6. Dependency
7. Hazard
8. Scenario
9. Intervention
10. Resource Budget
11. Simulation
12. Simulation State
13. Cascade Event
14. Recovery Process
15. Outcome
16. Experiment
17. Dataset
18. Data Provenance

These concepts may evolve as implementation and research requirements become clearer.

---

# 4. Urban Entity

## 4.1 Definition

An **Urban Entity** represents a real or modeled component of the urban environment.

Examples include:

* electricity substations
* water pumping stations
* hospitals
* schools
* roads
* emergency facilities
* buildings
* population groups
* geographic service areas

Every entity should have a stable identifier.

---

## 4.2 Core Attributes

Conceptually:

```text id
entity_type
name
status
location
attributes
source
confidence
```

Not every entity type requires every attribute.

---

## 4.3 Entity Status

An entity may have a state such as:

```text
NORMAL
DEGRADED
DISRUPTED
FAILED
RECOVERING
RECOVERED
```

The exact state model may be simplified for the initial simulator.

State transitions must be explicitly defined rather than inferred implicitly.

---

# 5. Geographic Area

A **Geographic Area** represents a spatial unit used for analysis.

Examples:

* city
* district
* neighborhood
* grid cell
* service area
* hazard zone

An area may contain or intersect multiple urban entities.

---

## 5.1 Spatial Relationships

Potential relationships include:

```text
CONTAINS
WITHIN
INTERSECTS
NEAR
ADJACENT_TO
SERVES
```

Spatial relationships should be represented separately from functional dependencies.

For example:

```text
Hospital A
     │
     ├── geographically near ── Road B
     │
     └── depends on ── Electricity C
```

Proximity does not automatically imply dependency.

---

# 6. Infrastructure Component

An **Infrastructure Component** represents an urban infrastructure asset or system component.

Examples:

* electricity substation
* water pump
* water treatment facility
* road segment
* telecommunications node
* drainage facility

Infrastructure components may:

* provide services,
* consume services,
* depend on other components,
* support other entities,
* experience disruption,
* recover over time.

---

# 7. Critical Facility

A **Critical Facility** represents a facility whose disruption may have significant consequences for the urban population or emergency response.

Examples:

* hospitals
* emergency response centers
* schools
* shelters
* major water facilities

Criticality should be represented as an explicit attribute or derived metric rather than assumed solely from facility type.

---

# 8. Population Group

A **Population Group** represents an aggregated population unit used for impact analysis.

Examples:

* residents within a geographic area
* population exposed to a hazard
* population served by a utility
* population affected by a service disruption

The initial system should favor aggregated population representation.

Individual-level personal data is outside the initial scope.

---

# 9. Dependency

## 9.1 Definition

A **Dependency** represents a functional relationship in which the condition of one urban component can affect another.

Conceptually:

```text
Source
   │
   │ dependency
   ↓
Dependent
```

Example:

```text
Electricity Grid
       ↓
Water Pump
```

The water pump depends on electricity availability.

---

## 9.2 Dependency Attributes

A dependency may contain:

```text
id
source_entity_id
dependent_entity_id
dependency_type
strength
threshold
direction
geographic_relation
confidence
source
```

Some attributes may be optional depending on the dependency type.

---

## 9.3 Dependency Direction

Dependencies are directional.

If:

```text
A → B
```

this means B depends on A.

It does not automatically mean:

```text
B → A
```

---

## 9.4 Dependency Strength

Dependency strength represents the degree to which disruption of the source can affect the dependent component.

A normalized representation may initially use:

```text
0.0 ≤ strength ≤ 1.0
```

The exact interpretation must be defined before experiments use the value.

Dependency strength must not be presented as empirically validated unless supported by evidence.

---

## 9.5 Dependency Types

Potential dependency types include:

```text
POWER
WATER
TRANSPORT
COMMUNICATION
SUPPLY
SERVICE
ACCESS
GEOGRAPHIC
OPERATIONAL
```

The initial implementation should introduce only dependency types required by the research scenarios.

---

# 10. Hazard

A **Hazard** represents an external event or condition capable of disrupting urban entities.

Examples:

* extreme heat
* flooding
* power outage
* water disruption

A hazard may affect:

* entities,
* geographic areas,
* services,
* infrastructure components,
* population groups.

---

## 10.1 Hazard Attributes

Conceptually:

```text
id
hazard_type
intensity
geometry
duration
start_time
source
confidence
```

Hazard intensity must have an explicitly defined unit where applicable.

---

# 11. Scenario

## 11.1 Definition

A **Scenario** represents a complete, reproducible experimental situation.

A scenario combines:

```text
Hazard
   +
Urban Environment
   +
Initial Conditions
   +
Dependency Model
   +
Simulation Configuration
   +
Recovery Assumptions
   +
Optional Interventions
```

---

## 11.2 Scenario Attributes

A scenario should identify:

```text
scenario_id
name
description
hazard
geographic_scope
affected_entities
simulation_duration
recovery_configuration
dependency_configuration
intervention_configuration
resource_budget
random_seed
dataset_version
```

---

## 11.3 Scenario Immutability

Once used in a primary experiment, a scenario configuration should be treated as immutable.

If the scenario changes materially, it should receive a new version or identifier.

This prevents accidental modification of experimental conditions.

---

# 12. Intervention

## 12.1 Definition

An **Intervention** represents an action intended to reduce disruption, limit cascading effects, improve recovery, or protect urban services.

Examples:

* backup power
* water-pump redundancy
* infrastructure hardening
* emergency capacity increase
* road resilience improvement
* distributed resource provision

---

## 12.2 Intervention Attributes

Conceptually:

```text
id
name
type
target_entities
cost
effect
implementation_time
assumptions
```

---

## 12.3 Intervention Effect

An intervention may change one or more system properties.

Examples:

```text
Intervention
      ↓
Increase component resilience
      ↓
Lower failure probability

Intervention
      ↓
Increase redundancy
      ↓
Reduce dependency vulnerability

Intervention
      ↓
Improve recovery capacity
      ↓
Reduce downtime
```

The mathematical representation of intervention effects must be documented before primary experiments.

---

# 13. Resource Budget

A **Resource Budget** represents the constraint under which interventions are selected.

A budget may be expressed as:

```text
financial cost
implementation units
number of interventions
capacity
```

The initial research evaluation should use a clearly defined budget type.

---

# 14. Simulation

## 14.1 Definition

A **Simulation** executes a scenario over a defined period or sequence of discrete steps.

Conceptually:

```text
Initial State
     ↓
Hazard Applied
     ↓
Initial Disruptions
     ↓
Dependency Propagation
     ↓
Cascade
     ↓
Recovery
     ↓
Final State
```

---

## 14.2 Simulation Inputs

A simulation may require:

* scenario
* urban entities
* dependency graph
* hazard
* intervention set
* recovery model
* simulation duration
* time-step configuration
* random seed

---

## 14.3 Simulation Outputs

A simulation should produce:

* final state
* state transitions
* cascade events
* affected entities
* affected population
* affected facilities
* service disruption
* recovery information
* computed metrics

---

# 15. Simulation State

A **Simulation State** represents the condition of the urban system at a specific point in simulation time.

A state may contain:

```text
simulation_time
entity_states
service_states
active_disruptions
active_cascades
recovery_states
```

The simulator should make state transitions observable enough to support debugging and research analysis.

---

# 16. Cascade Event

A **Cascade Event** represents a disruption propagated from one component to another.

Conceptually:

```text
Event A
  ↓
Dependency
  ↓
Event B
```

A cascade event should record, where possible:

```text
event_id
simulation_id
source_entity
target_entity
trigger
dependency
time_step
severity
```

This enables later analysis of cascade pathways.

---

# 17. Recovery

Recovery represents the process by which a disrupted entity or service returns toward normal operation.

A recovery model may define:

* recovery start time
* recovery duration
* recovery rate
* dependency on other services
* intervention effects
* recovery state

The initial implementation may use simplified deterministic recovery rules.

More complex recovery models should only be introduced when justified by research requirements.

---

# 18. Outcome

An **Outcome** represents a measurable consequence of a simulation.

Potential outcomes include:

### Population

```text
affected_population
protected_population
```

### Facilities

```text
affected_critical_facilities
protected_critical_facilities
```

### Services

```text
service_downtime
service_availability
```

### Cascade

```text
cascade_size
cascade_depth
cascade_breadth
```

### Recovery

```text
recovery_time
```

### Intervention

```text
intervention_cost
resilience_benefit
benefit_per_cost
```

Outcomes must correspond to defined metrics in `research/METRICS.md`.

---

# 19. Experiment

## 19.1 Definition

An **Experiment** is a controlled research execution designed to answer a research question or test a hypothesis.

An experiment is different from a simulation.

```text
Experiment
    │
    ├── Method
    ├── Baseline
    ├── Scenario(s)
    ├── Configuration
    ├── Repetitions
    └── Metrics
          │
          ↓
      Simulations
          │
          ↓
       Results
```

---

## 19.2 Experiment Attributes

An experiment should identify:

```text
experiment_id
hypothesis
method
baseline
dataset_version
scenario_version
configuration
random_seeds
repetitions
metrics
software_version
```

---

# 20. Dataset

A **Dataset** represents a collection of data used by the system or research workflow.

Potential dataset categories include:

* infrastructure
* population
* geographic boundaries
* roads
* facilities
* weather
* satellite-derived information
* historical incidents
* hazard data

Dataset selection shall be documented in `research/DATASETS.md`.

---

# 21. Data Provenance

A **Data Provenance** record describes where data came from and how it was transformed.

Potential attributes:

```text
source
dataset_name
version
acquisition_date
license
geographic_coverage
temporal_coverage
preprocessing
transformations
limitations
```

Provenance is part of the research evidence chain.

---

# 22. Domain Relationships

The major relationships can be represented as:

```text
Geographic Area
      │
      ├── contains ── Urban Entity
      │
      └── affected by ── Hazard

Urban Entity
      │
      ├── depends on ── Urban Entity
      │
      ├── provides service to ── Urban Entity
      │
      └── affected by ── Hazard

Scenario
      │
      ├── defines ── Hazard
      ├── selects ── Urban Entities
      ├── uses ── Dependency Model
      ├── applies ── Intervention
      └── defines ── Resource Budget

Experiment
      │
      ├── uses ── Dataset
      ├── executes ── Scenario
      ├── compares ── Methods
      └── produces ── Outcomes
```

---

# 23. Baseline and Proposed Method as Domain Concepts

The research comparison should distinguish between methods.

## 23.1 Isolated Risk Assessment

The isolated method evaluates components independently.

```text
Hazard
  ↓
Component Risk
  ↓
Component Outcome
```

Dependencies are not propagated through the system.

---

## 23.2 Dependency-Aware RESOLVE

The proposed method evaluates system-level consequences.

```text
Hazard
  ↓
Initial Impact
  ↓
Dependency Graph
  ↓
Cascade Propagation
  ↓
System-Level Impact
  ↓
Intervention Evaluation
```

Both methods should consume equivalent scenario inputs where possible.

---

# 24. Domain Invariants

The following invariants should hold.

### INV-001 — Stable Identity

Every persistent domain entity must have a unique identifier within its entity namespace.

### INV-002 — Valid Dependency Direction

A dependency must identify both a source and dependent entity.

### INV-003 — No Self-Dependency by Default

An entity should not depend directly on itself unless a future domain rule explicitly permits it.

### INV-004 — Valid Strength

If dependency strength is represented as a normalized value, it must remain within its documented range.

### INV-005 — Scenario Completeness

A simulation cannot execute without the minimum scenario configuration required by the selected simulation model.

### INV-006 — Intervention Cost

An intervention used in budget-constrained evaluation must have a defined cost under the chosen budget model.

### INV-007 — Reproducibility

A reproducible experiment must record the configuration and random seed where stochastic behavior is present.

### INV-008 — Metric Consistency

An outcome must use the metric definition active for the corresponding experiment version.

---

# 25. Domain Events

The system may eventually expose domain events such as:

```text
EntityDisrupted
DependencyActivated
CascadePropagated
EntityRecovered
InterventionApplied
SimulationStarted
SimulationCompleted
ExperimentStarted
ExperimentCompleted
```

These events should only be introduced when they provide practical value for simulation, observability, or research analysis.

---

# 26. Domain Boundaries

The domain should be separated into logical areas:

```text
Urban Model
    ├── Entities
    ├── Geographic Areas
    └── Dependencies

Risk & Hazard
    ├── Hazards
    ├── Exposure
    └── Initial Impact

Simulation
    ├── Scenario
    ├── State
    ├── Cascade
    └── Recovery

Intervention
    ├── Intervention
    ├── Budget
    └── Prioritization

Research
    ├── Experiment
    ├── Dataset
    ├── Metrics
    └── Outcomes
```

These boundaries are conceptual and do not require one-to-one mapping to Python packages or database schemas.

---

# 27. Initial Domain Model Diagram

The conceptual model is:

```text
                         ┌─────────────────┐
                         │ Geographic Area │
                         └────────┬────────┘
                                  │
                               contains
                                  │
                                  ▼
                         ┌─────────────────┐
                         │  Urban Entity   │
                         └────────┬────────┘
                                  │
                      depends on  │  provides
                                  │
                    ┌─────────────┴─────────────┐
                    ▼                           ▼
             ┌─────────────┐             ┌─────────────┐
             │ Dependency  │             │   Service   │
             └─────────────┘             └─────────────┘
                    ▲
                    │
                    │ propagates
                    │
              ┌─────┴─────┐
              │ Simulation │
              └─────┬─────┘
                    ▲
                    │ executes
                    │
              ┌─────┴─────┐
              │  Scenario  │
              └─────┬─────┘
                    │
             ┌──────┴──────┐
             ▼             ▼
        ┌─────────┐   ┌────────────┐
        │ Hazard  │   │Intervention│
        └─────────┘   └─────┬──────┘
                            │
                         constrained
                            │
                            ▼
                      ┌──────────┐
                      │  Budget  │
                      └──────────┘

Experiment
    │
    ├── Dataset
    ├── Scenario
    ├── Method
    └── Metrics
          │
          ▼
       Outcomes
```

---

# 28. Domain Evolution

The domain model is expected to evolve as empirical data, simulation requirements, and research experiments expose missing concepts.

Changes should follow this process:

```text
Observed requirement
        ↓
Domain discussion
        ↓
Research / engineering justification
        ↓
Domain model update
        ↓
Decision log entry
        ↓
Implementation
        ↓
Tests
```

Domain complexity should not be increased merely because a concept is theoretically possible.

---

# 29. Initial Domain Model Freeze

The initial domain model should be considered sufficiently stable for implementation when:

* core entity types are defined,
* dependency semantics are documented,
* scenarios are reproducible,
* interventions are explicit,
* simulations have defined inputs and outputs,
* outcomes map to research metrics,
* experiments can trace results to their configurations.

After this point, major domain changes should be recorded in `docs/DECISION_LOG.md`.

---

# 30. Guiding Principle

The domain model exists to make the research problem computationally explicit.

The central abstraction is not the dashboard, API, or database.

It is:

```text
Urban Systems
      +
Dependencies
      +
Hazards
      +
Interventions
      +
Simulation
      =
Measurable Resilience Analysis
```

Every implementation decision should preserve this core purpose.
