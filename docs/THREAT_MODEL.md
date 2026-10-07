# RESOLVE — Threat Model

## 1. Purpose

This document defines the initial threat model for RESOLVE — Resilient Urban Systems & Vulnerability Observatory.

The objective is to identify realistic threats to:

* application security;
* data integrity;
* research integrity;
* simulation execution;
* infrastructure;
* confidentiality;
* availability.

The threat model is intentionally proportional to the current project stage.

RESOLVE is initially a research-oriented system rather than a public emergency-management platform. Security controls should therefore address realistic risks without introducing unnecessary enterprise complexity.

---

# 2. Threat Modeling Objectives

The threat model should help answer:

1. What can go wrong?
2. Who or what could cause it?
3. What assets could be affected?
4. What trust boundary is crossed?
5. What is the potential impact?
6. What controls reduce the risk?
7. What evidence would demonstrate that the control works?

Threat modeling should be updated as the architecture evolves.

---

# 3. System Under Analysis

The primary system is:

```text
Client
  |
  v
Frontend
  |
  v
FastAPI
  |
  v
Application Layer
  |
  +------------------+
  |                  |
  v                  v
Domain           Simulation
  |                  |
  +--------+---------+
           |
           v
     PostgreSQL/PostGIS
           |
           +--> Dataset Storage
           |
           +--> Research Results
Optional:
           |
           +--> Redis
           |
           +--> Celery Workers
           |
           +--> External Data Sources
```

---

# 4. Assets

## 4.1 Research Assets

High-value assets include:

* research datasets;
* dataset versions;
* scenario definitions;
* dependency graphs;
* experiment configurations;
* simulation outputs;
* research metrics;
* experiment results;
* evidence artifacts;
* paper-supporting results.

---

## 4.2 System Assets

System assets include:

* database;
* source code;
* configuration;
* API;
* background workers;
* Redis;
* file storage;
* deployment infrastructure.

---

## 4.3 Security Assets

Security-sensitive assets include:

* database credentials;
* API keys;
* authentication secrets;
* external service credentials;
* deployment credentials.

---

# 5. Threat Actors

The initial threat model considers the following actors.

## 5.1 Untrusted Internet User

A user who can reach a deployed API without authorization.

Potential objectives:

* access protected data;
* execute expensive simulations;
* exploit API vulnerabilities;
* disrupt availability.

---

## 5.2 Malicious Authenticated User

A legitimate account holder attempting to exceed authorized permissions.

Potential objectives:

* access another user's data;
* modify research data;
* execute unauthorized experiments;
* obtain restricted datasets.

---

## 5.3 Malicious Data Provider

An external or compromised data source providing malformed or manipulated data.

Potential objectives:

* corrupt datasets;
* introduce malicious payloads;
* influence research results.

---

## 5.4 Compromised Dependency

A malicious or vulnerable third-party package.

Potential impact:

* code execution;
* credential theft;
* data corruption;
* supply-chain compromise.

---

## 5.5 Accidental Internal Actor

A developer or researcher who unintentionally:

* exposes credentials;
* modifies research data;
* deletes experiment results;
* introduces insecure configuration;
* changes an experiment without documenting it.

This threat is particularly relevant to research integrity.

---

# 6. Trust Boundaries

Important trust boundaries include:

```text id="zv8k9e"
Boundary 1
Client -> API

Boundary 2
API -> Application

Boundary 3
Application -> Database

Boundary 4
Application -> File Storage

Boundary 5
Application -> External Data

Boundary 6
Application -> Background Worker

Boundary 7
Developer Environment -> Repository
```

Each boundary should validate assumptions made by the receiving component.

---

# 7. Threat Classification

Threats are classified using:

```text id="kz98dd"
Likelihood:
Low / Medium / High

Impact:
Low / Medium / High

Priority:
Low / Medium / High / Critical
```

Priority is based on the combination of likelihood and potential impact.

The classification is qualitative during the initial project stage.

---

# 8. Threat T01 — Unauthorized API Access

### Description

An attacker accesses API resources without appropriate authorization.

### Assets

* urban data;
* experiment data;
* restricted datasets;
* system operations.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

* authentication for exposed deployments;
* server-side authorization;
* least privilege;
* resource ownership checks;
* API tests for unauthorized access.

### Verification

