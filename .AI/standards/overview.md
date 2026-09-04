---
title: ALDS Standards Overview
doc_type: standard
status: active
scope: ALDS
updated: 2026-09-04
summary: Summary of the ALDS standards layer and reading guidance.
related_docs:
  - .AI/standards/alds_standards.md
  - .AI/standards/workflow_engine.md
  - .AI/standards/enums.md
---

# ALDS Standards Overview

This document is a summary and reading guide for `.AI/standards/alds_standards.md`; it does not define new rules on its own. If this document differs from `alds_standards.md`, `alds_standards.md` prevails.

## 1. Standards Layer Responsibilities

`.AI/standards/` holds ALDS general specifications, not project-specific facts.

| File | When to Read |
| :--- | :--- |
| `alds_standards.md` | Need to confirm ALDS methodology, layering model, artifact lifecycle, approval, verification, risk, archiving, and prohibitions |
| `workflow_engine.md` | Need to decide which workflow a task enters, how to fall back, and when to create a new workflow |
| `enums.md` | Need to record controlled values such as approval conclusions, statuses, task types, priorities, and risk levels |
| `task_profile.md` | Need to determine the five task profile fields, expected minimum read set, negative list, and skills hit count |
| `index.md` | Need a quick view of the standards directory's file responsibilities |

## 2. Core Summary

ALDS baseline requirements:

1. First read `.AI/index.md` and check the instance fact source status of `.AI/project/project.yaml`; read the project fact source when the status is `initialized`
2. Regular tasks enter `.AI/start.md`
3. Project initialization enters `.AI/project/init.md`
4. Project facts come only from `.AI/project/project.yaml` and `.AI/project/`
5. All regular tasks must enter an explicit workflow
6. Task briefs, work orders, write plans, and verification results must wait for user approval
7. The project pulse records task brief status; task briefs record the status of their work orders
8. Verification evidence takes precedence over subjective statements
9. After completion, documents must be updated, archived, and a git commit written
10. Task weight is decided by the task path determination in `start.md` §6.1 (micro task / simple task / engineering task), no longer by the project-level `governance_mode`; criteria and escalation triggers are in `start.md` §6.1 and `enums.md` §13

## 3. Quick Decisions

| Question | Read |
| :--- | :--- |
| What are ALDS's overall rules | `.AI/standards/alds_standards.md` |
| Which flow does this kind of request follow | `.AI/standards/workflow_engine.md` |
| Which English value should an approval or status record | `.AI/standards/enums.md` |
| Is a task micro, simple, or engineering | `.AI/start.md` §6.1, `.AI/standards/enums.md` §13 |
| What files are in the standards directory | `.AI/standards/index.md` |

## 4. Boundaries

- Project facts live in `.AI/project/`
- Workflow definitions live in `.AI/workflows/`
- Review, verification, audit, and other processes live in `.AI/process/`
- Optional skills live in `.AI/skills/`
- Task briefs, work orders, and archive records live in `reports/`
