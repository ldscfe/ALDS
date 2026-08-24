---
version: 1.0.0
status: active
scope: ALDS
updated: 2026-08-24
---

# Templates Index

This directory stores standard templates: document templates and the project structured fact source template. Templates define the required structure of generated documents; concrete content comes from project context and task execution.

## Documents

| Template | Purpose |
| :--- | :--- |
| `project.yaml.template` | Project structured fact source template; initialization flow creates `.AI/project/project.yaml` from it |
| `task_brief_template.md` | Task brief for engineering tasks |
| `work_order_template.md` | Work order for execution units |
| `technical_design_template.md` | Technical design document |
| `prd_template.md` | Product requirements document |
| `project_pulse_template.md` | Project pulse ledger |
| `simple_task_list_template.md` | Simple task ledger (`reports/simple_tasks.md`) |
| `session_log_template.md` | Session log ledger (`reports/session_log.md`) |
| `technical_debt_template.md` | Technical debt record |
| `frontend_verification_guard.md` | Frontend E2E verification guard; copied to project `.AI/project/guards/` during initialization |

## Usage Rules

- Generated documents must follow template structure; fields may be added, required fields must not be removed
- Templates are not project facts; concrete content is written to generated documents under `reports/` or `.AI/project/`
