---
version: 1.0.0
status: active
scope: ALDS
updated: 2026-08-24
---

# Audit Process Index

This directory stores audit, closure, and archive rules. Audit runs after verification and before archive: it checks that required records exist and closure rules are satisfied before work may be marked complete.

## Documents

| Document | Purpose |
| :--- | :--- |
| `code_change_audit_protocol.md` | Minimum audit loop, required records, and closure rule for implementation work |

## Usage Rules

- Audit is the final gate before archive; must not mark complete before recording verification results and approval results
- Task briefs and work orders should reference this protocol when defining acceptance and closure criteria
