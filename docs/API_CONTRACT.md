# RESOLVE — API Contract

## 1. Purpose

This document defines the API contract for RESOLVE — Resilient Urban Systems & Vulnerability Observatory.

The API provides a controlled interface between clients and the RESOLVE application layer.

The API must expose research and system capabilities without placing domain logic, simulation algorithms, or persistence logic inside HTTP route handlers.

The initial API is REST-oriented and implemented with FastAPI.

---

# 2. API Design Principles

## 2.1 API as an Interface Boundary

The API is an interface to the application layer.

```text
Client
  |
  v
FastAPI
  |
  v
Application Services
  |
  v
Domain / Simulation / Research
```

The API must not become the location of core business logic.

---

## 2.2 Explicit Contracts

Requests and responses must use explicit schemas.

Pydantic models should define:

* required fields;
* optional fields;
* valid types;
* constraints;
* enumerations;
* nested structures;
* response formats.

---

## 2.3 Versioning

The initial API should use:

```text
/api/v1
```

Breaking changes should require a new API version.

Non-breaking changes may be introduced within the existing version when backward compatibility is preserved.

---

# 3. Base URL

Development:

```text
http://localhost:8000/api/v1
```

Production deployment configuration may provide a different host.

The application must not hard-code production URLs.

---

# 4. Content Type

Requests containing JSON data should use:

```text
Content-Type: application/json
```

Responses should normally use:

```text
Content-Type: application/json
```

Large research artifacts may use file-based responses where appropriate.

---

# 5. Common Response Conventions

Successful responses should return structured JSON.

Example:

```json
{
  "data": {},
  "meta": {}
}
```

For collection endpoints:

```json
{
  "data": [],
  "meta": {
    "page": 1,
    "page_size": 50,
    "total": 0
  }
}
```

The exact envelope may be simplified for endpoints where an envelope provides no meaningful value.

Consistency is more important than unnecessary abstraction.

---

# 6. Common Error Format

API errors should use a consistent structure.

Example:

```json
{
  "error": {
    "code": "SCENARIO_NOT_FOUND",
    "message": "The requested scenario does not exist.",
    "details": {}
  }
}
```

Errors should not expose:

* database credentials;
* internal stack traces;
* secrets;
* infrastructure details that create unnecessary security risk.

---

# 7. HTTP Status Codes

The API should use conventional HTTP status codes.

| Status | Meaning                                     |
| ------ | ------------------------------------------- |
| 200    | Successful request                          |
| 201    | Resource created                            |
| 202    | Request accepted for asynchronous execution |
| 204    | Successful request with no response body    |
| 400    | Invalid request                             |
| 401    | Authentication required                     |
| 403    | Access denied                               |
| 404    | Resource not found                          |
| 409    | Resource conflict                           |
| 422    | Validation error                            |
| 429    | Rate limit exceeded                         |
| 500    | Unexpected server error                     |
| 503    | Service temporarily unavailable             |

Authentication and rate limiting may initially remain minimal for local research use and become stricter during deployment.

---

# 8. Resource Naming

Resources should use plural nouns.

Examples:

```text
/entities
/dependencies
/hazards
/scenarios
/interventions
/experiments
/simulations
/results
/datasets
```

Actions that represent domain operations may use explicit action endpoints where a conventional CRUD resource does not adequately describe the operation.

Example:

```text
POST /simulations/{simulation_id}/run
```

---

# 9. Health Endpoints

## GET `/health`

Purpose:

Determine whether the API process is running.

Example response:

```json
{
  "status": "ok"
}
```

This endpoint should not necessarily verify every dependency.

---

## GET `/health/ready`

Purpose:

Determine whether the application is ready to serve requests.

Possible checks:

* database connectivity;
* required configuration;
* required infrastructure dependencies.

Example:

```json
{
  "status": "ready",
  "checks": {
    "database": "ok"
  }
}
```

---

# 10. Urban Entity API

## POST `/entities`

Creates an urban entity.

Example request:

```json
{
  "type": "critical_facility",
  "name": "Example Hospital",
  "geometry": {},
  "attributes": {},
  "source": "dataset-001",
  "confidence": 0.95
}
```

Response:

```json
{
  "data": {
    "id": "entity-001",
    "type": "critical_facility",
    "name": "Example Hospital"
  }
}
```

---

## GET `/entities`

Returns urban entities.

Supported filtering may include:

```text
type
state
geographic_area
dataset_version
```

