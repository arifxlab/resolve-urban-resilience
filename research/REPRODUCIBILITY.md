# RESOLVE — Reproducibility Framework

## 1. Purpose

This document defines the reproducibility requirements for RESOLVE research experiments.

The objective is to ensure that a result can be traced from its published or reported value back to the exact:

* source code;
* dataset;
* dataset version;
* preprocessing pipeline;
* scenario;
* dependency graph;
* intervention configuration;
* experiment configuration;
* software environment;
* random seed;
* execution conditions;
* generated evidence.

The fundamental reproducibility chain is:

```text
Research Claim
      ↓
Result
      ↓
Experiment Run
      ↓
Experiment Configuration
      ↓
Scenario Version
      ↓
Dataset Version
      ↓
Dependency Graph Version
      ↓
Code Version
      ↓
Environment
      ↓
Execution
```

A result that cannot be traced through this chain MUST NOT be treated as fully reproducible evidence.

---

## 2. Reproducibility Principles

RESOLVE follows these principles:

1. **Traceability over convenience** — important results must be traceable to their inputs.
2. **Version everything important** — code, datasets, scenarios, configurations, and model definitions require identifiable versions.
3. **Record execution context** — software and hardware conditions may affect results.
4. **Separate observed from generated data** — derived and simulated outputs must not be confused with source observations.
5. **Determinism where possible** — deterministic behavior is preferred for research-critical calculations.
6. **Explicit randomness** — stochastic processes must use recorded seeds.
7. **Immutable research evidence** — completed experiment outputs should not be silently overwritten.
8. **Automate reproduction** — reproduction should rely on scripts/configuration rather than undocumented manual steps.
9. **Preserve provenance** — source and transformation history must remain available.
10. **Document limitations** — exact reproduction may be constrained by unavailable data, external services, hardware, or proprietary dependencies.
11. **Environment parity matters** — different environments should be identified when exact parity is impossible.
12. **Reproducibility is part of research integrity** — it is not an optional documentation feature.

---

## 3. Reproducibility Levels

RESOLVE distinguishes several levels of reproducibility.

### Level R0 — Informal

The result exists but the execution context is poorly documented.

Examples:

* manually executed experiment;
* missing configuration;
* missing seed;
* unclear dataset version.

R0 is insufficient for primary research evidence.

---

### Level R1 — Documented

The experiment has documented:

* inputs;
* configuration;
* code version;
* execution procedure;
* output metrics.

R1 provides traceability but may require manual reconstruction.

---

### Level R2 — Script-Reproducible

The experiment can be reproduced using version-controlled scripts and configuration.

Required:

* reproducible environment;
* versioned configuration;
* dataset manifest;
* execution command;
* deterministic or seeded execution.

R2 is the minimum target for primary research experiments.

---

### Level R3 — Independently Reproducible

An independent researcher can reproduce the experiment using the documented repository, data, environment instructions, and permitted datasets.

The reproduction should produce equivalent results within defined tolerances.

R3 is the target for major published experiments where data licensing permits.

---

### Level R4 — Fully Reproducible Research Package

The complete research package contains:

* source code;
* exact experiment configuration;
* accessible dataset or legally shareable equivalent;
* environment specification;
* execution scripts;
* expected outputs;
* validation checks;
* result-generation scripts;
* evidence metadata.

R4 is desirable but may not always be possible because of licensing, data availability, or infrastructure constraints.

---

## 4. Reproducibility Requirements

Every primary experiment MUST record:

```text
experiment_id
run_id
code_version
dataset_version
scenario_version
dependency_graph_version
intervention_version
configuration_version
environment_version
random_seed
execution_timestamp
execution_host
software_versions
result_version
```

Where a field is not applicable, the experiment record should explicitly state that it is not applicable.

Missing metadata must not silently be interpreted as reproducible.

---

## 5. Code Version

Every research result must identify the source code version used for execution.

The preferred identifier is the Git commit SHA.

Example:

```text
code_version:
  git_commit: <commit-sha>
```

The working tree should ideally be clean when a primary experiment is executed.

