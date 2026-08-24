---
title: Architecture Evolution Workflow
doc_type: workflow
status: active
scope: workflow
updated: 2026-08-10
description: Controlled evolution workflow for architecture, specifications, process, and governance changes
variables: [languages, active_language, doc_type, change_scope, impact_level, intent_mode]
---

# Workflow: Architecture Evolution

For controlled changes to architecture, specifications, processes, and governance documents.

## Required Documents

> Load rules: Strictly in this table order, skip already-read documents (READ_SET deduplication).

| Order | Document | Condition | Variables |
| :--- | :--- | :--- | :--- |
| 1 | `.AI/project/project.yaml` | Project task and fact source status `initialized`; ALDS standard package self-maintenance with fact source `absent` records missing and skips | — |
| 2 | `.AI/start.md` | Always | — |
| 3 | `.AI/standards/overview.md` | Always (understand standards framework) | — |
| 4 | `.AI/standards/enums.md` | When first need to write change proposal status | — |
| 5 | `.AI/process/review/architecture_review.md` | Required for architecture changes | — |
| 6 | `.AI/process/review/spec_review.md` | Required for spec changes | — |
| 7 | `.AI/project/architecture/` | Always (core context for architecture evolution) | — |
| 8 | `.AI/project/specs/` | Spec evolution involves specific module | `target_area` |
| 9 | `.AI/project/invariants/` | Evolution may affect system guarantees | `target_area` |
| 10 | `.AI/project/guards/` | Evolution involves security constraint changes | `impact_level` |
| 11 | General skills (see table below) | Match by change type | `task_type` |
| 12 | Language-specific skills (see table below) | General skill loaded and `languages` contains corresponding language | `task_type`, `languages` |

### Skill Matching

| Change Type | General Skill | Language-Specific |
| :--- | :--- | :--- |
| Performance architecture adjustment | `skills/performance.md` | `skills/performance-{lang}.md` |
| Security architecture adjustment | `skills/security.md` | `skills/security-{lang}.md` |
| Database/storage adjustment | `skills/database.md` | `skills/database-{lang}.md` |
| API/protocol adjustment | `skills/api.md` | `skills/api-{lang}.md` |

## Flow

1. Analyze proposed change boundaries
2. Read general documents first, then match language or module specialized documents
3. Form proposal; if proposal involves changing task path judgment (`start.md` §6.1), simple task conditions, or upgrade triggers, must explicitly explain change rationale, impact scope, backfill plan for existing task ledger
4. Review and wait for approval
5. After approval update governance documents
6. If subsequent implementation needed, route to `feature_development.md`

## Output

- Change proposal
- Review results
- Updated governance documents

## Prohibited

- Modify governance without approval
- Mix implementation development in this flow

## Fallback

- Proposal rejected and needs reclassification → `.AI/workflows/unified_task_flow.md`
- Subsequent implementation needed → `.AI/workflows/feature_development.md`
- Involves code review feedback → `.AI/workflows/code_review.md`
- Existing processes cannot safely express new governance structure → `.AI/workflows/create_workflow.md`

Variables required for fallback see `.AI/standards/workflow_engine.md` §fallback contract.