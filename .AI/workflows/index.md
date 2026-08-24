---
version: 1.0.2
status: active
scope: ALDS
updated: 2026-08-10
---

# Workflow Index

This directory stores executable ALDS workflows.

## Usage

1. First check `.AI/project/project.yaml` instance fact source status; if `initialized` read project fact source, ALDS standard package self-maintenance with `absent` status records missing and skips
2. Then read `.AI/start.md`
3. Then match workflow per this index
4. If no match, fallback to `unified_task_flow.md`

## Workflow Table

| Trigger | Workflow | Purpose |
| :--- | :--- | :--- |
| General routing, mixed input, unclear task | `unified_task_flow.md` | Default routing |
| Development, fix, bounded refactor, documentation implementation | `feature_development.md` | Implementation flow |
| Performance bottleneck, optimization analysis | `performance_analysis.md` | Analyze first, implement later |
| Review existing changes | `code_review.md` | Findings first |
| Adopt review feedback | `review_feedback_adoption.md` | Disposition then advance |
| Quality assessment, technical debt identification | `code_quality_assessment.md` | Assessment flow |
| Architecture, specs, process, governance updates | `architecture_evolution.md` | Evolution flow |
| Existing processes insufficient | `create_workflow.md` | Controlled extension |

> **Task Path Note**: All workflows routed per this index, then at execution §flow per `.AI/start.md` §6.1 judge task path (simple task/engineering task). Simple tasks directly generate single work order (frontend includes layout diagram), engineering tasks generate task brief + work order(s). Task path does not change workflow routing, only decides whether task brief needed.

## Fallback Specification

Each workflow's `## Fallback` section at end filled per unified template, specifying target workflows for inter-workflow jumps and fallback contract. Variable passing spec in `.AI/standards/workflow_engine.md` §fallback contract.