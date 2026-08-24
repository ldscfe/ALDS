---
title: ALDS Glossary
doc_type: standard
status: active
scope: ALDS
updated: 2026-08-10
related_docs:
  - .AI/standards/alds_standards.md
  - .AI/standards/enums.md
  - .AI/standards/workflow_engine.md
---

# ALDS Glossary

This table centrally defines key terms used throughout ALDS. Each term's **authoritative definition** remains with the corresponding standard document; this table unifies semantics and eliminates ambiguity.

## 1. Framework & Positioning

| Term | Definition | Authoritative Source |
| :--- | :--- | :--- |
| ALDS | AI-driven Lifecycle Delivery System, document-first AI collaborative delivery governance framework. Is process governance protocol, not runtime/software package/build tool, does not make engineering judgments. | `alds_standards.md` §1–§3 |
| Process Governance Protocol | Rule set constraining AI-assisted development in traceable, verifiable, auditable delivery closed loop; contains no business rules, language implementation details, test/release commands. | `alds_standards.md` §1 |
| Engineering Judgment | Data structures, algorithms, consistency models, protocol selection, etc. design decisions; ALDS does not make these for Agent, must be provided by `.AI/project/` context or humans. | `alds_standards.md` §1, §10 |
| Task Path | `simple` (simple task, single work order, add line to `simple_tasks.md` after completion) or `full` (engineering task, task brief + work order). Judgment in `start.md` §6.1. | `start.md` §6.1, `enums.md` §13 |

## 2. Facts & Contracts

| Term | Definition | Authoritative Source |
| :--- | :--- | :--- |
| Project Fact Source | `.AI/project/project.yaml`, sole source of structured project facts. | `alds_standards.md` §10 |
| Instance Fact Source Status | `absent` / `empty_or_incomplete` / `initialized`, determines entry routing. | `enums.md` §11, `index.md` §1.1 |
| Contract | Behavioral agreements of what system should do, source is `.AI/project/specs/`. | `alds_standards.md` §3, §10 |
| Specification | Behavioral contract, interface rules, error rules, boundary conditions definitions. Implementing spec-undefined behavior as deterministic behavior is prohibited (except cold start definitions). | `alds_standards.md` §6/L2, §27, §27.1 |
| Invariant | Unbreakable system-level guarantees, stored in `.AI/project/invariants/`. Consistency/persistence/concurrency systems strongly recommended to attach formal statements (TLA+/Alloy, etc.). | `init.md` §8.1–§8.2 |
| Guard | Forbidden patterns, security boundaries, high-risk prohibited zones, stored in `.AI/project/guards/`. | `alds_standards.md` §10, `init.md` §8.1 |

## 3. Control Documents

| Term | Definition | Authoritative Source |
| :--- | :--- | :--- |
| Task Brief | Task-level control document, defines objective/scope/non-goals/workflow/verification methods; placed in `reports/active/brief/`. | `alds_standards.md` §15 |
| Work Order | Execution-level control document, defines specific what to change/how to change/how to verify; placed in `reports/active/plan/`. Simple tasks directly generate work order, no task brief. | `alds_standards.md` §16 |
| Simple Task List | Line-by-line ledger of completed simple tasks (one line/task, newest on top); fixed at `reports/simple_tasks.md`. Simple task's only ledger record, not recorded in project pulse. | `alds_standards.md` §16, `start.md` §6.2, §10 |
| Session Log | Line-by-line ledger of substantive user instructions + AI response summary + metadata (environment/model/session/repo status) (one line/instruction, newest on top); fixed at `reports/session_log.md`. Only records substantive instructions, no auto-commit. | `start.md` §12 |
| Project Pulse | Project-level task brief ledger (not phase task brief, not work order list); fixed at `reports/project_pulse.md`. | `alds_standards.md` §17 |
| Write Plan | Plan that must be listed and approved before modifying `.AI/project/` (target files/section summaries/fact sources/inferred items/pending confirmations/change scope). | `alds_standards.md` §12, §20 |

## 4. Process & Routing

| Term | Definition | Authoritative Source |
| :--- | :--- | :--- |
| Workflow | Specialized execution tracks in `.AI/workflows/`; their "required documents" table is sole authoritative basis for loading context. | `alds_standards.md` §14, `workflow_engine.md` |
| Minimum Context | Only read minimum relevant documents needed for judgment and execution; must not load entire context for convenience. | `alds_standards.md` §13, `start.md` §5 |
| READ_SET | AI-maintained set of already-read documents in conversation (not persisted), for deduplication, passes whole across workflow routing without reset. | `workflow_engine.md` §7 |
| Fallback Contract | Minimum variable set that must be passed during inter-workflow fallback (`task_brief_id`/`current_step`/`block_reason`/`evidence`/`retry_hint`). | `workflow_engine.md` §fallback contract |
| Suspend | Status node appended to end of task brief or work order due to failure/block/unable to continue, containing three elements needed for recovery. | `error_handling.md` §1–§4 |
| Operational Intent Command | Three meta-task types: summary/next steps/conclusion; shortcuts dev closed loop; vocabulary and format solely per `skills/operational.md`. | `skills/operational.md`, `start.md` §3.0 |

## 5. Verification & Risk

| Term | Definition | Authoritative Source |
| :--- | :--- | :--- |
| Machine-First Verification | Runnable, reproducible verification evidence takes priority over subjective explanations; manual review cannot substitute runnable verification. | `alds_standards.md` §5, §21 |
| Verification Matrix | Coverage table planned when task affects multiple dimensions (dimension/coverage value/verification method/coverage level/result/evidence/uncovered risk). | `validation_matrix.md` |
| Coverage Level | `full` / `representative` / `risk-based` / `smoke` / `not_applicable`. | `validation_matrix.md` §coverage level |
| Downgrade | Explicitly recorded status choice when verification cannot fully execute (`blocked`/`deferred`/`not_applicable`/`accepted_risk`), judged per decision tree. | `validation_matrix.md` §downgrade decision tree, `error_handling.md` §2 |
| accepted_risk | Uncovered risk explicitly accepted by human; Agent must not self-accept, must have accepter/time/reason. | `validation_matrix.md` §accepted_risk human-AI boundary |
| Risk Quantification Criteria | Determines `critical`/`high`/`medium`/`low`/`unknown` per objective triggers (funds/persistence/consensus/invariants/guards, etc.). | `enums.md` §6.1, `alds_standards.md` §21 |

## 6. Status Semantics (Common Confusion Points)

| Term | Definition | Authoritative Source |
| :--- | :--- | :--- |
| `done` | **Execution complete** unified status (project pulse, task brief, work order, milestone completions all use `done`). | `enums.md` §3, `alds_standards.md` §18 |
| `archived` | **Document physical archive location or document's own static lifecycle**, must not be used as execution complete status. | `enums.md` §2, `alds_standards.md` §18 |
| `completed` | Document status: task brief executed complete and archived. | `enums.md` §2 |
| `implemented` | Analysis report identified items all routed (task brief created/technical debt registered/rejected), independent of whether task brief itself complete. | `enums.md` §2, §26 |

> State machine visualization in `enums.md` §14.