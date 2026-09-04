---
version: 1.1.0
status: active
scope: ALDS
updated: 2026-08-02
---

# Workflow Engine

Workflow engine is the mandatory routing layer for every task.

## Core Rules

1. Route first, act later
2. First check `.AI/project/project.yaml` instance fact source status; if `initialized` read, ALDS standard package self-maintenance with `absent` records missing and continues
3. Prefer existing workflows
4. Fallback to default process when no match
5. After workflow selection must execute `skills` hit judgment
6. Only read minimum relevant context, following "general first, specialized later"
7. Produce task brief or work order first, then execute changes

## Standard Skeleton

1. Check `.AI/project/project.yaml` instance fact source status; if `initialized` read and bind project facts, standard package self-maintenance with `absent` records missing and continues
2. Identify user intent and input artifacts
3. Bind variables
4. Select workflow
5. Match general and language-specific skills
6. Load minimum context
7. Form task brief or work order
8. Execute or review
9. Verify and audit
10. Status sync and archive

## Routing Order

1. Read `.AI/start.md`
2. Read this file
3. Check `.AI/workflows/index.md`
4. If no specialized process, fallback to `.AI/workflows/unified_task_flow.md`
5. Only when existing processes cannot express task, use `create_workflow.md`

## Recommended Variables

- `project`
- `governance_mode` — from `project.yaml → governance.governance_mode` (fixed as `full`; task weight determined by `start.md` §6.1 task path judgment, reference `enums.md` §12)
- `languages` — from `project.yaml → languages`, all project-supported languages
- `active_language` — current task language, determined per §4.1 rules
- `summary`
- `artifact_kind`
- `task_type`
- `intent_mode`
- `change_scope`
- `risk_level`
- `target_area`
- `module`

### 4.1 active_language Determination Rules

`active_language` used for language-specific file selection during skill matching. In multi-language projects, determine by following priority:

1. **Work Order Declaration**: If work order `languages` field explicitly specifies single language, `active_language = that language`
2. **Workflow Variable**: If workflow document `variables` specifies `active_language`, use that
3. **Module Mapping**: Match `target_area` or `module` to module-language mapping in `.AI/project/project.yaml`
4. **User Explicit Specification**: Language explicitly specified in user input
5. **Default Fallback**: Above all unsatisfied, use first value of `project.yaml → languages`

> In multi-language projects, different work orders may specify different `active_language`. Re-determine per work order execution.

## Input Artifacts

Following input artifacts supported by default:

- `user_request`
- `review_feedback`
- `test_failure`
- `build_failure`
- `static_analysis_report`
- `performance_report`
- `spec_gap`
- `architecture_conflict`

## Minimum Reading Rules

Read in following order. Each workflow's "required documents" table is the **sole authoritative basis**.

> **Priority Rule**: Once workflow selected, must use that workflow's required documents table as sole authoritative basis. This section's default load order only for reference when creating or revising workflows; must not override selected workflow's required table at execution time.

> **Task Profile First**: Before loading per this section order, first determine five-field profile per `.AI/standards/task_profile.md` §3 (only from task description + `project.yaml`, not reading project context subdirectories), use it to evaluate required table conditional rows, and apply §5 negative list and §6 skills hit count constraints.

1. `.AI/project/project.yaml` — Only when instance fact source status `initialized`; standard package self-maintenance with fact source `absent` records missing and skips
2. `.AI/start.md`
3. Selected workflow document
4. Load per workflow required documents table (project context → general skills → language-specific skills → process documents)

Additional rules:

- `skills` do not participate in workflow routing, but must do hit judgment after workflow selection
- When hitting `skills`, must read base-name skill file first, then language-specific skill file
- **Hit Count Constraint**: One task defaults to max 1 general skill + 1 language-specific skill (most relevant to `task_type` main axis); if second skill domain truly needed, record reason. Authoritative definition in `.AI/standards/task_profile.md` §6
- When project doc naming not fixed, prefer directory-locating minimum relevant docs; if specialized files exist, may match `*_<language>.md`, `*-<language>.md`, or language subdirectories

## Document Deduplication Principle

READ_SET maintained by AI in conversation, not persisted.

### 7.1 Rules

1. At task start, READ_SET = `{}`
2. Before loading any document, check if document path already in READ_SET
3. Exists → skip load, directly reference existing conclusion
4. Not exists → load full text, add path to READ_SET
5. Default deduplication granularity is **document path** (operational rule): same document already read skips entirely. Anchor-level fine-grained deduplication (same document different sections not duplicate) is **optional optimization**, only enabled when Agent can reliably maintain anchor-level READ_SET; when unreliable, must not re-load entire doc just because "section not read"

### 7.2 Cross-Workflow Transfer

When workflow A routes to workflow B, READ_SET transfers whole, no reset. B only loads new documents not in READ_SET.

## Skill Matching Rules

### 8.1 Matching Formula

> Language-specific skill path = `skills/{base_name}-{language}.md`
> Where `{base_name}` from general skill hit, `{language}` determined by: `active_language` (current task specified) → `project.yaml → languages` first value → fallback to general skill when no language-specific file

### 8.2 General Skill Hit Table

