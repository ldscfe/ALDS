# Reports

`reports/` stores task briefs, work orders, archive records, and analysis and tracking intermediate artifacts.

| Path | Purpose |
| :--- | :--- |
| `reports/active/brief/` | Current active task briefs |
| `reports/active/plan/` | Current active work orders |
| `reports/active/analysis/` | Active analysis reports |
| `reports/active/debt/` | Technical debt records, for tracking only, not in mandatory flow |
| `reports/active/review/` | Review findings, risk conclusions, and routing recommendations |
| `reports/completed/brief/` | Completed task briefs |
| `reports/completed/plan/` | Archived work orders |
| `reports/completed/analysis/` | Archived analysis reports |
| `reports/completed/debt/` | Archived technical debt |
| `reports/completed/review/` | Archived review documents |

Flat ledger files (at `reports/` root, not flowing through active/completed):

| File | Purpose |
| :--- | :--- |
| `reports/simple_tasks.md` | Ledger of completed simple tasks (one line per task, newest on top) |
| `reports/micro_tasks.md` | Ledger of micro tasks / one-sentence requirements (one line per task, newest on top, status column carries deferred verification closure) |
| `reports/session_log.md` | Ledger of substantive user instructions + AI response summary + metadata (one line per instruction, newest on top) |

Note: This document is for guidance only; modification is prohibited.