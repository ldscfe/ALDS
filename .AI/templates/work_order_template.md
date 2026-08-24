---
title: Work Order Template
doc_type: work_order
status: draft
project:
languages: []
summary:
workflow:
source_request:
updated:
related_docs: []
---

# Work Order

## 1. Basic Information

- Work Order ID:
- Owning Task Brief: (simple tasks fill "Simple Task")
- Matched Workflow:
- Matched Skills:
- Estimated Duration:
- Estimated Code Size:
- Estimated Effort:

## 2. Execution Definition

- Execution Objective:
- Summary:
- Input Documents:
- Implementation Scope:
- Outputs:
- Preconditions: (engineering tasks may reference owning task brief, avoid duplication)
- Dependencies: (engineering tasks may reference owning task brief, avoid duplication)

### 2.1 Layout Diagram & Functional Overview (Frontend Tasks)

Only fill when work order involves frontend page/UI component/interaction path changes; non-frontend work orders delete this section. Spec in `alds_standards.md` §16.3.

**Layout Diagram** (dot-line ASCII, in code block, each area tagged with `[short label]`, only expresses area structure & functional mapping, not pixels/visuals):

```
┌──────────────┬────────────┐
│ [header]     │ [nav]      │
├──────────────┴────────────┤
│ [main]                    │
│                           │
└───────────────────────────┘
```

**Functional Overview** (indexed by area tags, one line per area describing role/behavior/data; no separate "element at position" prose):

- [header]:
- [nav]:
- [main]:

### 2.2 Write Plan

Only fill when work order involves creating or modifying `.AI/project/` documents; leave empty or delete when not applicable.

| Target File | Section Summary | Fact Source | Inferred Items | Pending Confirmations | Estimated Change Scope | Approval Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|  |  |  |  |  |  | `pending` |

## 3. Operation Steps

1.
2.
3.

## 4. Verification Methods

- If work order involves frontend page changes, verification must include browser automation E2E tests (Playwright preferred), covering core user paths of new/modified features
- Code acceptance must pass ALL checks in `.AI/process/review/code_review_checklist.md` "Work Order Code Acceptance Hard Requirements" (C1-C6)

### 4.1 Verification Matrix Execution Items

When owning task brief declares verification matrix, fill this work order's responsible matrix items per `.AI/process/validation/validation_matrix.md`.

| Dimension | Coverage Value | Verification Method | Coverage Level | Result | Evidence | Uncovered Risk |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|  |  |  |  | `pending` |  |  |

## 5. Execution Results

Fill after work order completion; leave empty when incomplete.

- Actual Change Volume:
- Actual Completion Time:
- Actual Modified Files List:
- Key Decisions & Deviations:
- Verification Evidence:

## 6. Status

- Approval Status: `draft`
- Execution Status: `pending`

## 7. Approval

- User Conclusion: `approved / rejected / needs_update`