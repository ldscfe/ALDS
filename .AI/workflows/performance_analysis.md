---
title: Performance Analysis Workflow
doc_type: workflow
status: active
scope: workflow
updated: 2026-06-07
description: Evidence-first workflow for performance analysis and optimization routing
variables: [project, languages, active_language, target_area, analysis_type, artifact_kind, intent_mode, change_scope, risk_level]
---

# Workflow: Performance Analysis

For pre-optimization analysis, bottleneck identification, and performance evidence organization.

## Required Documents

> Load rules: Strictly in this table order, skip already-read documents (READ_SET deduplication).

| Order | Document | Condition | Variables |
| :--- | :--- | :--- | :--- |
| 1 | `.AI/project/project.yaml` | Project task and fact source status `initialized`; ALDS standard package self-maintenance with fact source `absent` records missing and skips | — |
| 2 | `.AI/start.md` | Always | — |
| 3 | `.AI/standards/enums.md` | When first need to write analysis conclusion status | — |
| 4 | `skills/performance.md` | Required for performance analysis | — |
| 5 | `skills/performance-{lang}.md` | Corresponding language-specific file exists | `languages` |
| 6 | `.AI/process/review/optimization_guide.md` | Required for performance analysis | — |
| 7 | `.AI/project/architecture/` | Involves architecture layer hotspot analysis | `target_area` |

## Flow

1. Clarify performance goals and metrics
2. Read general performance documents first, then match language-specific performance documents
3. Establish baseline or collect existing evidence
4. Form analysis conclusions
5. If implementation needed, back to `feature_development.md` to generate task brief and work orders

## Output

- Analysis report, placed in `reports/active/analysis/`
- Routing recommendations
- Task brief draft or implementation suggestions

## Prohibited

- Claim optimization success without evidence
- Make unapproved code changes in analysis flow

## Fallback

- Optimization conclusions need implementation → `.AI/workflows/feature_development.md`
- Involves architecture/specs changes → `.AI/workflows/architecture_evolution.md`
- Unclear routing → `.AI/workflows/unified_task_flow.md`
- Existing processes cannot safely express → `.AI/workflows/create_workflow.md`

Variables required for fallback see `.AI/standards/workflow_engine.md` §fallback contract.