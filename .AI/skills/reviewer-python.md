---
title: Reviewer Python
doc_type: skill
status: active
scope: ALDS
updated: 2026-06-03
summary: Python-specific code review checklist for PEP8 compliance, type annotations, performance, and Pythonic idioms.
based_on: "skills/reviewer.md"
languages: ["python"]
---

# Reviewer - Python Specialization

> This skill is a Python specialization of `skills/reviewer.md`, for coding convention, type annotation, and idiom checks in Python project code reviews.

## 1. Applicability

- PEP8 / PEP257 code style and docstring convention review
- Correctness and completeness of type annotations (`typing`, PEP585, PEP604)
- Pythonic idiom review (list comprehensions, generators, context managers, etc.)
- Performance anti-patterns (string concatenation, repeated list extension, global variables, etc.)
- Async code (async/await) pattern review

## 2. Trigger Conditions

Load in addition to `skills/reviewer.md` when:

- `languages` in `.AI/project/project.yaml` contains `python`
- Or the current task is explicitly bound to a Python implementation

## 3. Output Constraints

Inheriting `skills/reviewer.md`, may additionally include:

1. PEP8 / autoflake / black formatting checks
2. Type annotation coverage and correctness analysis
3. Pythonic refactoring recommendations
4. Async code pattern health

## 4. Core Rules

1. **Type annotation coverage**: check annotations with `mypy`; function signatures and public APIs must be annotated; unclear returns may be marked `-> None` or `-> Any`
2. **Prefer context managers**: resource release must use `with` statements (file I/O, database connections, locks); avoid manual `close()` and `release()`
3. **Prefer generators over list comprehensions**: for large data volumes, use generator expressions / `yield` instead of list comprehensions to avoid memory blowups
4. **Correct use of `is` vs `==`**: use `is` for identity (e.g., `None is None`), `==` for value equality (e.g., `value == 42`); never `if x is True`
5. **Catch specific exception subclasses**: prefer catching concrete exceptions (e.g., `ValueError`, `KeyError`); avoid bare `except:` or `except Exception:` swallowing everything

## 5. Prohibitions

- Mutable objects as default arguments, e.g., `def func(a=[])`, which leaves residual mutability
- Bare `except:` catching and swallowing all exceptions (specify the exception type and at least log it)
- String concatenation in loops like `s += other` (use a list and `join`)
- Type checks with `type()` instead of `isinstance()` (breaks inheritance and polymorphism)
- Frequent `list.append()` in large loops without pre-allocated capacity

## 6. Recommended Checks

- Formatting and static checks run with `ruff`, `black`, `isort`
- Type annotations cover the main public APIs
- Tests written with `pytest` with adequate coverage
- `logging` used instead of `print` for diagnostics
- Python review recommendations align with the project ecosystem