Attempt protected API operations without valid authorization and confirm access is rejected.

---

# 9. Threat T02 — Broken Authorization

### Description

An authenticated user accesses or modifies resources they do not have permission to use.

### Assets

* datasets;
* experiments;
* results;
* configuration.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

* centralized authorization;
* explicit resource ownership rules;
* role-based permissions;
* backend enforcement;
* authorization integration tests.

### Verification

Test cross-user and cross-role resource access.

---

# 10. Threat T03 — SQL Injection

### Description

Malicious input is interpreted as SQL.

### Assets

* database;
* research data;
* credentials;
* experiment integrity.

### Likelihood

Medium.

### Impact

Critical.

### Priority

High.

### Mitigations

* SQLAlchemy parameterized operations;
* validated inputs;
* no dynamic SQL from raw user input;
* secure database permissions.

### Verification

Automated injection tests against relevant endpoints.

---

# 11. Threat T04 — Path Traversal

### Description

An attacker manipulates file paths to access files outside the intended storage directory.

### Assets

* configuration files;
* research data;
* system files.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

* filename normalization;
* controlled storage paths;
* path validation;
* no direct filesystem access from user-controlled paths.

### Verification

Test traversal payloads such as parent-directory sequences and confirm rejection.

---

# 12. Threat T05 — Malicious File Upload

### Description

An attacker uploads a malicious or malformed file.

### Assets

* application;
* storage;
* ingestion pipeline.

### Likelihood

Medium if uploads are exposed.

### Impact

High.

### Priority

Medium/High.

### Mitigations

* restrict file types;
* enforce file-size limits;
* validate file contents;
* store uploads outside executable paths;
* scan files when appropriate;
* safely process archives.

### Verification

Upload malformed, oversized, unsupported, and intentionally suspicious test files.

---

# 13. Threat T06 — Resource Exhaustion

### Description

An attacker submits workloads that consume excessive CPU, memory, storage, or execution time.

This is especially relevant because RESOLVE performs simulations and graph analysis.

### Assets

* API availability;
* CPU;
* memory;
* database;
* worker capacity.

### Likelihood

High for publicly exposed simulation endpoints.

### Impact

High.

### Priority

High.

### Mitigations

* request limits;
* graph-size limits;
* simulation time limits;
* experiment-combination limits;
* concurrency controls;
* rate limiting;
* execution timeouts;
* worker isolation.

### Verification

Run controlled oversized workloads and confirm resource limits are enforced.

---

# 14. Threat T07 — Malicious Simulation Configuration

### Description

An attacker supplies configuration designed to trigger unexpected behavior or excessive resource consumption.

Examples:

* extremely large simulation duration;
* excessive time steps;
* huge graph traversal;
* extreme iteration counts.

### Assets

* simulation engine;
* system availability.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

* strict schema validation;
* domain validation;
* configurable limits;
* execution timeouts;
* resource monitoring.

---

# 15. Threat T08 — Dependency Graph Abuse

### Description

Malformed graph structures cause excessive processing or pathological traversal.

Examples:

* extremely large graphs;
* dense dependency relationships;
* cyclic structures where cycles are not supported;
* intentionally expensive traversal patterns.

### Assets

* graph engine;
* simulation engine;
* system availability.

### Likelihood

Medium.

### Impact

Medium/High.

### Priority

Medium/High.

### Mitigations

* graph-size limits;
* dependency validation;
* cycle handling;
* traversal limits;
* execution timeouts.

---

# 16. Threat T09 — Credential Exposure

### Description

Secrets are accidentally committed, logged, displayed, or packaged.

### Assets

* database;
* external services;
* deployment infrastructure.

### Likelihood

Medium.

### Impact

Critical.

### Priority

High.

### Mitigations

* environment variables;
* secret-management systems where appropriate;
* `.gitignore`;
* secret scanning;
* no credentials in logs;
* developer documentation.

### Verification

Repository scans must confirm that known secret patterns are absent.

---

# 17. Threat T10 — Supply-Chain Compromise

### Description

A vulnerable or malicious third-party dependency compromises the application.

### Assets

* application;
* developer environment;
* research data;
* deployment.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

* dependency review;
* vulnerability scanning;
* minimize dependencies;
* controlled upgrades;
* lockfiles or equivalent reproducibility controls;
* review unexpected dependency changes.

### Verification

