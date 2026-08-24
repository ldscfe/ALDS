# ALDS

AI-driven Lifecycle Delivery System.

ALDS is a document-first AI collaborative delivery governance framework. It defines how AI Agents read project context, enter controlled processes based on user requests, generate task briefs and work orders, verify results, and archive completed work.

ALDS aims to keep AI-assisted development contract-driven, process-driven, verifiable, and auditable.

## What It Is

ALDS treats software delivery as a controlled lifecycle, not an open-ended conversation.

It provides:

- **Project Context Layer**: For carrying architecture, specifications, invariants, guards, project processes, and roadmaps.
- **Entry Routing Layer**: Determines whether a user request enters project initialization or standard task flow.
- **Workflow Definitions**: Covering development, review, optimization, quality assessment, governance evolution, and default fallback.
- **Standard Templates**: For generating task briefs, work orders, and technical debt records.
- **Reports Directory**: For storing active tasks, work orders, archives, completion records, analysis artifacts, and technical debt.
- **Controlled Enumerations**: Unified management of approval conclusions, statuses, task types, priorities, and risk levels.

ALDS is not an application runtime, software package, or build tool. It is a repository structure and human-AI collaboration operations protocol.

> **Framework Positioning**: ALDS is a process governance protocol that does not make engineering judgments (data structures, algorithms, consistency models, protocol selection, etc.). These decisions must be provided by `.AI/project/` context or humans; ALDS only enforces recording, verification, and auditing of them.

## Core Model

Typical ALDS execution model:

```text
User Request
-> Project Definition
-> Workflow Routing
-> Minimum Context Load
-> Task Brief
-> User Approval
-> Work Order
-> User Approval
-> Execution or Review
-> Verification
-> User Approval
-> Archive
```

Project initialization uses a separate process for generating project governance context from raw requirements, designs, and reference materials in `.AI/source/`.

## Repository Structure

```text
.AI/
  index.md                 # AI routing entry
  start.md                 # Standard task main flow
  project/
    project.yaml           # Project structured definition
    init.md                # Project initialization flow
    architecture/          # Architecture and design context
    specs/                 # Functional and behavioral contracts
    invariants/            # Unbreakable system guarantees
    guards/                # Security constraints and forbidden patterns
    process/               # Project-specific process extensions
    roadmap/               # Milestones, phases, and follow-up work
  source/                  # Raw input materials
    requirements/          # Raw requirements, PRDs, user stories, business rules
    design/                # Raw designs, architecture, interactions, interface docs
    references/            # Supplementary materials, meeting notes, screenshots, research
  workflows/               # Executable workflow definitions
  standards/               # Global standards and controlled enumerations
  process/                 # Review, audit, verification, and adoption protocols
  skills/                  # Optional task-specialized AI skill descriptions
  templates/               # Task brief, work order, and technical debt templates

reports/
  project_pulse.md         # Project Pulse
  active/                  # Current task briefs
  task/                    # Current work orders
  archived/                # Archived work orders
  completed/               # Completed task briefs
  analysis/                # Analysis and assessment artifacts
  technical_debt/          # Technical debt records
```

## How to Use

### 1. Start from AI Entry

Every AI collaboration task should first read:

```text
.AI/project/project.yaml
.AI/index.md
```

`.AI/index.md` routes the request to one of two real flows:

- `.AI/project/init.md`: Initialize project governance context from `.AI/source/` raw materials.
- `.AI/start.md`: Handle standard tasks like development, review, optimization, testing, documentation, and governance.

#### 1.1 Initialize Project Context

When existing requirements, designs, or supplementary materials are available and you need to create or complete `.AI/project/`, use `.AI/project/init.md`.

The initialization flow produces:

- Initialization task brief.
- One or more initialization work orders.
- Project governance context under `.AI/project/`.
- `reports/project_pulse.md` project pulse.

Before creating or modifying any project context documents, AI must first provide a write plan and wait for explicit user approval.

#### 1.2 Execute Standard Tasks

Standard tasks use `.AI/start.md`.

Applicable tasks include:

- Feature development
- Bug fixes
- Refactoring
- Performance analysis
- Test failure troubleshooting
- Code review
- Review feedback adoption
- Code quality assessment
- Architecture, process, or governance evolution

Standard task execution sequence:

1. Select workflow.
2. Load minimum necessary context.
3. Generate task brief in `reports/active/`.
4. Wait for user approval.
5. Generate one or more work orders in `reports/task/`.
6. Wait for user approval.
7. Execute, review, or analyze.
8. Verify results.
9. Wait for user approval.
10. Archive and update records.

### 2. Use Controlled Status Values

ALDS document record values uniformly use English enumerations; when communicating with users, default to the language input by the user.   

Example:

| User Response | Recorded Value |
| :--- | :--- |
| Approve | `approved` |
| Reject | `rejected` |
| Needs Update | `needs_update` |

Full enumeration definitions in `.AI/standards/enums.md`.

## Key Concepts

### Project Pulse

Project-level task ledger generated by initialization flow. Records milestones, development phases, feature tasks, optimization tasks, change requests, technical debt, dependencies, estimated effort, estimated code size, and status.

### Task Brief

Task-level control document. Defines objectives, scope, non-goals, selected workflow, required reading, expected outputs, and verification plan.

### Work Order

Execution-level control document. Defines specific objectives, input documents, implementation scope, execution steps, deliverables, and verification methods.

### Minimum Context

ALDS declares required reading in each workflow; AI loads only the minimum context needed for judgment and execution.

## Approval Rules

AI must stop at controlled nodes and wait for explicit user confirmation.

## Use in Other Repositories

To use ALDS in a project:

1. Copy `.AI/`, `reports/` structure to target repository.
2. Place raw requirements in `.AI/source/requirements/`, raw design docs `{project}_design_v1.0.0.md` in `.AI/source/design/`, supplementary materials in `.AI/source/references/`.
3. Prompt: Read `.AI/index.md`, execute project initialization task (AI reads from `.AI/source/`, generates project context in `.AI/project/`, and project pulse in `reports/`).
4. Prompt: Read `.AI/index.md`, start development task.

## License

[Apache License 2.0](LICENSE)