If uncommitted changes are used, the experiment MUST record this explicitly.

Primary paper results SHOULD preferably originate from committed repository states.

---

## 6. Dataset Version

Every experiment must identify the exact dataset versions used.

A dataset identifier should distinguish:

```text
Dataset
+
Version
+
Acquisition Date
+
Processing Version
```

Example:

```text
dataset_id: POPULATION-001
dataset_version: v1.2
processing_version: PREP-003
```

A generic dataset name is insufficient.

For example:

```text
population data
```

is not sufficient reproducibility metadata.

Instead:

```text
population dataset X
version Y
processed using pipeline Z
```

should be recorded.

---

## 7. Dataset Manifest

Research datasets should have a manifest containing at least:

* dataset identifier;
* dataset version;
* source;
* acquisition date;
* licensing information;
* geographic coverage;
* temporal coverage;
* coordinate reference system;
* source format;
* processed format;
* preprocessing version;
* quality status;
* checksum where practical;
* known limitations.

Conceptually:

```yaml
dataset_id: DATASET-001
version: v1.0
source: ...
acquired_at: ...
license: ...
crs: ...
processing_version: PREP-001
checksum: ...
quality_status: validated
```

The exact schema may evolve during implementation.

---

## 8. Data Integrity

Important data files SHOULD have integrity checks.

Suitable mechanisms include:

* SHA-256 checksums;
* file hashes;
* database snapshot identifiers;
* immutable object identifiers;
* version-control references.

Example:

```text
dataset_file
    ↓
SHA-256
    ↓
recorded checksum
```

If a file changes, its checksum should change.

This provides a simple mechanism for detecting accidental or undocumented modifications.

---

## 9. Data Lineage

Derived data must retain lineage information.

The transformation chain should be traceable:

```text
Raw Dataset
    ↓
Validation
    ↓
Cleaning
    ↓
Normalization
    ↓
Spatial Processing
    ↓
Canonical Dataset
    ↓
Research Dataset
    ↓
Simulation Input
```

Each important transformation should identify:

* input dataset;
* transformation;
* transformation version;
* output dataset;
* execution configuration;
* relevant assumptions.

---

## 10. Preprocessing Reproducibility

Preprocessing should be automated wherever practical.

Manual transformations SHOULD NOT be required for primary research datasets.

If manual processing is unavoidable, document:

* exact procedure;
* software used;
* operator action;
* input files;
* output files;
* date;
* assumptions;
* validation performed.

Automated preprocessing is strongly preferred because it reduces undocumented variation.

---

## 11. Scenario Versioning

Every research scenario must have a stable identifier and version.

Example:

```text
SCN-001
SCN-001-v1
SCN-001-v2
```

A scenario version should identify:

* hazard;
* intensity;
* geographic extent;
* affected entities;
* simulation duration;
* initial state;
* recovery assumptions;
* configuration;
* dependency graph;
* relevant dataset versions.

Changing a research-critical scenario parameter should normally create a new scenario version.

---

## 12. Dependency Graph Reproducibility

Because dependency modeling is central to RESOLVE, dependency graphs require explicit provenance.

A graph version should identify:

* graph identifier;
* graph version;
* source datasets;
* construction method;
* dependency types;
* dependency confidence;
* thresholds;
* edge weights;
* geographic rules;
* filtering rules;
* graph statistics.

Conceptually:

```text
Source Data
    ↓
Dependency Extraction
    ↓
Validation
    ↓
Graph Construction
    ↓
Graph Version
```

A dependency graph MUST NOT be treated as an unexplained static artifact.

---

## 13. Intervention Reproducibility

Every intervention used in a research experiment must have a reproducible definition.

Record:

* intervention identifier;
* target entities;
* intervention type;
* cost;
* expected effect;
* implementation assumptions;
* recovery effect;
* geographic scope;
* configuration version.

If intervention effectiveness is modeled rather than directly observed, this MUST be identified as a modeling assumption.

---

## 14. Randomness

Any stochastic process MUST record its random seed.

Example:

```text
random_seed: 20261008
```

