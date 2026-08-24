---
title: Validation Matrix Design
doc_type: process
status: active
scope: ALDS
updated: 2026-08-10
description: Rules for designing verification coverage across affected dimensions
related_docs:
  - .AI/process/review/testing_completeness_review.md
---

# Validation Matrix Design Specification

This specification defines ALDS verification matrix design methods. Used when task affects multiple runtime conditions, data conditions, config conditions, or deliverables to clarify verification coverage scope, coverage levels, verification evidence, and uncovered risks.

This spec does not define specific project test tools, commands, environment versions, or deployment platforms; specific values determined by project context, task brief, and work order.

## Boundary with Testing Completeness Review

- **This spec (validation_matrix)** addresses "**planning stage** — what to verify, to what granularity, how to annotate abandoned combinations". It is a **planning & risk explicitization tool** for task brief/work order stages.
- **Testing Completeness Review (testing_completeness_review)** addresses "**implemented test suites** — are they adequate across boundary/exception/security/performance/business five dimensions". It is a **review tool** for `code_review` or `feature_development` acceptance stages.
- They differ in focus on "functional completeness & boundary values" and "exception handling & fault tolerance": this spec focuses on **verification combination coverage** (which environments/data/configs/entries need testing), testing completeness focuses on **test case collection depth** (are boundary values, exception paths, adversarial inputs tested).
- Task brief may reference both: planning stage uses this spec to plan matrix; execution/acceptance stage uses testing completeness review to diff matrix against test cases.

## Applicability Conditions

Task brief or work order must plan verification matrix when any condition met:

- Changes cross multiple modules, platforms, runtimes, data sources, configs, or deliverables
- Changes involve persistence, permissions, concurrency, cache, network, build, packaging, deployment, or release
- Changes modify shared libraries, public APIs, protocols, schemas, migration logic, or compatibility boundaries
- Defects or risks only appear in specific environments, data states, permission combos, or config combos
- Task type is `optimization`, `testing`, `technical_debt`, and analysis conclusion requires verifying multiple impact surfaces
- Task involves CI, E2E, integration tests, build artifacts, or release artifacts

Low-risk, single-point, pure copy or pure format changes may omit full matrix, but must explain why verification matrix not applicable and retain minimum verification method.

## Verification Dimensions

Verification matrix should select dimensions per actual task impact. Common dimensions:

| Dimension | Examples |
| :--- | :--- |
| Functional Level | unit, integration, e2e, manual |
| Runtime Environment | OS, runtime, browser, device, CPU architecture |
| Data Layer | database, schema, migration, seed data, cache |
| Config Layer | feature flag, env var, permission, locale, tenant |
| Entry Layer | API, UI, CLI, background job, webhook |
| State Layer | empty data, existing data, upgraded data, error state |
| Build/Release Layer | build, package, artifact, deployment |
| Permission Boundary | internal, external contributor, limited token, readonly mode |

Must not list irrelevant dimensions for formality. Each dimension must explain its relation to task risk or behavioral outcome.

### Distributed & Concurrent Systems Supplementary Dimensions

When task involves distributed, concurrent, persistence, or consistency systems, besides above common dimensions, should additionally cover following dimensions (hit = include in matrix):

| Dimension | Examples |
| :--- | :--- |
| Event Sequence Layer | Message reorder, duplicate, loss, disorder, network partition, clock drift |
| Fault Injection Layer | Node crash, process kill, disk full, GC pause, dependency timeout, network disconnect |
| Concurrency Interleaving Layer | Race conditions, deadlocks, livelocks, read-write concurrency, transaction isolation conflicts |
| Recovery Layer | Post-restart state consistency, WAL replay, replica re-sync, idempotent retry, compensation |

### Distributed & Concurrent Verification Methodology

Correctness of above dimensions often cannot be exhausted by deterministic cases; should select methods per system nature, verification evidence must be reproducible:

- **Property-Based Testing**: Generate random inputs/operation sequences against invariants, assert invariants always hold.
- **Fault Injection (Chaos / Jepsen-style)**: Inject node-level faults during runtime (crash, partition, clock jump), verify post-fault consistency.
- **Concurrency Stress + Invariant Assertion**: High concurrency load then verify data consistency invariants unbroken.
- **Formal Specs & Model Checking (see `.AI/project/init.md` §8.2 Invariants Formalization)**: Reachability analysis for key protocols (consensus, replication, transactions).

> For `critical` risk consistency/persistence systems, must use at least two above methods, one must be fault injection or formal verification; unit tests alone cannot record that dimension as `passed`.

## Coverage Levels

Verification matrix must declare coverage level:

| Level | Meaning |
| :--- | :--- |
| `full` | All key dimension combinations covered |
| `representative` | Each key dimension at least one representative combination covered |
| `risk-based` | Prioritize high-risk combinations, others record uncovered risks |
| `smoke` | Only verify main path, build availability, or minimal behavioral closure |
| `not_applicable` | Verification matrix not applicable, record reason |

Coverage level must match task risk. High-risk tasks must not use `smoke` or `not_applicable` without reason.

## Matrix Recording Format

Task brief or work order verification matrix recommended format:

| Dimension | Coverage Value | Verification Method | Coverage Level | Result | Evidence | Uncovered Risk |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|  |  |  |  | `pending` |  |  |

Field rules:

- `Dimension`: Impact category, e.g., data layer, config layer, entry layer
- `Coverage Value`: Specific items or abstract values covered this run
- `Verification Method`: Test, build, review, manual acceptance, or other verifiable method
- `Coverage Level`: Use coverage levels defined in this spec
- `Result`: `pending`, `passed`, `failed`, `blocked`, `deferred`, `not_applicable`, `accepted_risk`
- `Evidence`: Logs, reports, command output summaries, screenshots, review records, artifact paths
- `Uncovered Risk`: Unexecuted combos, reasons, residual risks

## Downgrade Rules

When verification cannot fully execute, must explicitly downgrade, must not disguise as passed:

| State | Use Condition |
| :--- | :--- |
| `blocked` | Environment, permissions, data, or dependencies unavailable, verification cannot execute |
| `deferred` | Verification executable but cost/timing unsuitable for current task, needs follow-up task |
| `not_applicable` | Dimension unrelated to current task |
| `accepted_risk` | User or process explicitly accepts uncovered risk |

Prohibitions:

- Must not write unexecuted verification as `passed`
- Must not substitute "self-tested" for matrix coverage explanation
- Must not omit high-risk uncovered combinations
- Must not write human review as runnable test substitute, unless task nature truly only allows human confirmation

### Downgrade Decision Tree

Judge in sequence, hit = stop, must not skip:

1. Dimension unrelated to task? → `not_applicable` (must record exclusion reason)
2. Environment/permissions/data/dependencies unavailable, verification **cannot execute**? → `blocked`
3. Executable but cost/timing unsuitable for this task, needs follow-up? → `deferred` (must register `reports/active/debt/`)
4. Executed but has explicitly accepted uncovered risks? → `accepted_risk`

`blocked` vs `deferred` distinction: Verification **cannot execute** → `blocked`; verification **can execute but not suitable for this task** → `deferred`. Both true simultaneously → `blocked` (cannot execute takes priority).

### accepted_risk Human-AI Boundary

- `accepted_risk` **must be explicitly accepted by human user**; accepter, time, reason all three required.
- Agent **must not self-accept risk**. If judges uncovered risk but no human acceptance, item must be `blocked` or `deferred`, stop and wait for user adjudication, must not self-fill `accepted_risk`.
- `critical`/`high` risk dimension `accepted_risk` must be user-confirmed item by item, no batch acceptance.

## Process Integration

Verification matrix advances in ALDS by stages:

1. Task brief stage: Define expected verification dimensions, coverage levels, high-level verification methods
2. Work order stage: Clarify matrix items this execution covers, verification methods, evidence sources
3. Execution stage: Update matrix if code changes expand impact surface
4. Verification stage: Fill results, evidence, uncovered risks
5. Archive stage: Convert uncovered risks to project pulse, technical debt records, or follow-up tasks

Must not mark related work order or task complete before forming verification results and uncovered risk records.