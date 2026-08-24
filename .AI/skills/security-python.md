---
title: Security Python
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: Python-specific security guidance for Flask/Django web defenses, input validation, and dependency auditing.
based_on: "skills/security.md"
languages: ["python"]
---

# Security - Python Specialization

> This skill is a Python specialization of `skills/security.md`, for web framework defenses, input validation, and dependency security auditing in Python services.

## 1. Applicability

- Security middleware configuration for Django/Flask/FastAPI
- SQL injection defense (ORM vs raw SQL)
- XSS and CSRF protection
- Dependency security auditing (pip-audit / safety)
- Python-specific pickle deserialization risks

## 2. Trigger Conditions

Load in addition to `skills/security.md` when:

- `languages` in `.AI/project/project.yaml` contains `python`
- Or the current task is explicitly bound to a Python security implementation

## 3. Output Constraints

Inheriting `skills/security.md`, may additionally include:

1. Security middleware configuration checks for Python web frameworks
2. Dependency vulnerability scanning recommendations
3. Pickle and yaml deserialization security notes

## 4. Core Rules

1. Prefer ORM parameterized queries; never concatenate user input into raw SQL
2. In Django, `@csrf_exempt` must have a documented justification and be limited to stateless APIs
3. In FastAPI, validate input automatically with Pydantic models; do not trust raw `request.json()` dictionaries
4. Scan dependencies regularly for known vulnerabilities with `pip-audit` or `safety`
5. If the project already has security framework conventions, follow them first

## 5. Prohibitions

- Using `pickle.loads()` on untrusted data
- Using `eval()`, `exec()`, or `compile()` on user input
- Disabling auto-escaping in Django templates (`|safe` / `autoescape off`) without extra validation
- Using `assert` for security checks (`python -O` removes asserts)
- Committing DEBUG=True or sensitive configuration to version control

## 6. Recommended Checks

- Security-related middleware ordering in the middleware chain is correct
- The template engine's auto-escaping is in use
- No known security vulnerabilities in dependencies
- Secret management in container environments follows best practices
- Python-specific security recommendations align with the project's framework ecosystem
