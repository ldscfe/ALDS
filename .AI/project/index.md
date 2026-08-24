---
version: 1.0.0
status: active
scope: project
updated: 2026-06-01
title: Project Context
doc_type: index
project:
languages: []
summary:
---

# Project Context Layer

`.AI/project/` stores all project-level governance context (Project Governance Context).

This directory defines:

- Project facts
- System structure
- Module boundaries
- Project-specific process extensions
- Unbreakable constraints (Invariants)
- Security guards (Guards)
- Roadmap and phase scopes

All project-specific facts must remain under `.AI/project/`, must not be written to global standards directories.

---

# Project Definition Entry

The sole structured definition entry for the project is:

- `.AI/project/project.yaml`

All project variables must use this file as the formal source.

Including but not limited to:

- Project ID
- Project name
- Development languages
- Project summary
- Project goals
- Current scope
- Module definitions
- Global constraints

`index.md` serves only as:

- Directory navigation
- Project context description

Must not be used as structured fact source.

---

# Recommended Directory Structure

| Path | Purpose |
| :--- | :--- |
| `.AI/project/project.yaml` | Project structured definition |
| `.AI/project/architecture/` | System structure, module boundaries, integration relationships |
| `.AI/project/specs/` | Functional contracts, interfaces, behavioral specifications |
| `.AI/project/invariants/` | Unbreakable global guarantees |
| `.AI/project/guards/` | Security constraints, forbidden patterns, boundary guards |
| `.AI/project/process/` | Project-specific process extensions |
| `.AI/project/roadmap/` | Plans, debt, phase goals |

---

# Recommended Naming Patterns

When generating context documents during project initialization, prefer the following naming patterns; if user input or source materials have explicit filenames, use explicit input.

| Directory | Recommended Naming |
| :--- | :--- |
| `.AI/project/architecture/` | `overview.md`, `module_{module}.md`, `integration_{boundary}.md` |
| `.AI/project/specs/` | `{module}.md`, `{capability}.md`, `api_{surface}.md` |
| `.AI/project/invariants/` | `{domain}.md`, `{guarantee}.md` |
| `.AI/project/guards/` | `{risk_area}.md`, `{boundary}.md` |
| `.AI/project/process/` | `{process_name}.md` |
| `.AI/project/roadmap/` | `{phase}.md`, `initial_delivery.md` |

Naming patterns only provide consistency suggestions; must not replace source material body or explicit user requirements.

---

# Usage Rules

Before executing development tasks:

1. First check `.AI/project/project.yaml` instance fact source status; if `initialized` read project fact source
2. Then read this index
3. Only load other project documents on demand when more context needed

---

# Variable Source Rules

The following fields are considered project definition variables:

- `project.id`
- `project.name`
- `project.languages`
- `project.summary`
- `governance.governance_mode` (fixed as `full`; task weight determined by `start.md` §6.1 task path judgment)

The sole formal source for these fields is:

- `.AI/project/project.yaml`

---

# Project Initialization Rules

Project initialization phase allows extracting project facts from `.AI/source/` source materials first, then backfilling to `project.yaml`.

Initialization phase source priority:

1. Explicit project facts declared in source material body
2. Business goals, scope, non-goals, acceptance criteria in requirements
3. Architecture, modules, interfaces, interactions, technical constraints in designs
4. Explicit background, meeting conclusions, screenshots, research facts in references
5. Structured naming in source material filenames
6. Explicit supplements in user initialization instruction

If source material paths match:

- `.AI/source/design/{Project}_design_vx.x.x.md`
- `.AI/source/{kind}/{Project}_{kind}_vx.x.x.md`

Then allowed to extract from filename:

- `{Project}`

As temporary project identifier candidate.

Example:

- `rc-agent_design_v1.0.0.md`
  → `project.id = rc-agent`

When body declaration conflicts with filename inference:

- Body explicit declaration takes precedence

When both missing:

- Must not guess
- Must leave empty
- And mark as pending confirmation

---

# Project Initialization Flow

Project initialization uses separate flow document:

- `.AI/project/init.md`

This flow used for:

- Generating project governance structure from `.AI/source/` source materials
- Initializing project context layer
- Establishing project definition file
- Generating initial governance directory

This flow is not part of standard development execution flow.