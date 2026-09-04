---
title: Feature Development Workflow
doc_type: workflow
status: active
scope: workflow
updated: 2026-09-04
description: Controlled implementation workflow for bounded changes
variables: [project, languages, active_language, module, artifact_kind, task_type, change_scope, risk_level]
---

# Workflow: Feature Development

For feature development, bug fixes, bounded refactoring, and documentation implementation.

## Required Documents

> Load rules: Strictly in this table order, skip already-read documents (READ_SET deduplication).

| Order | Document | Condition | Variables |
| :--- | :--- | :--- | :--- |
| 1 | `.AI/project/project.yaml` | Project task and fact source status `initialized`; ALDS standard package self-maintenance with fact source `absent` records missing and skips | — |
| 2 | `.AI/start.md` | Always | — |
| 3 | `.AI/standards/enums.md` | When first need to write task brief or work order status | — |
| 4 | `.AI/process/audit/code_change_audit_protocol.md` | Required for feature development | — |
| 5 | `.AI/process/review/code_review_checklist.md` | Required for feature development | — |
| 6 | `.AI/project/architecture/` | `target_area` involves architecture or `change_scope` contains architecture changes | `target_area`, `change_scope` |
| 7 | `.AI/project/specs/` | `target_area` involves functional module | `target_area` |
| 8 | `.AI/project/invariants/` | Involves core logic changes or `risk_level ≥ high` | `target_area`, `risk_level` |
| 9 | `.AI/project/guards/` | Involves security constraints or `risk_level ≥ high` | `risk_level` |
| 10 | `.AI/project/process/` | Task involves process changes | `task_type` |
| 11 | `.AI/project/roadmap/` | Task involves milestone or planning adjustments | `task_type`, `change_scope` |
| 12 | General skills (see table below) | Match by task intent | `task_type` |
| 13 | Language-specific skills (see table below) | General skill loaded and `languages` contains corresponding language | `task_type`, `languages` |

### Skill Matching

| Task Intent | General Skill | Language-Specific |
| :--- | :--- | :--- |
| Performance optimization | `skills/performance.md` | `skills/performance-{lang}.md` |
| Security related | `skills/security.md` | `skills/security-{lang}.md` |
| Debugging/troubleshooting | `skills/debugging.md` | `skills/debugging-{lang}.md` |
| Database related | `skills/database.md` | `skills/database-{lang}.md` |
| API/interface | `skills/api.md` | `skills/api-{lang}.md` |
| Refactoring/technical debt | `skills/refactoring.md` | `skills/refactoring-{lang}.md` |
| Testing related | `skills/testing.md` | `skills/testing-{lang}.md` |
| Documentation writing | `skills/writing.md` | `skills/writing-{lang}.md` |

## Flow

1. Confirm scope and target files first
2. Read general docs first, then locate project docs by directory, finally match language or module specialized docs
3. Hit general and language-specific skills per task intent and language
4. Per `.AI/start.md` §6.1 judge task path:
   1. **Micro Task**: Direct execution, no work order, after completion add line to `reports/micro_tasks.md` (status `unverified`, verification deferred)
   2. **Simple Task**: Directly generate single work order (frontend tasks must include layout diagram and functional overview, `alds_standards.md` §16.3), skip task brief, after completion add line to `reports/simple_tasks.md`
   3. **Engineering Task**: Generate task brief first, then split one or more work orders based on task brief
5. Wait for user approval then implement
6. After completion execute verification and audit (test default action, `start.md` §9)
7. Archive work orders; engineering tasks backfill work order status in owning task brief, if task brief complete sync backfill `reports/project_pulse.md` task brief status

## Output

- Task brief (engineering task), work order (simple task), or micro task ledger line (micro task)
- Code or document changes
- Verification records

## Prohibited

- Implementation without contract
- Direct development without approval
- Touch architecture boundary without upgrade

## Fallback

- Boundary changes (architecture/specs/governance) → `.AI/workflows/architecture_evolution.md`
- Need code review feedback → `.AI/workflows/code_review.md`
- Unclear routing → `.AI/workflows/unified_task_flow.md`
- Existing processes cannot safely express → `.AI/workflows/create_workflow.md`

Variables required for fallback see `.AI/standards/workflow_engine.md` §fallback contract.