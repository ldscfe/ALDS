---
title: Project Initialization Flow
doc_type: process
status: active
scope: project
updated: 2026-05-26
description: Initialization flow for building project governance context from source requirements, design documents, and references
related_docs:
  - .AI/templates/project.yaml.template
  - .AI/project/index.md
  - .AI/standards/enums.md
  - .AI/source/
  - .AI/source/requirements/
  - .AI/source/design/{Project}_design_vx.x.x.md
  - .AI/source/references/
  - .AI/templates/task_brief_template.md
  - .AI/templates/work_order_template.md
  - .AI/templates/project_pulse_template.md
---

# Project Initialization Flow

`.AI/project/init.md` is used only to establish or complete `.AI/project/` project governance baseline from raw requirements, designs, and supplementary materials in `.AI/source/`. Standard development, fix, review, optimization, test, and governance evolution do not execute here; should return to `.AI/start.md`.

## 1. Entry Conditions

Before entering this flow, must have read:

1. `.AI/index.md`
2. If exists, `.AI/project/project.yaml`

Applicable inputs:

- User explicitly requests initializing project governance context
- User provides raw requirements, designs, or references and requests generating `.AI/project/` baseline
- `.AI/project/project.yaml` does not exist, need to create project instance fact source from template
- `.AI/project/project.yaml` still empty or clearly incomplete, need to backfill from source materials

If input does not satisfy above, pause flow, ask user for explicit confirmation.

### 1.1 Project Fact Source Creation Rules

ALDS standard package only retains template `.AI/templates/project.yaml.template`; must not carry `.AI/project/project.yaml` as project instance fact source.

After entering initialization flow, must first check `.AI/project/project.yaml` and output pre-initialization check results (see §1.2), then decide next step per results:

- If not exist (`absent`): Inform user will create blank `project.yaml` from template, wait for user confirmation before creating.
- If exists but empty or incomplete (`empty_or_incomplete`): Inform user current fact source incomplete, suggest backfilling from source materials, wait for user confirmation.
- If exists and initialized (`initialized`): Must pause and require user to choose `overwrite`, `merge`, or `stop`.
- Before user confirms or chooses, must not modify `.AI/project/project.yaml` or any `.AI/project/`, `reports/` files.

> Creating blank `project.yaml` from template is preparatory action but still requires user confirmation, unless user explicitly requests initialization and has approved creating instance fact source. Backfilling project facts must enter task brief, work order, and write plan approval.

Choice semantics:

- `overwrite`: Recreate `.AI/project/project.yaml` from template, then backfill from source materials.
- `merge`: Retain existing `.AI/project/project.yaml`, only fill missing fields or resolve conflicts in subsequent approved steps.
- `stop`: Stop initialization, do not modify files.

### 1.2 Pre-Initialization Check Results

After entering initialization flow, must first output following structured check results, then wait for user confirmation, must not auto-execute suggested action:

```
Pre-initialization Check Results:

- Fact Source Status: `absent` / `empty_or_incomplete` / `initialized`
- Fact Source Path: `.AI/project/project.yaml`
- Template Path: `.AI/templates/project.yaml.template`
- Source Materials: (user-specified or auto-discovered source material paths)
- Governance Mode Suggestion: Fixed as `full` (task weight no longer determined by project-level `governance_mode`, but by `start.md` §6.1 task path judgment)
- Suggested Action: (based on fact source status, e.g., "create blank fact source from template" or "backfill from source materials")
- Pending Confirmations: (list of items requiring user confirmation)
```

## 2. Required Documents

> Load rules: Read in order, skip already-read documents (READ_SET deduplication).

| Order | Document | Condition |
| :--- | :--- | :--- |
| 1 | `.AI/templates/project.yaml.template` | When `.AI/project/project.yaml` not exist, for creating instance fact source |
| 2 | `.AI/project/project.yaml` | Read when exists; when not exist, create from template per §1.1 then read |
| 3 | `.AI/project/index.md` | Always |
| 4 | `.AI/project/init.md` | Always (this flow) |
| 5 | `.AI/standards/enums.md` | When first need to write status or enum values |
| 6 | User-specified source materials | Mandatory when user specified |
| 7 | Relevant requirements in `.AI/source/requirements/` | When user not specified, select per §2.1 rules |
| 8 | Relevant designs in `.AI/source/design/` | When user not specified, select per §2.1 rules |
| 9 | Relevant references in `.AI/source/references/` | When user not specified, select per §2.1 rules |
| 10 | `.AI/templates/task_brief_template.md` | Before generating initialization task brief |
| 11 | `.AI/templates/work_order_template.md` | Before splitting work orders |
| 12 | `.AI/templates/project_pulse_template.md` | Before generating project pulse |
| 13 | Existing `.AI/project/` docs directly related to generation targets | Load on demand per §5 initialization scope identification results |

