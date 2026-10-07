# RESOLVE — Data Architecture

## 1. Purpose

This document defines how RESOLVE represents, ingests, validates, transforms, stores, versions, and serves data used by the urban resilience model and research experiments.

The data architecture must support two goals simultaneously:

1. reliable software operation;
2. reproducible scientific experimentation.

The architecture therefore treats data provenance, versioning, validation, uncertainty, and spatial consistency as first-class concerns.

---

# 2. Data Architecture Principles

RESOLVE follows these principles:

1. **Data is evidence** — datasets used for research must be traceable to their sources.
2. **Raw data is preserved** — source data should remain distinguishable from processed data.
3. **Transformations are explicit** — preprocessing must be documented.
4. **Schemas are explicit** — data should conform to defined structures.
5. **Spatial consistency matters** — coordinate systems and geographic transformations must be documented.
6. **Temporal consistency matters** — datasets must not be combined across incompatible time periods without justification.
7. **Uncertainty is represented** — missing or uncertain information must not silently become false precision.
8. **Experiments are reproducible** — datasets used by experiments must be identifiable by version or snapshot.
9. **Research and operational data are separated conceptually** — experimental artifacts should not be confused with authoritative real-world operational systems.
10. **Minimum necessary data** — personal or sensitive information is avoided unless explicitly justified.

---

# 3. High-Level Data Flow

The conceptual data pipeline is:

```text
External Sources
      │
      ▼
Raw Data
      │
      ▼
Ingestion
      │
      ▼
Validation
      │
      ▼
Normalization
      │
      ▼
Spatial / Temporal Processing
      │
      ▼
Canonical Data Model
      │
      ├───────────────┐
      ▼               ▼
Urban Model       Research Dataset
      │               │
      └───────┬───────┘
              ▼
       Scenario Builder
              │
              ▼
          Simulation
              │
              ▼
        Experiment Results
              │
              ▼
       Research Evidence
```

---

# 4. Data Categories

RESOLVE data is divided into several conceptual categories.

## 4.1 Reference Data

Stable information describing the urban environment.

Examples:

* administrative boundaries
* geographic areas
* infrastructure inventories
* facility locations
* road networks
* population aggregates

---

## 4.2 Hazard Data

Information describing hazards or disruptive conditions.

Examples:

* temperature
* precipitation
* flood extent
* outage events
* hazard intensity
* historical incidents

---

## 4.3 Dependency Data

Information describing relationships between urban entities.

Examples:

* electricity → water
* roads → emergency access
* water → hospital operations
* communication → emergency services

Dependency information may be directly sourced, inferred, modeled, or manually defined.

The origin of each dependency must be identifiable.

---

## 4.4 Intervention Data

Information describing potential resilience interventions.

Examples:

* intervention type
* target component
* cost
* expected effect
* implementation assumptions
* capacity

---

## 4.5 Simulation Data

Generated state and event information from simulation execution.

Examples:

* entity states
* dependency activations
* cascade events
* recovery states
* simulation metrics

---

## 4.6 Research Data

Experiment-level data used to evaluate hypotheses.

Examples:

* experiment configurations
* baseline results
* proposed-method results
* repeated-run outputs
* statistical summaries
* sensitivity-analysis outputs

---

# 5. Data Lifecycle

Every research dataset should conceptually pass through the following lifecycle:

```text
Discovered
   ↓
Acquired
   ↓
Recorded
   ↓
Validated
   ↓
Normalized
   ↓
Transformed
   ↓
Versioned
   ↓
Used in Experiment
   ↓
Archived / Reproducibility Artifact
```

A dataset should not be considered experiment-ready merely because it can be loaded successfully.

---

# 6. Raw Data Layer

The raw data layer contains data as obtained from its original source, subject to licensing and storage constraints.

Raw data should generally be:

* immutable,
* identifiable,
* source-linked,
* versioned where possible,
* accompanied by acquisition metadata.

No destructive preprocessing should overwrite the original representation.

If licensing prevents redistribution, the repository should store metadata and reproducible acquisition instructions rather than restricted data.

---

# 7. Staging Layer

The staging layer contains data undergoing validation and normalization.

Typical operations include:

* schema validation
* field normalization
* type conversion
* duplicate detection
* coordinate validation
* unit normalization
* missing-value analysis
* temporal normalization

Staging data may be regenerated from raw data.

---

# 8. Canonical Data Layer

The canonical layer provides the standardized representation consumed by application and research components.

It should represent concepts consistently regardless of the original source format.

Examples:

```text
Source Dataset A ─┐
                  ├──> Canonical Urban Entity
Source Dataset B ─┘
```