Run dependency security checks during development and release preparation.

---

# 18. Threat T11 — Research Dataset Tampering

### Description

Research data is modified without appropriate versioning or provenance.

### Assets

* research validity;
* experiment results;
* paper evidence.

### Likelihood

Medium.

### Impact

Critical.

### Priority

High.

### Mitigations

* immutable dataset versions;
* provenance metadata;
* checksums where appropriate;
* controlled data pipelines;
* experiment-to-dataset linkage.

### Verification

Modify an underlying dataset and confirm previous experiment records still reference the original version.

---

# 19. Threat T12 — Scenario Tampering

### Description

A scenario used by an experiment is modified after execution.

### Assets

* reproducibility;
* experiment validity.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

* scenario versioning;
* immutable completed experiment configuration;
* version identifiers;
* experiment metadata.

### Verification

Attempt to modify a scenario referenced by a completed experiment and confirm that the original version remains identifiable.

---

# 20. Threat T13 — Result Overwriting

### Description

A new execution silently replaces previous research results.

### Assets

* research evidence;
* experiment history.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

* unique experiment execution identifiers;
* immutable completed results;
* result versioning;
* append-oriented experiment history.

### Verification

Run the same experiment twice and confirm both executions remain distinguishable.

---

# 21. Threat T14 — Reproducibility Failure

### Description

An experiment cannot be reproduced because critical execution information was not recorded.

### Assets

* scientific validity;
* paper evidence.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

Record:

```text id="r5h3cq"
Dataset Version
Scenario Version
Simulation Configuration
Interventions
Budget
Random Seed
Software Version
Environment Information
```

### Verification

Re-run a completed experiment from its recorded configuration and compare results.

---

# 22. Threat T15 — Data Provenance Loss

### Description

A derived dataset or metric cannot be traced to its source.

### Assets

* research transparency;
* reproducibility.

### Likelihood

Medium.

### Impact

High.

### Priority

Medium/High.

### Mitigations

* provenance metadata;
* transformation documentation;
* dataset manifests;
* versioned processing steps.

### Verification

Trace a research result backward to the source dataset and transformation pipeline.

---

# 23. Threat T16 — Sensitive Data Exposure

### Description

Sensitive infrastructure or personal information is unintentionally exposed.

### Assets

* privacy;
* infrastructure security;
* research participants or populations if applicable.

### Likelihood

Low/Medium.

### Impact

High.

### Priority

Medium/High.

### Mitigations

* data minimization;
* aggregation;
* dataset classification;
* authorization;
* restricted APIs;
* controlled exports.

### Verification

Review API responses and research datasets for unnecessary sensitive fields.

---

# 24. Threat T17 — Information Leakage Through Errors

### Description

API errors reveal internal system details.

Examples:

* SQL statements;
* stack traces;
* filesystem paths;
* credentials;
* internal service addresses.

### Assets

* application security.

### Likelihood

Medium.

### Impact

Medium.

### Priority

Medium.

### Mitigations

* structured error responses;
* production exception handling;
* controlled server-side logging;
* security tests.

### Verification

Trigger representative application failures and inspect responses.

---

# 25. Threat T18 — Insecure CORS Configuration

### Description

Overly permissive CORS configuration allows unintended browser-based access.

### Assets

* authenticated API operations;
* user data.

### Likelihood

Medium.

### Impact

Medium/High.

### Priority

Medium.

### Mitigations

* explicit allowed origins;
* environment-specific configuration;
* no unnecessary wildcard origins.

### Verification

Test requests from unauthorized origins.

---

# 26. Threat T19 — Compromised Background Worker

### Description

A compromised or improperly configured worker executes unauthorized operations.

### Assets

* database;
* simulation infrastructure;
* research data.

### Likelihood

Low/Medium.

### Impact

Critical.

### Priority

Medium/High.

### Mitigations

* restricted worker permissions;
* validated task inputs;
* internal-only Redis;
* container isolation;
* no arbitrary command execution;
* worker monitoring.

### Verification

Confirm worker credentials and network permissions are limited to required operations.

---

# 27. Threat T20 — Redis Exposure

### Description

Redis is exposed to an untrusted network.

### Assets

* task queues;
* cached data;
* application integrity.

### Likelihood

Low for local development, higher for misconfigured deployments.

### Impact

High.

### Priority

