---
title: Testing Go
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: Go-specific testing guidance for table-driven tests, testify, test fixtures, and race detection.
based_on: "skills/testing.md"
languages: ["go"]
---

# Testing - Go Specialization

> This skill is a Go specialization of `skills/testing.md`, for test organization, mocking strategies, and concurrency safety verification in Go projects.

## 1. Applicability

- Go test file organization and naming conventions
- Table-driven tests
- Using testify / gomock / mockgen
- Concurrency testing and race detection
- Test fixtures and golden file patterns

## 2. Trigger Conditions

Load in addition to `skills/testing.md` when:

- `languages` in `.AI/project/project.yaml` contains `go`
- Or the current task is explicitly bound to a Go testing implementation

## 3. Output Constraints

Inheriting `skills/testing.md`, may additionally include:

1. Structure and naming conventions for table-driven tests
2. Usage boundaries of testify's assert/require and suite packages
3. TestMain and global fixture design for integration tests

## 4. Core Rules

1. Write unit tests in table-driven style: define an anonymous struct slice `[]struct{name, input, expected, wantErr}` and run a single loop
2. Generate mocks from interfaces with `mockgen`; place mock implementations under `internal/mocks/`
3. Put integration tests under `tests/`; initialize database connections in `TestMain` and clean up at the end
4. Run concurrency tests with `go test -race`; construct concurrent scenarios in tests to verify synchronization primitives
5. If the project already has testing conventions, follow them first

## 5. Prohibitions

- Using `log.Fatal` or `os.Exit` in tests (aborts all tests)
- Relying on test function execution order (set up state independently in each Test function)
- Waiting for goroutines with `time.Sleep` (synchronize with channels or WaitGroups)
- Ignoring cleanup functions registered with `t.Cleanup`
- Sharing mutable state in parallel tests (`t.Parallel`)

## 6. Recommended Checks

- Tests cover the happy path, boundaries, and error paths
- Table-driven test expectations cover every case
- `go test -race` passes
- Mock behavior matches the real implementation
- Go-specific testing recommendations align with the project ecosystem
