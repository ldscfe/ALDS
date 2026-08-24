---
title: Frontend Verification Guard
doc_type: guard
status: draft
scope: project
updated: 2026-06-18
summary: Frontend page changes must pass browser automation E2E verification before marking complete.
related_docs:
  - .AI/standards/alds_standards.md
---

# Frontend Verification Guard

This guard defines project-level mandatory verification rules for frontend page changes. During project initialization, copy this file from `.AI/templates/` to `.AI/project/guards/` and adjust per actual project paths and frameworks.

## 1. Trigger Conditions

When changes meet any condition below, must execute browser automation E2E verification:

- Modified files match interaction path glob patterns (adjust per project) — triggers full E2E functional interaction verification:
  - `src/**/*.tsx`
  - `src/**/*.vue`
  - `src/**/*.svelte`
  - `src/**/*.jsx`
  - `src/**/*.html`
- Modified files match style file glob patterns — triggers visual regression testing (screenshot comparison), not mandatory full E2E:
  - `src/**/*.css`
  - `src/**/*.scss`
- New page routes added or existing route configs modified
- New or modified UI components, forms, navigation, or user interaction logic

## 2. Verification Tools

| Tool | Priority | Description |
|:---|:---|:---|
| Playwright | Preferred | Cross-browser unified, officially maintained, supports screenshots & interaction assertions |
| Cypress | Alternative | Equivalent browser automation |
| Selenium | Alternative | Multi-language bindings, equivalent automation |

## 3. Coverage Requirements

- Cover all new/modified feature core user paths
- Cover interaction types:
  - Page navigation & routing
  - Form input, validation, submission
  - Button operations & state transitions
  - Error state & boundary condition display
- Coverage Level: at least `representative`; high-risk changes should reach `full`, must not downgrade to `smoke` (frontend interaction path changes not suitable for main-path-only coverage)
- Visual regression testing (screenshot comparison) may supplement, but must not replace functional interaction verification

## 4. Verification Evidence

Verification results must include traceable evidence:

- Test script paths (e.g., `tests/e2e/feature-name.spec.ts`)
- Test execution commands & output summary
- Pass/fail statistics
- Uncovered risk explanations (if any)

## 5. Prohibitions

- Frontend page changes marked task/work order complete without browser automation verification
- Visual regression testing (screenshot comparison) replacing functional interaction verification
- "Manually clicked verified" substituting automated test evidence
- Skipping E2E test steps in CI

## 6. Verification Matrix Integration

Task brief or work order verification matrix, Entry Layer (UI) dimension should reference this guard:

| Dimension | Coverage Value | Verification Method | Coverage Level | Result | Evidence | Uncovered Risk |
|:---|:---|:---|:---|:---|:---|:---|
| Entry Layer | UI (Core User Paths) | Playwright E2E | `representative` | `pending` | `tests/e2e/*.spec.ts` execution results | |

## 7. Test Data Lifecycle

When E2E tests involve DB writes (users, roles, process instances), must manage complete data lifecycle:

- **Dedicated Test Data**: Test roles/users should be independently created test data, must not reuse `admin` or other prod accounts. Test roles must precisely match permissions required by process approval nodes.
- **Seed Scripts**: Test data must provide reproducible seed scripts (SQL or API), ensuring environment rebuildable.
- **Cleanup Scripts**: Post-test must provide independent cleanup scripts, runnable detached from execution scripts. Cleanup should support subcommands (e.g., `--sql` DB only, `--runs` output dir only).
- **Interactive Confirmation**: All cleanup ops must list pending deletions pre-execution, wait for user interactive confirmation (y/N), no silent deletion. Execute only after confirmation.
- **Closure Verification**: Cleanup scripts must output residual stats (remaining users, roles, process instances), confirm expected 0.

## 8. Test Asset Organization

Test-related docs, code, run artifacts should be separated, not mixed:

- **Reusable Regression Assets** in project docs area, as long-term maintained test suites:
  - Test plans, test cases, execution logs → `docs/demo/<feature>/`
  - Playwright specs, configs, execution scripts, cleanup scripts → same
  - SQL seed/cleanup scripts → same
- **Per-Run Artifacts** in test output area, only retain current run screenshots, traces, videos:
  - Output path → `tests/<feature>/<timestamp>/`
  - Each run auto-creates timestamp-isolated subdir (format `YYYY-MM-DD_HH-MM-SS`), avoids multi-run overwrites
- **Cleanup Script Independence**: Cleanup scripts independent of execution scripts, directly callable without preceding env vars or `run.sh` context

## 9. Deviation Handling

If browser automation verification impossible (e.g., only non-interactive copy changes, pure content changes without user interaction behavior), must record deviation reason in task brief or work order, and await user approval per `alds_standards.md` §28.