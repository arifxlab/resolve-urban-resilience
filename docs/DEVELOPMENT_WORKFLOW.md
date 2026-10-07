# RESOLVE — Development Workflow

## 1. Purpose

This document defines the engineering and research workflow for RESOLVE — Resilient Urban Systems & Vulnerability Observatory.

RESOLVE is both:

* a software engineering project, and
* a research implementation.

Development must therefore preserve both software quality and research validity.

The workflow is designed to ensure that:

* research questions guide implementation,
* architecture remains understandable,
* changes are traceable,
* experiments are reproducible,
* results are evidence-based,
* documentation remains synchronized with implementation,
* and the IEEE paper reflects actual completed work rather than planned claims.

---

# 2. Core Development Principle

The primary workflow is:

```text
Research Question
      ↓
Hypothesis
      ↓
Requirement
      ↓
Design Decision
      ↓
Implementation
      ↓
Test
      ↓
Measurement
      ↓
Experiment
      ↓
Evidence
      ↓
Documentation
      ↓
Paper
      ↓
Git Commit
      ↓
Git Push
```

No major research claim should bypass this chain.

---

# 3. Development Priorities

Development priority follows this order:

1. Research correctness
2. Domain correctness
3. Reproducibility
4. Automated testing
5. Data quality
6. Evaluation capability
7. Performance
8. Security hardening
9. API usability
10. Frontend presentation

A visually impressive interface must not be allowed to hide an incomplete research core.

---

# 4. Implementation Strategy

RESOLVE follows a backend-first implementation strategy.

The initial implementation order is:

```text
Domain Foundation
        ↓
Persistence
        ↓
Baseline Assessment
        ↓
Dependency Graph
        ↓
Simulation Engine
        ↓
Intervention Evaluation
        ↓
Research Engine
        ↓
API
        ↓
Frontend
```

This order may change when evidence demonstrates a better approach.

Changes to implementation order should be documented when they materially affect architecture or research execution.

---

# 5. Sprint-Based Development

Development is organized into explicit sprints.

Each sprint should have:

* a defined objective,
* a bounded scope,
* implementation tasks,
* tests,
* documentation requirements,
* acceptance criteria,
* evidence,
* Git commit,
* Git push.

A sprint should not remain open indefinitely because unrelated features were discovered.

New work should normally be moved into a later sprint unless it is required to complete the current objective.

---

# 6. Sprint Lifecycle

Each sprint follows:

```text
Sprint Definition
      ↓
Pre-Implementation Review
      ↓
Implementation
      ↓
Testing
      ↓
Verification
      ↓
Documentation
      ↓
Evidence
      ↓
Commit
      ↓
Push
      ↓
Sprint Complete
```

A sprint is complete only when its acceptance criteria are satisfied.

---

# 7. Sprint Definition

Before implementation begins, define:

* sprint number,
* objective,
* research relevance,
* requirements addressed,
* files/components affected,
* dependencies,
* expected tests,
* acceptance criteria,
* expected evidence.

Example:

```text
Sprint: 01
Objective: Establish backend application foundation

Research relevance:
Provide the executable foundation for the urban resilience research system.

Requirements:
ER-001
ER-005
ER-006

Acceptance:
Application starts,
health endpoint works,
configuration loads correctly,
tests pass.
```

The exact sprint scope should be recorded in the corresponding evidence document.

---

# 8. Pre-Implementation Review

Before writing code, verify:

1. The requirement is understood.
2. The relevant domain concept exists.
3. The architecture supports the change.
4. Existing documentation is sufficient.
5. The change does not unnecessarily introduce infrastructure.
6. Testing strategy is known.
7. Research implications are understood.

If a change exposes a missing architectural decision, document that decision before proceeding.

---

# 9. Source Code Workflow

Source code changes should follow a controlled workflow.

For each meaningful change:

1. Inspect the existing implementation.
2. Identify affected files.
3. Define the intended behavior.
4. Modify the complete source file.
5. Run formatting/linting where configured.
6. Run targeted tests.
7. Run the broader test suite when appropriate.
8. Inspect the resulting behavior.
9. Update documentation if behavior or architecture changed.
10. Record evidence.

The implementation must remain understandable to another engineer reading the repository.

---

# 10. Dependency Management

Dependencies must be introduced only when justified.

Before adding a dependency, consider:

* Is it required?
* Does the standard library already provide sufficient functionality?
* Does an existing dependency already solve the problem?
* Does it increase security or maintenance risk?
* Does it improve research reproducibility?
* Does it create deployment complexity?
* Is the license compatible with the project?

Each significant dependency should have an identifiable purpose.

Examples of planned technology directions include:

* FastAPI for HTTP API delivery,
* Pydantic for validation,
* SQLAlchemy for persistence,
* Alembic for database migrations,
* PostgreSQL/PostGIS for structured spatial data,
* NetworkX for graph processing,
* Pytest for testing.

Optional infrastructure such as Celery, Redis, OpenTelemetry, Prometheus, and machine-learning libraries should be introduced only when justified by requirements or measurements.

---

# 11. Domain-First Development

Business and research behavior should not be implemented directly inside HTTP route handlers.

The preferred separation is:

```text
API
 ↓
Application
 ↓
Domain / Research / Simulation
 ↓
Infrastructure
```

For example:

```text
POST /simulations
        ↓
Simulation Application Service
        ↓
Simulation Domain
        ↓
Dependency Graph
        ↓
Simulation Engine
        ↓
Persistence
```

This allows the same research logic to be used by:

* API requests,
* tests,
* command-line experiments,
* research scripts,
* batch execution.

---

# 12. Database Change Workflow

Database schema changes must use migrations.

The workflow is:

```text
Domain Change
      ↓
Model Change
      ↓
Migration
      ↓
Migration Review
      ↓
Database Upgrade
      ↓
Tests
```

Direct undocumented production-style schema modifications must not be used as the normal development workflow.

Each migration should have a clear purpose.

Destructive migrations require additional review and explicit evidence.

---

# 13. API Change Workflow

API changes must begin with the API contract.

For a new endpoint:

1. Define the resource.
2. Define request schema.
3. Define response schema.
4. Define validation behavior.
5. Define error behavior.
6. Implement application use case.
7. Connect the API layer.
8. Add tests.
9. Verify OpenAPI output.
10. Update `docs/API_CONTRACT.md` if required.

API routes should remain thin.

---

# 14. Simulation Development Workflow

Simulation logic requires additional discipline because simulation behavior directly affects research results.

A simulation change should include:

* behavioral definition,
* affected domain rules,
* deterministic test cases where possible,
* expected state transitions,
* cascade behavior,
* recovery behavior,
* performance implications,
* reproducibility implications.

Before changing simulation semantics, identify whether existing experiment results become invalid.

If results may no longer be comparable, the affected model/version must be clearly identified.

---

# 15. Baseline Development

The isolated baseline is a first-class research component.

It must not be implemented as a simplified version of the proposed system merely for convenience.

The baseline must represent the intended comparison method faithfully.

Changes to the baseline must be documented because they can alter experimental conclusions.

Baseline and proposed methods should use equivalent:

* input data,
* scenarios,
* interventions,
* resource budgets,
* outcome definitions,
* evaluation procedures,

where methodologically appropriate.

---

# 16. Experiment Development Workflow

Experiments should be treated as reproducible computational artifacts.

Before execution, define:

* research question,
* hypothesis,
* baseline,
* proposed method,
* dataset version,
* scenario,
* intervention set,
* budget,
* metrics,
* random seed,
* configuration.

Execution should produce:

* run ID,
* configuration metadata,
* execution metadata,
* results,
* runtime information,
* evidence artifacts.

Experiments should not be manually modified after seeing results in ways that invalidate the original comparison.

---

# 17. Metric Freeze

Primary metrics must be defined before the main evaluation.

If metrics are changed after results are observed:

1. record the change,
2. explain why it was necessary,
3. preserve previous results,
4. distinguish exploratory analysis from primary evaluation.

The primary metric must not be selected simply because it produces the most favorable result.

The current primary candidate is defined in `research/METRICS.md`.

---

# 18. Data Workflow

Data changes must preserve provenance.

For each research-relevant dataset, record where practical:

* source,
* version,
* acquisition date,
* license,
* transformation steps,
* spatial reference system,
* temporal coverage,
* quality information,
* missing-data treatment,
* uncertainty,
* derived-data relationship.

Raw source data should not be silently overwritten by processed data.

---

# 19. Data Quality Gates

Before research data enters a primary experiment, validate:

* schema,
* required fields,
* coordinate validity,
* CRS consistency,
* temporal consistency,
* duplicate records,
* missing values,
* invalid relationships,
* dependency integrity.

Data-quality failures should be recorded.

A dataset should not silently pass through validation merely because downstream code can tolerate the problem.

---

# 20. Testing Strategy

Testing occurs at multiple levels.

### Unit tests

Validate individual domain functions and components.

### Integration tests

Validate interactions between:

* application and database,
* graph construction and domain model,
* simulation and persistence,
* API and application services.

### API tests

Validate:

* request validation,
* response schemas,
* status codes,
* error handling,
* endpoint behavior.

### Simulation tests

Validate:

* state transitions,
* cascade propagation,
* recovery,
* intervention effects,
* deterministic behavior.

### Research tests

Validate:

* experiment configuration,
* metric calculations,
* baseline comparison,
* reproducibility metadata,
* result persistence.

### Performance tests

Validate important performance requirements defined in `docs/PERFORMANCE.md`.

---

# 21. Test Execution Rules

Before completing a meaningful sprint:

1. Run targeted tests.
2. Run the complete relevant test suite.
3. Inspect failures.
4. Fix root causes rather than hiding failures.
5. Repeat until acceptance criteria are satisfied.

A passing test suite does not automatically prove research validity.

Tests prove defined software behavior.

Experiments provide evidence for research claims.

---

# 22. Verification Hierarchy

Verification should progress from narrow to broad:

```text
Syntax
  ↓
Import
  ↓
Unit Tests
  ↓
Integration Tests
  ↓
API Tests
  ↓
Simulation Tests
  ↓
Full Test Suite
  ↓
Benchmark
  ↓
Experiment
```

Not every sprint requires every level.

The appropriate verification level should match the change.

---

# 23. Documentation Synchronization

Documentation is part of implementation.

Update documentation when:

* architecture changes,
* domain concepts change,
* API contracts change,
* data models change,
* research methodology changes,
* metrics change,
* experiments change,
* security assumptions change,
* performance targets change.

Documentation must describe the current system, not an ideal future system.

Planned features must be clearly distinguished from implemented features.

---

# 24. Research Documentation

Research documentation should be updated alongside implementation.

Relevant files include:

```text
research/
├── RESEARCH_QUESTION.md
├── HYPOTHESES.md
├── DATASETS.md
├── BASELINES.md
├── EXPERIMENTS.md
├── METRICS.md
├── RESULTS.md
└── REPRODUCIBILITY.md
```

A research document should never claim a result that has not actually been measured.

---

# 25. Evidence Documentation

Each completed sprint should have an evidence document.

Example:

```text
evidence/
├── SPRINT_00_EVIDENCE.md
├── SPRINT_01_EVIDENCE.md
├── SPRINT_02_EVIDENCE.md
└── ...
```

Evidence should record:

* objective,
* implementation,
* files changed,
* commands executed,
* tests,
* measured results,
* screenshots or artifacts where useful,
* known limitations,
* acceptance criteria,
* Git commit.

Evidence should be factual and reproducible.

---

# 26. Change Detection

Before a change is accepted, determine:

### What changed?

Identify the exact behavior, architecture, schema, or documentation change.

### Why did it change?

Connect the change to:

* requirement,
* bug,
* research need,
* performance evidence,
* security requirement,
* usability requirement.

