# RESOLVE — Performance Requirements and Engineering Plan

## 1. Purpose

This document defines the performance requirements, measurement strategy, benchmark methodology, and optimization principles for RESOLVE — Resilient Urban Systems & Vulnerability Observatory.

Performance is treated as an engineering property that must be **measured rather than assumed**.

The primary objective is not maximum throughput. The system must first provide:

1. correct results;
2. reproducible execution;
3. scientifically valid comparisons;
4. predictable resource usage;
5. sufficient performance for the intended research workload.

Optimization should only be introduced when measurements demonstrate a meaningful bottleneck.

---

# 2. Performance Principles

## 2.1 Correctness Before Speed

A faster simulation that produces incorrect results is not an improvement.

The priority order is:

```text
Correctness
    ↓
Reproducibility
    ↓
Measurement
    ↓
Optimization
```

---

## 2.2 Measure Before Optimizing

Performance changes must be supported by measurements.

Avoid premature optimization based on assumptions such as:

* a database query must be slow;
* graph traversal must require caching;
* simulations must require Celery;
* Redis must be introduced immediately;
* distributed execution must be necessary.

---

## 2.3 Research Workloads Are Different From Web Workloads

RESOLVE has two major performance categories:

```text
Interactive Application
        |
        +--> API latency
        +--> database queries
        +--> graph queries

Research Execution
        |
        +--> simulation runtime
        +--> experiment throughput
        +--> memory consumption
        +--> batch execution
```

These workloads should be measured separately.

---

# 3. Performance Objectives

The system should eventually measure:

* API latency;
* API throughput;
* database query latency;
* graph construction time;
* graph traversal time;
* simulation execution time;
* experiment execution time;
* memory consumption;
* CPU utilization;
* storage usage;
* scenario throughput;
* concurrent workload behavior.

Not every metric must be optimized immediately.

---

# 4. Performance Categories

## 4.1 API Performance

Measures the responsiveness of HTTP endpoints.

Important measurements:

```text
p50 latency
p95 latency
p99 latency
requests/second
error rate
```

Latency should be measured separately for different endpoint classes.

---

## 4.2 Database Performance

Measures:

* query latency;
* insert throughput;
* update throughput;
* spatial query performance;
* graph-related data retrieval;
* indexing effectiveness.

---

## 4.3 Graph Performance

Measures:

* graph construction time;
* graph loading time;
* dependency traversal time;
* downstream impact analysis;
* graph memory consumption.

---

## 4.4 Simulation Performance

Measures:

* simulation runtime;
* time per simulation step;
* number of state transitions;
* number of cascade events;
* memory usage;
* scenarios processed per unit time.

---

## 4.5 Experiment Performance

Measures:

* complete experiment duration;
* baseline execution time;
* proposed-method execution time;
* intervention evaluation time;
* number of scenarios processed;
* number of intervention combinations evaluated.

---

# 5. Performance Model

A simplified execution model is:

```text
Experiment Runtime
=
Data Loading
+
Validation
+
Graph Construction
+
Baseline Execution
+
Simulation
+
Metric Calculation
+
Persistence
+
Evidence Generation
```

This decomposition is important because total execution time alone does not identify the bottleneck.

---

# 6. Initial Performance Targets

Initial targets are engineering goals rather than research claims.

They should be revised after real measurements.

| Component                      |                   Initial Target |
| ------------------------------ | -------------------------------: |
| Health endpoint                |                         < 100 ms |
| Simple read API                |                     < 300 ms p95 |
| Standard CRUD API              |                     < 500 ms p95 |
| Small graph query              |                        < 1 s p95 |
| Small deterministic simulation |                            < 5 s |
| Small experiment               |                           < 30 s |
| Test suite                     | Practical for frequent execution |

These targets assume local or development-scale workloads.

They must not be presented as achieved results until measured.

---

# 7. Performance Test Dataset

Benchmarks must use explicit dataset sizes.

Initial benchmark categories:

```text
Small
Medium
Large
```

The exact entity counts should be established when the first representative dataset exists.

For example:

```text
Small:
hundreds of entities

Medium:
thousands of entities

Large:
tens of thousands of entities
```

These categories are placeholders for benchmark planning and must be replaced with measured dataset definitions before final evaluation.

---

# 8. Simulation Benchmark Scenarios

Simulation benchmarks should vary:

* number of entities;
* number of dependencies;
* graph density;
* scenario duration;
* number of state transitions;
* number of interventions;
* recovery complexity.

Example:

```text
Benchmark A
100 entities
200 dependencies

Benchmark B
1,000 entities
3,000 dependencies

Benchmark C
10,000 entities
30,000 dependencies
```

These values are initial engineering examples, not claims about the final system capacity.

---

# 9. API Benchmarking

API performance tests should measure representative endpoints.

Examples:

```text
GET /entities
GET /entities/{id}
GET /dependencies
GET /graph
POST /scenarios
POST /simulations
GET /simulations/{id}
POST /experiments/{id}/run
GET /experiments/{id}/results
```

Expensive research endpoints should be measured separately from simple CRUD operations.

---

# 10. Latency Percentiles

Average latency alone is insufficient.

The system should report:

```text
p50
p95
p99
```

Definitions:

* **p50:** median request latency;
* **p95:** latency below which 95% of requests complete;
* **p99:** latency below which 99% of requests complete.

High-percentile latency is particularly important for identifying inconsistent behavior.

---

# 11. Throughput

Throughput may be measured as:

```text
requests / second
```

for API workloads and:

```text
scenarios / minute
experiments / hour
```

for research workloads.

Throughput measurements must include the workload definition.

A throughput value without a workload description is not a meaningful benchmark.

---

# 12. Database Performance

Database benchmarks should measure:

* simple lookups;
* filtered queries;
* joins;
* spatial queries;
* dependency retrieval;
* bulk insertion;
* experiment result persistence.

Indexes should be introduced based on measured query behavior.

Potential indexes include:

```text
entity identifiers
entity type
entity state
dependency source
dependency target
scenario identifiers
experiment identifiers
spatial geometry
dataset version
```

The final index set must follow actual query patterns.

---

# 13. Spatial Performance

PostGIS spatial operations may become expensive as geographic data grows.

Important operations include:

```text
Point-in-polygon
Distance queries
Intersection
Containment
Nearest-neighbor queries
Spatial filtering
```

Performance should be measured before introducing advanced spatial optimization.

Spatial indexes should be used where measurements justify them.

---

# 14. Graph Performance

Graph performance depends on:

```text
Number of Nodes
+
Number of Edges
+
Graph Density
+
Traversal Depth
+
Algorithm Complexity
```

Measurements should include:

* graph construction;
* graph serialization;
* graph loading;
* dependency traversal;
* downstream impact analysis.

Graph algorithms should avoid unnecessary repeated traversal.

---

# 15. Simulation Performance

Simulation performance should be decomposed into:

```text
Initialization
    +
Hazard Application
    +
Dependency Evaluation
    +
State Transition
    +
Recovery
    +
Event Recording
    +
Metric Calculation
```

Profiling should identify the dominant component before optimization.

---

# 16. Event Recording Overhead

Cascade event recording provides important research evidence but may increase execution cost.

The system should measure the difference between:

```text
Simulation without detailed event recording
```

and:

```text
Simulation with detailed event recording
```

If event recording becomes a major bottleneck, possible strategies include:

* compact event representations;
* batch persistence;
* buffered writes;
* configurable detail levels.

Research-critical evidence must not be removed merely to improve performance without evaluating the scientific consequences.

---

# 17. Experiment Scaling

Experiment cost may grow rapidly when evaluating multiple interventions.

A simplified model is:

```text
Experiment Cost
≈
Number of Scenarios
×
Number of Intervention Configurations
×
Simulation Cost
```

If intervention combinations grow combinatorially, exhaustive evaluation may become impractical.

The initial implementation should use exhaustive evaluation where the experimental search space is small and manageable.

Optimization or search algorithms should be introduced only after establishing the baseline computational cost.

---

# 18. Parallel Execution

Independent simulations may eventually be executed in parallel.

Example:

```text
Scenario A ──> Simulation Worker 1
Scenario B ──> Simulation Worker 2
Scenario C ──> Simulation Worker 3
Scenario D ──> Simulation Worker 4
```

Parallelism should be introduced only when:

* simulations are sufficiently independent;
* execution time justifies the complexity;
* deterministic behavior can still be controlled;
* resource requirements are understood.

---

# 19. Background Processing

Long-running simulations may eventually use:

```text
FastAPI
   |
   v
Task Queue
   |
   v
Worker
   |
   v
Simulation
```

Celery and Redis are candidate technologies.

They should not be mandatory for the initial implementation.

The first implementation should prefer synchronous execution for small deterministic workloads.

---

# 20. Caching

Caching may be introduced for expensive operations that are:

* deterministic;
* repeatedly requested;
* safe to reuse.

Potential cache candidates:

```text
Graph Construction
Static Dataset Queries
Repeated Spatial Queries
Derived Metadata
```

Simulation results should only be cached when the cache key captures every parameter that can affect the result.

A cache key may conceptually depend on:

```text
Dataset Version
Scenario Version
Model Version
Intervention Set
Simulation Configuration
Random Seed
Software Version
```

Incorrect caching can create scientifically invalid results and is therefore a research-integrity concern.

---

# 21. Memory Performance

Memory usage should be monitored for:

* large urban models;
* dense dependency graphs;
* simulation histories;
* cascade events;
* experiment batches.

Potential optimization strategies include:

* streaming large datasets;
* compact representations;
* bounded histories;
* batch processing;
* selective persistence.

Optimization must preserve required research evidence.

