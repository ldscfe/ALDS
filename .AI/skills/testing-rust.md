---
title: Testing Rust
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: Rust-specific testing guidance for cargo test, property-based testing, mocking, and async test patterns.
based_on: "skills/testing.md"
languages: ["rust"]
---

# Testing - Rust Specialization

> This skill is a Rust specialization of `skills/testing.md`, for test organization, mocking strategies, and async test patterns in Rust projects.

## 1. Applicability

- Rust unit test and integration test organization
- Modular tests with `#[cfg(test)]`
- Usage boundaries of mock libraries (mockall / mockito)
- Async test runtime selection
- Property-based testing (proptest / quickcheck)

## 2. Trigger Conditions

Load in addition to `skills/testing.md` when:

- `languages` in `.AI/project/project.yaml` contains `rust`
- Or the current task is explicitly bound to a Rust testing implementation

## 3. Output Constraints

Inheriting `skills/testing.md`, may additionally include:

1. Rust test file organization and naming conventions
2. Mock object definition and usage patterns
3. Async test runtime setup and usage

## 4. Core Rules

1. Put unit tests inside source files in `#[cfg(test)] mod tests {}`; put integration tests in the `tests/` directory
2. Use mocks only to isolate external dependencies; test internal modules with real implementations first
3. Async tests use `#[tokio::test]` or `#[actix_web::test]`, never `block_on`
4. Organize integration tests as separate files by module under `tests/`
5. If the project already has test framework conventions, follow them first

## 5. Prohibitions

- Using database or network connections in unit tests
- Skipping tests with `#[ignore]` without recording why
- Using `unwrap()` in tests instead of `?` and `assert!`
- Mock implementations inconsistent with real behavior
- Ignoring `cargo test -- --nocapture` usage in CI

## 6. Recommended Checks

- Test file organization follows Rust conventions
- Async tests use runtime macros correctly
- Which external boundaries the mocks cover
- Property tests cover boundary values
- Rust-specific testing recommendations align with the project ecosystem