If multiple random generators are used, the system should document how seeds are propagated.

For example:

```text
Experiment Seed
      ↓
Scenario Generator
      ↓
Simulation
      ↓
Sampling
```

Randomness MUST NOT be silently introduced into a deterministic research experiment.

---

## 15. Determinism

Where possible, the simulation engine should produce identical results for identical:

```text
Code
+
Data
+
Configuration
+
Seed
+
Environment
```

Deterministic tests should verify this property.

Example conceptual test:

```text
Run A
  ↓
Result A

Run B
  ↓
Result B

A == B
```

If exact equality is not appropriate because of floating-point or parallel execution effects, acceptable numerical tolerances must be defined.

---

## 16. Environment Specification

Research execution should record the software environment.

At minimum:

* operating system;
* Python version;
* package manager;
* dependency versions;
* database version;
* PostGIS version where applicable;
* simulation libraries;
* relevant system libraries.

Recommended environment artifacts include:

```text
pyproject.toml
uv.lock
Dockerfile
docker-compose configuration
environment manifest
```

The exact environment strategy may evolve as implementation progresses.

---

## 17. Dependency Locking

Research-critical Python dependencies should be locked to reproducible versions.

Unbounded requirements such as:

```text
package>=1.0
```

may allow future environments to change behavior.

The project should prefer a lockfile or equivalent mechanism for primary experiment environments.

Dependency updates should be treated as potentially research-relevant changes.

---

## 18. Containerized Reproduction

Containerization may be used when it materially improves reproducibility.

A container SHOULD capture:

* base operating system;
* Python version;
* system dependencies;
* application dependencies;
* database/client requirements;
* execution scripts.

Containers are not mandatory for every local development task.

They become more valuable for:

* primary experiments;
* CI;
* paper result generation;
* independent reproduction;
* deployment.

---

## 19. Database Reproducibility

Database-backed experiments must identify:

* database schema version;
* migration version;
* dataset version;
* relevant seed data;
* database engine version;
* PostGIS version when applicable.

The database schema should be reconstructed using migrations rather than undocumented manual changes.

Example:

```text
Database
    ↓
Alembic Migration Version
    ↓
Dataset Load Version
    ↓
Experiment
```

---

## 20. Configuration Reproducibility

Research configuration must be version controlled when possible.

Configuration should define:

* scenario;
* model parameters;
* intervention parameters;
* budget;
* simulation duration;
* metrics;
* random seed;
* execution options.

Secrets MUST NOT be stored in experiment configuration.

Environment-specific secrets should remain outside version-controlled research artifacts.

---

## 21. Execution Command

Every primary experiment should have a documented execution procedure.

Conceptually:

```text
Prepare Environment
        ↓
Validate Dataset
        ↓
Validate Configuration
        ↓
Run Experiment
        ↓
Validate Results
        ↓
Generate Evidence
```

The exact commands will be defined once the research engine is implemented.

The command should not depend on undocumented interactive steps.

---

## 22. Reproduction Validation

A reproduction attempt should compare the original and reproduced outputs.

Candidate comparisons include:

* primary metric;
* secondary metrics;
* intervention ranking;
* selected interventions;
* cascade size;
* cascade depth;
* affected population;
* affected facilities;
* runtime;
* result hashes.

For numerical outputs, acceptable tolerances should be defined before declaring reproduction successful.

---

## 23. Reproducibility Tolerances

Exact equality is not always required.

Potential tolerance types include:

### Exact

Used for:

* identifiers;
* categorical states;
* intervention selections;
* deterministic graph structure.

### Absolute tolerance

Used when:

```text
|x_original - x_reproduced| ≤ ε
```

### Relative tolerance

Used when:

```text
|x_original - x_reproduced|
/
|x_original|
≤ ε
```

The appropriate tolerance must depend on the metric and numerical behavior.

---

## 24. Reproduction Status

Each important experiment may be assigned a reproduction status:

```text
NOT_ATTEMPTED
ATTEMPTED
REPRODUCED
PARTIALLY_REPRODUCED
FAILED
BLOCKED
```

