# RESOLVE — Research Datasets

## 1. Purpose

This document defines the dataset strategy for RESOLVE — Resilient Urban Systems & Vulnerability Observatory.

The purpose is to establish a reproducible and research-oriented approach for selecting, acquiring, validating, transforming, versioning, and evaluating datasets used by RESOLVE.

The dataset strategy must support the research question:

> Can a dependency-aware urban digital twin improve the prioritization of resilience interventions compared with isolated risk assessment?

Datasets are therefore treated as research inputs rather than merely application resources.

---

# 2. Dataset Principles

RESOLVE follows these principles:

1. Data must have identifiable provenance.
2. Raw source data should be preserved where licensing permits.
3. Transformations must be documented.
4. Dataset versions must be identifiable.
5. Spatial reference systems must be explicit.
6. Temporal coverage must be explicit.
7. Missing and uncertain data must be represented honestly.
8. Synthetic data must be clearly distinguished from real-world data.
9. Dataset limitations must be documented before primary evaluation.
10. Data should be sufficient for the research question without unnecessary collection.
11. Licensing and usage restrictions must be respected.
12. The same dataset conditions should be used across comparable experimental methods.

---

# 3. Dataset Role in the Research

The primary research comparison requires data that can support:

* urban entity representation,
* geographic relationships,
* infrastructure representation,
* critical facilities,
* population exposure,
* hazards,
* dependencies,
* interventions,
* scenario construction,
* outcome measurement.

The conceptual research pipeline is:

```text
External Sources
      ↓
Raw Data
      ↓
Validation
      ↓
Normalization
      ↓
Canonical Data
      ↓
Urban System Model
      ↓
Scenario
      ↓
Baseline / Proposed Method
      ↓
Simulation
      ↓
Metrics
      ↓
Research Evidence
```

---

# 4. Dataset Categories

RESOLVE organizes datasets into the following categories:

| Category            | Purpose                                                      |
| ------------------- | ------------------------------------------------------------ |
| Urban Reference     | Geographic and administrative context                        |
| Infrastructure      | Representation of urban systems                              |
| Critical Facilities | Hospitals, schools, emergency and other important facilities |
| Population          | Exposure and vulnerability estimation                        |
| Hazard              | Heat, flooding, outages, or other hazards                    |
| Transportation      | Roads and accessibility                                      |
| Dependency          | Functional relationships between systems                     |
| Historical Incident | Historical disruption evidence where available               |
| Intervention        | Candidate resilience actions                                 |
| Environmental       | Weather, land cover, elevation, or related context           |
| Synthetic           | Controlled development and experimental datasets             |
| Derived             | Data generated through documented transformations            |

Not every category is required for every experiment.

---

# 5. Initial Dataset Strategy

The project should begin with a minimal dataset set sufficient to validate the architecture and research methodology.

The initial development dataset should prioritize:

1. Geographic boundaries
2. Urban entities
3. Critical facilities
4. Transportation or accessibility data
5. Population-related data
6. At least one hazard dataset
7. Dependency relationships
8. Synthetic intervention scenarios

Additional datasets should be added only when they improve the research experiment or satisfy a documented requirement.

---

# 6. Dataset Selection Criteria

A dataset should be evaluated against:

### Relevance

Does it directly support the research question or a defined requirement?

### Coverage

Does it cover the intended geographic and temporal scope?

### Quality

Are the values sufficiently reliable for the intended use?

### Resolution

Is the spatial and temporal resolution appropriate?

### Provenance

Can the source and transformation history be established?

### Accessibility

Can the dataset be obtained reproducibly?

### Licensing

Does the license permit the intended use?

### Stability

Is the source reasonably stable or versionable?

### Reproducibility

Can another researcher obtain or reconstruct the same dataset?

### Privacy

Does the dataset contain unnecessary personal or sensitive information?

---

# 7. Dataset Registry

Every research-relevant dataset should eventually have a registry entry.

The conceptual registry fields are:

```text id="1t0gqj"
dataset_id
dataset_name
category
source
publisher
source_url
license
version
acquisition_date
geographic_scope
temporal_scope
spatial_resolution
temporal_resolution
coordinate_reference_system
format
record_count
quality_status
missing_data_summary
transformation_version
provenance
limitations
intended_use
research_experiments
```

The registry may initially be maintained as structured documentation before being implemented in the database.

---

# 8. Dataset Identifier

Each dataset should receive a stable identifier.

Example:

```text id="nqaxhy"
DATASET-URBAN-001
DATASET-FACILITY-001
DATASET-POP-001
DATASET-HAZARD-001
```

