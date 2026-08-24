---
title: Simple Task List Template
doc_type: simple_task_list
status: active
scope: ALDS
updated: 2026-08-10
---

# Simple Task List

Line-by-line ledger of completed simple tasks. Each simple task occupies **one row**, newest on top (new entries inserted at first row below header, fixed insert position). Spec in `.AI/standards/alds_standards.md` §16, `.AI/start.md` §6.2 and §10.

Simple tasks not recorded in project pulse; this list is their only ledger.

| Date | Summary | Type | Risk | Net Change | Verification/Evidence | Work Order Path |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `YYYY-MM-DD` | One-line what was done | `bugfix`/`optimization`/`feature`/`documentation`/... | `low`/`medium` | `+12/-3 · 2 files` | Test results or evidence path | `reports/completed/plan/{base}_{YYYYMMDD_HHMM}.md` |

Field Rules:

- **Date**: Completion date (`YYYY-MM-DD`)
- **Summary**: One-line what was done
- **Type**: Task type enum (`enums.md` §4)
- **Risk**: Risk level (≤ `medium` for simple tasks, `enums.md` §6.1)
- **Net Change**: Net added/deleted lines & affected file count
- **Verification/Evidence**: Test results or evidence path/link; no evidence = no entry
- **Work Order Path**: Archived work order path (`reports/completed/plan/{base}_{YYYYMMDD_HHMM}.md`)