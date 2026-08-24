---
title: Performance Go
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: Go-specific performance guidance for goroutine efficiency, memory allocation profiling, GC tuning, and benchmarking.
based_on: "skills/performance.md"
languages: ["go"]
---

# Performance - Go Specialization

> This skill is a Go specialization of `skills/performance.md`, for concurrency efficiency analysis, memory allocation optimization, and benchmark design in Go projects.

## 1. Applicability

- Goroutine pool and worker pattern design
- Memory allocation and escape analysis
- GC parameter tuning (GOGC, memory limit)
- Benchmark design and analysis
- Choosing concurrent data structures (sync.Map vs Mutex vs Channel)

## 2. Trigger Conditions

Load in addition to `skills/performance.md` when:

- `languages` in `.AI/project/project.yaml` contains `go`
- Or the current task is explicitly bound to Go performance work

## 3. Output Constraints

Inheriting `skills/performance.md`, may additionally include:

1. Recommendations on Go memory allocation and escape analysis
2. Criteria for choosing goroutine concurrency models
3. Directions for GC parameter optimization

## 4. Core Rules

1. Profile before optimizing: collect CPU, heap, and goroutine profiles with pprof; work from evidence, not intuition
2. Reducing allocations matters more than reducing instructions: reuse objects (sync.Pool), avoid unnecessary pointer escapes
3. Keep goroutine counts controlled: use the worker pool pattern; unbounded goroutine creation stresses the scheduler
4. Write benchmarks with `testing.B`; run `go test -bench=. -benchmem` for allocation statistics
5. If the project already has a performance baseline, reference it first

## 5. Prohibitions

- Micro-optimizing without profiling
- Replacing Mutex with channels for simple counters (introduces needless goroutine coordination overhead)
- Reaching for unsafe or cgo prematurely for performance
- Ignoring escape analysis reports (`go build -gcflags="-m"`)
- Tuning GC without considering Go version differences (GC behavior varies across versions)

## 6. Recommended Checks

- pprof points to the correct hotspot functions
- No unnecessary allocations in hot paths
- Goroutine count is controlled
- Benchmarks are statistically significant (multiple runs, averaged)
- Go-specific performance recommendations align with the project's Go version
