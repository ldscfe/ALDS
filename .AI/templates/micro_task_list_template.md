---
title: Micro Task List Template
doc_type: micro_task_list
status: active
scope: ALDS
updated: 2026-10-27
---

# Micro Task List

Line-by-line ledger of micro tasks (one-sentence requirements). Each micro task occupies **one row**, newest on top (new entries inserted at first row below header, fixed insert position). Spec in `.AI/standards/alds_standards.md` §11/§21.1, `.AI/start.md` §6.2 and §10.

Micro tasks generate no task brief and no work order, not recorded in project pulse; this list is their only ledger. Before development, the pre-development summary and approval gate (`start.md` §3.1) applies: present summary and handling proposal first (BUG: cause first); execute only after explicit user consent/approval; after execution, perform only necessary minimal verification and compile pass. Verification deferred to user test or centralized test; closure per status column. Ledger rows and git commits are not one-to-one (periodic categorized merge per `alds_standards.md` §23.1).

| Date | User Request | Result Summary | Type | Net Change | Status | Evidence |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `YYYY-MM-DD` | User one-sentence request (original or compact paraphrase) | One-line what was done and outcome | `bugfix`/`optimization`/`feature`/`documentation`/... | `+12/-3 · 2 files` | `unverified` | `—` |

Field Rules:

- **Date**: Completion date (`YYYY-MM-DD`)
- **User Request**: User's original one-sentence request or compact paraphrase; replaces task brief's requirement record
- **Result Summary**: One-line execution result summary; replaces execution result record
- **Type**: Task type enum (`enums.md` §4)
- **Net Change**: Net added/deleted lines & affected file count (calculation rules per `alds_standards.md` §16.1)
- **Status**: Micro task verification status (`enums.md` §15): `unverified` / `verified` / `failed` / `upgraded`; entry written with `unverified`, no evidence = no `verified`
- **Evidence**: Verification evidence (centralized test result path or user-returned test summary); `—` until closure