### 2.1 Source Material Discovery Rules

If user specified source materials, must prioritize reading user-specified files. If user not specified, discover candidate source materials in following order:

1. Requirements in `.AI/source/requirements/` with body explicitly matching project name, goals, scope, or business rules to `.AI/project/project.yaml` or user input
2. Designs in `.AI/source/design/` with body explicitly matching project name, architecture, modules, interfaces, or interactions to `.AI/project/project.yaml` or user input
3. Supplementary materials in `.AI/source/references/` directly related to project initialization goals
4. Source materials with filenames `{Project}_{kind}_vx.x.x.md` or `{Project}_design_vx.x.x.md` where `{Project}` matches user input or candidate project name
5. Source materials semantically closest to project initialization goals
6. Highest version source materials
7. Most recently updated source materials

If still cannot uniquely determine after sorting, pause initialization and ask user to specify source materials, must not auto-select.

### 2.2 Cold Start Without Source Materials

When `.AI/source/` empty and user provided no requirements/design documents, but explicitly requests developing a system, must not directly judge as uninitializable and terminate. Handle per cold start branch:

1. **Structured Requirements Interview**: Agent asks minimal question set per `architecture/specs/invariants/guards` dimensions—system boundaries & external dependencies, core entities & data models, consistency requirements (strong/eventual), durability requirements (tolerable data loss window), concurrency model, critical failure semantics.
2. **Persist as Source Materials**: Write interview conclusions to `.AI/source/requirements/` (e.g., `cold_start_interview_{YYYYMMDD}.md`), each fact tagged source "cold start interview", items not user-confirmed marked "pending confirmation".
3. **Generate Minimum Context**: Per §5–§10 generate minimum `.AI/project/` context and `reports/project_pulse.md`.
4. **Engineering Decisions Not by Agent**: Critical engineering selections (data structures, algorithms, consistency/consensus protocols, storage formats, etc.) if user cannot answer, must mark "pending human decision" and register `reports/active/debt/`, Agent must not decide unilaterally. Distinguish: **spec definition** (writing pending behaviors into `specs/`, compliant, see `alds_standards.md` §27.1) vs **engineering selection** (choosing among valid solutions, requires human).

> Cold start produced context must explicitly tagged as interview-based not existing documents; risk level default not below `medium`, critical modules default `high`/`critical` (per `enums.md` §6.1).

## 3. Fact Source Priority

1. Explicit facts in source material body
2. Business goals, scope, non-goals, acceptance criteria in requirements
3. Architecture, modules, interfaces, interactions, technical constraints in designs
4. Explicit background, meeting conclusions, screenshots, research facts in references
5. Structured info in source material filenames, e.g., `{Project}_design_vx.x.x.md`
6. Explicit supplements in user initialization instruction

Body facts take priority over filename inference. Still unconfirmable content must be marked pending confirmation, must not guess as fact.

## 4. Flow Overview

`Source Materials -> Initialization Scope -> Init Task Brief -> Approval -> Work Orders -> Approval -> Context Documents -> Verification -> Project Pulse (draft -> confirm -> generate) -> Approval -> Archive -> Git Commit`

User confirmation uses user's current language; document records must use English enumerations from `.AI/standards/enums.md`.

## 5. Initialization Scope Identification

1. Extract project name, language, summary, goals, current scope, modules, constraints, milestones
2. Identify gaps in architecture, specs, invariants, guards, project processes, roadmap
3. Clarify `.AI/project/` files needing creation or update
4. Mark unknowns, assumptions, and items pending user confirmation

## 6. Initialization Task Brief

1. Use `.AI/templates/task_brief_template.md`
2. Generate project initialization task brief, place in `reports/active/brief/`
3. Task brief must record source materials, target directories, planned files, verification methods, risks, pending confirmations
4. Initialization task brief must fill "Initialization Task Supplement" in template:
   - Source Flow: Fixed as `.AI/project/init.md`
   - Source Materials: User-specified or rule-selected requirements, designs, references paths
   - Initialization Scope: Governance context scope to establish or complete this time
   - Target Directories: `.AI/project/` and `reports/` paths planned to write
   - Planned Files: Expected new or updated file list
   - Fact Source Priority: Reference this flow Section 3
   - Inferred Items: Content inferred from source material filenames, structures, context but not explicitly stated in body
   - Pending Confirmations: Content unconfirmable from source materials and user input
5. Stop and wait for user `approved`, `rejected`, or `needs_update`

No work order generation or project governance document modification before task brief `approved`.