A reproduction attempt should document why it failed or was blocked.

Examples:

* unavailable licensed dataset;
* changed external source;
* unavailable software version;
* hardware-specific behavior;
* missing historical data;
* undocumented original configuration.

Failure to reproduce should be treated as evidence about the research process, not hidden.

---

## 25. External Data Sources

External data sources introduce reproducibility risk because the source may change after acquisition.

For each external source, record:

* source name;
* source location;
* access date;
* dataset version where available;
* acquisition method;
* license;
* checksum where possible;
* local archival status.

The system should prefer preserved copies or legally shareable derived artifacts when permitted.

---

## 26. Dynamic External APIs

Live APIs SHOULD NOT be used directly as the sole source for a reproducible primary experiment.

Preferred approach:

```text
External API
    ↓
Acquisition
    ↓
Versioned Snapshot
    ↓
Validation
    ↓
Research Dataset
    ↓
Experiment
```

This prevents future API changes from silently altering research results.

---

## 27. Time-Dependent Data

For temporal datasets, record:

* observation period;
* acquisition timestamp;
* timezone;
* temporal resolution;
* interpolation method;
* aggregation method;
* missing periods.

Temporal transformations must be reproducible.

For example, converting hourly observations into daily values requires a documented aggregation rule.

---

## 28. Spatial Reproducibility

Spatial processing must record:

* coordinate reference system;
* spatial resolution;
* geographic boundary;
* spatial join rules;
* buffering rules;
* distance calculations;
* raster resampling;
* geometry simplification;
* clipping rules.

Spatial operations can materially change research outcomes and therefore cannot remain undocumented.

---

## 29. Hardware Metadata

Primary experiments should record relevant hardware information.

Potential metadata:

* CPU;
* GPU where used;
* RAM;
* storage;
* operating system;
* parallel worker count.

Hardware metadata is particularly important for:

* performance experiments;
* large simulations;
* GPU-based processing;
* parallel execution.

Hardware differences should not be treated as scientific differences unless specifically studied.

---

## 30. Execution Metadata

A run should capture:

```text
run_id
experiment_id
timestamp
host
OS
Git commit
dataset versions
scenario version
graph version
configuration version
seed
Python version
dependency versions
database version
runtime
status
```

This metadata should be generated automatically where practical.

---

## 31. Result Immutability

Completed primary research results should not be silently overwritten.

Preferred model:

```text
RUN-0001 → Result Version 1
RUN-0002 → Result Version 2
RUN-0003 → Result Version 3
```

If an experiment is rerun after a meaningful configuration change, it should produce a new run identifier.

Corrections to derived reports should preserve the underlying original evidence when practical.

---

## 32. Result Hashing

Important result artifacts may use checksums or hashes.

Example:

```text
result.json
    ↓
SHA-256
    ↓
recorded result_hash
```

This can help verify that evidence files have not changed after execution.

---

## 33. Reproducibility Manifest

Each major experiment should eventually generate a reproducibility manifest.

Conceptual structure:

```yaml
experiment_id: EXP-001
run_id: RUN-0001

code:
  git_commit: ...

datasets:
  - id: DATASET-001
    version: v1.0

scenario:
  id: SCN-001
  version: v1.0

dependency_graph:
  id: GRAPH-001
  version: v1.0

interventions:
  version: v1.0

configuration:
  version: v1.0

environment:
  python: ...
  database: ...

random_seed: ...

results:
  primary_metric: ...
  result_hash: ...
```

The actual schema may evolve during implementation.

---

## 34. Reproducibility Package

For major research results, the project should aim to produce a reproducibility package containing:

```text
reproduction/
├── README
├── environment
├── configuration
├── manifests
├── scripts
├── experiment metadata
├── expected outputs
└── validation instructions
```

Large datasets may be referenced rather than included when licensing or storage constraints prevent redistribution.

---

## 35. Independent Reproduction

Where feasible, at least one important experiment should be independently reproduced.

The independent reproduction should ideally be performed by:

* another project contributor;
* a separate environment;
* a fresh checkout;
* or another researcher.

