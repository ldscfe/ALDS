---
title: ALDS Standards
doc_type: standard
status: active
scope: ALDS
updated: 2026-09-04
version: 1.4.0
related_docs:
  - .AI/index.md
  - .AI/start.md
  - .AI/project/init.md
  - .AI/source/
  - .AI/standards/enums.md
  - .AI/standards/workflow_engine.md
---

# ALDS Standards

ALDS (AI-driven Lifecycle Delivery System) is a lifecycle delivery governance system for AI Agents. It constrains AI-assisted development within a traceable, verifiable, auditable delivery closed loop through project facts, process routing, task briefs, work orders, verification, approval, archive, and commit records.

This document defines ALDS general specifications. Project-specific facts, architecture boundaries, module constraints, compatibility targets, and forbidden patterns must not be written into this document; they belong in `.AI/project/`.

## 1. Applicability

This document applies to:

- `.AI/` governance directory structure design
- AI Agent task entry, routing, execution, verification, and archive behaviors
- Task briefs, work orders, project pulse, technical debt records, and other process artifacts
- Workflow, process, standard, skill, and template responsibility boundaries
- Cross-document unified status, approval, and enumeration recording methods

This document does not define:

- Specific project business rules
- Specific programming language or framework implementation details
- Specific repository test commands, release commands, or deployment strategies
- Ad-hoc task decisions and one-off execution records

## 2. Origin & Positioning

ALDS originates from ACDS (AI Controlled Development System) specification concepts. ACDS core goal was transforming AI from free coding assistant into production pipeline constrained by architecture, specs, verification, risk control, and audit. ACDS was used in early development of SRDS (Sharded Redis Distributed System).

ALDS generalizes ACDS:

- Removes specific project, language, runtime, and toolchain constraints
- Retains contract-driven, process-driven, machine-first verification, risk explicit, archive audit principles
- Abstracts strongly project-bound mechanisms into general governance requirements deployable by projects

ALDS is not an industry standard or software package, but a portable AI collaborative delivery methodology and repository governance protocol.

## 3. Methodology

ALDS methodology summarizes to:

| Component | Methodology Position |
| :--- | :--- |
| Project Context | Project fact boundary, answers "what is this project" |
| Specification | Behavioral contract, answers "what should system do" |
| Architecture | Structural constraint, answers "how should system be organized" |
| Invariants and Guards | Unbreakable constraints, answers "what must never be broken" |
| Workflow | Execution track, answers "how should task advance" |
| Task Brief | Task contract, answers "what to do/not do this time, how to accept" |
| Work Order | Construction contract, answers "what specifically to change, how, how to verify" |
| Project Pulse | Project status hub, answers "how are all task briefs progressing" |
| Verification | Completion basis, answers "why consider result valid" |
| Approval | Human decision point, answers "allow next stage" |
| Archive and Git Commit | Audit loop, answers "where is completion record, how to trace code history" |
| Task Path | Task path, answers "is this task micro (no control document, ledger line only), simple (single work order), or engineering (task brief + work order)" |

ALDS prohibits AI free play; requires AI to execute within confirmed facts, approved processes, and verifiable boundaries.

## 4. Specification Keywords

Keywords in this document mean:

| Keyword | Meaning |
| :--- | :--- |
| Must | Mandatory requirement, violation = process violation |
| Must Not | Mandatory prohibition, violation = process violation |
| Should | Default requirement, unless explicit reason recorded as deviation |
| May | Permitted behavior, whether to execute decided by specific flow |

## 5. Core Principles

All ALDS processes must obey:

| Principle | Requirement |
| :--- | :--- |
| Contract First | AI must use `.AI/project/project.yaml` and `.AI/project/` as project fact sources, must not fabricate requirements |
| Unified Entry | Every task must first read `.AI/index.md` and check `.AI/project/project.yaml` instance fact source status; if `initialized` read project fact source, ALDS standard package self-maintenance with `absent` records missing and skips |
| Workflow Driven | Standard tasks must enter `.AI/start.md` and select explicit workflow |
| Default Fallback | When cannot judge process must enter `unified_task_flow`, must not implement without workflow |
| Minimum Context | Only read minimum relevant documents needed for judgment and execution |
| Approve Before Write | Task briefs, work orders, write plans, verification results must be approved before next stage (micro tasks exempt: direct execution per user instruction, no control document approval nodes) |
| Machine-First Verification | Runnable, reproducible verification evidence takes priority over subjective explanations |
| Risk Explicit | Unknowns, verification gaps, scope deviations, high-risk changes must be explicitly recorded |
| Auditable | Objectives, scope, decisions, verification, approvals, archive, commits must be traceable |
| Path Judgment | Tasks judged per `.AI/start.md` §6.1 as micro, simple, or engineering; micro tasks direct execution (no task brief, no work order, ledger line only, verification deferred per §21.1), simple tasks direct work order (no task brief), engineering tasks task brief + work order. All three share workflow routing, required docs table. |

