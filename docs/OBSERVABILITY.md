# RESOLVE — Observability

## 1. Purpose

This document defines the observability strategy for RESOLVE — Resilient Urban Systems & Vulnerability Observatory.

Observability exists to make the system's internal behavior understandable from measurable external evidence.

For RESOLVE, observability has two equally important purposes:

1. **Engineering observability** — understand application health, failures, performance, resource usage, and operational behavior.
2. **Research observability** — understand simulation execution, experiment configuration, data lineage, model behavior, and reproducibility.

Observability must support research integrity without turning the project into an unnecessarily complex production monitoring platform.

The guiding principle is:

> Measure enough to explain system behavior, validate research execution, and diagnose failures — but do not introduce infrastructure merely because it is technically available.

---

## 2. Observability Principles

RESOLVE follows these principles:

### 2.1 Correctness before observability

A well-instrumented incorrect simulation is still incorrect.

Observability supports correctness but does not replace domain validation, testing, or research methodology.

### 2.2 Measurement before optimization

Performance instrumentation should provide evidence about actual bottlenecks.

Optimization decisions must be based on measurements rather than assumptions.

### 2.3 Research execution must be observable

A completed experiment should provide enough metadata to determine:

* what was executed,
* with which data,
* with which scenario,
* with which configuration,
* with which random seed,
* using which code version,
* and what results were produced.

### 2.4 Deterministic execution where possible

Deterministic simulations should produce reproducible observations under equivalent inputs.

When stochastic behavior is intentionally used, the random seed and stochastic configuration must be recorded.

### 2.5 No sensitive data in telemetry

Logs, traces, and metrics must not become an accidental source of sensitive information.

Personal or unnecessary identifying data must not be placed into telemetry.

### 2.6 Structured evidence over uncontrolled logging

Structured events and metadata are preferred over large amounts of unstructured debug output.

### 2.7 Complexity must be justified

OpenTelemetry, Prometheus, distributed tracing, dashboards, and other tooling may be introduced incrementally.

The system must remain useful without requiring a large observability stack during early development.

---

# 3. Observability Scope

Observability covers five primary areas:

1. Application health
2. API behavior
3. Data and persistence operations
4. Simulation and experiment execution
5. Research reproducibility

The initial system should prioritize application-level structured logging, deterministic experiment metadata, and measurable execution timings.

Metrics and distributed tracing can be introduced as implementation maturity increases.

---

# 4. Observability Model

The initial observability model consists of:

```text
                    RESOLVE
                       │
        ┌──────────────┼──────────────┐
        │              │              │
      Logs          Metrics         Traces
        │              │              │
        └──────────────┼──────────────┘
                       │
                Execution Context
                       │
        ┌──────────────┼──────────────┐
        │              │              │
      API          Simulation      Research
    Execution       Execution      Experiment
```

Every observable operation should provide enough context to connect technical behavior with the operation that produced it.

---

# 5. Correlation and Execution Identity

RESOLVE should use explicit identifiers to correlate related operations.

Potential identifiers include:

* `request_id`
* `simulation_id`
* `experiment_id`
* `scenario_id`
* `dataset_id`
* `run_id`
* `intervention_id`

These identifiers allow logs, metrics, experiment results, and persisted records to be connected.

## 5.1 Request ID

Each API request should have a request correlation identifier.

If supplied by a trusted client, the identifier may be propagated subject to validation.

Otherwise, the application should generate one.

The request ID should appear in relevant structured logs.

## 5.2 Simulation ID

Every simulation execution should have a unique simulation identifier.

The identifier should connect:

* simulation configuration,
* scenario,
* execution events,
* cascade events,
* recovery events,
* outcomes,
* errors,
* runtime measurements.

## 5.3 Experiment Run ID

Each research execution should have a unique run identifier.

The run should connect:

* experiment definition,
* dataset version,
* scenario version,
* baseline configuration,
* proposed-model configuration,
* intervention configuration,
* random seed,
* software version,
* execution environment,
* metrics,
* generated evidence.

---

# 6. Structured Logging

RESOLVE should use structured logging rather than relying exclusively on free-form text logs.

A conceptual log record may contain:

```text
timestamp
level
event
service
component
request_id
simulation_id
experiment_id
scenario_id
run_id
duration_ms
status
error_code
message
```

Not every field is required for every event.

Fields should be included only when meaningful.

---

# 7. Log Levels

The application should support standard severity levels.

## DEBUG

Detailed information useful during development.

Examples:

* graph construction steps,
* scenario transformation details,
* intermediate calculations.

Debug logging should not be enabled excessively in normal execution.

## INFO

Normal meaningful application events.

Examples:

* API request completed,
* simulation started,
* simulation completed,
* experiment started,
* dataset loaded.