Identifiers should remain stable while versions change.

For example:

```text id="7st1zq"
DATASET-FACILITY-001
    v1
    v2
    v3
```

This distinguishes the dataset identity from a particular dataset version.

---

# 9. Dataset Versioning

Dataset versions must be explicit.

A version may change because of:

* source updates,
* corrected records,
* new geographic coverage,
* changed preprocessing,
* changed normalization,
* changed spatial resolution,
* changed temporal coverage.

A changed dataset should not silently replace an earlier version used in a completed experiment.

Experiments should reference the exact dataset version used.

---

# 10. Raw Data

Where licensing permits, original source data should be preserved.

The raw dataset should remain distinct from processed data.

Conceptually:

```text id="kq2c0e"
Raw Dataset
     ↓
Staging
     ↓
Validation
     ↓
Normalization
     ↓
Canonical Dataset
```

Raw data should not be modified in place.

If raw data cannot legally or practically be stored in the repository, metadata should preserve enough information to identify and reconstruct the source.

---

# 11. Data Acquisition

Data acquisition should record:

* source,
* retrieval date,
* retrieval method,
* source version,
* file or API identifier where available,
* license,
* checksum where appropriate,
* geographic scope,
* temporal scope.

Automated acquisition should be preferred when stable and legally permitted.

Manual acquisition should be documented when automation is not practical.

---

# 12. Geographic Scope

The initial case study may focus on Karachi.

However, the dataset architecture should remain city-agnostic.

A dataset should explicitly identify:

* city,
* administrative region,
* country,
* geographic bounding area,
* coordinate reference system.

Karachi-specific datasets must not cause Karachi-specific assumptions to leak into the core domain model.

---

# 13. Spatial Reference Systems

Every spatial dataset must identify its coordinate reference system.

The system should distinguish between:

* source CRS,
* canonical CRS,
* display CRS where applicable.

Transformations must be documented.

Spatial calculations must use an appropriate projected or geographic coordinate system depending on the operation.

For example, distance calculations should not blindly treat longitude/latitude degrees as linear distance.

---

# 14. Spatial Resolution

Dataset resolution must be recorded.

Examples include:

* individual facility points,
* road segments,
* administrative polygons,
* census areas,
* raster cells,
* satellite pixels.

Resolution affects interpretation.

A coarse population dataset cannot support precise individual-level population claims.

Similarly, a coarse hazard raster cannot establish fine-scale exposure without additional assumptions.

---

# 15. Temporal Coverage

Each dataset should document:

* start date,
* end date,
* update frequency,
* timestamp format,
* timezone where relevant,
* temporal resolution.

Temporal mismatches should be identified.

For example:

```text id="2b7f0w"
Population data: 2024
Infrastructure data: 2025
Hazard data: 2026
```

Such differences must be documented rather than hidden.

---

# 16. Urban Reference Data

Urban reference data may include:

* administrative boundaries,
* neighborhoods,
* geographic areas,
* land-use information,
* elevation,
* water bodies,
* urban extent.

Purpose:

* define geographic context,
* support spatial relationships,
* constrain scenario areas,
* support visualization.

Reference data should not automatically be interpreted as resilience outcomes.

---

# 17. Infrastructure Data

Infrastructure data may represent:

* electricity infrastructure,
* water infrastructure,
* telecommunications,
* transport,
* waste systems,
* other relevant urban services.

Possible attributes include:

* identifier,
* type,
* location,
* service area,
* capacity where available,
* operational state,
* source,
* confidence.

Unknown attributes should remain unknown rather than being fabricated.

---

# 18. Critical Facility Data

Potential critical facilities include:

* hospitals,
* clinics,
* schools,
* emergency facilities,
* shelters,
* water facilities,
* electricity facilities,
* transportation hubs.

Relevant attributes may include:

* facility ID,
* facility type,
* location,
* service capacity,
* operating state,
* geographic area,
* source,
* confidence.

Facility categories should be defined consistently across experiments.

---

# 19. Population Data

Population data may be used to estimate exposure and potential impact.

Potential representations include:

* population counts,
* population density,
* geographic population groups,
* demographic aggregates where legally and ethically appropriate.

The project should prefer aggregated population data.

Individual-level personal data is outside the intended research scope.

Population estimates must clearly distinguish:

* observed population,
* estimated population,
* modeled population.

---

# 20. Hazard Data

Hazard datasets may include:

* extreme heat,
* flooding,
* heavy precipitation,
* storms,
* power disruption,
* water disruption,
* other relevant hazards.

Each hazard dataset should define:

* hazard type,
* intensity representation,
* spatial extent,
* temporal extent,
* severity scale,
* source,
* uncertainty.

A hazard observation should not automatically be interpreted as an infrastructure failure.

The relationship between hazard intensity and system state must be explicitly modeled.

---

# 21. Historical Incident Data

Historical incident data may include:

* outages,
* flooding,
* infrastructure failures,
* extreme weather events,
* service disruptions,
* emergency events.

Historical data can support:

* scenario construction,
* validation,
* contextual analysis,
* model assumptions.

Historical incidents should not be treated as a complete record of all urban disruptions.

Reporting bias and missing events must be considered.

---

# 22. Transportation Data

Transportation data may include:

* roads,
* intersections,
* bridges,
* transit infrastructure,
* travel-access relationships.

Potential uses include:

* accessibility analysis,
* facility access,
* dependency relationships,
* disruption propagation.

The initial system should avoid attempting to model complete real-time traffic behavior unless a research requirement specifically requires it.

---

# 23. Dependency Data

Dependencies are central to RESOLVE.

A dependency may be:

* directly observed,
* sourced from documentation,
* inferred from spatial or functional relationships,
* modeled from domain knowledge,
* synthetically generated.

Each dependency should record its provenance and confidence.

Example:

```text id="9o7u3x"
Source: Power facility
Target: Water pumping facility
Type: POWER
Strength: 0.85
Confidence: 0.70
Source: documented relationship
```

The exact dependency semantics will be defined by the domain and simulation model.

---

# 24. Dependency Data Quality

Dependency quality should consider:

* existence,
* direction,
* dependency type,
* strength,
* threshold,
* geographic relationship,
* confidence,
* provenance.

An incomplete dependency graph must not be presented as a complete representation of the real city.

This is especially important because dependency-aware results may be sensitive to graph completeness.

---

# 25. Environmental Data

Environmental datasets may include:

* temperature,
* precipitation,
* elevation,
* land cover,
* vegetation,
* surface characteristics,
* satellite-derived indicators.

Environmental datasets should only be incorporated when they contribute to a defined hazard, exposure, vulnerability, or research analysis.

---

# 26. Intervention Data

Interventions may initially be modeled rather than sourced from an external dataset.

Examples include:

* backup power,
* water storage,
* infrastructure reinforcement,
* redundancy,
* cooling capacity,
* emergency access improvements,
* dependency decoupling.

An intervention definition should include:

```text id="x9k9cj"
intervention_id
target
type
cost
resource_requirements
affected_dependencies
expected_effect
constraints
assumptions
```

Intervention effects must be treated as model assumptions unless supported by evidence.

---

# 27. Synthetic Datasets

Synthetic data is permitted for development and controlled experiments.

Synthetic datasets should clearly identify:

* synthetic status,
* generation method,
* random seed,
* generation configuration,
* intended purpose.

Example purposes:

* unit testing,
* graph testing,
* cascade testing,
* edge-case testing,
* scalability benchmarking.

Synthetic data should not be represented as real-world evidence.

---

# 28. Derived Datasets

Derived datasets are produced from one or more source datasets.

Examples:

* normalized facility dataset,
* spatially joined population exposure,
* generated dependency relationships,
* canonical hazard layers.

Each derived dataset should record:

```text id="7g2e6a"
source datasets
transformation
transformation version
parameters
software version
execution date
output version
```

Derived data must be reproducible where practical.

---

# 29. Missing Data

Missing information must be explicitly represented.

The preferred states are:

```text id="x1r2qv"
UNKNOWN
NOT_AVAILABLE
NOT_APPLICABLE
WITHHELD
ESTIMATED
```

Missing data must not automatically become zero.

For example:

```text
capacity = 0
```

is different from:

```text
capacity = UNKNOWN
```

The distinction can materially affect simulation results.

---

# 30. Data Uncertainty

Uncertainty may exist in:

* hazard intensity,
* facility capacity,
* population estimates,
* dependency strength,
* dependency existence,
* intervention effectiveness,
* recovery duration.

Where possible, uncertainty should be represented explicitly.

The research system should support sensitivity analysis where uncertainty may affect conclusions.

---

# 31. Data Quality Assessment

Before primary research use, datasets should undergo quality assessment.

Potential checks include:

### Structural

* schema validity,
* required fields,
* duplicate records,
* invalid types.

### Spatial

