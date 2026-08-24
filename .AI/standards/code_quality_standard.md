---
title: Code Quality Standard
doc_type: standard
status: active
scope: ALDS
version: 1.0.0
updated: 2026-06-27
related_docs:
  - .AI/standards/alds_standards.md
  - .AI/process/review/code_review_checklist.md
  - .AI/workflows/code_quality_assessment.md
  - .AI/skills/reviewer.md
  - .AI/project/guards/
---

# Code Quality Standard

This standard defines the code content governance baseline applicable to all ALDS projects: prohibits test/debug-grade code from entering production paths, prohibits non-professional code and architecture. Project-specific extensions go into `.AI/project/guards/`; this file does not carry project-specific facts.

Keyword meanings follow `.AI/standards/alds_standards.md` §4 (must / must not / should / may).

## 1. Applicability

Applies to all projects, all languages' production code (non-test modules, non-test directory source code) and architecture changes. Language-specific anti-pattern details carried by `.AI/skills/reviewer-{lang}.md`; this standard only defines cross-language general principles.

## 2. Core Principles

| Principle | Requirement |
|:---|:---|
| Production/Test Isolation | Test and debug constructs must not appear in production paths |
| Explicit Over Implicit | Architecture and control flow decisions must have basis, be traceable |
| No Bypassing Invariants | No implementation may ad-hoc bypass invariants or guards |
| No Residue | Temporary, placeholder, debug constructs must not enter declared-complete deliveries |
| Local Submits to Global | New code obeys existing style, naming, error handling conventions |

## 3. Prohibitions

### 3.1 Test/Debug-Grade Code Must Not Enter Production Paths

- Must not retain diagnostic probes, debug prints, or temporary instrumentation in production source (e.g., `eprintln!`, `dbg!`, `println!` debug output, temporary `assert`/`panic` probes, code with `[DIAG]`/`[DEBUG]` markers).
- Must not retain branches, fields, parameters, or config items solely for supporting tests or one-off debugging in production paths.
- Test doubles, stubs, mocks must be isolated in test modules or test directories, must not appear in production modules.
- Permanent diagnostics/observability must use formal channels (structured logs, metrics), must not remain as ad-hoc prints or probes.

> Verification-phase temporary probes must be cleared before verification completes and changes archived; clearance recorded in verification record.

### 3.2 Non-Professional Code Prohibited

- Must not leave `TODO`/`FIXME`/`HACK`/`XXX` markers without corresponding technical debt record; each marker must be resolved on the spot or logged as `.AI` technical debt.
- Must not deliver placeholder implementations (`todo!()`, `unimplemented!()`, stub returns, pseudo-constants) as declared-complete functionality.
- Must not introduce god objects, cross-module circular dependencies, copy-paste duplicate implementations as finalized structures.
- Must not silently expand or bypass approved work order scope (consistent with `alds_standards.md` §20).

### 3.3 Non-Professional Architecture Prohibited

- Must not ad-hoc hack bypass `.AI/project/invariants/` or `.AI/project/guards/`; if breakthrough needed must go through governance evolution flow (`alds_standards.md` §10, §28).
- Must not introduce implicit structural changes conflicting with existing architecture contracts; architecture changes must be explicitly recorded and pass architecture review (`.AI/process/review/architecture_review.md`).
- Architecture decisions must have source (requirements, specs, or invariants), must not be fabricated from nothing (consistent with contract-first principle).

### 3.4 Configuration Governance

- Must not hardcode config values in production code (ports, paths, thresholds, timeouts, format names, etc.); all config items must be injected via config files, environment variables, or runtime parameters, and declared in documentation with their sources and precedence.
- Must not introduce parameters without explicit lifecycle management: new runtime parameters, config fields, or env vars must declare their scope (process-level/session-level/request-level), default values, effective timing, and expiration conditions.
- Config item changes (add, rename, deprecate, default value change) must sync update config spec document (`.AI/project/specs/config/`) and corresponding config template.
- Must not use conditional compilation (`#[cfg(...)]`) or feature flags as hardcoding alternatives to hide configs; config decisions must be runtime-observable, auditable.

> Verification method: At work order review, check all new/modified config items have: (1) explicit source declaration; (2) reasonable defaults; (3) config doc synced update.

## 4. Language-Specific

Specific anti-patterns (e.g., Rust `unsafe` minimal principle, `unwrap`→panic, `Box::leak`, cross-thread `Rc<RefCell>`) defined by `.AI/skills/reviewer-{lang}.md`, loaded as language supplements to this standard at review time. This standard does not repeat language details.

## 5. Enforcement & Execution

This standard relies on layered mechanisms for enforcement; the contract itself does not self-execute:

| Layer | Mechanism | Coverage |
|:---|:---|:---|
| L1 Process Gates | Code review (`.AI/process/review/code_review_checklist.md`), code quality assessment (`.AI/workflows/code_quality_assessment.md`), archive audit (`.AI/process/audit/code_change_audit_protocol.md`) | All clauses, including judgment-based (e.g., architecture professionalism) |
| L2 Automated Static Checks | clippy `#![deny(...)]`, grep residue check scripts | Mechanical clauses: §3.1 debug prints/probe markers, §3.2 placeholder macros/debt-less TODOs |
| L3 Compile-time/Type System | Module visibility, `!Send`/`!Sync`, newtype | §3.3 invariant-related, typeable portions |

- Hitting any §3 prohibition, review conclusion must not be `approved`, must downgrade to `needs_update` or `rejected`.
- Mechanical clauses must sink to L2/L3, must not rely solely on L1 human memory; judgment clauses stay in L1.
- Pre-archive (`alds_standards.md` §23) must confirm no test/debug residues.

## 6. Deviation Handling

Deviations from this standard must follow `alds_standards.md` §28: explain deviation content, reason, risk, wait for user approval, record in task brief, work order, or verification result. Unrecorded/unapproved deviations treated as process violations.

## 7. Relationship with Project Guards

This standard is cross-project common baseline. Projects may append project-specific forbidden patterns in `.AI/project/guards/`, but must not relax this standard's §3 prohibitions.