## WARNING

Unexpected but recoverable conditions.

Examples:

* optional dataset unavailable,
* non-critical data quality issue,
* fallback behavior activated,
* incomplete optional metadata.

## ERROR

An operation failed.

Examples:

* simulation execution failure,
* database operation failure,
* invalid experiment execution,
* external data ingestion failure.

## CRITICAL

Severe conditions requiring immediate attention during an operational deployment.

Examples:

* persistent database unavailability,
* unrecoverable worker failure,
* severe infrastructure failure.

Critical logging should remain rare.

---

# 8. Logging Rules

Logs must follow these rules:

1. Never log secrets.
2. Never log passwords or authentication tokens.
3. Never log database credentials.
4. Avoid unnecessary personal information.
5. Avoid complete request bodies unless explicitly required for debugging.
6. Avoid unrestricted dataset dumps.
7. Use stable event names.
8. Include correlation identifiers where available.
9. Include error context without exposing sensitive internals.
10. Prefer structured fields over concatenated strings.

---

# 9. API Observability

API observability should capture:

* request count,
* response status,
* request duration,
* endpoint,
* HTTP method,
* error rate,
* validation failures,
* request correlation ID.

Performance analysis should support:

* p50 latency,
* p95 latency,
* p99 latency,
* throughput,
* error rate.

These measurements correspond to the performance requirements defined in `docs/PERFORMANCE.md`.

API telemetry should distinguish between:

* successful requests,
* client errors,
* server errors,
* validation failures,
* dependency failures,
* timeout failures.

---

# 10. Database Observability

Database observability should help identify:

* slow queries,
* failed queries,
* connection failures,
* transaction failures,
* connection pool exhaustion,
* migration problems,
* spatial query performance issues.

Database telemetry should not expose sensitive query parameters unnecessarily.

For performance investigations, query plans may be collected separately from normal application logs.

Database optimization should be based on measured evidence.

---

# 11. Dependency Graph Observability

Dependency graph construction and processing are central to RESOLVE.

The system should be able to measure:

* number of entities,
* number of dependencies,
* graph density,
* graph construction duration,
* graph traversal duration,
* connected components,
* dependency types,
* dependency strength distributions,
* graph validation failures.

Research experiments may additionally record:

* cascade depth,
* cascade breadth,
* number of propagated events,
* affected nodes,
* critical dependency paths,
* intervention-sensitive dependencies.

Graph telemetry should distinguish observed relationships from modeled or inferred relationships.

---

# 12. Simulation Observability

Simulation execution must be observable enough to explain its behavior.

At minimum, a simulation should record:

* simulation ID,
* scenario ID,
* configuration version,
* model version,
* random seed where applicable,
* start time,
* end time,
* duration,
* initial state,
* number of entities,
* number of dependencies,
* number of simulation steps,
* number of state transitions,
* number of cascade events,
* number of recovery events,
* final state,
* termination reason,
* errors if execution failed.

Example lifecycle:

```text
SIMULATION_CREATED
        ↓
SIMULATION_VALIDATED
        ↓
SIMULATION_STARTED
        ↓
STATE_TRANSITIONS
        ↓
CASCADE_EVENTS
        ↓
RECOVERY_EVENTS
        ↓
SIMULATION_COMPLETED
```

A failed execution should instead produce:

```text
SIMULATION_STARTED
        ↓
SIMULATION_FAILED
```

with a structured error classification.

---

# 13. Cascade Event Observability

Cascade events are particularly important because they represent the behavior that distinguishes dependency-aware analysis from isolated assessment.

Each recorded cascade event should, where applicable, identify:

* source component,
* affected component,
* dependency type,
* triggering condition,
* source state,
* target state,
* simulation step or timestamp,
* dependency strength,
* confidence,
* event classification.

This allows researchers to inspect why a cascade occurred rather than only observing its final outcome.

---

# 14. Intervention Observability

Intervention evaluation should record:

* intervention ID,
* intervention type,
* target component or area,
* resource cost,
* constraints,
* expected effect definition,
* simulation configuration,
* resulting outcomes,
* comparison against baseline.

Where intervention rankings are produced, the system should retain sufficient information to reconstruct the ranking.

---

# 15. Experiment Observability

An experiment is an important research execution boundary.

Every experiment run should record:

```text
Experiment
├── experiment_id
├── run_id
├── research_question
├── hypothesis
├── dataset_version
├── scenario_version
├── baseline_version
├── proposed_model_version
├── intervention_configuration
├── budget
├── random_seed
├── software_version
├── environment
├── execution_time
├── metrics
└── evidence_artifacts
```

This metadata is required for meaningful reproducibility.

---

# 16. Research Provenance

Observability must preserve the distinction between:

* observed data,
* transformed data,
* inferred data,
* modeled data,
* simulated data,
* experimental results.

A result should never be presented as directly observed if it was generated through simulation.

Research artifacts should therefore preserve provenance information.

A simplified provenance chain is:

```text
Dataset
   ↓
Preprocessing
   ↓
Canonical Data
   ↓
Scenario
   ↓
Model
   ↓
Simulation
   ↓
Experiment
   ↓
Metrics
   ↓
Evidence
```

---

# 17. Metrics

Metrics should be introduced incrementally.

Initial application metrics may include:

* request count,
* request duration,
* error count,
* simulation duration,
* experiment duration,
* database operation duration,
* graph construction duration.

Research metrics may include:

* cascade size,
* cascade depth,
* cascade breadth,
* recovery time,
* population affected,
* critical facilities affected,
* service downtime,
* intervention cost,
* resilience benefit,
* intervention efficiency.

Research metrics are defined formally in `research/METRICS.md`.

Operational metrics and scientific metrics must not be confused.

For example:

> Lower simulation runtime is an engineering improvement.

It is not automatically:

> Better resilience.

---

# 18. Distributed Tracing

Distributed tracing is optional during early development.

Tracing becomes more valuable when RESOLVE introduces multiple independently executing components such as:

```text
API
 ↓
Application Service
 ↓
Background Worker
 ↓
Database
 ↓
Simulation Engine
```

If background workers or external services create meaningful execution boundaries, OpenTelemetry may be introduced.

Tracing should focus on high-value paths rather than instrumenting every function.

---

# 19. Background Job Observability

If Celery and Redis are introduced, background tasks must expose execution state.

Relevant task states include:

```text
QUEUED
STARTED
RUNNING
COMPLETED
FAILED
RETRYING
CANCELLED
```

A task should be correlated with:

* task ID,
* experiment ID,
* simulation ID,
* request ID where applicable.

The system should record task duration and failure classification.

Redis must not be treated as the authoritative research result store.

---

# 20. Health Checks

The system should provide health information appropriate to its deployment environment.

Possible health categories:

### Liveness

Indicates whether the application process is running.

### Readiness

Indicates whether required dependencies are available for normal operation.

### Dependency health

May include:

* PostgreSQL connectivity,
* Redis connectivity when required,
* external dependency availability where applicable.

Health endpoints must not expose credentials or unnecessary internal information.

---

# 21. Failure Observability

Failures should be classified rather than represented only as generic exceptions.

Initial categories may include:

```text
VALIDATION_ERROR
CONFIGURATION_ERROR
DATA_ERROR
DATA_QUALITY_ERROR
DATABASE_ERROR
GRAPH_ERROR
SIMULATION_ERROR
EXPERIMENT_ERROR
EXTERNAL_SERVICE_ERROR
TIMEOUT_ERROR
AUTHENTICATION_ERROR
AUTHORIZATION_ERROR
RESOURCE_ERROR
INTERNAL_ERROR
```

The classification should help distinguish user errors from system failures.

---

# 22. Research Failure Handling

A failed experiment must not silently produce apparently valid results.

If an experiment fails:

1. Mark the run as failed.
2. Preserve the failure reason.
3. Preserve available execution metadata.
4. Avoid publishing incomplete metrics as complete results.
5. Preserve the code and configuration identifiers.
6. Record whether partial artifacts exist.
7. Allow the researcher to reproduce or diagnose the failure.

Partial execution must be distinguishable from successful execution.

---

# 23. Reproducibility Metadata

Every primary experiment should capture, where applicable:

* Git commit SHA,
* dataset version,
* scenario version,
* model version,
* dependency graph version,
* configuration version,
* random seed,
* Python version,
* package versions,
* operating system,
* hardware information,
* execution timestamp,
* experiment identifier,
* run identifier.

The objective is not merely to know that an experiment ran.

The objective is to know **what exact computational conditions produced the result**.

---

# 24. Performance Instrumentation

Performance instrumentation should measure meaningful execution boundaries.

For example:

```text
Experiment
│
├── Data Loading
├── Validation
├── Graph Construction
├── Baseline Assessment
├── Dependency Simulation
├── Intervention Evaluation
├── Metric Calculation
└── Persistence
```

Each stage may expose:

* duration,
* success/failure,
* input size,
* output size where useful.

This supports bottleneck identification and experiment-runtime analysis.

---

# 25. Monitoring Stack Evolution

The observability stack should evolve gradually.

### Stage 1 — Local development

Use:

* structured application logs,
* execution timing,
* test output,
* experiment metadata.

### Stage 2 — Research execution

Add:

* persistent experiment metadata,
* simulation execution records,
* benchmark measurements,
* structured evidence artifacts.

