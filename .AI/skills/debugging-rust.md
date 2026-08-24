---
title: Debugging Rust
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: Rust-specific debugging guidance for borrow checker, unsafe code, async runtime, and panic diagnostics.
based_on: "skills/debugging.md"
languages: ["rust"]
---

# Debugging - Rust Specialization

> This skill is a Rust specialization of `skills/debugging.md`, for compile errors, borrow checker conflicts, unsafe code review, and async runtime problem localization in Rust projects.

## 1. Applicability

- Borrow checker related compile error analysis
- Correctness review of unsafe code blocks
- Async runtime deadlock and task cancellation diagnostics
- Panic backtraces and unwind analysis
- Locating deadlocks and race conditions

## 2. Trigger Conditions

Load in addition to `skills/debugging.md` when:

- `languages` in `.AI/project/project.yaml` contains `rust`
- Or the current task is explicitly bound to Rust debugging or troubleshooting

## 3. Output Constraints

Inheriting `skills/debugging.md`, may additionally include:

1. Repair recommendations for borrow checker errors
2. Safety boundary analysis for unsafe code blocks
3. Async runtime task stack tracing
4. Reproduction paths for concurrency problems

## 4. Core Rules

1. First separate compile-time errors from runtime errors; use different localization strategies for each
2. For borrow checker errors, first check lifetime annotations and ownership transfer paths
3. Review unsafe blocks line by line; every unsafe occurrence must have a safety comment stating its preconditions
4. For async deadlocks, first check whether the `.await` call chain contains blocking synchronous operations
5. If the project already has panic hook or error handling conventions, follow them first

## 5. Prohibitions

- Bypassing borrow checker errors directly with `unsafe`
- Sharing data across threads without confirming Send/Sync safety
- Ignoring the panic risk of `unwrap()` and `expect()`
- Using `std::mem::transmute` without verifying type layout compatibility
- Treating compiler lint warnings as unimportant noise

## 6. Recommended Checks

- Compile errors, logic errors, and concurrency errors are distinguished
- unsafe blocks are minimal and have safety comments
- No blocking operations in async paths
- Panic boundaries are properly handled at public APIs
- Rust-specific tools (`cargo check`, `clippy`, `miri`, `cargo-audit`) have been used
