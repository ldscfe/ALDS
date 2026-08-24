---
title: Debugging Go
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: Go-specific debugging guidance for goroutine leaks, pprof profiling, delve tracing, and panic diagnostics.
based_on: "skills/debugging.md"
languages: ["go"]
---

# Debugging - Go Specialization

> This skill is a Go specialization of `skills/debugging.md`, for concurrency problem localization, performance profiling, and panic diagnostics in Go projects.

## 1. Applicability

- Goroutine leak and deadlock diagnostics
- pprof performance analysis and flame graph reading
- Delve breakpoint debugging
- Panic stack trace analysis
- Locating channel deadlocks and race conditions

## 2. Trigger Conditions

Load in addition to `skills/debugging.md` when:

- `languages` in `.AI/project/project.yaml` contains `go`
- Or the current task is explicitly bound to Go debugging or troubleshooting

## 3. Output Constraints

Inheriting `skills/debugging.md`, may additionally include:

1. Detection and repair methods for goroutine leaks
2. Analysis paths for pprof sampling data
3. Reproduction strategies for concurrency-safety issues

## 4. Core Rules

1. First separate compile errors from runtime errors; for panics, look at the stack-top trigger point, not the recovery point
2. Investigate goroutine leaks with `runtime.NumGoroutine()` and the pprof goroutine profile
3. For deadlocks, first check send/receive pairing of channels in select statements and WaitGroup Add/Done counts
4. With pprof, sample CPU and heap profiles first; compare two sampling periods to find problem functions
5. If the project already has monitoring and profiling conventions, follow them first

## 5. Prohibitions

- Using a `default` branch in select without considering the busy-wait it introduces
- Using `time.Sleep` instead of channel synchronization for goroutine coordination
- Ignoring scheduler latency information from `go tool trace`
- Locating concurrency issues by eyeballing code alone when a data race is uncertain
- Treating vet and staticcheck warnings as unimportant noise

## 6. Recommended Checks

- Any long-lived goroutines left unclosed
- Channel sends and receives are paired on concurrent paths
- Tests are compiled and run with the `-race` flag
- pprof data points to the correct hotspots
- Go-specific troubleshooting recommendations align with the project's framework ecosystem
