---
version: 1.0.0
status: active
scope: ALDS
updated: 2026-05-21
---

# Project Pulse Process Index

This directory stores reusable rules for project pulse maintenance processes. Project pulse is the task ledger and high-level implementation pulse during project development, does not store specific work order contents.

## Documents

| Document | Purpose |
| :--- | :--- |
| `task_brief_derivation.md` | Deriving formal task briefs from project pulse "Other Task Ledger" candidates |

## Usage Rules

- This directory's rules apply to project pulse documents generated from `.AI/templates/project_pulse_template.md`
- This directory's rules not used for project initialization flow itself
- Specific project facts still only written to project-generated `.AI/project/` or project report directories