## 6. ALDS Layered Model

ALDS abstracts AI collaborative delivery into 8 universal layers. Each layer may be deployed by projects as needed, but responsibility boundaries must not be confused.

| Layer | Name | Responsibility | Must Satisfy | Prohibited | Main Artifacts |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Project Context Layer | Carries project facts, architecture, specs, invariants, guards, roadmap | Project facts traceable to `.AI/project/project.yaml` or `.AI/project/` | Writing project facts into general standards | `project.yaml`, project index, architecture, specs, guards |
| 2 | Specification and Contract Layer | Defines behavioral contracts, interface rules, boundary conditions, acceptance criteria | Behavior must have explicit source | Implementing spec-undefined behavior as deterministic | specs, invariants, guards |
| 3 | Planning Layer | Transforms design, specs, user goals into task briefs/work orders, project pulse | Scope, non-goals, verification methods clear | Construction without work order (micro tasks exempt: direct execution per `start.md` §6.1-§6.2, ledger line only) | Task brief, work order, project pulse |
| 4 | Workflow Routing Layer | Selects workflow and minimum reading context per input intent | Unified entry, traceable process | Construction without workflow, or new workflow per input format difference | Routing conclusion, workflow selection |
| 5 | AI Execution Layer | Executes code, docs, review, analysis tasks within work order constraints | Execution scope constrained by work order | Silent scope expansion, unapproved writes | Code changes, doc changes, review records |
| 6 | Verification Layer | Forms verification evidence via machine checks, rule verification, tests, human review | Verification methods and results must record; test default action | Substituting subjective explanations for verification | Verification records, audit results |
| 7 | Risk Control Layer | Identifies risk levels, unknowns, verification gaps, upgrade conditions | High risk must upgrade confirmation | Hiding risk or bypassing approval | Risk explanations, upgrade records |
| 8 | Integration and Archive Layer | Completion status backfill, archive, git commit | Archive and commit traceable | Complete without archive, without commit | archived, completed, git commit |

## 7. Directory Responsibilities

`.AI/` index files uniformly use `index.md`. Processes, protocols, templates, and specific standards use semantic filenames.

| Path | Responsibility |
| :--- | :--- |
| `.AI/index.md` | AI entry routing, only judges entering standard task flow or project initialization flow |
| `.AI/start.md` | Standard task main flow, responsible for workflow routing, task brief, work order, verification, archive, commit |
| `.AI/project/init.md` | Project initialization flow, generates project governance context and project pulse from `.AI/source/` source materials |
| `.AI/project/project.yaml` | Project structured fact source |
| `.AI/project/` | Project facts, architecture, specs, invariants, guards, project processes, roadmap |
| `.AI/source/` | Raw requirements, designs, supplementary references |
| `.AI/workflows/` | Specialized workflow definitions |
| `.AI/standards/` | ALDS general standards, controlled enumerations, workflow engine rules |
| `.AI/process/` | Review, verification, audit, archive, feedback adoption reusable processes |
| `.AI/skills/` | Optional skill descriptions loaded on match |
| `.AI/templates/` | Task brief, work order, technical debt templates |
| `reports/project_pulse.md` | Project pulse |
| `reports/active/brief/` | Current task briefs |
| `reports/active/plan/` | Current work orders |
| `reports/active/analysis/` | Active analysis reports (identified items not all routed) |
| `reports/active/debt/` | Technical debt records |
| `reports/active/review/` | Review findings, risk conclusions, follow-up routing recommendations |
| `reports/completed/brief/` | Completed task briefs |
| `reports/completed/plan/` | Archived work orders |
| `reports/completed/analysis/` | Completed analysis reports (identified items all routed) |
| `reports/completed/debt/` | Technical debt archive |
| `reports/completed/review/` | Archived review documents |

## 8. Document Hierarchy

ALDS documents interpreted by following priority. Higher priority prevails when conflicts with lower.

