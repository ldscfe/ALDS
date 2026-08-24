---
title: Create Workflow
doc_type: workflow
status: active
scope: workflow
updated: 2026-06-07
description: Controlled workflow for defining a new workflow when existing paths are insufficient
variables: [gap_reason, scope, approval_state]
---

# Workflow: Create Workflow

Only used when existing workflows cannot cover the task.

## Required Documents

> Load rules: Strictly in this table order, skip already-read documents (READ_SET deduplication).

| Order | Document | Condition | Variables |
| :--- | :--- | :--- | :--- |
| 1 | `.AI/project/project.yaml` | Project task and fact source status `initialized`; ALDS standard package self-maintenance with fact source `absent` records missing and skips | — |
| 2 | `.AI/start.md` | Always | — |
| 3 | `.AI/standards/workflow_engine.md` | Always (understand workflow definition spec) | — |
| 4 | `.AI/standards/enums.md` | When first need to write proposal status | — |
| 5 | `.AI/process/audit/code_change_audit_protocol.md` | Understand new workflow audit requirements | — |

## Flow

1. Explain why existing workflows insufficient
2. Define new workflow's inputs, branches, reading scope, outputs, fallbacks
3. Wait for approval
4. After approval write to `.AI/workflows/`

## Output

- New workflow proposal
- New workflow document

## Fallback

This workflow is a terminal workflow; output is new workflow document. If downstream judges "proposal rejected" or "task should use existing workflow", must return workflow draft with rejection reasons to `.AI/workflows/unified_task_flow.md` for reclassification, **must not** retain half-finished workflow document.

Variables required for fallback see `.AI/standards/workflow_engine.md` §fallback contract.