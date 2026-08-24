---
title: API Python
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: Python-specific API design guidance for FastAPI/Flask/Django REST, serialization, and middleware patterns.
based_on: "skills/api.md"
languages: ["python"]
---

# API - Python Specialization

> This skill is a Python specialization of `skills/api.md`, for web framework selection, serialization, and middleware patterns in Python projects.

## 1. Applicability

- API design for FastAPI / Flask / Django REST Framework
- Pydantic / Marshmallow serialization and validation
- Python middleware patterns (WSGI / ASGI)
- API documentation generation (OpenAPI / Swagger)

## 2. Trigger Conditions

Load in addition to `skills/api.md` when:

- `languages` in `.AI/project/project.yaml` contains `python`
- Or the current task is explicitly bound to a Python API implementation

## 3. Output Constraints

Inheriting `skills/api.md`, may additionally include:

1. Routing and dependency injection patterns for Python web frameworks
2. Validation and serialization configuration of Pydantic models
3. OpenAPI document auto-generation configuration

## 4. Core Rules

1. In FastAPI, declare request/response bodies with Pydantic models to leverage automatic type validation and OpenAPI generation
2. In Django REST Framework, use Serializers to explicitly declare fields, validation rules, and documentation comments
3. Complete request parameter validation at the entry layer; do not re-validate inside Handlers
4. Use a unified error response format `{"detail": "message"}` (FastAPI) or custom exception handlers
5. If the project already has web framework conventions, follow them first

## 5. Prohibitions

- Operating the database directly inside Handlers (go through the Service layer)
- Using `Union` response models in FastAPI path functions, which confuses OpenAPI types
- Ignoring Pydantic `Config` settings (orm_mode, schema_extra, etc.)
- Modifying the request body in middleware without considering streaming reads
- Putting framework-level security settings (CSRF, CORS) at route level instead of application level

## 6. Recommended Checks

- Request validation is handled uniformly at the entry layer
- Response models exclude sensitive fields
- OpenAPI documentation is consistent with the implementation
- Dependency injection lifecycles match actual needs
- Python-specific recommendations align with the project's framework ecosystem
