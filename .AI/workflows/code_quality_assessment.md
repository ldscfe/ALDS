---
title: Code Quality Assessment Workflow
doc_type: workflow
status: active
scope: workflow
updated: 2026-06-07
description: Assessment workflow for code quality, maintainability, and technical debt
variables: [project, languages, active_language, scope, analysis_mode, artifact_kind, intent_mode, risk_level]
---

# Workflow: Code Quality Assessment

For code quality, technical debt, code smells, maintainability assessment; does not directly implement.

## Required Documents

> Load rules: Strictly in this table order, skip already-read documents (READ_SET deduplication).

| Order | Document | Condition | Variables |
| :--- | :--- | :--- | :--- |
| 1 | `.AI/project/project.yaml` | Project task and fact source status `initialized`; ALDS standard package self-maintenance with fact source `absent` records missing and skips | — |
| 2 | `.AI/start.md` | Always | — |
| 3 | `.AI/standards/enums.md` | When first need to write assessment conclusion status | — |
| 4 | `.AI/process/review/code_review_checklist.md` | When involves code quality issue investigation | `task_type` |
| 5 | General skills (see table below) | Match by assessment intent | `task_type` |
| 6 | Language-specific skills (see table below) | General skill loaded and `languages` contains corresponding language | `task_type`, `languages` |

### Skill Matching

| Assessment Intent | General Skill | Language-Specific |
| :--- | :--- | :--- |
| Code quality/code smells | `skills/reviewer.md` | `skills/reviewer-{lang}.md` |
| Technical debt analysis | `skills/refactoring.md` | `skills/refactoring-{lang}.md` |
| Performance issues | `skills/performance.md` | `skills/performance-{lang}.md` |
| Security issues | `skills/security.md` | `skills/security-{lang}.md` |

## Flow

1. Confirm assessment boundaries
2. Read general documents first, then match language or module specialized documents
3. Hit relevant general and language-specific skills
4. Record findings with evidence
5. Route immediately implementable items back to `feature_development.md`

## Output

- Assessment results, placed in `reports/active/analysis/`
- Priority recommendations
- Follow-up routing
- Generate technical debt records in `reports/active/debt/` when necessary

## Fallback

- Assessment requires implementation fix → `.AI/workflows/feature_development.md`
- Involves architecture/specs/governance changes → `.AI/workflows/architecture_evolution.md`
- Unclear routing → `.AI/workflows/unified_task_flow.md`
- Existing processes cannot safely express → `.AI/workflows/create_workflow.md`

Variables required for fallback see `.AI/standards/workflow_engine.md` §fallback contract.