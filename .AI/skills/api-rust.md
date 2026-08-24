---
title: API Rust
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: Rust-specific API design guidance for Actix-web, Axum, serialization, and middleware patterns.
based_on: "skills/api.md"
languages: ["rust"]
---

# API - Rust Specialization

> This skill is a Rust specialization of `skills/api.md`, for web framework selection, routing design, and middleware patterns in Rust projects.

## 1. Applicability

- API design under Actix-web / Axum / Rocket
- Serde serialization and type mapping
- Rust middleware patterns (tower Service / middleware functions)
- Request extractors and response wrapping

## 2. Trigger Conditions

Load in addition to `skills/api.md` when:

- `languages` in `.AI/project/project.yaml` contains `rust`
- Or the current task is explicitly bound to a Rust API implementation

## 3. Output Constraints

Inheriting `skills/api.md`, may additionally include:

1. Routing and extractor patterns for Rust web frameworks
2. Serde serialization annotations and custom serializers
3. Usage of the tower Service middleware pattern

## 4. Core Rules

1. Prefer framework-provided extractors to parse request parameters; avoid manual parsing
2. Response types should implement `Serialize`; error types should implement `IntoResponse`
3. Prefer standard middleware from the framework ecosystem; implement custom middleware with the tower Service pattern
4. Derive Serde macros for request and response structs; annotate fields with `#[serde(rename_all = "camelCase")]` for naming consistency
5. If the project already has web framework conventions, follow them first

## 5. Prohibitions

- Operating the database directly inside Handlers (go through the Service layer)
- Using `unsafe` to process request data
- Exposing internal error types at the API layer (use a unified error response format)
- Ignoring error handling for extractor validation failures
- Treating framework routing macros' default behavior as a security guarantee

## 6. Recommended Checks

- Request extractors cover all validation scenarios
- Error response format is unified
- Middleware chain order is correct (logging → auth → rate limiting → Handler)
- Serde field naming is consistent with the API specification
- Rust-specific recommendations align with the project's framework ecosystem