## 7. Initialization Work Orders

After task brief `approved`, use `.AI/templates/work_order_template.md` to split work orders, place in `reports/active/plan/`.

Recommended split:

1. Project Definition and Project Index
2. Architecture Documents
3. Spec Documents
4. Invariants Documents
5. Guards Documents
6. Project-Specific Processes
7. Roadmap

Each work order must declare input source materials, target files, operation steps, verification methods (using `work_order_template.md`). After generation, stop and wait for user `approved`, `rejected`, or `needs_update`.

Target file naming suggestions:

| Directory | Recommended Naming |
| :--- | :--- |
| `.AI/project/architecture/` | `overview.md`, `module_{module}.md`, `integration_{boundary}.md` |
| `.AI/project/specs/` | `{module}.md`, `{capability}.md`, `api_{surface}.md` |
| `.AI/project/invariants/` | `{domain}.md`, `{guarantee}.md` |
| `.AI/project/guards/` | `{risk_area}.md`, `{boundary}.md` |
| `.AI/project/process/` | `{process_name}.md` |
| `.AI/project/roadmap/` | `{phase}.md`, `initial_delivery.md` |

If user specifies filenames or source materials have explicit naming, use user input or source materials; recommended naming must not override explicit facts.

## 8. Document Generation

After work orders `approved`, execute each:

1. Read work order specified inputs
2. Backfill or update `.AI/project/project.yaml` (where `governance.governance_mode` field fixed as `full`; task weight determined by `start.md` §6.1 task path judgment, no longer written to project-level tier)
3. Create or update target `.AI/project/` documents
4. Keep content traceable to source materials or explicit user input
5. Explicitly write out inferences, assumptions, unconfirmed items

Must not write project-specific facts into `.AI/standards/`, `.AI/workflows/`, or `.AI/process/`.

### 8.1 Recommended Document Structure

Recommended structure for each project context document type. When source materials or user have explicit requirements, use those; otherwise generate per following recommended structure.

| Directory | Recommended Content Structure |
| :--- | :--- |
| `architecture/` | System structure, module list & responsibilities, tech stack, inter-module dependencies, key design decisions |
| `specs/` | Module responsibilities, interface definitions (endpoints/inputs/outputs), behavior rules, state machines (if any), boundary conditions |
| `invariants/` | Uniqueness constraints, state machine irreversible rules, data consistency guarantees, each tagged with violation consequences |
| `guards/` | Security constraints, auth/authorization rules, forbidden patterns, password/credential policies, external integration boundaries |
| `process/` | Process purpose, trigger conditions, step list, checkpoints, deliverables, approval rules |
| `roadmap/` | Phase list & time windows, functional scope, technical debt records, dependencies, risks |

Each document body starts with fact source: "Source: `{source_material_path}`", inferences tagged "Inferred: `{inference_basis}`".

### 8.2 Invariants Formalization (Strongly Recommended for Consistency/Persistence/Concurrency Systems)

`invariants/` defaults to prose descriptions. When system involves consistency, persistence, concurrency, or consensus, each invariant besides prose **strongly recommended** to attach:

- **Formal Statement**: TLA+, Alloy, linearizability/sequential consistency definitions, etc., pick one;
- **Machine-Checkable Assertions**: Property tests or model checking targets for that invariant (integrates with `.AI/process/validation/validation_matrix.md` distributed verification methodology);
- **Violation Consequences & Minimum Reproduction Conditions**.

Invariants without formal layer in verification matrix that dimension only records `risk-based` or lower coverage level, and must register uncovered risks. Machine-first verification principle (`alds_standards.md` §21) requires: for claimed `passed` critical invariants, must have reproducible machine evidence; prose description not evidence.

## 9. Verification and Confirmation

After each work order completion verify:

1. Traceable to source materials or explicit user input
2. Consistent with `.AI/project/project.yaml`
3. No conflicts with existing `.AI/project/` docs
4. Pending confirmations and assumptions preserved
5. Correct templates and metadata used

Initialization verification checklist:

| Check Item | Requirement |
| :--- | :--- |
| Fact Source | Every project fact traceable to source material body, filename inference, or explicit user input |
| Inferred Items | All inferences explicitly marked, not written as confirmed facts |
| Pending Confirmations | Unconfirmable content kept pending, not guessed |
| Enums | Approvals, statuses, task types, priorities, risk levels use `.AI/standards/enums.md` |
| Paths | Project facts only to `.AI/project/`, task artifacts only to `reports/` |
| Conflicts | No unexplained fact conflicts between new and old project context |
| Templates | Task briefs, work orders, project pulse use corresponding templates or record deviation reasons |
| Project Pulse | Only registers task briefs or candidates, not work orders as main entries |
| Doc Structure | Generated `.AI/project/` docs follow §8.1 recommended structure, deviations recorded |

