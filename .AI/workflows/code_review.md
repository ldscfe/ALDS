---
title: Code Review Workflow
doc_type: workflow
status: active
scope: workflow
updated: 2026-06-07
description: Findings-first workflow for reviewing existing changes
variables: [languages, active_language, module, review_level, artifact_kind, risk_level]
---

# Workflow: Code Review

Used when task is reviewing existing changes.

## Required Documents

> Load rules: Strictly in this table order, skip already-read documents (READ_SET deduplication).

| Order | Document | Condition | Variables |
| :--- | :--- | :--- | :--- |
| 1 | `.AI/project/project.yaml` | Project task and fact source status `initialized`; ALDS standard package self-maintenance with fact source `absent` records missing and skips | — |
| 2 | `.AI/start.md` | Always | — |
| 3 | `.AI/process/review/code_review_checklist.md` | Required for code review | — |
| 4 | `.AI/standards/enums.md` | When first need to write review conclusion status | — |
| 5 | `.AI/project/architecture/` | Review involves architecture changes | `target_area`, `change_scope` |
| 6 | `.AI/project/specs/` | Review involves functional spec deviations | `target_area` |
| 7 | `.AI/project/guards/` | Review involves security constraints | `risk_level` |
| 8 | `skills/reviewer.md` | Required for code review | — |
| 9 | `skills/reviewer-{lang}.md` | Corresponding language-specific file exists | `languages` |
| 10 | Related task briefs, work orders, verification records | Review scope involves existing deliverables | — |

## Flow

1. Define review target and scope
2. Read general review documents first, then match language-specific review documents
3. Prioritize checking contract alignment, risk, verification adequacy
4. Output findings first, then give follow-up routing

## Output

- Review findings → `reports/active/review/`
- Risk conclusions
- Follow-up routing recommendations

## Fallback

- Actually need to implement fix → `.AI/workflows/feature_development.md`
- Involves architecture/specs/governance changes → `.AI/workflows/architecture_evolution.md`
- Unclear routing → `.AI/workflows/unified_task_flow.md`
- Existing processes cannot safely express → `.AI/workflows/create_workflow.md`
- Review feedback needs formal adoption process → `.AI/workflows/review_feedback_adoption.md`

Variables required for fallback see `.AI/standards/workflow_engine.md` §fallback contract.