Pagination should be supported for sufficiently large datasets.

---

## GET `/entities/{entity_id}`

Returns one urban entity.

---

## PUT `/entities/{entity_id}`

Updates an urban entity where modification is permitted.

Research datasets used by completed experiments should be versioned rather than silently modified.

---

## DELETE `/entities/{entity_id}`

Deletes an entity where permitted.

Deletion must respect dependency and experiment integrity rules.

A referenced entity may instead require deprecation or dataset versioning.

---

# 11. Dependency API

## POST `/dependencies`

Creates a dependency relationship.

Example:

```json
{
  "source_entity_id": "power-001",
  "dependent_entity_id": "pump-001",
  "dependency_type": "POWER",
  "direction": "source_to_dependent",
  "strength": 0.9,
  "threshold": 0.5,
  "confidence": 0.85
}
```

---

## GET `/dependencies`

Returns dependency relationships.

Filters may include:

```text
source_entity_id
dependent_entity_id
dependency_type
confidence
```

---

## GET `/dependencies/{dependency_id}`

Returns a specific dependency.

---

## DELETE `/dependencies/{dependency_id}`

Removes or deprecates a dependency when permitted.

Completed research datasets should remain reproducible through versioning.

---

# 12. Graph API

## GET `/graph`

Returns a representation of the dependency graph.

Optional filters may include:

```text
geographic_area
entity_type
dependency_type
dataset_version
```

The API should avoid returning unnecessarily large graph payloads.

---

## GET `/entities/{entity_id}/dependencies`

Returns dependencies connected to an entity.

Optional direction:

```text
incoming
outgoing
both
```

---

## GET `/entities/{entity_id}/downstream-impact`

Returns downstream entities that may be affected through dependency relationships.

This endpoint should expose analysis results rather than embed graph algorithms directly in the API router.

---

# 13. Hazard API

## POST `/hazards`

Creates a hazard definition.

Example:

```json
{
  "type": "extreme_heat",
  "intensity": 42.0,
  "unit": "celsius",
  "geographic_scope": {},
  "start_time": "2026-07-01T12:00:00Z",
  "duration_hours": 12
}
```

---

## GET `/hazards`

Lists available hazards.

---

## GET `/hazards/{hazard_id}`

Returns a hazard definition.

---

# 14. Scenario API

## POST `/scenarios`

Creates a scenario.

Example:

```json
{
  "name": "Extreme Heat Scenario A",
  "hazard_id": "hazard-001",
  "geographic_scope": {},
  "affected_entities": [
    "entity-001",
    "entity-002"
  ],
  "duration_hours": 24,
  "recovery_configuration": {},
  "version": 1
}
```

---

## GET `/scenarios`

Lists scenarios.

Possible filters:

```text
hazard_type
geographic_area
version
status
```

---

## GET `/scenarios/{scenario_id}`

Returns a scenario.

---

## PUT `/scenarios/{scenario_id}`

Updates a scenario only when it has not been frozen by a completed experiment.

Once used by a completed research experiment, changes should create a new scenario version.

---

## POST `/scenarios/{scenario_id}/validate`

Validates whether a scenario is suitable for execution.

Example:

```json
{
  "data": {
    "valid": true,
    "errors": [],
    "warnings": []
  }
}
```

Validation should detect issues such as:

* missing hazard;
* missing entities;
* invalid geographic scope;
* invalid simulation duration;
* invalid recovery configuration;
* unresolved dependencies.

---

# 15. Intervention API

## POST `/interventions`

Creates an intervention definition.

Example:

```json
{
  "name": "Backup Power Installation",
  "target_entities": [
    "hospital-001"
  ],
  "implementation_cost": 100000,
  "resource_requirements": {
    "equipment_units": 1
  },
  "expected_effects": {
    "power_dependency_reduction": 0.8
  }
}
```

---

## GET `/interventions`

Lists intervention candidates.

---

## GET `/interventions/{intervention_id}`

Returns an intervention.

---

## POST `/interventions/{intervention_id}/evaluate`

Evaluates an intervention against a specified scenario.

Example:

```json
{
  "scenario_id": "scenario-001",
  "budget": 500000
}
```

Response should contain measurable outcomes rather than only a qualitative score.

---

# 16. Baseline Assessment API

## POST `/assessments/baseline`

Runs the isolated baseline assessment.

Example:

```json
{
  "scenario_id": "scenario-001",
  "intervention_ids": [],
  "configuration": {}
}
```