### Stage 3 — Service-level monitoring

If justified, introduce:

* Prometheus,
* OpenTelemetry,
* dashboards,
* distributed tracing.

### Stage 4 — Larger deployment

Only if required, consider:

* centralized log aggregation,
* alerting,
* advanced tracing,
* service-level objectives,
* infrastructure monitoring.

The project must not adopt Stage 4 complexity simply because those tools are available.

---

# 26. Alerting

Alerting is primarily relevant to deployed environments.

Potential alerts include:

* high API error rate,
* sustained latency degradation,
* database unavailable,
* worker failure,
* queue backlog,
* repeated simulation failures,
* resource exhaustion.

Research experiments should generally report failures rather than rely on operational alerting infrastructure.

---

# 27. Observability and Security

Observability must follow the security model defined in `docs/SECURITY_MODEL.md`.

In particular:

* secrets must never be logged,
* authentication tokens must never be logged,
* personal data should be minimized,
* sensitive datasets should not be dumped into logs,
* stack traces should not expose internal information to untrusted API clients,
* telemetry access must be controlled,
* monitoring endpoints should not expose unnecessary system details.

Observability data itself is an asset and must be protected accordingly.

---

# 28. Observability and Research Integrity

Observability has a direct relationship with research validity.

A research result should be considered stronger when the system can demonstrate:

```text
What data was used?
        ↓
What scenario was executed?
        ↓
What model was used?
        ↓
What dependencies were considered?
        ↓
What intervention was evaluated?
        ↓
What configuration was used?
        ↓
What execution occurred?
        ↓
What metrics were produced?
```

This does not prove scientific validity by itself.

It provides the execution evidence required to investigate and reproduce the result.

---

# 29. Testing Observability

Observability functionality should itself be tested.

Tests may verify:

* request IDs are generated,
* simulation IDs are persisted,
* experiment runs record required metadata,
* failures produce appropriate classifications,
* secrets are excluded from logs,
* important execution timings are recorded,
* failed experiments cannot be mistaken for successful experiments,
* deterministic simulations preserve expected metadata,
* correlation identifiers propagate across supported boundaries.

Testing should avoid making implementation-specific telemetry details unnecessarily brittle.

---

# 30. Evidence Artifacts

Important experiments should produce structured evidence artifacts containing:

* experiment identifier,
* run identifier,
* Git commit,
* dataset version,
* scenario version,
* configuration,
* random seed,
* execution environment,
* metrics,
* runtime,
* outcome,
* status,
* failure information if applicable.

Evidence artifacts should be stored separately from ordinary application logs.

Research evidence should be versioned or otherwise associated with the exact experiment execution.

---

# 31. Observability Anti-Patterns

RESOLVE should avoid:

### Logging everything

Large volumes of logs can obscure important events.

### Logging sensitive information

Telemetry should never become an accidental data leak.

### Instrumenting every function

Excessive instrumentation creates maintenance and performance overhead.

### Treating monitoring as scientific validation

Operational telemetry cannot replace research methodology.

### Mixing engineering and scientific metrics

Runtime and resilience benefit answer different questions.

### Recording results without provenance

A number without its dataset, scenario, model, and configuration is weak research evidence.

### Adding infrastructure without a measurement need

A monitoring stack should solve a demonstrated problem.

---

# 32. Acceptance Criteria

Observability implementation is considered adequate for the current research stage when:

* [ ] Structured logging exists for important application events.
* [ ] API requests can be correlated.
* [ ] Simulation executions have unique identifiers.
* [ ] Experiment runs have unique identifiers.
* [ ] Important execution durations can be measured.
* [ ] Simulation failures are distinguishable from successful execution.
* [ ] Experiment metadata supports reproducibility.
* [ ] Research results retain dataset/scenario/model context.
* [ ] Secrets are excluded from logs and telemetry.
* [ ] Important cascade events can be inspected.
* [ ] Performance measurements align with `docs/PERFORMANCE.md`.
* [ ] Research metrics align with `research/METRICS.md`.
* [ ] Observability tests exist for critical behavior.
* [ ] Additional monitoring infrastructure is introduced only when justified.

---

# 33. Evolution

This document is expected to evolve with implementation.

Observability decisions should be updated when:

* asynchronous processing is introduced,
* the simulation engine becomes more complex,
* distributed components are introduced,
* experiments become computationally expensive,
* deployment moves beyond local development,
* new research evidence requirements emerge.

Every significant observability architecture change should be recorded in `docs/DECISION_LOG.md`.

---

# 34. Guiding Principle

RESOLVE should be observable enough that a researcher or engineer can answer:

> **What happened, why did it happen, under what conditions did it happen, and can we reproduce it?**

Observability exists to make those answers measurable, traceable, and trustworthy.
