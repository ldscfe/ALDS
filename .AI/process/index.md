---
version: 1.1.0
status: active
scope: ALDS
updated: 2026-06-07
---

# Process Layer

This directory stores reusable operating procedures for review, verification, audit, and archive.

## Subdirectories

| Path | Responsibility |
| :--- | :--- |
| `.AI/process/review/` | Review checklists and review protocols |
| `.AI/process/audit/` | Audit, closure, archive rules |
| `.AI/process/validation/` | Verification matrix, coverage levels, verification gap records |
| `.AI/process/project_pulse/` | Project pulse ledger maintenance and task brief derivation rules |

## Top-Level Process Files

| Path | Responsibility |
| :--- | :--- |
| `.AI/process/error_handling.md` | Unified fallback for execution failures, verification failures, user rejection, AI stalls |

## Usage Rules

- Process documents referenced by workflows
- Process documents are not project facts
- Task briefs and work orders should reference required process documents
- Error handling is cross-cutting; any workflow encountering failure scenarios should first reference `.AI/process/error_handling.md`