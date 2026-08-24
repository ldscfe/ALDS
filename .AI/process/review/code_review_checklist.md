---
version: 1.0.0
status: active
scope: ALDS
updated: 2026-04-21
---

# Code Review Checklist

Project-specific constraints must be loaded from `.AI/project/`.

## Review Focus

1. Contract Alignment
2. Scope Control
3. Risk Control
4. Verification Adequacy
5. Maintainability

## High-Signal Findings

- Incorrect Behavior
- Contract Drift
- Regression Risk
- Missing Verification
- Unsafe Scope Expansion

---

## Work Order Code Acceptance Hard Requirements

Work order must pass ALL following checks before submission; any unsatisfied → review conclusion must not be `approved`:

| # | Check Item | Judgment Standard | Corresponding Standard |
|---|------------|-------------------|------------------------|
| C1 | No Temporary Implementation | Production code no diagnostic probes, debug prints, `[DIAG]`/`[DEBUG]` markers, temporary `assert`/`panic`, branches/fields/params added solely for one-off debugging | §3.1 |
| C2 | No Non-Production Implementation | Test doubles, stubs, mocks not in production modules; branches/fields/params/configs solely for test support not residual in production path | §3.1 |
| C3 | No Hard-coded Configuration | Production code no hardcoded ports, paths, thresholds, timeouts, format names; all config items injected via config files, env vars, or runtime params | §3.4 |
| C4 | No Incomplete Implementation | No `todo!()`, `unimplemented!()`, stub returns, pseudo-constants as declared-complete delivery; no `TODO`/`FIXME`/`HACK`/`XXX` without technical debt record | §3.2 |
| C5 | No Architecture Bypass | No ad-hoc hack bypassing `.AI/project/invariants/` or `.AI/project/guards/`; architecture changes explicitly recorded and architecture-reviewed | §3.3 |
| C6 | All Parameters Have Explicit Config Source & Lifecycle | New config items have: (1) explicit source declaration; (2) reasonable defaults; (3) scope/effective timing/expiration; (4) config doc synced update | §3.4 |

---

## Testing Coverage Deep-Dive Review

When review scope involves test code or needs to assess testing thoroughness, execute five-dimension deep review per `.AI/process/review/testing_completeness_review.md`:

1. Functional Completeness & Boundary Value Coverage
2. Exception Handling & Fault Tolerance (Robustness)
3. Security & Adversarial Testing (Security & Red Teaming)
4. Performance & Non-Functional Characteristics
5. Business Context Specificity

**Minimum Trigger**: Review scope includes test files, or review focus 4 (verification adequacy) has concerns.

**Output Requirements**: Structured defect list, supplementary test case suggestions, uncovered risk declarations.

---

## Visual Testing Review

When review scope involves frontend UI components, pages, or visual regression testing, additionally focus on:

### Visual Testing Review Checklist

| Check Item | Severity | Description |
|:---|:---:|:---|
| Component/Page Visual Regression Coverage | High | Core business pages, shared component libraries, design system components in visual tests |
| Baseline Management Standards | High | Baseline image naming, version control, human approval process |
| Diff Threshold Configuration | High | Reasonable `threshold` / `maxDiffPixels` set, avoid zero-tolerance noise |
| Cross-Browser/Environment Consistency | Medium | CI environment (Linux container) vs local (macOS/Windows) screenshot baseline consistency |
| Dynamic Content Handling | High | Timestamps, random data, third-party ads, user-generated content mocked/excluded |
| Accessibility Theme Compatibility | Medium | Dark mode, high contrast, zoom level visual stability |
| Visual vs Functional Test Decoupling | High | Visual regression must not mask functional defects; functional tests must run first |
| CI Integration Strategy | Medium | Blocking/non-blocking, baseline auto-update triggers and approval mechanism |