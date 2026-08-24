---
title: Testing Completeness Review
version: 1.0.0
status: active
scope: ALDS
doc_type: process
updated: 2026-06-07
summary: Multi-dimensional testing review process for identifying coverage gaps, edge cases, and design defects in test suites.
related_docs:
  - .AI/process/validation/validation_matrix.md
---

# Testing Completeness Review

This specification defines the standard process for **multi-dimensional deep review** of test suites to identify testing blind spots, boundary coverage gaps, and design defects.

Project-specific constraints (business rules, compliance requirements) should be loaded from `.AI/project/`.

## Boundary with Verification Matrix

- **This spec (testing_completeness_review)** addresses "**implemented test suites** — are they adequate across functional/exception/security/performance/business five dimensions". It is a **review tool** for `code_review` or `feature_development` acceptance stages, outputting defect lists and supplementary test case suggestions.
- **Verification Matrix (validation_matrix)** addresses "**planning stage** — what environments/data/configs/entries to verify, to what granularity, how to annotate abandoned combinations". It is a **planning & risk explicitization tool** for task brief/work order stages.
- They differ in focus on "functional completeness & boundary values" and "exception handling & fault tolerance": this spec focuses on **test case collection depth** (are boundary values, exception paths, adversarial inputs tested), verification matrix focuses on **verification combination coverage** (which environments/data/configs/entries need testing).
- Task brief may reference both: planning stage uses verification matrix to plan matrix; execution/acceptance stage uses this spec to diff matrix against test cases.

## 1. Applicability

- Reviewing completeness and effectiveness of existing test suites
- Task brief or review instruction explicitly requires "check testing thoroughness", "supplement test cases", "assess test coverage"
- `code_review.md` flow triggers testing coverage deep-dive review
- `feature_development.md` flow accepts test deliverables

## 2. Trigger Conditions

Start this review process when any condition met:

- Code review reveals "verification adequacy" concerns
- Task type is `testing` or `feature`, test deliverables need professional assessment
- Project pulse `testing` type candidates enter analysis report stage
- User explicitly requests multi-dimensional test coverage check

## 3. Review Dimensions & Checklists

### 3.1 Functional Completeness & Boundary Value Coverage

Review whether tests cover all critical scenarios beyond normal paths.

| Check Item | Severity | Description |
|:---|:---:|:---|
| Happy Path | High | Core business flow main path must have ≥1 positive test |
| Boundary Values | High | Min, max, zero, empty, overflow, extreme lengths |
| Special Characters & Encoding | Medium | Unicode control chars, BOM, Emoji, Null byte (`\x00`) |
| Timezone & Date Boundaries | Medium | Year boundary, leap year, DST transitions, UTC+12 to UTC-12 |
| Empty & Default States | High | Empty collections, empty strings, null pointers/None/empty refs |
| Concurrency Race States | High | Multi-thread/coroutine simultaneous shared resource ops |
| Idempotent Requests | High | Duplicate submissions, retries, network timeout replays |

### 3.2 Exception Handling & Fault Tolerance (Robustness)

Review whether system behavior under non-ideal conditions is adequately verified.

| Check Item | Severity | Description |
|:---|:---:|:---|
| Dependency Service Timeouts | High | Downstream API, DB, cache timeouts & fallbacks |
| Network Exceptions | High | Connection drops, network flapping, partial failures |
| Database Exceptions | High | Connection pool exhaustion, replica lag, deadlocks, write timeouts |
| Resource Exhaustion | High | Disk full, OOM, file descriptor exhaustion |
| Input Exceptions | High | Format errors, type mismatches, oversized/undersized inputs |
| Dependency Returns Unexpected Data | Medium | Third-party API returns unexpected format, out-of-enum values |

### 3.3 Security & Adversarial Testing (Security & Red Teaming)

Review whether tests cover security-related negative scenarios.

| Check Item | Severity | Description |
|:---|:---:|:---|
| Unauthorized Access / Privilege Escalation | High | Missing auth, horizontal/vertical privilege escalation |
| Injection Attacks | High | SQL injection, command injection, LDAP injection, NoSQL injection |
| Cross-Site Scripting (XSS) | Medium | Reflected, stored, DOM-based |
| Sensitive Info Leakage | High | Passwords/keys in logs, debug info leaked to prod APIs |
| Parameter Tampering | High | URL param tampering, form field tampering, header forgery |
| Session & Token Security | Medium | CSRF, session fixation, JWT tampering/forgery |

### 3.4 Performance & Non-Functional (Non-Functional)

Review whether tests include stability guarantees under long-term/high-load operation.

| Check Item | Severity | Description |
|:---|:---:|:---|
| High-Load Stress Testing | High | Peak concurrency, throughput bottlenecks |
| Long-Running Stability / Leak Testing | High | Continuous operation memory, handle, connection leaks (7x24h+) |
| Resource Reclamation | Medium | Active/passive release cleans resources (DB connections, file handles) |
| Large Data Volume Processing | Medium | Large file transfers, bulk import/export, large list processing |
| Post-Load Recovery | Medium | System auto-recovers to healthy after stress release |

### 3.5 Business Context Specificity

Review whether tests align with specific business rules and compliance requirements.

| Check Item | Severity | Description |
|:---|:---:|:---|
| Core Business Rule Coverage | High | State machine transitions, business constraints, calculation rules |
| Business Scenario Variants | High | Branching conditions, exceptional branches in business flows |
| Data Consistency Constraints | High | Related data changes, cascading ops, eventual consistency |
| Compliance Checks | Medium | Data privacy (e.g., GDPR), audit logs, data retention policies |

---

## 4. Review Execution Modes

### 4.1 Minimum Execution

For **high-risk or core modules** test suites, mandatory execute dimensions **3.1** and **3.2** (functional completeness & exception fault tolerance).

### 4.2 Full Execution

For **complete features** or **security-sensitive modules** test reviews, execute all **5 dimensions**.

### 4.3 Tool Recommendations

- Before review, ensure `skills/testing.md` and corresponding `testing-{lang}.md` loaded
- During review, may combine `skills/security.md` or `skills/performance.md` for concurrent security/performance dimension review

---

## 5. Output Requirements

Review output must include following structured content:

### 5.1 Defect / Blind Spot List

| Seq | Missing Dimension | Severity | Specific Issue | Impact |
|:---|:---|:---:|:---|:---|
| 1 | 3.2 | High | Missing DB connection interruption fault tolerance test | May cause connection leaks |

Severity per `.AI/standards/enums.md`.

### 5.2 Supplementary Test Case Suggestions

Each high-value supplementary case in **structured format**:

```
[Case ID]: TC-XXX
[Dimension]: 3.1 Functional & Boundary
[Severity]: High
[Input Conditions]: User submits amount = 0 (boundary value)
[Expected Behavior]: System returns biz error code 400, prompts "amount must be > 0"
[Acceptance Criteria]: Error response structure conforms to ApiError contract
```

### 5.3 Uncovered Risk Declaration

- Explicitly state dimensions or combinations not covered in this review
- Explain reasons (e.g., insufficient info, scope limits, pending confirmations)
- Provide residual risk assessment

---

## 6. Prohibitions

- Prohibited from speculating test blind spots solely from interface docs when test code access unavailable
- Prohibited from using "low test coverage" as sole conclusion; must give specific missing dimensions
- Prohibited from claiming test dimension unimportant without explaining impact
- Prohibited from substituting "self-tested" or "verified" vague conclusions for structured review output