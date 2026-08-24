---
title: ALDS Task Profile Standard
doc_type: standard
status: active
scope: ALDS
updated: 2026-08-10
version: 1.1.0
related_docs:
  - .AI/standards/workflow_engine.md
  - .AI/standards/enums.md
  - .AI/standards/alds_standards.md
  - .AI/start.md
---

# ALDS Task Profile Standard

## 1. Positioning

Task Profile is **not a new process**, but a **one-time variable determination step after intent routing, before minimum context loading**.

Its sole purpose: turn conditional rows in workflow required documents tables (depending on `task_type` / `target_area` / `change_scope` / `risk_level` / `active_language`) **from fuzzy judgment into determined evaluation**.

> **Authoritative Boundary**: Workflow required documents table remains the "what to read" sole authority (see `start.md` §5, `workflow_engine.md` §minimum reading rules). Task Profile does not replace required table, only responsible for computing table condition variables clearly, and providing negative list to narrow loading.

## 2. Five-Field Profile

| Field | Source | Value Domain | Determines |
| :--- | :--- | :--- | :--- |
| `task_type` | User request + `enums.md` §task types | `feature` / `bugfix` / `refactor` / `review` / `optimization` / `testing` / `governance` / `documentation` / `technical_debt` | Workflow selection, base read set, skills main axis |
| `target_area` | Request/target file paths | Module name or file path | Which `specs/`, `architecture/` sub-docs to load |
| `change_scope` | Change surface | `single_point` / `single_module` / `cross_module` / `architecture_level` | Whether to load `architecture/`, `invariants/`, `guards/` |
| `risk_level` | `enums.md` §6.1 quantification criteria | `low` / `medium` / `high` / `critical` / `unknown` | Task path (simple/engineering), whether to load invariants/guards |
| `active_language` | `workflow_engine.md` §4.1 | One of project languages | Which language-specific skill to load |

## 3. Determination Steps (Only from Task Description + project.yaml)

This step **reads no `.AI/project/` subdirectories**, only uses already-mandatory-read `project.yaml` and user request:

1. Read `project.yaml` (already forced read per §5) → get `languages`
2. Extract `task_type` from user request, map to `enums.md` task type enum; multiple candidates take **narrower scope or higher risk** and record reason
3. Extract `target_area` (module/path) from request or target files
4. Define `change_scope` from expected change surface
5. Determine `risk_level` using `enums.md` §6.1 quantification criteria; Agent self-assessment conflicts with objective criteria take higher tier and record reason
6. Determine `active_language` per `workflow_engine.md` §4.1

Output profile quintuple, write into task brief/work order "Task Profile" field. Profile determined then serves as input for required table condition evaluation.

## 4. Profile → Expected Minimum Read Set (Lookup, Reduces Misjudgment)

Below table shows typical profiles' **expected results after evaluating workflow required table conditions**. Final read set still per workflow required table; this table suppresses "just in case" over-loading.

| Profile | Expected Read Set | Expected No-Read |
| :--- | :--- | :--- |
| `bugfix` · `single_module` · `≤ medium` · no invariant/guard touch | `project.yaml`, `enums`, `code_change_audit_protocol`, `code_review_checklist`, `specs/{module}` | `architecture/`, `invariants/`, `guards/`, `process/`, `roadmap/`, any skill |
| `feature` · `cross_module` · `high` · touches invariant | Above base + `architecture/`, `invariants/`, `guards/`, most relevant 1 general skill + 1 language-specific | `roadmap/` (unless milestone change) |
| `refactor` · `single_module` · `≤ medium` | Base + `specs/{module}` + `refactoring` skill (+language-specific) | `architecture/`, `invariants/`, `guards/` |
| `governance` | Base + `architecture/`, `process/`, `roadmap/` | Business `specs/` (unless directly relevant) |

## 5. Negative List Principle (Narrow Read Set Lower Bound)

1. **Off-table no load**: Documents not listed in workflow required documents table never loaded (`start.md` §5.4 enforced).
2. **Subdirectories only on direct hit**: `.AI/project/` subdirectories only loaded when profile fields **directly hit**; "possibly relevant" "just in case" not loading basis.
3. **No reverse补读**: After profile determined, must not补读 due to uncertainty; if补读 truly needed must record deviation per `alds_standards.md` §28.
4. **Simple tasks stricter**: When task judged simple task (see `start.md` §6.1), §4 table "Expected No-Read" column upgrades to mandatory no-read.

## 6. Skills Narrowing

1. One task defaults to max **1 general skill + 1 language-specific skill**, taking most relevant to `task_type` main axis.
2. Only when task **explicitly contains** second skill domain simultaneously, load second general skill, and record reason in task brief/work order.
3. When `active_language` undetermined, must not reverse-infer language just because language-specific file exists (consistent with `workflow_engine.md` §8.3).

> This section is `workflow_engine.md` §8.2-8.3 hit count constraint authority.

## 7. Relationship with Task Path

Profile's `risk_level` + `change_scope` + effort/change volume + whether part of existing engineering task, together determine task path (simple/engineering), see `start.md` §6.1. Profile itself does not decide path, only provides input fields for path judgment.