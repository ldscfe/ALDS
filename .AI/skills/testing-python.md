---
title: Testing Python
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: Python-specific testing guidance for pytest, mocking, async test patterns, and fixture organisation.
based_on: "skills/testing.md"
languages: ["python"]
---

# Testing - Python Specialization

> This skill is a Python specialization of `skills/testing.md`, for test organization, mocking strategies, and fixture management in Python projects.

## 1. Applicability

- Organizing pytest fixtures and managing their scopes
- Using unittest.mock and pytest-mock
- Async testing (pytest-asyncio)
- Django/Flask/FastAPI test clients
- conftest.py layering

## 2. Trigger Conditions

Load in addition to `skills/testing.md` when:

- `languages` in `.AI/project/project.yaml` contains `python`
- Or the current task is explicitly bound to a Python testing implementation

## 3. Output Constraints

Inheriting `skills/testing.md`, may additionally include:

1. Fixture scope selection and reuse strategies
2. Mocking boundaries and cleanup
3. Using web framework test clients

## 4. Core Rules

1. Prefer `function` scope for fixtures; use `module` or `session` only for stateless fixtures
2. Mock only external I/O (database, network, filesystem); test internal modules with real implementations
3. Write async tests with the `pytest.mark.asyncio` decorator, run with `pytest-asyncio`
4. Organize conftest.py by directory level; fixtures in parent directories are automatically visible to children
5. If the project already has testing conventions, follow them first

## 5. Prohibitions

- Mutating global state in fixtures without cleaning up via `yield`
- Replacing standard library functions with `monkeypatch` without recording why
- Mock return values inconsistent with the real API signature
- Tests depending on specific filesystem state or environment variables without setting them explicitly
- Waiting for async operations with `time.sleep()`

## 6. Recommended Checks

- Fixture scope fits the test scenario
- Mocks cover all external boundaries
- Tests run independently (no dependence on execution order)
- Coverage reports exclude test code itself
- Python-specific testing recommendations align with the project's framework ecosystem
