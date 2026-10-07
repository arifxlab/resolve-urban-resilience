# RESOLVE — Project Vision

## 1. Vision Statement

RESOLVE aims to become a research-oriented platform for understanding how interconnected urban systems behave under disruption and for evaluating which resilience interventions provide the greatest measurable benefit under constrained resources.

The central vision is to move beyond treating urban risks as isolated problems.

Instead, RESOLVE will represent an urban environment as a network of connected infrastructure, services, facilities, populations, hazards, and dependencies.

The platform will then use this representation to investigate how disruptions propagate through the system and how targeted interventions can reduce cascading impacts.

---

## 2. The Problem We Want to Understand

Modern cities are composed of tightly coupled systems.

Electricity supports water pumping.

Water supports hospitals and communities.

Road networks influence emergency response.

Communication infrastructure supports emergency coordination.

Healthcare facilities depend on electricity, water, transportation, and other services.

A disruption in one system can therefore create secondary and tertiary consequences in systems that were not directly affected by the original hazard.

This creates a fundamental challenge:

> **Understanding the risk of an urban system requires understanding not only individual components, but also the dependencies connecting them.**

RESOLVE exists to investigate this challenge computationally.

---

## 3. Long-Term Vision

The long-term vision is a dependency-aware urban resilience observatory capable of:

1. Representing important urban systems as interconnected components.
2. Combining heterogeneous urban and environmental datasets.
3. Modeling dependencies between infrastructure and services.
4. Representing hazards and disruption scenarios.
5. Simulating cascading effects.
6. Quantifying system-level consequences.
7. Evaluating alternative resilience interventions.
8. Comparing intervention strategies under resource constraints.
9. Providing transparent evidence for why one strategy performs better than another.
10. Supporting reproducible research across different scenarios and cities.

The system should ultimately make it possible to ask questions such as:

> If a city can invest in only a limited number of resilience measures, which interventions reduce the greatest amount of cascading risk?

---

## 4. Research Vision

RESOLVE is not designed around the assumption that a dependency-aware model will automatically outperform simpler approaches.

The research goal is to test that proposition.

The project will therefore establish:

* explicit research questions
* falsifiable hypotheses
* measurable evaluation criteria
* appropriate baselines
* controlled experiments
* reproducible configurations
* quantitative results

If the experiments show that the proposed approach does not provide meaningful improvement, that result will be treated as valid research evidence.

The system should make it possible to distinguish:

```text
Assumption
    ↓
Hypothesis
    ↓
Experiment
    ↓
Measurement
    ↓
Evidence
    ↓
Conclusion
```

rather than:

```text
Interesting Technology
    ↓
Implementation
    ↓
Claim
```

---

## 5. Urban Digital Twin Direction

RESOLVE will investigate the concept of a lightweight, research-oriented urban digital twin.

In this context, the digital twin is not intended to reproduce every physical detail of a city.

Instead, it represents the subset of urban systems, relationships, state variables, and dependencies necessary to investigate the selected research questions.

This distinction is important.

A useful research model should prioritize:

* meaningful system representation
* measurable relationships
* reproducibility
* computational feasibility
* data availability
* explicit assumptions

over attempting to model an entire city with unrealistic precision.

---

## 6. Dependency-Aware Modeling

A central component of RESOLVE is dependency-aware system representation.

A simplified dependency structure might look like:

```text
              ┌─────────────┐
              │   Hazard    │
              └──────┬──────┘
                     │
                     ▼
             ┌───────────────┐
             │ Electricity   │
             └───────┬───────┘
                     │
            ┌────────┴────────┐
            ▼                 ▼
      ┌──────────┐      ┌──────────┐
      │  Water   │      │ Telecom  │
      └────┬─────┘      └────┬─────┘
           │                 │
           └────────┬────────┘
                    ▼
             ┌────────────┐
             │ Hospitals  │
             └─────┬──────┘
                   │
                   ▼
             ┌────────────┐
             │ Population │
             └────────────┘
```

The exact dependency structure will be determined from available evidence and domain research rather than arbitrary assumptions.

Dependencies should eventually be represented with attributes such as:

* dependency type
* dependency strength
* direction
* threshold
* failure condition
* recovery behavior
* confidence
* data source

---

## 7. Cascading Risk Vision

The system should be able to represent a disruption as a process rather than a single event.

For example:

```text
Initial Hazard
      ↓
Component Degradation
      ↓
Dependency Failure
      ↓
Secondary Failure
      ↓
Additional Service Disruption
      ↓
Population / Facility Impact
      ↓
System-Level Consequence
```

This allows RESOLVE to investigate questions such as:

* Which components act as critical points of failure?
* Which dependencies contribute most to cascade propagation?
* Which systems are most vulnerable to indirect disruption?
* Where can an intervention interrupt a cascade?
* How much risk reduction can be achieved by protecting different components?

---

## 8. Intervention-Centered Vision

The purpose of modeling risk is not merely to produce a map of vulnerable locations.

The ultimate research objective is to evaluate possible actions.