1. User explicit instruction this round
2. Approved task briefs, work orders, approval records
3. `.AI/project/project.yaml`
4. `.AI/project/` project facts, architecture, specs, invariants, guards
5. `.AI/index.md`, `.AI/start.md`, `.AI/project/init.md`
6. `.AI/standards/`
7. `.AI/workflows/`
8. `.AI/process/`
9. `.AI/skills/`
10. Index, explanatory docs, historical archives

If user instruction conflicts with project facts or approved constraints, AI must point out conflict and wait for user adjudication, must not silently choose one.

## 9. Entry and Routing

Every task must start in following order:

1. Check and evaluate `.AI/project/project.yaml` instance fact source status: `absent` (non-existent), `empty_or_incomplete` (exists but empty or incomplete), `initialized` (initialization complete).
2. Read `.AI/index.md` entry routing rules.
3. Per fact source status and user intent, route to corresponding control entry:
   - Standard project tasks with missing `project.yaml` (`absent` status) must route to `.AI/project/init.md` or wait for user confirmation, must not directly start as standard task.
   - For **ALDS standard package self-maintenance tasks** (e.g., standard package revision, audit, template evolution), allowed to start with fact source `absent`, directly enter `.AI/start.md` standard task flow.
4. Per user input enter `.AI/start.md` or `.AI/project/init.md`.

Routing rules:

- Project initialization, establish `.AI/project/` baseline, complete project governance context from `.AI/source/` source materials → `.AI/project/init.md`
- Development, fix, refactor, optimization, test, review, documentation, technical debt assessment, governance evolution → `.AI/start.md`
- ALDS standard package self-maintenance tasks directly enter `.AI/start.md` using standard self-maintenance path, task brief must record "no project instance fact source, current task is standard package self-maintenance"
- Cannot determine → `.AI/start.md`, standard task flow loads final decision basis

`.AI/index.md` only permitted to carry entry routing responsibility, must not carry task briefs, work orders, or specific execution steps.

## 10. Project Facts and Contracts

Project facts and contracts distributed by responsibility:

| Location | Specification Responsibility |
| :--- | :--- |
| `.AI/project/project.yaml` | Structured project fact source |
| `.AI/project/architecture/` | System structure, module boundaries, data flows, key design relationships |
| `.AI/project/specs/` | Behavioral contracts, interface rules, error rules, boundary conditions |
| `.AI/project/invariants/` | Unbreakable system-level guarantees |
| `.AI/project/guards/` | Forbidden patterns, security boundaries, high-risk prohibited zones |
| `.AI/project/roadmap/` | Phase plans, milestones, deferred items |
| `.AI/source/requirements/` | Raw requirements, PRDs, user stories, business rules |
| `.AI/source/design/` | Raw designs, architecture, interactions, interface documents |
| `.AI/source/references/` | Supplementary materials, meeting notes, screenshots, research materials |

AI must not use speculation to replace project facts. Unconfirmable content must be marked pending confirmation, must not be written as confirmed facts.

Architecture, specs, invariants, guards changes must enter governance evolution flow, must not be mixed into ordinary development work orders. If implementation needs to break existing constraints, AI must pause current work order and upgrade flow.

## 11. Standard Task Flow

Standard tasks must be controlled by `.AI/start.md`. Tasks have three tiers (judgment in `.AI/start.md` §6.1):

- **Micro Task**: `Intent Routing -> Minimum Context -> Direct Execution (no work order) -> micro_tasks.md add line (unverified) -> Git Commit` (verification deferred per §21.1)
- **Simple Task**: `Intent Routing -> Minimum Context -> Single Work Order -> Approval -> Execution -> Verification(test default) -> simple_tasks.md add line + archive -> Git Commit`
- **Engineering Task**: `Project Definition -> Intent Routing -> Minimum Context -> Task Brief -> Approval -> Work Order -> Approval -> Execution or Review -> Verification -> Approval -> Archive -> Git Commit`

Standard tasks must at minimum satisfy:

- Select one `.AI/workflows/` workflow
- Read workflow declared required documents
- After workflow selection judge `.AI/skills/` hits
- Control document tier per `.AI/start.md` §6.1:
  - **Micro Task**: no task brief or work order; execute directly per user instruction, add line to `reports/micro_tasks.md` (status `unverified`), verification deferred per §21.1
  - **Simple Task**: single work order generated and wait for approval before execution; no task brief, no project pulse registration
  - **Engineering Task**: generate task brief and wait for approval; register or update task brief entry and status in `reports/project_pulse.md`; generate work order(s) per approved task brief and wait for approval; register or update work order entries and status in owning task brief