Form verification results, use as input for project pulse generation. Issues found in verification presented and handled in §10.2 user confirmation.

## 10. Project Pulse

After all initialization work orders complete and `approved`, enter project pulse generation.

### 10.1 Draft Task Ledger

From source materials, initialization scope, and generated `.AI/project/` docs, extract functional task candidates, **draft project pulse task ledger section**:

- Functional Task Brief Ledger: Candidate task items arranged by module and phase
- Other Task Ledger: Non-functional candidates (optimization, defects, governance, technical debt, etc.)
- Each item tagged: task name, module, type, priority, risk level, estimated code size, estimated effort, high-level verification method

### 10.2 User Confirmation

Present drafted ledger list and verification results together to user, wait for confirmation:

- User may adjust task items, add/remove scope, modify priorities or phase assignments
- User may review verification result issues (fact conflicts, inference markers, path compliance), decide accept or require modifications
- Record all user modifications
- After user `approved`, proceed to full generation

### 10.3 Generate Complete Project Pulse

After user confirms ledger content, generate complete `reports/project_pulse.md` per following:

- Path: `reports/project_pulse.md`

Content at minimum includes:

- Project name, language, summary
- Overall goals and non-goals
- Major and minor milestones
- Functional Task Brief Ledger (arranged by planned dev sequence and module aggregation)
- Other Task Ledger (optimization, defects, governance, technical debt, etc.)
- Status, task types, priorities, risk levels (using `enums.md` enums)
- Estimated code size and effort
- Preconditions and dependencies
- High-level verification methods
- Associated source material sections or project governance docs
- Risks and pending confirmations

Project pulse verification methods only describe high-level verification types or evidence sources, e.g., "spec review", "regression test", "manual acceptance", "source material section cross-check". Must not write work-order-level verification steps, command outputs, execution logs, or acceptance details.

### 10.4 Approval

After generating complete project pulse, stop and wait for user `approved`, `rejected`, or `needs_update`.

Project pulse is not a phase task brief or work order list, but a project-level task brief ledger. Project pulse only registers existing task briefs or coarse-grained candidates for task briefs; work orders only registered in owning task brief, not as project pulse independent main entries.

Project pulse must comply:

- No "Current Task Big List" section name
- No "Next Steps" section
- Project overview must not contain "Current Scope", "Development Phase", "Governance Status" three items
- Milestone historical completions uniformly use `done`, must not mix `archived` for completion
- After task completion keep in corresponding ledger, append `completed` in tags column
- Task brief ledger associations only point to task briefs, project governance docs, design/spec docs, or analysis reports, must not point to `reports/active/plan/` or `reports/completed/plan/` work orders
- If item only has work order no task brief, must not write separately to project pulse; should merge into owning task brief entry, or register as coarse-grained candidate for task brief
- Tags must not use `draft_work_order`, `work_order_approved` etc. work order status words
- Subsequent standard tasks should select or add task brief entries from project pulse, then generate task brief per `.AI/start.md`; work orders continue managed by owning task brief

## 11. Archive and Commit

After project pulse `approved`:

1. Migrate completed work orders as `{base}_{YYYYMMDD_HHMM}.md` to `reports/completed/plan/`
2. Mark initialization task brief `completed`, migrate to `reports/completed/brief/`
3. Execute pre-commit diff audit, keep only initialization-related changes
4. Write git commit, message describes initialization scope, generated docs, verification results
5. Subsequent tasks enter from `.AI/index.md`, executed by `.AI/start.md` standard flow

Pre-commit diff audit requirements:

- Allowed paths:
  - `.AI/project/project.yaml`
  - `.AI/project/index.md`
  - `.AI/project/architecture/`
  - `.AI/project/specs/`
  - `.AI/project/invariants/`
  - `.AI/project/guards/`
  - `.AI/project/process/`
  - `.AI/project/roadmap/`
  - `reports/active/brief/`
  - `reports/active/plan/`
  - `reports/active/analysis/`
  - `reports/active/debt/`
  - `reports/active/review/`
  - `reports/completed/brief/`
  - `reports/completed/plan/`
  - `reports/completed/analysis/`
  - `reports/completed/debt/`
  - `reports/completed/review/`
  - `reports/project_pulse.md`
- Forbidden paths:
  - `.AI/standards/`
  - `.AI/workflows/`
  - `.AI/process/`
  - `.AI/skills/`
  - `.AI/templates/`
  - Source code, configs, or reports unrelated to this initialization
- If workspace has diffs outside allowed paths, must pause commit and explain to user, must not mix into initialization commit.