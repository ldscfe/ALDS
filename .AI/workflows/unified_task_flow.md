---
title: Unified Task Flow
doc_type: workflow
status: active
scope: workflow
updated: 2026-09-04
description: Default routing workflow for tasks that do not immediately match a specialized path
variables: [project, languages, active_language, summary, artifact_kind, task_type, intent_mode, change_scope, risk_level, target_area, module]
---

# Workflow: Unified Task Flow

Used when task temporarily cannot directly fit into a specialized process.

## Required Documents

> Load rules: Strictly in this table order, skip already-read documents (READ_SET deduplication).

| Order | Document | Condition | Variables |
| :--- | :--- | :--- | :--- |
| 1 | `.AI/project/project.yaml` | Project task and fact source status `initialized`; ALDS standard package self-maintenance with fact source `absent` records missing and skips | — |
| 2 | `.AI/start.md` | Always | — |
| 3 | `.AI/standards/workflow_engine.md` | Always (understand routing rules) | — |
| 4 | `.AI/standards/enums.md` | When first need to write routing conclusion or status | — |
| 5 | General skills (see table below) | Match by input artifact type | `artifact_kind` |
| 6 | Language-specific skills (see table below) | General skill loaded and `languages` contains corresponding language | `artifact_kind`, `languages` |

### Skill Matching

| Input Artifact | General Skill | Language-Specific |
| :--- | :--- | :--- |
| `performance_report` | `skills/performance.md` | `skills/performance-{lang}.md` |
| `static_analysis_report` | `skills/reviewer.md` | `skills/reviewer-{lang}.md` |
| `test_failure` | `skills/testing.md` | `skills/testing-{lang}.md` |
| `build_failure` | `skills/debugging.md` | `skills/debugging-{lang}.md` |
| `review_feedback` | `skills/reviewer.md` | `skills/reviewer-{lang}.md` |
| `spec_gap` | `skills/writing.md` | — |
| `architecture_conflict` | `skills/debugging.md` | `skills/debugging-{lang}.md` |

## Branching

| Condition | Routing |
| :--- | :--- |
| Architecture, specs, process, governance changes | `architecture_evolution.md` |
| Performance analysis or optimization priority | `performance_analysis.md` |
| Review feedback disposition | `review_feedback_adoption.md` |
| Clear development, fix, refactor task | `feature_development.md` |
| Clear review request | `code_review.md` |
| Clear quality assessment request | `code_quality_assessment.md` |
| Cannot clearly categorize | Current flow continues |

## Flow

1. Identify user intent and input artifact
2. Bind project name, language, summary from `.AI/project/project.yaml`
3. Match possible general and language-specific skills
4. Mark unknown scope, do not invent
5. Decide which specialized process to enter, and per `.AI/start.md` §6.1 judge task path:
   1. **Micro Task**: Direct execution, no work order, add line to `reports/micro_tasks.md` (status `unverified`), verification deferred
   2. **Simple Task**: Directly generate single work order, skip task brief
   3. **Engineering Task**: Generate independent task brief
6. If cannot implement yet, generate task brief (engineering task) or work order (simple task)

## Output

- Routing result
- Task brief draft (engineering task), work order draft (simple task), or micro task ledger line (micro task)
- Risk explanation

## Fallback

- Identifiable as feature implementation → `.AI/workflows/feature_development.md`
- Identifiable as architecture/governance change → `.AI/workflows/architecture_evolution.md`
- Identifiable as code review → `.AI/workflows/code_review.md`
- Identifiable as review feedback adoption → `.AI/workflows/review_feedback_adoption.md`
- Identifiable as quality/performance assessment → `.AI/workflows/code_quality_assessment.md` / `.AI/workflows/performance_analysis.md`
- Existing processes cannot safely express → `.AI/workflows/create_workflow.md`

Variables required for fallback see `.AI/standards/workflow_engine.md` §fallback contract.

## Mandatory Hard Stop

If after above fallback still judged "existing processes cannot cover" (i.e., back to `create_workflow.md` still not passed), must **force into task brief stage**:

1. Generate task brief with `task_type: governance`, `priority: high`
2. Task brief §4 must explicitly record:
   - Which existing workflows attempted
   - Why all rejected
   - Rationale for new workflow with minimum input/output/branch draft
3. **Stop and wait for user adjudication**, no further routing

Silent loops prohibited: `unified_task_flow → create_workflow → still insufficient → continue routing...` must terminate within 2 hops.