---
title: Project Pulse Task Brief Derivation
doc_type: process
status: active
scope: ALDS
updated: 2026-05-21
description: Rules for deriving task briefs from Project Pulse other-task ledger entries
---

# Project Pulse Task Brief Derivation Specification

This specification governs deriving formal task briefs from project pulse "4. Other Task Ledger" candidates during project development.

This spec not used for project initialization flow, and does not replace standard feature development task brief creation flow. Initialization flow may generate project pulse and initial ledger, but ledger items only subject to this spec when deriving task briefs in subsequent development phase.

## Applicability

This spec applies to:

- Project pulse documents generated from `.AI/templates/project_pulse_template.md`
- Candidates in project pulse "4. Other Task Ledger"
- Any items marked as candidate task brief, pending analysis, pending derivation, or need task brief completion

This spec particularly constrains:

- `optimization`
- `testing`
- `technical_debt`
- Candidates from review, verification, performance analysis, quality assessment, or technical debt organization

## Derivation Principles

1. Project pulse ledger items are not formal task briefs.
2. Optimization, testing, technical debt candidates must have analysis report first, then decide task brief derivation.
3. Analysis reports must not auto-equate to task brief derivation basis; whether to derive task brief must be decided by analysis conclusion.
4. Formal task briefs must record source analysis report, follow-up execution workflow, and related documents.
5. Project pulse ledger derivation status, analysis report paths, task brief paths must sync with actual file states.

## Derivation Status

Project pulse "Other Task Ledger" may use following derivation statuses:

| Status | Meaning |
| :--- | :--- |
| `candidate` | Candidate, only recorded in ledger, not yet judged if analysis needed |
| `analysis_required` | Must generate analysis report first, must not directly create task brief |
| `analysis_in_progress` | Analysis in progress, report not yet complete |
| `analysis_done` | Analysis report complete, awaiting derivation decision |
| `task_brief_required` | Analysis conclusion requires formal task brief generation |
| `task_brief_created` | Formal task brief generated and backfilled to ledger |
| `dismissed` | Not deriving task brief after analysis, reason recorded |
| `done` | Task completed and closed loop |

These statuses for project pulse ledger derivation process, may coexist with global execution status. If project pulse template has only one status column, prefer recording derivation status and supplement execution status in remarks.

## Derivation Flow

1. Register candidate in project pulse "Other Task Ledger".
2. Judge task type and source.
3. If type is `optimization`, `testing`, `technical_debt`, set derivation status to `analysis_required`.
4. Generate analysis report per corresponding workflow or analysis method.
5. After analysis complete, set derivation status to `analysis_done`, backfill analysis report path.
6. Decide task brief derivation per analysis conclusion.
7. If deriving, set derivation status to `task_brief_required`, create task brief using `.AI/templates/task_brief_template.md`.
8. After task brief creation, set derivation status to `task_brief_created`, backfill task brief path.
9. If not deriving, set derivation status to `dismissed`, record reason in remarks.
10. After task completion archive, set derivation status to `done`.

## Analysis Report Minimum Requirements

Optimization, testing, technical debt candidate analysis reports must include at minimum:

| Field | Requirement |
| :--- | :--- |
| Source Ledger Item | Points to candidate item number or title in project pulse |
| Analysis Type | `optimization`, `testing`, `technical_debt`, or other controlled type |
| Analysis Objective | States what this analysis judges |
| Scope / Non-Scope | Clearly states covered and uncovered objects |
| Evidence | Data, logs, code facts, test results, review findings, reproduction records |
| Impact Scope | States affected modules, interfaces, data, processes, user scenarios |
| Risk Level | Use `.AI/standards/enums.md` risk levels |
| Conclusion | Clearly states whether issue stands, whether action needed |
| Derivation Recommendation | Whether to derive task brief |
| Suggested Task Type & Priority | If deriving, give suggested type and priority |
| Unconfirmed Items | Records info still needing confirmation |

Type-specific additions:

| Type | Additional Requirements |
| :--- | :--- |
| `optimization` | Must include baseline data, target metrics, bottleneck evidence |
| `testing` | Must include test objectives, coverage scope, acceptance criteria |
| `technical_debt` | Must include risk level, impact scope, improvement suggestions |

## Non-Derivation Paths

Analysis complete may not derive task brief. Common reasons:

- Issue doesn't exist or insufficient evidence
- Risk low, not worth tasking
- Already covered by other task brief
- Should merge into existing task brief
- Only observation item or future risk reserve
- Need external input or project fact confirmation

When not deriving must:

1. Write non-derivation reason in analysis report.
2. Set project pulse ledger derivation status to `dismissed`.
3. Record reason or link to covering task in remarks.

## Task Brief Field Requirements

Task briefs derived from analysis reports must contain:

| Field | Rule |
| :--- | :--- |
| `source_analysis` | Required for optimization, testing, technical_debt; points to source analysis report |
| `workflow` | Points to follow-up execution workflow, not necessarily source analysis workflow |
| `related_docs` | Must include `source_analysis` pointed analysis report |

Task brief body must explain conclusions derived from analysis report, must not just copy user original request or ledger summary.

## Ledger Field Requirements

Project pulse "Other Task Ledger" should support:

| Field | Purpose |
| :--- | :--- |
| ID | Stable candidate identifier |
| Type | Task type |
| Summary | Candidate overview |
| Derivation Status | Derivation status per this spec |
| Source | User request, review findings, verification results, analysis conclusions, etc. |
| Priority | Controlled priority |
| Risk | Controlled risk level |
| High-Level Verification | High-level verification type or evidence source |
| Analysis Report | Analysis report path |
| Task Brief | Formal task brief path |
| Remarks | Non-derivation reason, merge explanation, blockers, supplements |

Sync Rules:

- At analysis start, must update derivation status.
- After analysis complete, must backfill analysis report path.
- After task brief generation, must backfill task brief path.
- When not deriving, must record reason.
- Ledger paths must not point to non-existent formal artifacts.

## Exception Rules

- Project initialization flow not subject to this spec.
- Project pulse reorganization, archiving, index repair, etc. low-risk governance tasks may directly generate task brief or work order.
- Architecture, process, standard change governance tasks still require corresponding review or architecture evolution flows.
- Documentation tasks just filling gaps may handle directly; if from quality issues, process gaps, or architecture disputes, should analyze or review first.

## Prohibitions

- Prohibited from directly deriving optimization, testing, technical_debt task briefs without analysis report.
- Prohibited from skipping analysis phase claiming optimization, testing补齐, or technical debt handling already established.
- Prohibited from leaving analysis report path, task brief path, or derivation status empty while claiming ledger synced.
- Prohibited from deriving task briefs for issues analysis concluded don't stand.
- Prohibited from using work orders as project pulse "Other Task Ledger" main entries.