The canonical layer prevents application logic from becoming tightly coupled to individual external datasets.

---

# 9. Canonical Entity Representation

A canonical urban entity should conceptually contain:

```text id="jtxq8f"
entity_id
entity_type
name
geometry
location
attributes
status
source_reference
confidence
created_at
updated_at
```

The exact physical schema belongs to the implementation phase.

---

# 10. Spatial Data Architecture

Spatial information is central to urban resilience analysis.

The architecture should support:

* points
* lines
* polygons
* geographic boundaries
* spatial relationships
* distance calculations
* intersections
* containment
* proximity queries

---

## 10.1 Coordinate Reference System

Every spatial dataset shall have a documented coordinate reference system.

Transformations between coordinate systems must be explicit.

The canonical storage CRS should be selected during implementation based on:

* accuracy requirements
* spatial database support
* geographic scope
* interoperability
* computational requirements

The decision shall be recorded in `docs/DECISION_LOG.md`.

---

## 10.2 Spatial Relationships

Spatial relationships may include:

```text
CONTAINS
WITHIN
INTERSECTS
NEAR
ADJACENT_TO
DISTANCE_FROM
SERVES
```

Spatial relationships must not automatically be interpreted as functional dependencies.

---

# 11. Temporal Data Architecture

Temporal information should be represented consistently.

Potential temporal attributes include:

```text
timestamp
start_time
end_time
duration
time_step
observation_period
```

The architecture should distinguish:

* observation time,
* event time,
* simulation time,
* acquisition time.

---

# 12. Units and Measurement Standards

Measurements must include or inherit a documented unit.

Examples:

```text
temperature → °C
distance → meters / kilometers
population → persons
duration → seconds / minutes / hours
cost → explicitly defined currency/unit
```

Unit conversion must occur in controlled preprocessing rather than silently inside unrelated business logic.

---

# 13. Data Quality Framework

Data validation should operate at multiple levels.

## 13.1 Structural Validation

Checks:

* required fields
* valid types
* valid schema
* unique identifiers
* valid relationships

---

## 13.2 Spatial Validation

Checks:

* valid geometries
* valid coordinates
* CRS consistency
* impossible geographic positions
* invalid geometry relationships

---

## 13.3 Temporal Validation

Checks:

* valid timestamps
* valid ranges
* temporal ordering
* incompatible periods
* duplicate observations

---

## 13.4 Semantic Validation

Checks:

* valid entity types
* valid dependency types
* meaningful status values
* plausible measurement ranges
* valid intervention definitions

---

# 14. Missing Data

Missing data must be explicitly represented.

Possible states include:

```text
UNKNOWN
NOT_AVAILABLE
NOT_APPLICABLE
WITHHELD
ESTIMATED
```

The implementation should avoid collapsing all missing conditions into a single null value when the distinction affects interpretation.

---

# 15. Uncertainty

Uncertainty may originate from:

* incomplete datasets
* inferred dependencies
* estimated infrastructure capacity
* uncertain hazard intensity
* uncertain intervention effectiveness
* incomplete geographic coverage
* measurement error

Where practical, uncertainty should be represented through:

* confidence values
* ranges
* distributions
* scenario variants
* sensitivity-analysis parameters

A modeled assumption must not be presented as an observed fact.

---

# 16. Data Provenance

Each research-relevant dataset should have provenance metadata.

Minimum conceptual provenance:

```text id="b0x4pv"
source_name
dataset_name
source_location
version
acquisition_date
license
geographic_coverage
temporal_coverage
processing_version
limitations
```

Where applicable, provenance should also identify:

* preprocessing code version
* transformation parameters
* filtering rules
* derived variables
* manual corrections

---

# 17. Dataset Versioning

A dataset used in a primary experiment must be uniquely identifiable.

Possible version identifiers include:

```text
dataset-v1
dataset-v2
snapshot-2026-XX-XX
content-hash
Git-tracked manifest
```

The exact strategy will be selected during implementation.

The important requirement is that a future researcher can determine which dataset state produced a result.

---

# 18. Data Manifests

Research datasets should have a machine-readable manifest.

Conceptually:

```text id="a8x74h"
dataset:
  id
  version
  source
  acquired_at
  license

coverage:
  geographic
  temporal

files:
  path
  format
  checksum

processing:
  pipeline_version
  transformations

quality:
  missing_data
  validation_status

limitations:
  notes
```

The exact format may be YAML, JSON, or another documented machine-readable representation.

---

# 19. Data Storage Architecture

The system may use multiple storage mechanisms for different workloads.

Conceptually:

```text
                    ┌──────────────────┐
                    │ PostgreSQL/PostGIS│
                    │ Canonical Data    │
                    └─────────┬────────┘
                              │
             ┌────────────────┼────────────────┐
             ▼                ▼                ▼
        Urban Model      Dependencies      Scenarios

Raw / Derived Files
        │
        ▼
 Data / Research Storage

Simulation Results
        │
        ▼
 Experiment Artifacts
```

The final storage implementation should be selected based on actual workload requirements rather than assumed complexity.

---

# 20. Database Responsibilities

The primary relational database should be responsible for structured canonical data such as:

* urban entities
* geographic metadata
* dependencies
* scenarios
* interventions
* experiment metadata
* structured results

Spatial data should use spatial database capabilities where required.

Large immutable datasets or generated artifacts may remain file-based when that is more appropriate.

---

# 21. File-Based Data Responsibilities

File-based storage may be preferable for:

* raw datasets
* large geospatial files
* raster data
* experiment exports
* model artifacts
* figures
* reproducibility packages

The project should avoid unnecessarily importing large immutable datasets into relational tables.

---

# 22. Simulation Data

Simulation outputs should distinguish between:

### Run Metadata

```text
simulation_id
scenario_id
configuration
seed
software_version
start_time
duration
```

### State Data

```text
simulation_time
entity_id
state
severity
```

### Event Data

```text
event_id
source
target
trigger
time_step
severity
```

### Outcome Data

```text
metric
value
unit
```

This separation supports both detailed debugging and efficient research aggregation.

---

# 23. Experiment Data

An experiment should reference, rather than duplicate unnecessarily:

* dataset version
* scenario version
* method version
* software commit
* configuration
* seeds

The experiment result should therefore be traceable through identifiers.

Conceptually:

```text
Experiment
   │
   ├── Dataset Version
   ├── Scenario Version
   ├── Code Commit
   ├── Configuration
   └── Simulation Runs
             │
             ▼
         Outcomes
```

---

# 24. Data Processing Pipeline

The initial processing pipeline should follow:

```text
Acquire
  ↓
Register Source
  ↓
Validate
  ↓
Normalize
  ↓
Transform
  ↓
Quality Check
  ↓
Store Canonical Representation
  ↓
Create Dataset Manifest
  ↓
Make Available to Experiments
```

Each transformation should be reproducible.

---

# 25. Dependency Graph Data

The dependency graph is a central research artifact.

Conceptually:

```text
Nodes
  ├── entity_id
  ├── entity_type
  └── attributes

Edges
  ├── source_entity_id
  ├── dependent_entity_id
  ├── dependency_type
  ├── strength
  ├── threshold
  └── confidence
```

The graph representation must support:

* directed relationships
* traversal
* dependency analysis
* cascade propagation
* graph metrics
* subgraph extraction

---

# 26. Graph Construction

Dependency graphs may be constructed from:

1. directly observed relationships,
2. authoritative infrastructure relationships,
3. spatial inference,
4. domain rules,
5. modeled assumptions,
6. synthetic relationships for controlled experiments.

Every dependency should have enough metadata to distinguish its origin where practical.

---

# 27. Graph Completeness

A dependency graph is unlikely to represent every real-world relationship.

Therefore the system shall distinguish between:

```text
Known Dependency
Modeled Dependency
Inferred Dependency
Unknown Dependency
```

Missing edges must not automatically be interpreted as proof that no dependency exists.

Graph completeness should be treated as an experimental limitation and, where feasible, evaluated through sensitivity analysis.

---

# 28. Derived Data

Derived data includes values calculated from other datasets.

Examples:

* exposure scores
* service areas
* population exposure
* dependency strength estimates
* hazard intersections
* graph metrics
* resilience scores

Derived data should identify:

* source datasets
* transformation method
* processing version
* assumptions

---

# 29. Data Lineage

The system should support a lineage chain such as:

```text
Original Source
      ↓
Raw Dataset
      ↓
Processed Dataset
      ↓
Canonical Entity
      ↓
Scenario
      ↓
Simulation
      ↓
Outcome
      ↓
Research Result
      ↓
Paper Figure / Table
```

This lineage is essential for research traceability.

---

# 30. Data Access Boundaries

Different components should access data according to their responsibilities.

```text
API
 │
 ▼
Application Services
 │
 ▼
Domain / Query Interfaces
 │
 ▼
Repositories
 │
 ▼
Database / Files
```

Domain logic should not depend directly on raw file formats or external APIs.

---

# 31. External Data Sources

External sources may be introduced incrementally.

Potential categories include:

* public geospatial datasets
* weather datasets
* satellite products
* population datasets
* infrastructure datasets
* historical disaster datasets
* transportation datasets

Before using a dataset in a primary experiment, the project must document:

* source
* licensing
* coverage
* quality
* limitations
* processing
* reproducibility strategy

Dataset selection belongs in `research/DATASETS.md`.

---

# 32. Data Licensing

Every externally sourced dataset must have a documented licensing status.

The repository should not redistribute restricted datasets without permission.

For restricted sources, the project should prefer:

* source metadata
* acquisition instructions
* preprocessing scripts
* dataset manifests
* checksums where permitted

---

# 33. Privacy

RESOLVE should prioritize aggregate and non-personal data.

The initial architecture does not require:

* individual names
* phone numbers
* personal addresses
* personal identifiers
* individual health records

If future research requires sensitive data, privacy and governance requirements must be addressed before implementation.

---

# 34. Data Security

Data security requirements include:

* secrets excluded from datasets and source control,
* access controlled where necessary,
* restricted datasets stored separately,
* sensitive logs avoided,
* credentials never embedded in data files,
* backups handled appropriately where applicable.

---

# 35. Data Retention

Research artifacts should be retained sufficiently to reproduce published or reported results.

At minimum, primary experiments should preserve:

* experiment configuration
* dataset identifiers
* scenario identifiers
* code version
* random seeds
* metric definitions
* result artifacts

---

# 36. Data Quality Gates

Before data enters a primary experiment, it should pass appropriate quality gates:

```text
Source Registered
      ↓
Schema Valid
      ↓
Spatial Valid
      ↓
Temporal Valid
      ↓
Semantic Valid
      ↓
Provenance Recorded
      ↓
Limitations Documented
      ↓
Experiment Approved
```

A dataset failing a quality gate may still be useful for exploratory analysis, but the limitation must be explicitly documented.

---

# 37. Research Dataset Freeze

Before the primary evaluation:

1. selected datasets shall be identified;
2. versions or snapshots shall be recorded;
3. preprocessing pipelines shall be frozen;
4. quality limitations shall be documented;
5. dataset manifests shall be generated;
6. changes after the freeze shall be versioned.

This prevents accidental dataset changes from invalidating comparisons.

---

# 38. Data and Model Separation

RESOLVE shall distinguish between:

### Observed Data

Information obtained from a source.

### Derived Data

Information calculated from observed data.

### Modeled Data

Information generated using domain assumptions or computational models.

### Simulated Data

Information generated by the simulation itself.

These categories must not be conflated in research reporting.

---

# 39. Synthetic Data

Synthetic data may be used when:

* validating system behavior,
* testing edge cases,
* developing simulation rules,
* evaluating scalability,
* conducting controlled experiments.

Synthetic data shall be clearly labeled and shall not be presented as real-world evidence.

---

# 40. Data Architecture and Reproducibility

The data architecture must support the following chain:

```text
Dataset Version
      +
Processing Version
      +
Scenario Version
      +
Code Commit
      +
Configuration
      +
Random Seed
      ↓
Reproducible Experiment
```

A result that cannot be traced through this chain should not be treated as a fully reproducible primary result.

---

# 41. Initial Data Architecture Decision

The initial architecture will favor:

* PostgreSQL/PostGIS for structured canonical urban and spatial data,
* version-controlled metadata and manifests,
* file-based storage for suitable raw and large derived datasets,
* explicit preprocessing pipelines,
* reproducible scenario definitions,
* structured experiment artifacts.

Additional infrastructure such as object storage, data lakes, streaming systems, or specialized graph databases shall only be introduced if measured requirements justify them.

---

# 42. Initial Acceptance Criteria

The data architecture will be considered ready for implementation when it can:

1. register source datasets,
2. preserve provenance,
3. validate incoming data,
4. represent canonical urban entities,
5. represent spatial information,
6. represent dependency relationships,
7. represent hazards,
8. represent scenarios,
9. version research datasets,
10. distinguish observed, derived, modeled, and simulated data,
11. support reproducible experiment inputs,
12. preserve sufficient lineage for research results,
13. document data limitations,
14. prevent restricted or sensitive data from being unintentionally exposed.

---

# 43. Guiding Principle

RESOLVE should treat data as part of the research methodology, not merely as application input.

The fundamental data chain is:

```text
Source
  ↓
Evidence
  ↓
Validated Data
  ↓
Urban Model
  ↓
Scenario
  ↓
Simulation
  ↓
Measurement
  ↓
Research Evidence
```

If the origin, transformation, or meaning of a value cannot be explained, its role in a research claim must be treated with caution.