| Task Intent Keywords | General Skill File | Language-Specific Pattern |
| :--- | :--- | :--- |
| Performance, throughput, latency, bottleneck | `skills/performance.md` | `skills/performance-{lang}.md` |
| Security, vulnerability, injection, permission | `skills/security.md` | `skills/security-{lang}.md` |
| Test, verification, regression, mock | `skills/testing.md` | `skills/testing-{lang}.md` |
| Debug, troubleshooting, logs, errors | `skills/debugging.md` | `skills/debugging-{lang}.md` |
| Database, query, ORM, storage | `skills/database.md` | `skills/database-{lang}.md` |
| API, interface, protocol, serialization | `skills/api.md` | `skills/api-{lang}.md` |
| Refactor, cleanup, technical debt | `skills/refactoring.md` | `skills/refactoring-{lang}.md` |
| Code review, review | `skills/reviewer.md` | — |
| Documentation, writing, comments | `skills/writing.md` | `skills/writing-{lang}.md` |
| Optimization, speedup, resource reduction | `skills/optimizer.md` | `skills/optimizer-{lang}.md` |
| Incident investigation, root cause | `skills/troubleshooting.md` | `skills/troubleshooting-{lang}.md` |
| **Git operations, commit, diff, push** | `skills/git.md` | — |

### 8.3 Matching Process

1. Derive general skill to hit from task intent and `task_type`
2. Load corresponding general skill file (check READ_SET deduplication)
3. Check `project.yaml → languages` or `active_language` for corresponding language-specific file
4. If exists, overlay load (check READ_SET deduplication)
5. When language undetermined, must not reverse-infer project language just because language-specific file exists

> **Task Path Independent Matching**: Skill matching rules do not vary by task path (micro/simple/engineering). All task tiers share skill hits and load order. `.AI/project/` subdirectory load conditions per §8.4 and `task_profile.md` §5 negative list strictly interpreted.

> **Hit Count Constraint**: Default max 1 general + 1 language-specific; multiple candidates take most relevant to `task_type` main axis, others not loaded. If second skill domain truly needed, record reason. Authoritative definition in `.AI/standards/task_profile.md` §6.

### 8.4 Project Context Directory Mapping

| Project Subdirectory | Load Condition | Related Variables |
| :--- | :--- | :--- |
| `.AI/project/architecture/` | `target_area` involves architecture, or `change_scope` contains architecture changes, or `task_type = governance` | `target_area`, `change_scope`, `task_type` |
| `.AI/project/specs/` | `target_area` involves functional module, or `task_type` is `feature` / `bugfix` | `target_area`, `task_type` |
| `.AI/project/invariants/` | Involves core logic changes, data flow modifications, or `risk_level ≥ high` | `target_area`, `risk_level` |
| `.AI/project/guards/` | Involves security constraints, external integrations, or `risk_level ≥ high` | `risk_level`, `task_type` |
| `.AI/project/process/` | `task_type = governance` or process change request | `task_type` |
| `.AI/project/roadmap/` | Involves milestone adjustments, phase planning, scope changes | `task_type`, `change_scope` |

> **Strict Loading**: `.AI/project/` subdirectories only loaded when `target_area` directly involves that subdirectory; "possibly relevant" "just in case" not loading basis; `project.yaml` remains required. Unified rules in `.AI/standards/task_profile.md` §5 and `.AI/start.md` §5.

### 8.5 Load Order

1. General governance documents (process docs, audit protocols, etc.)
2. Project context directories (per condition mapping table)
3. General skill files
4. Language-specific skill files
5. Process documents (audit protocols, review checklists, etc.)

## New Process Threshold

New process creation only allowed when one of:

1. Existing processes cannot safely express task
2. Task has unique approval or audit structure
3. Task introduces new governance boundaries

Default fallback: `.AI/workflows/unified_task_flow.md`

## Fallback Contract

When workflows fallback, must pass following minimum variable set to ensure target workflow can fully restore context on handoff:

| Variable | Meaning | Required |
| :--- | :--- | :---: |
| `task_brief_id` | Current task brief ID (format `TB-YYYY-MMDD-NNN`), empty if no task brief | ✓ |
| `current_step` | Current step name within workflow (e.g., "Flow 3 evaluate impact level") | ✓ |
| `block_reason` | Specific reason triggering fallback | ✓ |
| `evidence` | Intermediate artifacts produced (routing tables, spec drafts, review records, etc.), preserve paths | ✓ |
| `retry_hint` | Retry suggestions or preconditions | — |

Fallback operation sequence:

1. Append `## Suspend` section to current task brief or work order end, write above variables
2. Write `evidence` paths and `task_brief_id` into target workflow READ_SET
3. Jump to target workflow "required documents" table item 1 loading
4. **Must not rollback or delete already-produced intermediate artifacts**

Constraints:

- Workflow `## Fallback` section must be filled per above contract with target workflow + reference to this section
- No "naked fallback" (jump without passing variables)
- Multi-hop fallbacks: `evidence` accumulates, final landing workflow must parse all history
- Error handling (execution failure, verification failure, user rejection, AI unable to continue) fallback behavior in `.AI/process/error_handling.md`