---

# 22. Storage Performance

Storage usage should distinguish:

```text
Raw Data
Canonical Data
Derived Data
Simulation Outputs
Research Evidence
Logs
```

Large raw datasets should not automatically be duplicated unnecessarily.

Research-critical artifacts should be retained according to reproducibility requirements.

---

# 23. API Concurrency

API concurrency should be tested separately from simulation concurrency.

A basic test may compare:

```text
1 concurrent client
10 concurrent clients
50 concurrent clients
```

The exact concurrency levels should be adjusted to the available environment.

Measurements should include:

* latency;
* throughput;
* error rate;
* CPU;
* memory.

---

# 24. Database Connection Management

The application should use controlled database connection pooling.

The pool must be sized based on measured workload and database capacity.

Excessive connection counts can degrade rather than improve performance.

---

# 25. Performance Regression Testing

Important benchmark results should be retained so future changes can be compared.

A regression may be defined as:

```text
Current Measurement
significantly worse than
Established Baseline
```

The threshold should be chosen after enough benchmark history exists.

Not every performance increase is automatically a regression if it results from a deliberate research feature or correctness improvement.

---

# 26. Profiling Strategy

Profiling should be performed when measurements identify a bottleneck.

Potential tools include:

* Python `cProfile`;
* `py-spy`;
* `time.perf_counter`;
* database query analysis;
* PostgreSQL `EXPLAIN`;
* application-level timing;
* memory profiling tools.

Tool selection should remain lightweight unless deeper profiling is necessary.

---

# 27. Performance Instrumentation

Important operations should expose timing information.

Conceptual structure:

```text
Operation
├── start_time
├── end_time
├── duration
├── input_size
├── output_size
└── status
```

Research executions should record sufficient timing metadata to compare experiments.

---

# 28. Reproducible Benchmarks

Performance benchmarks must record:

```text
Hardware
Operating System
Python Version
Dependency Versions
Git Commit
Dataset Version
Scenario Version
Configuration
Random Seed
Concurrency
Benchmark Duration
```

Performance numbers without environment information should not be treated as universally comparable.

---

# 29. Hardware Awareness

Performance results depend on hardware.

The project should distinguish between:

```text
Development Benchmark
Research Benchmark
Deployment Benchmark
```

A development laptop result must not automatically be presented as a production capacity claim.

---

# 30. Performance and Scientific Validity

Performance optimizations must not alter the research method unintentionally.

Examples of potentially dangerous optimizations:

* skipping dependency edges;
* reducing simulation precision;
* dropping cascade events;
* changing time-step behavior;
* approximating metrics without documentation;
* changing intervention evaluation logic.

Any optimization that changes scientific semantics must be treated as a model change and documented accordingly.

---

# 31. Performance Experiments

Potential engineering experiments include:

### P1 — Graph Scaling

Measure graph construction and traversal as graph size increases.

### P2 — Simulation Scaling

Measure simulation runtime against entity and dependency counts.

### P3 — Event Recording Overhead

Measure simulation performance with and without detailed event persistence.

### P4 — Database Query Scaling

Measure query latency as dataset size increases.

### P5 — API Concurrency

Measure API behavior under increasing concurrent requests.

### P6 — Experiment Scaling

Measure total experiment runtime as intervention combinations increase.

### P7 — Parallel Execution

Compare sequential and parallel independent simulation execution.

---

# 32. Performance Evidence

Performance evidence should contain:

```text
Benchmark ID
Date
Git Commit
Environment
Dataset Version
Scenario
Configuration
Workload Size
Measurement
Result
Interpretation
```

Example:

```text
Benchmark: P2
Dataset: urban-v1
Scenario: heat-001
Entities: 1,000
Dependencies: 3,000
Seed: 42
Runtime: <measured value>
```

The actual result should only be entered after execution.

---

# 33. Performance Acceptance Criteria

The performance engineering plan is considered sufficiently implemented when:

* API latency can be measured;
* simulation runtime can be measured;
* database performance can be measured;
* graph performance can be measured;
* memory usage can be measured;
* representative workloads are defined;
* benchmark environments are recorded;
* reproducibility metadata is retained;
* performance regressions can be detected;
* optimizations are supported by measurements;
* scientific semantics are protected during optimization.

---

# 34. Initial Performance Strategy

The implementation sequence should be:

```text
1. Build correct system
2. Create representative workloads
3. Measure baseline performance
4. Identify bottlenecks
5. Optimize highest-impact bottleneck
6. Re-run correctness tests
7. Re-run performance benchmark
8. Compare measurements
9. Document the change
10. Keep the optimization only if justified
```

---

# 35. Guiding Performance Principle

> **RESOLVE should become faster because measurements demonstrate where improvement is needed, not because complexity is assumed to be valuable.**

Performance is successful when the system can execute scientifically valid experiments within practical resource limits while preserving correctness, reproducibility, and evidence quality.