Medium/High.

### Mitigations

* internal networking;
* authentication where appropriate;
* firewall rules;
* no public Redis port;
* restricted worker/application access.

### Verification

Network inspection confirms Redis is not publicly reachable.

---

# 28. Threat T21 — Database Exposure

### Description

PostgreSQL is directly exposed to an untrusted network.

### Assets

* all persistent data;
* research results;
* credentials.

### Likelihood

Low/Medium.

### Impact

Critical.

### Priority

High.

### Mitigations

* private network;
* firewall;
* least-privilege credentials;
* encrypted connections;
* restricted database users.

### Verification

Confirm external network access to the database is blocked.

---

# 29. Threat T22 — Malicious External Dataset

### Description

An external dataset contains malformed or deliberately malicious content.

### Assets

* ingestion pipeline;
* database;
* research results.

### Likelihood

Low/Medium.

### Impact

High.

### Priority

Medium.

### Mitigations

* schema validation;
* file validation;
* source provenance;
* checksums where appropriate;
* sandboxed processing for untrusted formats.

### Verification

Test ingestion with malformed and unexpected datasets.

---

# 30. Threat T23 — Supply-Chain Data Manipulation

### Description

An upstream data source changes its contents without clear versioning.

### Assets

* research reproducibility;
* experiment validity.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

* dataset snapshots;
* source metadata;
* retrieval timestamps;
* checksums;
* version identifiers;
* local preservation of required research inputs where licensing permits.

### Verification

Reproduce an experiment using the recorded dataset snapshot rather than retrieving the latest upstream data.

---

# 31. Threat T24 — Accidental Destructive Operation

### Description

A developer or researcher unintentionally deletes or changes important data.

### Assets

* datasets;
* experiments;
* results.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

* migrations;
* backups;
* versioning;
* protected production environments;
* restricted deletion permissions;
* confirmation for destructive operations.

### Verification

Test backup restoration and protected deletion behavior.

---

# 32. Threat T25 — Denial of Service

### Description

An attacker intentionally makes the service unavailable.

Potential vectors:

* repeated expensive simulations;
* excessive API requests;
* oversized payloads;
* graph abuse;
* database-heavy queries.

### Assets

* API availability;
* research execution capacity.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

* rate limiting;
* request size limits;
* workload limits;
* query timeouts;
* concurrency limits;
* monitoring.

---

# 33. Threat T26 — Frontend Trust Abuse

### Description

The system relies on frontend controls for security decisions.

Example:

```text id="ymt0n5"
Frontend hides "Run Experiment"
but backend allows anyone to call
POST /experiments/{id}/run
```

### Assets

* experiments;
* compute resources;
* protected data.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

All authorization and validation decisions must occur server-side.

---

# 34. Threat T27 — Logging of Secrets

### Description

Credentials or sensitive information are accidentally written to logs.

### Assets

* credentials;
* sensitive datasets.

### Likelihood

Medium.

### Impact

High.

### Priority

High.

### Mitigations

* structured logging;
* sensitive-field filtering;
* logging review;
* automated tests where practical.

---

# 35. Threat T28 — Vulnerable Container Image

### Description

A container image contains vulnerable or unnecessary components.

### Assets

* application;
* host environment;
* deployment.

### Likelihood

Medium.

### Impact

High.

### Priority

Medium.

### Mitigations

* minimal images;
* regular updates;
* vulnerability scanning;
* non-root execution where practical;
* restricted capabilities.

---

# 36. Threat Prioritization

Initial priority order:

| Threat                       | Priority    |
| ---------------------------- | ----------- |
| Credential Exposure          | High        |
| SQL Injection                | High        |
| Resource Exhaustion          | High        |
| Dataset Tampering            | High        |
| Scenario Tampering           | High        |
| Result Overwriting           | High        |
| Reproducibility Failure      | High        |
| Database Exposure            | High        |
| Denial of Service            | High        |
| Broken Authorization         | High        |
| Unauthorized API Access      | High        |
| Dependency/Supply Chain Risk | High        |
| Sensitive Data Exposure      | Medium/High |
| Worker Compromise            | Medium/High |
| Redis Exposure               | Medium/High |
| Malicious File Upload        | Medium/High |
| Path Traversal               | High        |
| Information Leakage          | Medium      |
| CORS Misconfiguration        | Medium      |
| Container Vulnerability      | Medium      |