### What technology was used?

Identify the relevant framework, library, infrastructure, or algorithm.

### What practical purpose does it serve?

Explain how the technology contributes to the system.

### How was it verified?

Record:

* tests,
* commands,
* benchmark,
* manual verification,
* experiment,
* or other evidence.

This creates an auditable engineering trail.

---

# 27. Git Workflow

Git is the source of truth for project history.

Before starting meaningful work:

```text
git status
```

After completing a sprint:

```text
git status
git diff
git add .
git commit
git push
```

Commits should describe meaningful completed changes.

Avoid commits such as:

```text
update
changes
fix stuff
work
final
```

Prefer:

```text
docs: define observability strategy
feat: add dependency graph domain model
test: add simulation cascade coverage
research: define primary evaluation experiment
```

The exact commit message should reflect the actual change.

---

# 28. Commit Boundaries

A commit should ideally represent one coherent unit of work.

Examples:

* one architectural foundation,
* one domain component,
* one migration,
* one simulation capability,
* one research experiment,
* one documentation milestone.

Large mixed commits should be avoided when practical.

---

# 29. Git Push Policy

Completed sprint work should be pushed to the remote repository.

The purpose is to preserve:

* project history,
* recovery points,
* research provenance,
* collaboration capability,
* evidence of development progression.

Uncommitted or unpushed work should not be treated as a completed sprint.

---

# 30. Branching Strategy

Early development may use a simple main-branch workflow when the project is developed by a single primary contributor.

Feature branches may be introduced when:

* parallel work begins,
* experimental changes need isolation,
* risky architectural changes are being evaluated,
* external collaboration is introduced.

Branching complexity should not exceed project needs.

---

# 31. Pull Requests and Review

When pull requests are used, review should consider:

* correctness,
* architecture,
* security,
* tests,
* research implications,
* reproducibility,
* documentation,
* performance,
* unnecessary complexity.

Research-sensitive changes require additional attention to experimental comparability.

---

# 32. Paper Synchronization

The IEEE paper is developed alongside the implementation.

The workflow is:

```text
Implementation
      ↓
Measurement
      ↓
Evidence
      ↓
Research Documentation
      ↓
Paper Update
```

The paper must not become a speculative design document disguised as completed research.

If a feature is planned but not implemented, it belongs in future work or methodology discussion rather than as a completed result.

---

# 33. Paper Claim Discipline

Every major paper claim must have supporting evidence.

Examples:

### Engineering claim

> The system processes scenarios within X seconds.

Required evidence:

* benchmark configuration,
* environment,
* measurement method,
* observed runtime.

### Research claim

> Dependency-aware prioritization improves cascading impact reduction.

Required evidence:

* baseline,
* proposed method,
* equivalent scenario conditions,
* defined metric,
* experiment results,
* statistical/practical analysis where appropriate.

Claims without evidence must not be presented as established findings.

---

# 34. Handling Negative Results

Negative, neutral, or inconclusive results are valid outcomes.

The workflow must not encourage changing:

* metrics,
* scenarios,
* baselines,
* intervention budgets,
* model assumptions,

solely to obtain a favorable result.

Unexpected results should trigger investigation.

If the hypothesis is not supported, the result should be documented honestly.

---

# 35. Handling Experimental Changes

If an experiment changes after initial execution:

1. Preserve the original configuration.
2. Assign a new run ID.
3. Record the change.
4. Explain why it changed.
5. Avoid silently replacing old results.
6. Determine whether the new run remains comparable.
7. Update research documentation.

Experimental history must remain traceable.

---

# 36. Reproducibility Workflow

A reproducibility check should attempt to reconstruct an experiment from:

```text
Git Commit
+
Dataset Version
+
Scenario Version
+
Configuration
+
Model Version
+
Random Seed
+
Environment
```

The objective is to reproduce the computational procedure and determine whether equivalent results are obtained within expected tolerance.

Reproducibility should be tested before major research conclusions are finalized.

---

