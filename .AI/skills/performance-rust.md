---
title: Performance Rust
doc_type: skill
status: active
scope: ALDS
updated: 2026-08-24
summary: Rust-specific performance guidance for async execution, lock contention, pooling, and template or service-layer hotspots.
based_on: "skills/performance.md"
languages: ["rust"]
---

# Performance - Rust Specialization

> This skill is a Rust specialization of `skills/performance.md`, for async, concurrency, and service layer performance analysis in Rust projects.

## 1. Applicability

- Rust async performance analysis
- Lock contention and shared state optimization
- Connection pool parameter tuning
- Service and template layer hotspot investigation
- Concurrent request and batch processing assessment

## 2. Trigger Conditions

Load in addition to `skills/performance.md` when:

- `languages` in `.AI/project/project.yaml` contains `rust`
- Or the current task is explicitly bound to Rust performance work

## 3. Output Constraints

Inheriting `skills/performance.md`, may additionally include:

1. Async execution path analysis
2. Concurrency and lock usage recommendations
3. Connection pool and resource reuse recommendations
4. Rust service layer hotspot localization recommendations

## 4. Core Rules

1. First separate compute hotspots, I/O hotspots, and lock contention hotspots
2. Concurrency recommendations must assess ordering guarantees and resource pressure
3. Pooling, batching, and shared state optimizations must state their costs
4. Prefer reducing unnecessary copies, blocking waits, and serial dependencies
5. If the project already has runtime or concurrency constraints, follow the project conventions first

## 5. Prohibitions

- Parallelizing blindly without assessing side effects
- Replacing clear design with complex unsafe optimizations
- Pushing lock-free or pooled designs without evidence
- Mistaking framework features for general Rust rules
- Ignoring blocking operations at async boundaries

## 6. Recommended Checks

- Async paths have no serial bottlenecks
- No lock contention or shared state hotspots
- Connection pool configuration matches the load
- No excessive copying or unnecessary allocations
- Rust-specific recommendations align with the project ecosystem
