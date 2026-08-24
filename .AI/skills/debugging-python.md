---
title: Debugging Python
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: Python-specific debugging guidance for traceback analysis, profiling, memory leaks, and async diagnostics.
based_on: "skills/debugging.md"
languages: ["python"]
---

# Debugging - Python Specialization

> This skill is a Python specialization of `skills/debugging.md`, for exception investigation, performance profiling, and memory leak localization in Python projects.

## 1. Applicability

- Python exception stack analysis and tracing
- Async event loop diagnostics
- Memory leak localization (objgraph / tracemalloc)
- CPU profiling (cProfile / py-spy)
- Dependency conflict and environment issue troubleshooting

## 2. Trigger Conditions

Load in addition to `skills/debugging.md` when:

- `languages` in `.AI/project/project.yaml` contains `python`
- Or the current task is explicitly bound to Python debugging or troubleshooting

## 3. Output Constraints

Inheriting `skills/debugging.md`, may additionally include:

1. Layer-by-layer analysis methods for Python exception stacks
2. Tools and methods for detecting event loop blocking
3. Localization and repair steps for memory leaks

## 4. Core Rules

1. First reproduce the full exception stack and context; confirm the exception type and triggering line
2. For async problems, first check Task creation and cancellation paths; confirm there are no fire-and-forget coroutines
3. For memory leaks, use `tracemalloc` to track large object allocations; check `__del__` methods and circular references first
4. For performance problems, collect runtime data with `cProfile` or `py-spy`; optimize based on evidence, not guesses
5. If the project already has logging and monitoring conventions, follow them first

## 5. Prohibitions

- Silencing exceptions with bare `except: pass`
- Solving memory problems with `gc.collect()` alone without understanding reference chains
- Setting breakpoints in production with `pdb.set_trace()`
- Debugging on your own while ignoring known third-party library bugs
- Treating virtual environment or dependency version differences as code errors

## 6. Recommended Checks

- The correct exception type is caught (not `Exception` or bare `except`)
- Async tasks have timeout and cancellation handling
- No memory leaks from circular references
- Profiler data points to the correct hotspots
- Python-specific troubleshooting recommendations align with the project's framework ecosystem