* invalid geometries,
* CRS consistency,
* coordinate ranges,
* spatial overlap.

### Temporal

* invalid timestamps,
* missing dates,
* temporal gaps,
* inconsistent timezones.

### Semantic

* invalid categories,
* contradictory attributes,
* impossible values.

### Provenance

* source identification,
* version identification,
* transformation traceability.

---

# 32. Dataset Quality Status

A dataset may receive a quality status such as:

| Status             | Meaning                                      |
| ------------------ | -------------------------------------------- |
| UNASSESSED         | Quality not yet evaluated                    |
| DEVELOPMENT        | Suitable for development/testing             |
| REVIEWED           | Basic quality checks completed               |
| RESEARCH_CANDIDATE | Suitable for evaluation pending final review |
| FROZEN             | Approved for a defined experiment            |
| DEPRECATED         | No longer recommended                        |
| REJECTED           | Not suitable for intended use                |

A dataset should be marked `FROZEN` only for a clearly defined experiment or evaluation stage.

---

# 33. Dataset Freezing

Before the primary experiment:

1. Select datasets.
2. Record exact versions.
3. Complete quality checks.
4. Record preprocessing versions.
5. Record limitations.
6. Freeze the dataset configuration.
7. Assign an identifiable dataset manifest.

Once frozen, changes should result in a new dataset version or experiment configuration.

---

# 34. Dataset Manifest

A research dataset manifest should eventually contain:

```text id="h8j4tx"
Dataset collection ID
Dataset versions
Source metadata
Preprocessing versions
CRS
Spatial coverage
Temporal coverage
Quality status
Missing-data treatment
Uncertainty notes
Licenses
Checksums where applicable
Creation timestamp
Git commit or pipeline version
```

The manifest becomes part of experiment reproducibility.

---

# 35. Data Lineage

RESOLVE should preserve lineage:

```text id="1qg0av"
External Source
      ↓
Raw Dataset
      ↓
Validation
      ↓
Normalization
      ↓
Spatial Processing
      ↓
Canonical Dataset
      ↓
Derived Dataset
      ↓
Scenario
      ↓
Simulation
      ↓
Experiment
```

A final result should be traceable back to its relevant data inputs.

---

# 36. Licensing

Every externally sourced dataset must have its license or usage conditions recorded.

The project must not assume that publicly downloadable data is automatically unrestricted.

For each dataset, document:

* license,
* attribution requirements,
* redistribution restrictions,
* commercial-use restrictions where relevant,
* derived-data restrictions where relevant.

If licensing is unclear, the dataset should not be used in a public research artifact until the issue is resolved.

---

# 37. Privacy

RESOLVE should minimize personal data.

The intended research architecture favors:

* aggregated population data,
* infrastructure-level information,
* geographic areas,
* public facilities,
* environmental data.

Individual-level tracking is outside the intended scope.

If a dataset unexpectedly contains personal information, it must undergo additional review before use.

---

# 38. Data Security

Research datasets must be protected according to their sensitivity.

Security measures may include:

* access control,
* encrypted storage where appropriate,
* secret management,
* restricted credentials,
* integrity checks,
* backup,
* controlled data export.

Sensitive data should not be committed to the public repository.

---

# 39. Dataset Storage

The conceptual storage architecture is:

```text id="q2x2v5"
Raw / External Data
        ↓
File Storage
        ↓
Processing
        ↓
PostgreSQL/PostGIS
        ↓
Research Dataset
        ↓
Experiment
```

Git should store:

* dataset metadata,
* manifests,
* schemas,
* preprocessing definitions,
* small synthetic datasets,
* reproducibility configuration.

Large or restricted datasets should not automatically be committed to Git.

---

# 40. Dataset Availability

If a dataset cannot be redistributed, the repository should still preserve:

* dataset name,
* source,
* version,
* acquisition instructions,
* preprocessing steps,
* expected structure,
* checksum where practical,
* license information.

This allows another researcher to reconstruct the data pipeline where legally and technically possible.

---

# 41. Dataset-to-Experiment Mapping

Every primary experiment should identify the datasets it uses.

Example:

```text id="gqg8l3"
Experiment EXP-001

DATASET-URBAN-001 v1
DATASET-FACILITY-001 v2
DATASET-POP-001 v1
DATASET-HAZARD-001 v3
DEPENDENCY-MODEL-001 v1
```

This mapping should be included in experiment evidence.

---

# 42. Dataset Limitations

Each research dataset must document known limitations.

Potential limitations include:

* incomplete geographic coverage,
* coarse resolution,
* outdated observations,
* reporting bias,
* missing infrastructure attributes,
* uncertain dependency relationships,
* licensing restrictions,
* temporal mismatch,
* measurement error.

Limitations must be considered when interpreting results.

---

# 43. Dataset Bias

Potential sources of bias include:

* uneven geographic coverage,
* underreported incidents,
* differences in facility mapping quality,
* population estimation errors,
* historical reporting bias,
* incomplete dependency information.

The project should not assume that a dataset is neutral merely because it is structured.

Bias should be considered when evaluating robustness.

---

# 44. Dataset Sensitivity

Primary experiments should consider how conclusions change when important dataset assumptions vary.

Potential sensitivity dimensions include:

* dependency completeness,
* dependency strength,
* population estimates,
* hazard severity,
* facility capacity,
* recovery assumptions.

This connects the dataset strategy to hypothesis H8 and H9.

---

# 45. Development Dataset vs Research Dataset

The project must distinguish between:

### Development dataset

Used for:

* coding,
* unit tests,
* integration tests,
* debugging,
* local demonstrations.

### Research dataset

Used for:

* primary experiments,
* evaluation,
* published results,
* paper figures and tables.

A development dataset should not automatically become the research dataset.

---

# 46. Dataset Freeze Criteria

A research dataset may be frozen when:

* [ ] source is identified,
* [ ] version is identified,
* [ ] license is recorded,
* [ ] spatial coverage is documented,
* [ ] temporal coverage is documented,
* [ ] CRS is documented,
* [ ] quality checks are completed,
* [ ] missing data is documented,
* [ ] uncertainty is documented,
* [ ] preprocessing is versioned,
* [ ] limitations are documented,
* [ ] experiment mapping is defined.

---

# 47. Initial Dataset Candidates

The following are categories to investigate rather than commitments to specific sources:

| Dataset Category              | Candidate Role                          |
| ----------------------------- | --------------------------------------- |
| Administrative boundaries     | Geographic reference                    |
| OpenStreetMap-derived data    | Roads, facilities, urban infrastructure |
| Population grids              | Exposure estimation                     |
| Satellite/environmental data  | Hazard and environmental context        |
| Weather observations          | Hazard characterization                 |
| Flood/heat datasets           | Hazard scenarios                        |
| Public infrastructure records | Infrastructure representation           |
| Historical incident records   | Scenario validation                     |
| Synthetic dependency data     | Controlled graph experiments            |

Actual sources must be selected after evaluating:

* availability,
* quality,
* licensing,
* spatial/temporal suitability,
* reproducibility.

No candidate should be treated as a final research dataset until reviewed.

---

# 48. Dataset Selection for the Primary Case Study

The primary case-study dataset collection should be selected using a documented process:

1. Define the experiment requirements.
2. Identify candidate sources.
3. Compare quality and coverage.
4. Review licensing.
5. Validate spatial and temporal compatibility.
6. Assess missing data.
7. Assess uncertainty.
8. Document limitations.
9. Select the dataset.
10. Freeze the dataset configuration.

The selection process itself should be preserved as research evidence.

---

# 49. Dataset Reproducibility

A researcher should be able to determine:

> Which datasets produced this result?

from the experiment metadata.

Ideally, the researcher should also be able to determine:

> Where did those datasets come from, how were they transformed, and which exact versions were used?

This is a core requirement for trustworthy experimental results.

---

# 50. Relationship to Other Documents

This document works together with:

* `docs/DATA_ARCHITECTURE.md`
* `docs/DOMAIN_MODEL.md`
* `docs/REQUIREMENTS.md`
* `research/RESEARCH_QUESTION.md`
* `research/HYPOTHESES.md`
* `research/BASELINES.md`
* `research/METRICS.md`
* `research/REPRODUCIBILITY.md`
* `research/EXPERIMENTS.md`

Dataset decisions must remain consistent across these documents.

---

# 51. Future Evolution

This document will be updated when:

* real datasets are selected,
* new data sources are introduced,
* preprocessing pipelines are implemented,
* data-quality results become available,
* the case-study geography is finalized,
* primary experiments are designed,
* licensing constraints are discovered,
* uncertainty models are developed.

Candidate datasets must be replaced with verified dataset records before primary research conclusions are drawn.

---

# 52. Guiding Principle

RESOLVE should never treat data as simply:

> "something we downloaded."

Instead, every research-relevant dataset should be understood as:

> **a versioned, sourced, transformed, quality-assessed, uncertainty-aware research input whose limitations are part of the evidence.**
