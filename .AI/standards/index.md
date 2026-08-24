---
version: 1.0.1
status: active
scope: ALDS
updated: 2026-08-10
---

# Standards Index

This directory stores global ALDS standards, not project-specific facts.

| File | Responsibility |
| :--- | :--- |
| `overview.md` | Standards layer summary and reading guidance, does not define new rules independently |
| `alds_standards.md` | ALDS general spec, methodology, layered model, artifact lifecycle, and red lines |
| `workflow_engine.md` | Workflow routing, variable binding, minimum reading, and fallback rules |
| `enums.md` | Controlled enumerations for approvals, statuses, task types, priorities, risk levels, risk quantification criteria, state machine diagrams |
| `task_profile.md` | Task profile five-field determination steps, profile→minimum read set lookup, negative list, skills hit count constraints |
| `glossary.md` | ALDS-wide key term unified definitions (authoritative definitions still per respective standard documents) |
| `code_quality_standard.md` | Cross-project code quality baseline: prohibits test/debug code in production path, prohibits non-professional code and architecture |

## Boundaries

- Project facts in `.AI/project/`
- Process constraints in `.AI/process/`
- Executable task flows in `.AI/workflows/`
- Routing entry is `.AI/index.md`, standard task flow is `.AI/start.md`