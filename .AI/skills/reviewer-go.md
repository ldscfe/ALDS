---
title: Reviewer Go
doc_type: skill
status: active
scope: ALDS
updated: 2026-06-03
summary: Go-specific code review checklist for error handling, concurrency patterns, interface design, and Go idioms.
based_on: "skills/reviewer.md"
languages: ["go"]
---

# Reviewer - Go Specialization

> This skill is a Go specialization of `skills/reviewer.md`, for error handling, concurrency pattern, and interface design checks in Go project code reviews.

## 1. Applicability

- Go idioms and `gofmt` / `golint` / `staticcheck` convention review
- `error` handling patterns and multi-error merging strategies
- `context.Context` propagation chains and lifecycle management
- Goroutine / Channel / WaitGroup / sync.Mutex concurrency pattern review
- Interface design and composition (embedding) review

## 2. Trigger Conditions

Load in addition to `skills/reviewer.md` when:

- `languages` in `.AI/project/project.yaml` contains `go`
- Or the current task is explicitly bound to a Go implementation

## 3. Output Constraints

Inheriting `skills/reviewer.md`, may additionally include:

1. Coverage of `if err != nil` branches and error handling consistency
2. Completeness of the `context.Context` propagation chain
3. Goroutine leak and data race detection
4. Interface granularity and composition design

## 4. Core Rules

1. **No missed `if err != nil`**: every call that can return an `error` must have `err` checked; no hidden paths with unhandled errors
2. **Propagate or handle locally**: errors may be propagated (`return err`) or logged then returned (`log.Printf`); wrapped errors should add context (`fmt.Errorf("context: %w", err)`)
3. **Context propagation throughout**: all I/O operations and async tasks must use `context.Context` to carry timeout and cancellation; never leave a `context.Background()` created in a loop uncanceled
4. **Concurrency safety review**: check that shared data is passed over channels or protected by `sync.Mutex` / `sync.RWMutex`; never capture loop variables inside goroutines (a bug before Go 1.22)
5. **Prefer small interfaces**: Go favors small interfaces (like `io.Reader`, `io.Writer`); watch for large do-everything God Interfaces

## 5. Prohibitions

- Using `panic` for foreseeable business errors (use `return error`)
- Accessing shared mutable state in goroutines without locks or channels
- Using empty `struct{}` as channel values and then not checking payloads
- Ignoring `err`, making subsequent logic unreliable (e.g., `val, _ := someFunc()`)
- Leaving errors unhandled in `defer`

## 6. Recommended Checks

- Static analysis run with `go vet` and `staticcheck`
- `error` handling covers all branches
- Concurrent tasks managed with `sync.WaitGroup` or `errgroup`
- `context` is propagated and canceled correctly through the call chain
- Go review recommendations align with the project ecosystem