The goal is to identify assumptions that are obvious to the original developer but invisible to another researcher.

---

## 36. CI Reproducibility Checks

Continuous integration MAY verify:

* deterministic unit tests;
* migration reproducibility;
* configuration validation;
* schema consistency;
* experiment configuration validity;
* small deterministic simulation outputs.

Full research experiments should not automatically run in CI if they are computationally expensive.

A smaller research smoke test may be used instead.

---

## 37. Reproducibility and Git

Git should provide the primary code-version reference.

The recommended chain is:

```text
Experiment
    ↓
Git Commit
    ↓
Repository State
```

Before a major experiment:

1. validate tests;
2. verify repository state;
3. commit research-relevant changes;
4. record commit SHA;
5. execute experiment;
6. preserve results;
7. document evidence.

---

## 38. Reproducibility and the Paper

Every quantitative paper result should be traceable to an experiment.

The paper should be able to answer:

* Which experiment produced this number?
* Which dataset version was used?
* Which method version was used?
* Which scenario was used?
* Which configuration was used?
* Can another researcher reproduce it?
* If not, why not?

The paper MUST NOT imply stronger reproducibility than the project actually provides.

---

## 39. Reproducibility Limitations

Exact reproduction may be impossible because of:

* restricted datasets;
* discontinued sources;
* changing external APIs;
* unavailable historical observations;
* proprietary software;
* hardware-specific computation;
* nondeterministic numerical libraries;
* unavailable infrastructure;
* licensing restrictions.

When exact reproduction is impossible, the project should provide the closest legally and technically reproducible alternative.

For example:

```text
Original Dataset
      ↓
Unavailable

Shareable Derived Dataset
      ↓
Reproduction
```

The difference must be documented.

---

## 40. Reproducibility Checklist

Before declaring a primary experiment reproducible:

### Code

* [ ] Git commit recorded.
* [ ] Repository state documented.
* [ ] Dependency versions recorded.
* [ ] Lockfile available.

### Data

* [ ] Dataset identifiers recorded.
* [ ] Dataset versions recorded.
* [ ] Provenance recorded.
* [ ] Processing version recorded.
* [ ] Checksums recorded where practical.

### Model

* [ ] Scenario version recorded.
* [ ] Dependency graph version recorded.
* [ ] Intervention version recorded.
* [ ] Model parameters recorded.

### Execution

* [ ] Configuration recorded.
* [ ] Random seed recorded.
* [ ] Environment recorded.
* [ ] Execution command documented.
* [ ] Hardware metadata recorded where relevant.

### Results

* [ ] Output validation performed.
* [ ] Result artifact preserved.
* [ ] Result hash recorded where practical.
* [ ] Metrics calculated using frozen definitions.
* [ ] Evidence linked to experiment.

### Reproduction

* [ ] Reproduction procedure tested.
* [ ] Output comparison performed.
* [ ] Tolerances defined.
* [ ] Differences documented.
* [ ] Reproduction status assigned.

---

## 41. Definition of Done

The reproducibility framework is considered sufficiently implemented when:

* [ ] primary experiments have stable identifiers;
* [ ] code versions are recorded;
* [ ] datasets are versioned;
* [ ] preprocessing is reproducible;
* [ ] scenarios are versioned;
* [ ] dependency graphs are versioned;
* [ ] interventions are versioned;
* [ ] configurations are preserved;
* [ ] random seeds are controlled where necessary;
* [ ] environments are documented;
* [ ] experiment execution is scriptable;
* [ ] results are preserved;
* [ ] evidence can be traced to experiment runs;
* [ ] at least one meaningful reproduction workflow has been tested;
* [ ] known reproduction limitations are documented.

---

## 42. Guiding Principle

> A research result is reproducible when another researcher can determine exactly what was executed, with which inputs, under which conditions, using which code, and can independently obtain equivalent evidence.

For RESOLVE, reproducibility is part of the scientific contribution infrastructure.

The goal is not merely to make the software run again.

The goal is to make the **research process itself traceable, inspectable, repeatable, and honest about what cannot be reproduced**.
