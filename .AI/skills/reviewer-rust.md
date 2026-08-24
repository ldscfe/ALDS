---
title: Reviewer Rust
doc_type: skill
status: active
scope: ALDS
updated: 2026-06-03
summary: Rust-specific code review checklist for ownership, lifetime, unsafe, concurrency, and idiomatic Rust patterns.
based_on: "skills/reviewer.md"
languages: ["rust"]
---

# Reviewer - Rust Specialization

> This skill is a Rust specialization of `skills/reviewer.md`, for ownership, lifetime, unsafe, and concurrency pattern checks in Rust project code reviews.

## 1. Applicability

- Checking ownership misuse and lifetime errors
- Reviewing safety and necessity of `unsafe` blocks
- Checking concurrency safety (`Send` / `Sync`) and race conditions
- Reviewing error handling patterns (`Result` / `?` / `panic!`)
- Checking idioms and performance anti-patterns

## 2. Trigger Conditions

Load in addition to `skills/reviewer.md` when:

- `languages` in `.AI/project/project.yaml` contains `rust`
- Or the current task is explicitly bound to a Rust implementation

## 3. Output Constraints

Inheriting `skills/reviewer.md`, may additionally include:

1. Correctness analysis of ownership transfers and borrows
2. Necessity of `unsafe` code and alternatives
3. Safety review of `Send`/`Sync` implementations
4. Error handling idiom checks

## 4. Core Rules

1. **Minimal `unsafe` principle**: `unsafe` code must carry a `SAFETY:` comment explaining why it is safe, and be reviewed for a pure safe API alternative
2. **Lifetime annotation check**: look for `'a` usage that is overly complex or missing; prefer owned values / `clone` to simplify lifetimes
3. **Concurrency safety review**: check that shared mutable state is properly wrapped in `Rc<RefCell<T>>` (single-threaded) or `Arc<Mutex<T>>` / `Arc<RwLock<T>>` (multi-threaded); scrutinize the justification of `unsafe impl Send/Sync`
4. **Error handling idioms**: propagate errors with `?`; manage custom errors with `thiserror` / `anyhow`; `unwrap()` only for unrecoverable errors and always with a comment explaining why
5. **Prefer owned returns over held references**: function signatures should prefer returning owned types like `Vec<T>` / `String`, avoiding lifetime complexity from references like `&'a str`

## 5. Prohibitions

- Bypassing ownership rules with `unsafe` and insufficient comments
- Using `unsafe { &mut *ptr }` without verifying pointer validity and lifetime
- Panicking directly via `Result::unwrap()` without weighing the risk
- Passing `Rc<RefCell<T>>` across threads into multi-threaded contexts
- Calling `Box::leak` without accounting for the leaked memory

## 6. Recommended Checks

- The ownership system is used effectively (no unnecessary `clone()`)
- `unsafe` boundaries are clear with documented safety guarantees
- Static checks run with `cargo clippy` and `cargo audit`
- Rust API naming conventions are followed (`snake_case` functions, `CamelCase` types)
- Rust review recommendations align with the project ecosystem
