---
title: Performance Python
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: Python-specific performance guidance for async IO, GIL considerations, profiling, and service-layer bottlenecks.
based_on: "skills/performance.md"
languages: ["python"]
---

# Performance - Python Specialization

> This skill is a Python specialization of `skills/performance.md`, for async performance analysis, GIL-related optimization, and data-intensive hotspot investigation in Python projects.

## 1. Applicability

- Performance analysis of async/await paths
- Assessing GIL impact on CPU-intensive operations
- Database query batching and lazy loading optimization
- Memory usage analysis and object allocation optimization
- Locating service layer hotspot functions

## 2. Trigger Conditions

Load in addition to `skills/performance.md` when:

- `languages` in `.AI/project/project.yaml` contains `python`
- Or the current task is explicitly bound to Python performance work

## 3. Output Constraints

Inheriting `skills/performance.md`, may additionally include:

1. Detecting event loop blocking in Python async
2. GIL release strategies and multiprocessing alternatives
3. cProfile / py-spy performance analysis recommendations

## 4. Core Rules

1. First separate I/O bottlenecks from CPU bottlenecks: use asyncio for I/O-intensive work, multiprocessing for CPU-intensive work
2. Avoid mixing synchronous blocking calls (e.g., `time.sleep()`, `requests.get()`) into async code
3. Reduce query counts with select_related / prefetch_related (Django) or eager loading (SQLAlchemy)
4. Cache repeated computation in hot paths with `functools.lru_cache` or `functools.cache`
5. If the project already has a performance baseline or profiling conventions, follow them first

## 5. Prohibitions

- Mutating shared objects without locks or with wrong synchronization primitives
- Wrapping CPU-intensive computation in `async` without explicitly releasing the GIL
- Using list comprehensions instead of generator expressions in hot paths, causing memory spikes
- Ignoring ORM N+1 query problems
- Introducing C extensions or JIT compilers without assessing performance impact

## 6. Recommended Checks

- No blocking synchronous calls in async paths
- No N+1 problems in database queries
- Hotspot functions are identified by the profiler
- Caching strategy is correct (invalidation conditions, TTL, cache size)
- Python-specific recommendations align with the project's framework ecosystem
