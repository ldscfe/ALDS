---
version: 1.2.0
status: active
scope: ALDS
updated: 2026-06-01
entry: .AI/index.md
title: ALDS Router
doc_type: index
---

# ALDS Router

`.AI/index.md` is responsible only for entry judgment: first read entry rules; then judge project instance fact source status; then enter the real flow file based on user input.

> **Framework Positioning**: ALDS is a process governance protocol, not making engineering judgments (data structures, algorithms, consistency models, protocol selection, etc.). These decisions must be provided by `.AI/project/` context or humans; ALDS only enforces recording, verification, and auditing. See `.AI/standards/glossary.md`.

## 1. Fixed Startup

Every task must first read:

1. `.AI/index.md`
2. Check `.AI/project/project.yaml` instance fact source status (see §1.1)
3. If status is `initialized`, read `.AI/project/project.yaml`

After reading, do only one thing: judge whether to enter `.AI/start.md` or `.AI/project/init.md`.

### 1.1 Instance Fact Source Status

`.AI/project/project.yaml` has three states:

| Status | Definition | Judgment |
| :--- | :--- | :--- |
| `absent` | File does not exist | `.AI/project/project.yaml` does not exist in filesystem |
| `empty_or_incomplete` | File exists but is empty or clearly incomplete | File exists but content is empty, only template placeholders, or key fields (`project_name`, `languages`, `summary`) unfilled |
| `initialized` | File exists and initialization complete | Key fields filled, usable as project fact source |

### 1.2 Entry Behavior by State

| Fact Source Status | User Intent | Entry Behavior |
| :--- | :--- | :--- |
| `absent` | Initialize project governance context | Enter `.AI/project/init.md`, initialization flow creates instance fact source from `.AI/templates/project.yaml.template` |
| `absent` | Standard project tasks (dev, fix, etc.) | Prompt user missing project fact source, suggest entering `.AI/project/init.md` or wait for confirmation |
| `absent` | ALDS standard package self-maintenance (standard revision, audit, template evolution) | Enter `.AI/start.md`, record "no project instance fact source, current task is standard package self-maintenance" |
| `empty_or_incomplete` | Any | Enter `.AI/project/init.md`, initialization flow completes fact source |
| `initialized` | Initialize or complete project context | Enter `.AI/project/init.md` |
| `initialized` | Standard tasks | Enter `.AI/start.md` |
| Any | Cannot determine | Enter `.AI/start.md`, standard flow loads final decision basis then routes |

> **Task Path Independent Routing**: Entry routing behavior does not differ by task path (micro task/simple task/engineering task). All enter `.AI/start.md`; path difference manifests in `start.md` §6.1 task path judgment (see `start.md` §6.1).

## 2. Routing Rules

| User Input | Enter File | Description |
| :--- | :--- | :--- |
| Initialize `.AI/project/`, establish project governance baseline, complete project context from `.AI/source/` raw requirements, designs, or references | `.AI/project/init.md` | Project initialization flow |
| Development, fix, refactor, optimization, test, review, governance evolution, documentation changes, technical debt assessment, handle review feedback | `.AI/start.md` | Standard task flow |
| ALDS standard package self-maintenance (standard revision, process revision, template revision, audit) | `.AI/start.md` | Standard package self-maintenance, allowed to continue with missing `project.yaml` |
| User instruction is operational intent command (summary / next steps·task / conclusion) | `.AI/start.md` | Entry recognizes as operational intent, handled by `start.md` §3.0 operational intent shortcut; non-dev meta-task, definition in `.AI/skills/operational.md` |
| Cannot determine category | `.AI/start.md` | Standard flow loads final decision basis then routes |

## 3. Judgment Boundaries

- `index.md` only carries entry routing, not task briefs, work orders, execution steps, or workflow details.
- Standard task flow control in `.AI/start.md`.
- Project initialization flow control in `.AI/project/init.md`.
- Project facts per new project instance's `.AI/project/project.yaml` and `.AI/project/`; ALDS standard package only carries `.AI/templates/project.yaml.template`, not `.AI/project/project.yaml`.
- Standard package self-maintenance tasks may continue with missing `project.yaml`, but must record "no project instance fact source" in task brief.
- Nodes requiring user confirmation must be declared by target flow file and wait.

## 4. Directory Responsibilities

| Path | Responsibility |
| :--- | :--- |
| `.AI/index.md` | Entry routing |
| `.AI/start.md` | Standard task main flow |
| `.AI/project/init.md` | Project initialization flow |
| `.AI/templates/project.yaml.template` | Project structured fact source template |
| `.AI/project/project.yaml` | Project structured fact source generated after new project initialization |
| `.AI/project/` | Project architecture, specs, invariants, guards, roadmap |
| `.AI/source/` | Raw requirements, designs, supplementary references |
| `.AI/workflows/` | Specialized workflow definitions |
| `.AI/standards/` | Global standards and workflow engine |
| `.AI/process/` | Review, audit, verification, adoption protocols |
| `.AI/skills/` | Optional skills loaded on match |
| `.AI/templates/` | Task brief, work order, technical debt templates |
| `reports/active/brief/` | Current task briefs |
| `reports/active/plan/` | Current work orders |
| `reports/active/analysis/` | Active analysis reports |
| `reports/active/debt/` | Technical debt records |
| `reports/active/review/` | Review findings and conclusions |
| `reports/completed/brief/` | Completed task briefs |
| `reports/completed/plan/` | Archived work orders |
| `reports/completed/analysis/` | Archived analysis reports |
| `reports/completed/debt/` | Archived technical debt |
| `reports/completed/review/` | Archived review documents |