Potential interventions could include:

* infrastructure hardening
* backup capacity
* redundancy
* improved connectivity
* additional emergency resources
* protection of critical facilities
* targeted maintenance
* resource reallocation
* dependency decoupling
* recovery-capacity improvements

Interventions will be evaluated quantitatively.

A possible conceptual comparison is:

```text
                    Available Resources
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
        Intervention A  Intervention B  Intervention C
             │             │             │
             ▼             ▼             ▼
        Simulation      Simulation      Simulation
             │             │             │
             └─────────────┼─────────────┘
                           ▼
                  Compare Outcomes
                           │
                           ▼
                 Rank by Evidence
```

The goal is not to produce an arbitrary recommendation.

The goal is to determine which interventions perform best according to predefined metrics and assumptions.

---

## 9. Real-World Data Philosophy

Urban datasets are rarely complete.

They may contain:

* missing values
* inconsistent formats
* different spatial resolutions
* different time periods
* uncertain measurements
* outdated infrastructure information
* incomplete dependency information
* incompatible geographic boundaries

RESOLVE will treat these limitations as part of the research problem.

The platform should explicitly record:

* data source
* data version
* coverage
* temporal resolution
* spatial resolution
* missingness
* preprocessing
* assumptions
* uncertainty
* limitations

The system should never imply a level of certainty that the underlying data cannot support.

---

## 10. Karachi as a Potential Case Study

Karachi is a potential initial case-study environment because it provides a complex urban context with interconnected infrastructure, population, transportation, environmental, and service challenges.

However, the research architecture should not be hard-coded around Karachi.

The platform should distinguish between:

```text
Generic RESOLVE Model
          +
City-Specific Data
          =
Case Study
```

This allows the same research framework to potentially be evaluated with another city or dataset in future work.

The final case-study selection will depend on data availability, research scope, and experimental feasibility.

---

## 11. Engineering Vision

RESOLVE should demonstrate that a research system can also be engineered as maintainable software.

The platform should therefore emphasize:

* modular architecture
* explicit domain models
* strong API contracts
* automated testing
* database migrations
* reproducible experiments
* observability
* security
* performance measurement
* clear documentation
* deterministic experiment configuration where possible

The research core should not depend unnecessarily on the HTTP layer.

Conceptually:

```text
                 ┌───────────────┐
                 │   REST API    │
                 └───────┬───────┘
                         │
                         ▼
              ┌────────────────────┐
              │ Application Layer  │
              └─────────┬──────────┘
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
      Risk Engine   Graph Engine   Simulation
          ▲             ▲             ▲
          └─────────────┼─────────────┘
                        │
                        ▼
                Research Experiments
```

This separation allows the same core capabilities to support both the application and the research experiments.

---

## 12. Responsible Research

RESOLVE will avoid presenting simulated results as direct predictions of future real-world events.

Simulation outputs are model-dependent.

They depend on:

* input data
* assumptions
* dependency definitions
* parameter choices
* model structure
* uncertainty
* scenario configuration

Therefore, outputs should be interpreted as evidence within the defined experimental model.

The platform is intended to support research and decision analysis, not replace qualified urban planners, engineers, emergency managers, policymakers, or other domain experts.

---

## 13. Success Criteria

The project will be considered successful if it can demonstrate, with reproducible evidence:

### Research Success

* a clearly defined research question
* defensible hypotheses
* appropriate baselines
* reproducible experiments
* measurable results
* transparent limitations

### System Success

* a working dependency-aware urban model
* functional cascade simulation
* intervention evaluation
* reliable APIs
* tested domain logic
* reproducible experiment execution

### Engineering Success

* maintainable architecture
* automated tests
* documented decisions
* measurable performance
* observable services
* reproducible environments

### Academic Success

The project should produce a technically rigorous research artifact that can support submission to an appropriate academic venue.

Publication or acceptance itself is not treated as a guaranteed outcome.

---

## 14. Guiding Principles

RESOLVE will follow these principles throughout development.

### Evidence Over Assumptions

Claims must be supported by data, experiments, literature, or clearly documented assumptions.

### Research Before Features

A feature should exist because it supports the research or engineering objectives.

### Measurement Before Optimization

Performance improvements should be based on measurements rather than intuition.

### Simplicity Before Complexity

The simplest model capable of answering the research question should be preferred.

### Reproducibility by Default

Important experiments should be repeatable from documented configurations and inputs.

### Transparency Over Hype

Limitations and negative results should be documented rather than hidden.

### Domain Before Technology

Technology choices should follow the research problem, not define it.

---

## 15. Final Vision

RESOLVE aims to provide a computational environment where researchers and engineers can move from:

```text
Urban Hazard
     ↓
System Dependencies
     ↓
Cascading Effects
     ↓
Measured Consequences
     ↓
Candidate Interventions
     ↓
Controlled Simulation
     ↓
Quantitative Comparison
     ↓
Evidence-Based Insight
```

The long-term goal is not simply to build another urban dashboard.

It is to build a **research instrument for studying interconnected urban resilience**.