Priorities should be revisited when the deployment model changes.

---

# 37. Security Control Matrix

| Threat Category         | Primary Control                      |
| ----------------------- | ------------------------------------ |
| Unauthorized access     | Authentication + authorization       |
| Injection               | Validation + parameterized queries   |
| Resource exhaustion     | Limits + timeouts + rate limiting    |
| Credential exposure     | Environment secrets + scanning       |
| Dataset tampering       | Versioning + provenance              |
| Scenario tampering      | Scenario versioning                  |
| Result overwriting      | Immutable experiment records         |
| Reproducibility failure | Execution metadata                   |
| File attacks            | Validation + controlled storage      |
| Database exposure       | Private networking + least privilege |
| Worker compromise       | Restricted workers                   |
| Information leakage     | Structured error handling            |
| Supply-chain risk       | Dependency management                |
| Data exposure           | Data minimization + access control   |

---

# 38. Abuse Cases

## Abuse Case A — Expensive Simulation Flood

```text
Attacker
   |
   +--> Repeated simulation requests
           |
           v
      CPU exhaustion
           |
           v
       API degraded
```

Controls:

* authentication;
* rate limiting;
* execution limits;
* concurrency controls.

---

## Abuse Case B — Dataset Manipulation

```text
Unauthorized User
       |
       v
Modify Dataset
       |
       v
Future Experiment
       |
       v
Invalid Research Result
```

Controls:

* authorization;
* dataset versioning;
* provenance;
* immutable experiment references.

---

## Abuse Case C — SQL Injection

```text
Attacker Input
      |
      v
API
      |
      X
Unsafe SQL
      |
      v
Database Compromise
```

Controls:

* Pydantic validation;
* SQLAlchemy parameterization;
* least-privilege database user;
* security testing.

---

## Abuse Case D — Secret Leakage

```text
Developer
   |
   v
Secret Added to Source
   |
   v
Git Repository
   |
   v
Credential Exposure
```

Controls:

* environment variables;
* `.gitignore`;
* secret scanning;
* repository review.

---

# 39. Research Integrity Threat Chain

Research-specific threats can form a chain:

```text
Dataset Change
     |
     v
Scenario Result Changes
     |
     v
Experiment Result Changes
     |
     v
Paper Figure Changes
     |
     v
Research Claim Changes
```

The system must make these dependencies visible.

Dataset, scenario, experiment, and result versions should therefore be linked.

---

# 40. Residual Risk

Some risks cannot be completely eliminated.

Examples include:

* incorrect external data;
* inaccurate dependency assumptions;
* incomplete geographic data;
* unknown infrastructure conditions;
* model simplification;
* incorrect recovery assumptions.

These are not purely cybersecurity threats, but they can affect research validity.

RESOLVE should address them through:

* uncertainty representation;
* sensitivity analysis;
* robustness testing;
* provenance;
* transparent assumptions.

Security controls cannot compensate for scientifically invalid assumptions.

---

# 41. Threat Model Evolution

The threat model should be updated when:

* authentication is introduced;
* public deployment begins;
* file uploads are added;
* external APIs are integrated;
* background workers are introduced;
* multi-user access is introduced;
* sensitive datasets are introduced;
* production infrastructure changes.

Each major architectural change should trigger a threat-model review.

---

# 42. Verification Strategy

Security controls should be verified through:

```text
Unit Tests
Integration Tests
API Tests
Dependency Scans
Secret Scans
Configuration Review
Container Scans
Network Checks
Backup Restoration Tests
Abuse Testing
```

Security verification results should be recorded in project evidence where they materially affect the research system.

---

# 43. Threat Model Acceptance Criteria

The threat model is considered sufficiently defined when:

* important assets are identified;
* trust boundaries are documented;
* realistic threat actors are considered;
* major API threats are covered;
* simulation-specific threats are covered;
* research-integrity threats are covered;
* data threats are covered;
* infrastructure threats are covered;
* mitigations are associated with threats;
* verification methods are defined;
* residual risks are acknowledged;
* threat-model evolution criteria are defined.

---

# 44. Guiding Threat-Model Principle

> **RESOLVE must protect both the system and the integrity of the evidence produced by the system; a secure research platform is not sufficient if its data, experiments, or results can be silently altered.**