# 37. Security Workflow

Security must be integrated into normal development.

Before accepting a change, consider:

* input validation,
* authorization,
* secret handling,
* dependency risks,
* file handling,
* resource exhaustion,
* logging exposure,
* database safety,
* external data trust.

Security requirements are defined in:

* `docs/SECURITY_MODEL.md`
* `docs/THREAT_MODEL.md`

---

# 38. Performance Workflow

Performance work follows:

```text
Requirement
   ↓
Measurement
   ↓
Bottleneck Identification
   ↓
Optimization
   ↓
Regression Test
   ↓
Benchmark
   ↓
Evidence
```

Performance optimization without measurement should be avoided.

An optimization that changes research semantics must not be accepted merely because it is faster.

---

# 39. Refactoring Workflow

Refactoring should preserve externally observable behavior unless the purpose of the refactor is explicitly to change behavior.

For research-sensitive code:

1. Establish test coverage.
2. Record relevant baseline behavior.
3. Refactor.
4. Run tests.
5. Compare important metrics.
6. Confirm experiment compatibility.
7. Document material differences.

Refactoring must not silently change simulation semantics.

---

# 40. Technical Debt

Technical debt should be recorded rather than forgotten.

A technical debt item should identify:

* problem,
* affected component,
* impact,
* reason it was deferred,
* suggested resolution,
* priority.

Technical debt is acceptable when consciously managed.

Untracked technical debt is not.

---

# 41. Architecture Decision Workflow

Material architecture decisions should be recorded in:

`docs/DECISION_LOG.md`

A decision should include:

* context,
* problem,
* options considered,
* selected option,
* rationale,
* consequences,
* status.

The purpose is to preserve why the architecture evolved.

---

# 42. Feature Acceptance

A feature is complete only when:

* implementation exists,
* tests exist where appropriate,
* documentation is updated,
* acceptance criteria are satisfied,
* relevant performance/security considerations are addressed,
* research implications are understood,
* evidence exists,
* Git history is preserved.

A partially implemented feature should be clearly marked as incomplete.

---

# 43. Definition of Done

A sprint or feature is considered **Done** when:

* [ ] Scope is completed.
* [ ] Acceptance criteria pass.
* [ ] Relevant tests pass.
* [ ] No known critical regression remains.
* [ ] Documentation reflects the implementation.
* [ ] Research documentation is updated where applicable.
* [ ] Evidence is recorded.
* [ ] Performance impact is understood where relevant.
* [ ] Security impact is understood where relevant.
* [ ] Git commit exists.
* [ ] Git push is complete.

---

# 44. Development Anti-Patterns

RESOLVE should avoid:

### Feature-first development

Building features without connecting them to research or requirements.

### Dashboard-first development

Building visualization before the analytical core works.

### Framework-first architecture

Letting a framework determine the domain model.

### AI-first development

Adding machine learning or LLM components without a research or engineering need.

### Metric shopping

Selecting metrics after observing which ones produce favorable results.

### Baseline manipulation

Weakening the baseline to make the proposed method appear stronger.

### Silent simulation changes

Changing cascade or recovery semantics without versioning or documentation.

### Documentation drift

Allowing documentation to describe a system that no longer exists.

### Unmeasured optimization

Adding complexity without evidence of a performance problem.

### Premature infrastructure

Introducing distributed infrastructure before the workload requires it.

---

# 45. Workflow Evolution

This workflow is intentionally iterative.

It may evolve as RESOLVE progresses from:

```text
Local Prototype
      ↓
Research Prototype
      ↓
Evaluated Research System
      ↓
Hardened Demonstration
```

Each stage may require stronger:

* testing,
* observability,
* reproducibility,
* performance measurement,
* security,
* deployment practices.

The workflow should grow with the actual system rather than anticipate every possible future requirement.

---

# 46. Guiding Principle

RESOLVE development follows one central rule:

> **Build what the research requires, verify what was built, measure what matters, preserve the evidence, and never let the paper claim more than the system demonstrates.**
