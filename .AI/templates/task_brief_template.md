---
title: Task Brief Template
doc_type: task_brief
status: draft
project:
languages: []
summary:
workflow:
source_analysis:
source_request:
updated: 2026-06-01
related_docs: []
---

# Task Brief

## 1. Basic Information

- Task ID:
- Project Name:
- Development Languages:
- Project Summary:
- Matched Workflow:
- Matched Skills:
- Project Pulse Entry:
- Source Analysis Report:

## 2. Task Definition

Task brief core records three items: **User Requirement**, **Functional Overview**, **Work Order List** (see `alds_standards.md` §15); others are supporting fields.

- User Original Instruction:
- Functional Overview: (Overall description of functionality to implement; frontend features may reference owned work order layout diagrams, no prose re-description of layout)
- Objective:
- Scope:
- Non-Goals:
- Task Summary:
- Task Type:
- Priority:
- Risk Level:
- Estimated Code Size:
- Estimated Effort:
- Preconditions:
- Dependencies:

## 2.1 Initialization Task Supplement

Only fill when task type is project initialization or project governance baseline completion; standard tasks may leave empty or delete this section.

- Source Flow:
- Source Materials:
- Initialization Scope:
- Target Directories:
- Planned Files:
- Fact Source Priority:
- Inferred Items:
- Pending Confirmations:

## 2.2 Write Plan Summary

Only fill when task involves creating or modifying `.AI/project/` documents; leave empty or delete when not applicable.

| Target File | Section Summary | Fact Source | Inferred Items | Pending Confirmations | Estimated Change Scope | Approval Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|  |  |  |  |  |  | `pending` |

## 3. Milestones & Major Tasks

- Major Milestones:
- Major Tasks:
- Each Task Estimated Duration:

## 4. Required Documents

- `.AI/project/project.yaml` (must read when instance fact source status `initialized`; ALDS standard package self-maintenance with fact source `absent` records missing and skips)
- `.AI/index.md` (always required)
- Corresponding process document: standard tasks `.AI/start.md`, initialization tasks `.AI/project/init.md`, determined by `workflow` field
- Other documents required by workflow/process (load per corresponding required documents table)

## 5. Expected Outputs

-

## 6. Verification Methods

- If task involves frontend page changes, verification must include browser automation E2E tests (Playwright preferred), covering core user paths of new/modified features
-

### 6.1 Verification Matrix

When task affects multiple runtime conditions, data conditions, config conditions, or deliverables, fill per `.AI/process/validation/validation_matrix.md`; if not applicable, explain reason.

- Coverage Level: `full / representative / risk-based / smoke / not_applicable`
- Not Applicable Reason:

| Dimension | Coverage Value | Verification Method | Coverage Level | Expected Evidence | Uncovered Risk |
| :--- | :--- | :--- | :--- | :--- | :--- |
|  |  |  |  |  |  |

## 7. Work Order Register

| Work Order ID | Summary | Path | Status | Estimated Code Size | Estimated Effort | Actual Change | Actual Completion | Verification Evidence | Preconditions | Dependencies |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|  |  |  | `pending` |  |  |  |  |  |  |  |

## 8. Status

- Task Brief Status: `draft`
- Project Pulse Sync Status: `pending`

## 9. Approval

- Current Status: `draft`
- User Conclusion: `approved / rejected / needs_update`