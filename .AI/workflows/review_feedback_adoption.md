---
title: Review Feedback Adoption Workflow
doc_type: workflow
status: active
scope: workflow
updated: 2026-06-07
description: Workflow for evaluating and routing review feedback before implementation
variables: [languages, active_language, feedback_scope, impact_level, artifact_kind, task_type, intent_mode, change_scope, risk_level]
---

# Workflow: Review Feedback Adoption

Used before adopting, deferring, or rejecting review feedback.

## Required Documents

> Load rules: Strictly in this table order, skip already-read documents (READ_SET deduplication).

| Order | Document | Condition | Variables |
| :--- | :--- | :--- | :--- |
| 1 | `.AI/project/project.yaml` | Project task and fact source status `initialized`; ALDS standard package self-maintenance with fact source `absent` records missing and skips | — |
| 2 | `.AI/start.md` | Always | — |
| 3 | `.AI/process/review/review_feedback_adoption_protocol.md` | Required for adopting review feedback | — |
| 4 | `.AI/standards/enums.md` | When first need to write disposition conclusion status | — |
| 5 | `.AI/project/architecture/` | Feedback involves architecture changes | `change_scope`, `impact_level` |
| 6 | `.AI/project/specs/` | Feedback involves spec deviations | `feedback_scope` |
| 7 | `.AI/project/guards/` | Feedback involves security constraints | `impact_level` |
| 8 | General skills (see table below) | Match by feedback content | `task_type` |
| 9 | Language-specific skills (see table below) | General skill loaded and `languages` contains corresponding language | `task_type`, `languages` |

### Skill Matching

| Feedback Type | General Skill | Language-Specific |
| :--- | :--- | :--- |
| Performance improvement suggestions | `skills/performance.md` | `skills/performance-{lang}.md` |
| Security improvement suggestions | `skills/security.md` | `skills/security-{lang}.md` |
| Refactoring suggestions | `skills/refactoring.md` | `skills/refactoring-{lang}.md` |
| Testing improvement suggestions | `skills/testing.md` | `skills/testing-{lang}.md` |

## Flow

1. Categorize feedback scope
2. Read general documents first, then match language or module specialized documents
3. Evaluate impact level
4. Choose `adopt_now`, `adopt_with_conditions`, `defer`, or `reject`
5. If adopt and implement, generate or update task brief/work order

## Output

- Feedback disposition results
- Follow-up routing

## Prohibited

- Treat feedback as automatic commands
- Directly implement when touching governance boundaries

## Fallback

- Decide to implement → `.AI/workflows/feature_development.md`
- Involves architecture/specs/governance changes → `.AI/workflows/architecture_evolution.md`
- Unclear routing → `.AI/workflows/unified_task_flow.md`
- Existing processes cannot safely express → `.AI/workflows/create_workflow.md`

Variables required for fallback see `.AI/standards/workflow_engine.md` §fallback contract.