- After execution form verification and audit results and wait for approval (micro tasks: deferred verification closure per §21.1)
- After approval update docs, archive, write git commit (micro tasks: ledger line replaces archive artifacts, commit per task)

## 12. Project Initialization Flow

Project initialization only controlled by `.AI/project/init.md`. This flow used to establish or complete `.AI/project/` project governance context from raw requirements, designs, and supplementary materials in `.AI/source/`.

Initialization flow must produce:

- Initialization task brief
- One or more initialization work orders
- `.AI/project/` project governance context
- `reports/project_pulse.md`
- Archived initialization work orders
- Completed and archived initialization task brief

Before each creation or modification of `.AI/project/` documents, AI must first list write plan. Write plan must include:

- Target files
- Section summary per file
- Fact sources
- Inferred items
- Pending confirmations
- Expected change scope

No writing or modifying `.AI/project/` documents before explicit user approval.

## 13. Minimum Reading Rules

Standard tasks after selecting workflow read in following order:

1. `.AI/project/project.yaml`
2. `.AI/start.md`
3. Selected `.AI/workflows/*.md`
4. Workflow declared `.AI/standards/` and `.AI/process/` documents
5. Minimum relevant `.AI/project/` documents
6. Hit general `.AI/skills/` documents
7. Hit language or module specialized skill documents

Must not load entire context for convenience. If insufficient info to judge, must explicitly state gaps and wait for user supplement.

## 14. Workflow Specification

Formal workflow files must be in `.AI/workflows/`, and at minimum define:

- Applicable scenarios
- Input artifacts
- Required documents
- Execution steps
- Output artifacts
- Prohibitions
- Fallback paths

Current standard workflow set includes:

- `unified_task_flow`
- `feature_development`
- `performance_analysis`
- `code_review`
- `review_feedback_adoption`
- `code_quality_assessment`
- `architecture_evolution` (includes standard package self-maintenance special branch: allows evolution tasks on standards, processes, templates, skills when `project.yaml` and `reports/project_pulse.md` instances missing, task type recorded as `governance`, exempt from project pulse sync)
- `create_workflow`

New workflow creation only allowed when existing processes cannot safely express task, task has independent approval/audit structure, or introduces new governance boundaries.

## 15. Task Brief Specification

Task brief is task-level control document (only engineering tasks generate, simple and micro tasks do not), must be placed in `reports/active/brief/`.

Task brief core records three items: **User Requirement**, **Functional Overview**, **Work Order List** (see `alds_standards.md` §15); other fields support these three.

Task brief must include:

- Task ID
- Project name, language, summary
- Project pulse entry reference
- User original request
- Matched workflow
- Matched skills
- Task summary
- Task type
- Priority
- Objective, scope, non-goals
- Functional overview (overall description of functionality to implement; frontend features may reference owned work order layout diagrams, no prose re-description of layout, see §16.3)
- Major milestones and major tasks
- Each major task estimated duration
- Estimated code size
- Estimated effort
- Preconditions and dependencies
- Required documents
- Expected outputs
- Verification methods
- Approval status
- Owned work order list and status

Task brief must wait for explicit user approval after generation. No work order generation or changes before task brief `approved`.

Task brief must record owned work order info. Each work order entry must include work order ID, summary, path, status, estimated code size, estimated effort, preconditions, dependencies.

## 16. Work Order Specification

Work order is execution-level control document, must be placed in `reports/active/plan/` (micro tasks generate no work order, see `start.md` §6.1-§6.2; simple tasks directly generate single work order without task brief).

Work order must include:

- Work order ID
- Owning task brief
- Matched workflow and skills
- Summary
- Execution objective
- Input documents
- Implementation scope
- Operation steps
- Outputs
- Verification methods
- Estimated duration
- Estimated code size
- Estimated effort
- Preconditions and dependencies
- Approval status
- Execution status

Work order must not execute before `approved`.

Work order should remain atomic, verifiable, archivable. Should prioritize splitting when any condition met:

- Single task estimated duration exceeds `5` working days
- Estimated modified files exceed `10`
- Estimated net change exceeds `1000` lines

If not splitting, must record reason in work order.

### 16.1 Net Change Calculation

