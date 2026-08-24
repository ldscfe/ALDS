---
title: Error Handling Protocol
doc_type: process
status: active
scope: ALDS
updated: 2026-08-10
version: 1.0.1
summary: Unified error handling for execution failures, verification failures, user rejection, and AI stalls.
related_docs:
  - .AI/start.md
  - .AI/standards/workflow_engine.md
---

# Error Handling Protocol

This protocol defines unified fallback behaviors for **four typical failure scenarios** during ALDS task execution, ensuring any failure lands in a recoverable, traceable, continuable state—not silent retry or abandonment.

Applicability:

- Any work order (`reports/active/plan/`) failures during execution stage
- Any task brief (`reports/active/brief/`, `reports/completed/brief/`) failures during verification stage
- Any workflow user rejection or AI block during routing or decision stages
- ALDS standard package self-maintenance and standard project tasks

## 1. Work Order Execution Failure

### 1.1 Trigger Conditions

- Environment, dependencies, tools, permissions, versions, or other infrastructure issues preventing command execution
- Code conflicts with existing implementation, cannot land without breaking existing behavior
- Missing required input materials (specs, source materials, API contracts)
- AI reasoning or code generation hits unrecoverable error

### 1.2 Fallback Behavior

1. **Preserve produced intermediate artifacts** (modified files, temp scripts, diagnostic output), must not rollback
2. Write failure status as execution status `blocked` (see `.AI/standards/enums.md` §3)
3. Append `## Suspend` section to work order end, write:
   - `block_reason`: Failure cause and scene
   - `evidence`: Files read, commands run, key output fragments
   - `retry_hint`: Retry suggestions or preconditions (e.g., "need Go 1.22+ installed first")
4. In owning task brief §7 work order register, update that work order status to `blocked`
5. If failure source exceeds current work order scope (affects task brief overall), fallback per `.AI/standards/workflow_engine.md` §fallback contract to appropriate workflow
6. **Stop and wait for user adjudication**, must not silently retry

### 1.3 Prohibitions

- Must not write `blocked` as `done` (even if partially complete)
- Must not delete or rollback intermediate artifacts
- Must not infinitely retry same command
- Must not continue subsequent work orders before failure registered

## 2. Verification Failure

### 2.1 Trigger Conditions

- Verification matrix item result `failed`
- Audit, code review, spec review conclusion `rejected` or `needs_update`
- User explicitly points out item not meeting expectations

### 2.2 Fallback Behavior

1. Write that item's matrix result as `failed` (must not disguise as `passed`)
2. Choose downgrade state, decision table in `.AI/process/validation/validation_matrix.md` §downgrade rules:
   - `blocked` — environment, dependencies, permissions unavailable
   - `deferred` — verification cost or timing unsuitable for current task, **must** register to `reports/active/debt/`
   - `accepted_risk` — user or process explicitly accepts uncovered risk, **must** record accepter, time, reason
   - `not_applicable` — dimension unrelated to current task, **must** record exclusion reason
3. Fill truthfully in task brief §6 verification matrix and work order §4 verification matrix
4. Verification failure impact assessment:
   - Local failure → fix work order and retry
   - Task brief level failure → register technical debt or append work order
   - Architecture/governance level failure → fallback per `.AI/standards/workflow_engine.md` §fallback contract to `architecture_evolution.md`
5. **Stop and wait for user adjudication**

### 2.3 Prohibitions

- Must not write unexecuted verification as `passed`
- Must not substitute "self-tested" vague conclusions for matrix items
- Must not skip decision table and directly pick downgrade state

## 3. User Rejection

### 3.1 Trigger Conditions

- Task brief approval conclusion `rejected`
- Work order approval conclusion `rejected`
- Analysis report or assessment explicitly rejected

### 3.2 Fallback Behavior

Per object:

| Object | Behavior |
| :--- | :--- |
| Task Brief | Retain original, write status as `rejected` (`enums.md` §2 document status), record rejection reason and time, migrate to `reports/completed/brief/` (retain as history) |
| Work Order | Revoke and archive to `reports/completed/plan/`, filename append `_rejected`, body §2 end record "user rejected" with reason |
| Analysis Report | Identified items status as `dismissed` (see `enums.md` §8 project pulse derivation status), analysis report itself per new rules (see §4) decides if still archivable |
| Assessment Conclusion | Record rejection reason in corresponding file §Conclusion in `reports/active/analysis/`, must not delete written analysis |

### 3.3 Sync Requirements

- Project pulse ledger corresponding entry status sync to `dismissed` or `rejected`
- Must not delete task brief or work order originals due to user rejection

## 4. AI Unable to Continue - Minimum Recoverable State

### 4.1 Trigger Conditions

- AI reasoning stuck in loop or deadlock
- Context limit exceeded, cannot continue reading new files
- AI uncertain of next step and cannot converge in reasonable attempts
- Hard blocker between AI and user not clarified

### 4.2 Minimum Recoverable State Three Elements

1. **Current Step**: Which workflow step stopped at
2. **Read Files**: All document paths in READ_SET (extractable from conversation)
3. **Pending Issues**: Unclarified hard blocker list (each issue with 2-3 optional directions)

### 4.3 Fallback Behavior

1. Append `## Suspend` section to work order end, write three elements
2. In task brief §7 register work order status as `blocked`, note "AI suspended"
3. **Must not** continue execution or retry
4. **Must not** modify non-participating files
5. Output "suspend report" to user, containing:
   - Trigger scenario (one sentence)
   - Three elements
   - Suggested next step (user takeover/switch context/split task)

### 4.4 Resumption Handoff

Next session by AI or user taking over:

1. First read `reports/active/brief/` all task briefs' `## Suspend` sections
2. Prioritize resuming earliest suspended task
3. At resumption, use `retry_hint` as precondition checklist
4. If suspension exceeds 7 days, must redo minimum reading validation (`start.md` §5)

### 4.5 Approval Gate Suspension & Resumption

During approval nodes (task brief, work order, write plan, verification results, project pulse) waiting for user conclusion:

- **No Auto-Advance**: Before user gives `approved` / `rejected` / `needs_update` (or `alds_standards.md` §19.1 translated equivalents), Agent must not enter next stage, must not self-approve.
- **Suspend & Resume**: Cross-session resumption, first read `reports/active/brief/` and `reports/active/plan/` all `## Suspend` sections, prioritize earliest suspended approval object; resumption **re-state approval object and request three-state conclusion**.
- **Idempotent**: Same approval object resumed multiple times must not regenerate task brief/work order, only re-state and request conclusion.
- **Unattended Scenarios**: If project declares autonomous execution (recommend adding `autonomy` field in `.AI/project/project.yaml`, values like `human_in_loop` (default) / `autonomous`), `autonomous` mode must explicitly declare **no-response downgrade strategy** (e.g., N consecutive no-response → set task status `blocked` and suspend, never self-approve). Without `autonomy` or `human_in_loop`, human presence required by default.

## 5. Relationship with Other Specifications

- Error handling does not replace workflow `## Fallback` section; **first** handle per workflow fallback contract, **then** judge if this protocol's fallback triggers
- Error handling does not replace technical debt records; any `deferred` verification, `accepted_risk` residue must land in `reports/active/debt/`
- Error handling is unified protocol shared by ALDS standard package self-maintenance and all project tasks; project layer rewrites not allowed

## 6. Prohibitions Summary

- Silent retry of failed commands prohibited
- Disguising `failed`/`blocked` as `passed` prohibited
- Suspending without writing three elements prohibited
- Deleting originals due to user rejection prohibited
- Bypassing workflow fallback to directly enter this protocol's fallback prohibited (fallback is first choice)