Response:

```json
{
  "data": {
    "assessment_id": "assessment-001",
    "status": "completed",
    "metrics": {}
  }
}
```

The baseline must not perform dependency propagation.

---

# 17. Simulation API

## POST `/simulations`

Creates a simulation execution.

Example:

```json
{
  "scenario_id": "scenario-001",
  "method": "dependency_aware",
  "intervention_ids": [],
  "configuration": {
    "time_step": 1,
    "random_seed": 42
  }
}
```

---

## POST `/simulations/{simulation_id}/run`

Starts simulation execution.

For short deterministic simulations, the operation may execute synchronously.

For long-running simulations, the API may return:

```text
202 Accepted
```

with a simulation identifier.

---

## GET `/simulations/{simulation_id}`

Returns simulation metadata and status.

Possible states:

```text
CREATED
QUEUED
RUNNING
COMPLETED
FAILED
CANCELLED
```

---

## GET `/simulations/{simulation_id}/events`

Returns recorded cascade events.

Optional filters:

```text
entity_id
event_type
start_time
end_time
```

---

## GET `/simulations/{simulation_id}/states`

Returns simulation state history.

Large histories should support pagination or time-range filtering.

---

## POST `/simulations/{simulation_id}/cancel`

Requests cancellation of a running simulation when cancellation is supported by the execution backend.

---

# 18. Experiment API

Experiments are the primary research execution unit.

## POST `/experiments`

Creates an experiment configuration.

Example:

```json
{
  "name": "Dependency Comparison Experiment 001",
  "dataset_version": "dataset-v1",
  "scenario_id": "scenario-001",
  "baseline_method": "isolated_risk",
  "proposed_method": "dependency_aware",
  "intervention_ids": [
    "intervention-001"
  ],
  "budget": 500000,
  "random_seed": 42
}
```

---

## POST `/experiments/{experiment_id}/run`

Runs the experiment.

The experiment runner should:

1. validate configuration;
2. freeze the execution configuration;
3. execute baseline;
4. execute proposed method;
5. calculate metrics;
6. compare results;
7. persist outputs.

---

## GET `/experiments`

Lists experiments.

Filters may include:

```text
status
dataset_version
scenario_id
method
created_at
```

---

## GET `/experiments/{experiment_id}`

Returns experiment metadata and execution status.

---

## GET `/experiments/{experiment_id}/results`

Returns calculated experiment results.

Example:

```json
{
  "data": {
    "primary_metric": {
      "name": "cascading_impact_reduction",
      "value": 18.4,
      "unit": "percent"
    },
    "secondary_metrics": {}
  }
}
```

---

# 19. Intervention Ranking API

## POST `/experiments/{experiment_id}/rankings`

Calculates or retrieves intervention rankings.

Possible ranking information:

```text
intervention_id
rank
benefit
cost
benefit_per_cost
population_protected
facilities_protected
cascade_reduction
```

Ranking results must include enough information to interpret how the ranking was produced.

---

# 20. Dataset API

## POST `/datasets`

Registers a dataset version.

Example:

```json
{
  "name": "Urban Infrastructure Dataset",
  "version": "2026-01",
  "source": "example-source",
  "license": "example-license",
  "checksum": "..."
}
```

---

## GET `/datasets`

Lists registered datasets.

---

## GET `/datasets/{dataset_id}`

Returns dataset metadata and provenance.

---

# 21. Research Results API

## GET `/results`

Returns research results where appropriate.

Filtering may include:

```text
experiment_id
scenario_id
metric
method
dataset_version
```

---

## GET `/results/{result_id}`

Returns a specific research result.

---

# 22. Pagination

Collection endpoints should use pagination when datasets can become large.

Preferred parameters:

```text
?page=1&page_size=50
```

The server should enforce a maximum page size.

Example:

```text
page_size <= 200
```

The exact limit may be adjusted after performance testing.

---

# 23. Filtering

Filtering should use explicit query parameters.

Example:

```text
GET /entities?type=critical_facility&state=DEGRADED
```

Filters should be validated.

Unsupported filters should not silently be ignored.

---

# 24. Sorting

Where sorting is supported:

```text
?sort=created_at
?sort=-created_at
```

A leading `-` may represent descending order.

Only documented sortable fields should be accepted.

---

# 25. Idempotency

Operations that may be retried should avoid unintentionally creating duplicate research executions.

Where asynchronous experiment creation or execution requires it, an idempotency key may be supported:

```text
Idempotency-Key: <unique-key>
```

The mechanism should be introduced when the execution workflow requires it.

---

# 26. Long-Running Operations

Long-running operations should not require clients to keep an HTTP request open indefinitely.

Potential pattern:

```text
POST /experiments/{id}/run
        |
        v
202 Accepted
        |
        v
Execution ID
        |
        v
GET /experiments/{id}
```

The initial implementation may execute small experiments synchronously.

Background execution should be introduced when measured execution time justifies it.

---

# 27. Authentication and Authorization

The initial research environment may operate without user authentication in local development.

When authentication is introduced, authorization should distinguish at least:

```text
Researcher
Operator
Viewer
Administrator
```

Authorization must be implemented at the application boundary rather than relying solely on frontend restrictions.

---

# 28. Rate Limiting

Rate limiting is not a core research requirement during local development.

For deployed environments, rate limiting may be applied to:

* expensive simulation endpoints;
* experiment execution;
* graph analysis;
* bulk data endpoints.

Rate limits should protect system resources without interfering with legitimate research workloads.

---

# 29. OpenAPI

FastAPI should generate OpenAPI documentation automatically.

Expected development endpoints:

```text
/docs
/redoc
/openapi.json
```

The generated contract should be treated as a useful development artifact but should not replace this explicit API design document.

---

# 30. API Testing Requirements

Every implemented endpoint should have tests covering:

### Successful Requests

* valid request;
* expected response schema;
* expected status code.

### Validation

* missing required fields;
* invalid types;
* invalid ranges;
* invalid identifiers.

### Domain Errors

* invalid state;
* invalid dependency;
* invalid scenario;
* invalid intervention.

### Resource Errors

* resource not found;
* duplicate/conflicting resource.

### Research Execution

* deterministic configuration;
* experiment creation;
* experiment execution;
* result retrieval.

---

# 31. API and Domain Separation

The following separation is mandatory:

```text
API Schema
    |
    v
Application Command / Query
    |
    v
Domain Model / Service
    |
    v
Infrastructure
```

The following pattern should be avoided:

```text
FastAPI Router
    |
    +--> SQL Query
    +--> Graph Algorithm
    +--> Simulation Algorithm
    +--> Metric Calculation
```

The router should coordinate request handling, not become the application.

---

# 32. Research Reproducibility Through the API

Research execution endpoints must record sufficient configuration to reproduce an experiment.

At minimum:

```text
dataset_version
scenario_version
method
interventions
budget
simulation_configuration
random_seed
software_version
```

An API request alone is not necessarily the complete experiment record.

The persisted experiment configuration is authoritative.

---

# 33. API Evolution

API changes must be classified as:

### Non-Breaking

Examples:

* adding optional response fields;
* adding new endpoints;
* adding optional filters.

### Potentially Breaking

Examples:

* changing required request fields;
* changing response structure;
* changing semantics of an existing field;
* removing an endpoint.

Breaking changes require API version consideration and documentation.

---

# 34. Initial Endpoint Summary

| Area          | Endpoints                            |
| ------------- | ------------------------------------ |
| Health        | `/health`, `/health/ready`           |
| Entities      | `/entities`                          |
| Dependencies  | `/dependencies`                      |
| Graph         | `/graph`, entity dependency analysis |
| Hazards       | `/hazards`                           |
| Scenarios     | `/scenarios`                         |
| Interventions | `/interventions`                     |
| Baseline      | `/assessments/baseline`              |
| Simulations   | `/simulations`                       |
| Experiments   | `/experiments`                       |
| Results       | `/results`                           |
| Datasets      | `/datasets`                          |

This list is an initial contract rather than a promise that every endpoint will be implemented immediately.

---

# 35. API Acceptance Criteria

The API contract is considered sufficiently defined when:

* API versioning is explicit;
* resources have consistent naming;
* request and response schemas are defined;
* error conventions are defined;
* simulation execution is represented;
* experiment execution is represented;
* baseline assessment is represented;
* intervention evaluation is represented;
* research results are retrievable;
* pagination and filtering rules are defined;
* long-running operations have a documented pattern;
* API/domain separation is explicit;
* reproducibility metadata is preserved;
* endpoint behavior can be tested independently.

---

# 36. Guiding API Principle

> **The RESOLVE API should expose research capabilities through stable, explicit contracts while keeping domain logic, simulation logic, and scientific evaluation outside the HTTP layer.**