- Net change = added lines + deleted lines + modified lines (sum of `git diff` `+`/`-`).
- **Exclude**: Auto-generated files (e.g., `*.gen.*`, `package-lock.json`, `Cargo.lock`, `go.sum`, `pnpm-lock.yaml`), pure formatting/whitespace changes, pure import sorting.
- Exclusions must be recorded in work order with reasons; must not use exclusions to circumvent split thresholds.

### 16.2 Verification Method Single Source of Truth

Work order verification method may share single source with owning task brief verification matrix: work order may reference task brief's planned dimension rows as "See task brief §X verification matrix", **only fill result and evidence columns**, no need to rebuild dimension rows. If work order verification scope differs from task brief, must explicitly declare difference in work order. Simple tasks have no task brief; verification method defined once in work order.

### 16.3 Frontend Work Order Layout Diagram & Functional Overview

When work order involves frontend page, UI component, or user interaction path changes, execution definition must use "layout diagram + functional overview" instead of prose layout description to compress I/O text:

- **Layout Diagram**: Dot-line (ASCII box chars `┌┐└┘─│├┤┬┴┼`, in ``` code block) draws page/component area structure, each area tagged with short bracketed label (e.g., `[Search]`, `[List]`).
- **Functional Overview**: Indexed by layout diagram area tags, one line per area describing role/behavior/data source; must not separately write "element at position" prose.
- Layout diagram only expresses area structure and functional mapping, not pixel dimensions or visual styles (visual details constrained by design system and §21.2 visual/E2E verification).
- Non-frontend work orders not required layout diagram; functional overview per conventional bullet points.

Layout diagram example (format demo only, not real work order):

```
┌────────────────────────────┐
│ [Logo]   [Search]    [User] │
├─────────┬──────────────────┤
│ [Sidebar]│ [List] Card × N  │
│  Nav     ├──────────────────┤
│          │ [Pager]          │
└─────────┴──────────────────┘
- [Search]: Input keyword → triggers list filter
- [List]: Display resources, empty state shows placeholder
- [Pager]: 20 per page
```

## 17. Project Pulse Specification

Project pulse is project-level task brief ledger, not phase task brief, not work order list. File fixed as:

- `reports/project_pulse.md`

Project pulse must include at minimum:

- Project name, language, summary
- Overall goals and non-goals
- Major and minor milestones
- Functional Task Brief Ledger, arranged by planned dev sequence and module aggregation
- Ad-hoc Task Brief Ledger, at minimum reserving optimization, defect, testing/verification, documentation, governance, technical debt categories
- Each task brief summary, path, completion status
- Each task brief task type, priority, risk level
- Each task brief estimated code size and effort
- Each task brief preconditions and dependencies
- Each task brief verification methods
- Each task brief associated source material sections or project governance docs
- Each task brief risks and pending confirmations

> **Single Source of Truth (Deduplication)**: To avoid duplicating task brief details, estimated code size, effort, verification methods, preconditions, dependencies, risks, pending confirmations may reference "See task brief §X" in project pulse entries; but `Task ID` / `Summary` / `Path` / `Status` / `Risk Level` must be inline, ensuring project pulse independently browsable as ledger.

Project pulse "Other Task Ledger" optimization, testing, technical debt candidates must judge per `.AI/process/project_pulse/task_brief_derivation.md` whether analysis report required first, then derive task brief. This ledger's derivation status uses status set defined in that flow document.

Status, task types, priorities, risk levels must use controlled enumerations from `.AI/standards/enums.md`.

Project pulse must record all task brief necessary info and completion status. When task brief created, approval status changes, execution status changes, or completion archived, must sync update corresponding task brief entry in project pulse.

Project pulse structure must comply:

- Project overview must not contain "Current Scope", "Development Phase", "Governance Status" three items
- Must not use "Current Task Big List" as section name
- Must not set "Next Steps" section
- Work orders only registered in owning task brief, not as project pulse independent main entries
- Task brief ledger associations only point to task briefs, project governance docs, design/spec docs, or analysis reports, must not point to `reports/active/plan/` or `reports/completed/plan/` work orders
- If item only has work order no task brief, must not write separately to project pulse; should merge into owning task brief entry, or register as coarse-grained candidate for task brief
- Tags must not use `draft_work_order`, `work_order_approved` etc. work order status words
- Project pulse only responsible for guiding task brief generation or update; work orders continue managed by owning task brief
- After task completion still keep in corresponding ledger, append `completed` in tags column
- Milestone historical completions uniformly use execution status `done`, must not mix `archived` for completion

## 18. Status Sync Specification

ALDS uses two-level status sync:

1. Project pulse records task brief status
2. Task brief records owned work order status

Micro tasks participate in neither level; their status maintained only in `reports/micro_tasks.md` per §21.1 deferred verification closure rules.

Status sync semantics must remain consistent:

- **Execution Status**: Unified use of controlled enum `done` for item execution complete. Project pulse, task brief, work order final execution complete status all recorded as `done`.
- **Document Status & Location**: `archived` only for document physical archive location (e.g., migration to `reports/completed/plan/`) or document's own static lifecycle status, must not mix as execution complete status. Task brief work order register may record both `Execution Status` and `Archive Path` simultaneously to avoid confusion.

After work order completion, must update corresponding work order status to `done` in owning task brief. If work order archive path changes, task brief work order path must also sync update.

After task brief completion, must update corresponding task brief execution status to `done` in `reports/project_pulse.md`, append `completed` in tags column. If task brief migrates from `reports/active/brief/` to `reports/completed/brief/`, project pulse task brief path must also sync update.

If status sync fails, must not declare task complete.

## 19. Approvals and Enumerations

ALDS document record values must use English enumerations from `.AI/standards/enums.md`. User-facing confirmation prompts should use user's language.

Approval mapping:

| User Response | Recorded Value |
| :--- | :--- |
| Approve | `approved` |
| Reject | `rejected` |
| Needs Update | `needs_update` |

Following nodes must wait for explicit user conclusion:

- After task brief generation
- After work order generation
- After write plan generation
- After verification and audit results formed
- After project pulse generation

Must not use undefined status values, e.g., `ok`, `yes`, `done-approved`.

### 19.1 Non-Enum User Input Handling

User natural language at approval nodes not used directly as recorded values. Processing:

1. Hits affirmative semantics → record as `approved`, and **echo back approved object and next step**: approve/agree/ok/yes/confirm/OK/go/proceed; "continue" only maps to `approved` when context clearly means "enter next stage".
2. Hits negative or reserved semantics → record as `rejected` or `needs_update` (per semantics), and echo reason: reject/no/don't/wait/fix it/look again/wait.
3. Semantics unclear (e.g., isolated "continue" cannot judge "approve" or "continue narrating") → **must not auto-advance**, echo current approval object, require explicit response using one of three states or §1 affirmative words.

Recorded values still only `approved` / `rejected` / `needs_update`; this section only translates natural language to these three states, does not add enum values.

## 20. AI Output Control

ALDS does not mandate patch-only output for all projects, but AI's any output must be constrained by task brief, work order, and approval nodes.

AI output must satisfy:

- Change scope consistent with work order
- New/modified files have explicit basis
- Code changes record verification methods and risks
- Doc changes record fact sources and pending confirmations
- Writes to `.AI/project/` only after write plan approval

AI must not bypass work order boundaries via one-off explanations, free play, or implicit assumptions.

### 20.1 Narrative & Presentation Discipline

ALDS constrains not only file artifacts but AI narrative to users. To reduce meaningless output:

- **No Re-reading**: Must not re-summarize already-read doc content in conversation unless user requests.
- **No Verbal Routing**: Must not step-by-step narrate "I read X, routed to Y, next will Z"; routing conclusion and read set reflected in control docs sufficient, no need to elaborate in conversation.
- **Compact Approval Gates**: Each approval node only presents "approval object + key fields + three-state request", no prose padding.
- **Delete Empty Shells**: Empty sections in control docs delete or replace with one line "not involved", must not keep empty placeholder rows or empty table rows.
- **Direct Answers**: User questions answered directly with conclusion and evidence, no prefacing lengthy process explanations.
- **Frontend Uses Layout Diagrams**: Frontend layout functional overviews use dot-line layout diagrams + area tag tables (see §16.3), no prose describing element positions.

## 21. Verification and Risk Control

ALDS does not specify specific test tools, but every execution must state verification method. Machine-reproducible verification prioritized over subjective explanations; human review cannot substitute runnable verification.

When task affects multiple runtime conditions, data conditions, config conditions, or deliverables, must plan verification matrix per `.AI/process/validation/validation_matrix.md`, clarifying coverage levels, verification evidence, and uncovered risks.

Verification may include:

- Architecture, specs, invariants, guards cross-check
- Format checks, static checks, build, tests
- Unit tests, integration tests, regression tests, compatibility verification
- Impact scope review
- Risk level review
- Document consistency checks

Risk factors include:

- Modified file count
- Net change volume
- Whether touches core modules
- Whether touches architecture, specs, invariants, guards
- Whether adds dependencies
- Whether changes public interfaces
- Whether lacks verification
- Whether has unknowns

Risk levels use `.AI/standards/enums.md` `low`, `medium`, `high`, `critical`, `unknown`. Judgment per `.AI/standards/enums.md` §6.1 quantification criteria; Agent self-assessment conflicts with objective criteria take higher tier and record reason.

| Risk Level | Handling Requirement |
| :--- | :--- |
| `low` | May execute per approved work order |
| `medium` | Must explicitly state verification methods and impact scope |
| `high` | Must wait for user approval and execute necessary reviews |
| `critical` | Must upgrade governance flow or reject current construction |
| `unknown` | Insufficient info, must supplement basis before continuing |

Verification results must answer at minimum:

1. What changed this time
2. Why consider change safe
3. What verifications ran
4. What risks unverified or unverifiable
5. Whether need human confirmation upgrade

### 21.1 Test Default & User Self-Test

After change execution, test is default action:

- AI defaults to running tests per work order verification method / verification matrix; frontend changes must include browser automation visual testing (§21.2).
- User may explicitly declare "user will self-test and return results": AI stops, does not self-mark complete, waits for user test results (must include verification evidence) before archiving.
- No verification/test results (whether AI runs or user returns), must not mark complete.
- **Micro Task Verification Deferral**: micro tasks (see `start.md` §6.1-§6.2) do not execute per-task test. Verification closes via:
  - **User test**: user tests and returns results; AI updates `reports/micro_tasks.md` row status to `verified` (or `failed`), evidence column records returned summary (screenshot/output path if any)
  - **Centralized test**: user explicitly triggers (e.g., "centralized test"); AI runs available tests covering all `unverified` rows, including browser automation visual verification (§21.2) for frontend rows; results backfill per row, failed rows set `failed` and enter fix flow
  - Micro task rows must not set `verified` without user test or centralized test evidence; frontend micro task visual verification obligation transfers to user/centralized test point, uncovered frontend rows must not close

### 21.2 Frontend Page Change Verification

When changes involve frontend pages, UI components, or user interaction paths, must use browser automation tools for visual verification testing. Playwright preferred, Cypress, Selenium, or equivalent tools acceptable.

Requirements:

- Cover core user paths for all new/modified features (page navigation, form submission, button operations, state transitions)
- Cover normal paths, boundary conditions, exception scenarios
- Verification evidence should be reproducible test scripts or commands
- Visual regression testing (screenshot comparison) may supplement, but must not substitute functional interaction verification
- Project should declare frontend verification rules in `.AI/project/guards/`; rule template in `.AI/templates/frontend_verification_guard.md`

## 22. Isolation Verification & Integration Principles

ALDS does not mandate all projects use independent sandbox clones, but high-risk changes should verify in isolated environments.

General requirements:

- Unverified results must not be treated as mergeable
- High-risk changes must not directly enter mainline
- Mainline changes must have verification evidence and approval records
- Verification environment, commands, conclusions should be written into work order or verification results

## 23. Archive and Commit

After verification results `approved`, must execute closure:

1. Update task brief, work orders, project docs, analysis reports, or technical debt records
2. Migrate completed work orders as `{base}_{YYYYMMDD_HHMM}.md` to `reports/completed/plan/`
3. Backfill work order status `done` in task brief
4. Sync update `reports/project_pulse.md` task brief status
5. If all work orders complete, mark task brief `completed` migrate to `reports/completed/brief/`
6. If task brief archive path changes, sync update `reports/project_pulse.md` task brief path
7. Check git diff, keep only task-related changes
8. Write git commit, message describes task, key changes, verification results

**Historical Archive Compatibility**:

- Newly generated archive artifacts always written to standard directory `reports/completed/plan/`.
- If historical projects or external migrations use `reports/archives/` path, only recognized as historical input, must not create any new documents or generate any new files under this path.

Archive timestamp format fixed as `YYYYMMDD_HHMM`, at filename base end before `.md` extension.

**Micro Task Closure**: Micro tasks have no archive artifacts; one line in `reports/micro_tasks.md` plus per-task git commit complete closure. Verification status and evidence backfill at deferred verification closure per §21.1.

## 24. Skills Specification

`.AI/skills/` is optional extension layer, not default required reading layer.

Skill enablement must satisfy:

- Completed `.AI/project/project.yaml` instance fact source status check; project fact source `initialized` already read, ALDS standard package self-maintenance with `absent` already recorded missing
- Completed entry routing and workflow selection
- Current task has explicit language, module, or professional capability match

Skill reading rules:

- Read general skill file first
- Then read language or module specialized skill file
- When language undetermined, must not reverse-infer language specialized skill
- Hit results must be written into task brief or work order

Skill files must not carry project-specific facts, one-off process descriptions, or temporary indexes.

## 25. Technical Debt Specification

Technical debt records placed in `reports/active/debt/`, for tracking confirmed but currently unaddressed issues.

Technical debt records applicable scenarios:

- Issues found after quality assessment but not entering implementation
- Issues found in standard tasks but beyond current scope
- Architecture, performance, maintainability risks confirmed but deferred

Technical debt records should use `.AI/templates/technical_debt_template.md`. Whether to convert technical debt to task brief or work order decided by subsequent task flows.

## 26. Analysis Report Lifecycle

Analysis reports placed in `reports/active/analysis/`, for recording code quality scans, performance benchmarks, design-code gaps, implementation status assessments, etc.

Analysis report lifecycle rules:

| Stage | frontmatter status | Description |
|:---|:---|:---|
| Creation | `draft` | Analysis report initial state |
| Identified items all routed | `implemented` | Analysis report identified items all established task briefs/technical debt/rejected (task brief completion independent) |

Status sync requirements:

- After every analysis report identified item has clear destination (task brief established, technical debt registered, explicitly rejected), must update frontmatter `status` to `implemented`, and archive to `reports/completed/analysis/` per `reports/completed/analysis/README.md` rules
- `reports/active/analysis/` directory does not maintain index file (active docs are directory itself)
- Archived analysis reports uniformly viewed in `reports/completed/analysis/`

Analysis reports must not be used for:

- Directly substituting task briefs or work orders
- Bypassing task brief approval for direct implementation
- As project fact source (project facts only from `.AI/project/`)

## 27. Prohibitions

Following behaviors prohibited:

- Project tasks skip reading `.AI/project/project.yaml` when `initialized`, or bypass initialization confirmation when fact source missing/incomplete
- Skip `.AI/index.md`
- Standard tasks bypass `.AI/start.md`
- Execute tasks without workflow
- Generate work order before task brief approved
- Execute before work order approved (micro tasks exempt: direct execution per `start.md` §6.1-§6.2)
- Modify `.AI/project/` before write plan approved
- Implement spec-undefined behavior as deterministic behavior
- Disguise architecture, specs, invariants, guards changes as ordinary implementation
- Declare complete without verification (micro tasks: set ledger row `verified` without user test or centralized test evidence)
- Silent scope expansion
- Write project-specific facts into `.AI/standards/`
- Misjudge input format differences as new workflow needs
- Use undefined enum values
- Archive without status backfill
- Work order complete without updating owning task brief
- Task brief complete without updating project pulse
- Complete task without git commit
- Micro task completed without `reports/micro_tasks.md` ledger line
- Micro task boundary exceeded without upgrade (staying ledger-only after over-boundary)

### 27.1 "Undefined Behavior" Prohibition Scope

§27 prohibits: **Implementation layer** determining implementation semantics for behaviors not defined in specs, and not backfilling specs. Following paths **do not violate** this section (and are encouraged compliant paths):

- In `feature_development` / `architecture_evolution`, **first write pending behaviors into `.AI/project/specs/`** (source tagged "derived definition", record inference basis and pending confirmations), then implement per that.
- New project cold start, specs from zero to defined "definition work" itself is controlled task, not violation.

Judgment standard: Does implementation leave traceable definition in **spec layer**. If yes, compliant; if no, violation.

**Frontend Verification Prohibitions**:

- Frontend page changes declare complete without browser automation verification
- Substitute visual regression testing (screenshot comparison) for functional interaction verification

## 28. Deviation Handling

If must deviate from this document, AI must:

1. Explicitly state deviation content
2. Explain deviation reason
3. Explain risks and impacts
4. Wait for user approval
5. Write deviation record into task brief, work order, or verification result

Unrecorded/unapproved deviations treated as process violations.

## 29. Condensed Conclusion

ALDS condenses to three sentences:

1. Project facts are boundaries, specs and architecture are contracts.
2. AI may execute, but must execute within process, work order, and verification chain.
3. Risks must be visible, approvals traceable, completion auditable.