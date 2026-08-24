---
title: API Go
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: Go-specific API design guidance for gin/echo/net-http middleware, routing, and request handling patterns.
based_on: "skills/api.md"
languages: ["go"]
---

# API - Go Specialization

> This skill is a Go specialization of `skills/api.md`, for web framework selection, middleware patterns, and request handling in Go projects.

## 1. Applicability

- Framework selection among Gin / Echo / Chi / net/http
- Go middleware patterns (HandlerFunc chains)
- Request context propagation (context.Context)
- Request binding and response serialization
- Error wrapping and unified response formats

## 2. Trigger Conditions

Load in addition to `skills/api.md` when:

- `languages` in `.AI/project/project.yaml` contains `go`
- Or the current task is explicitly bound to a Go API implementation

## 3. Output Constraints

Inheriting `skills/api.md`, may additionally include:

1. Route grouping and middleware registration for Go web frameworks
2. Request binding and validation patterns
3. Mapping of error types to HTTP status codes

## 4. Core Rules

1. Prefer framework struct binding and validation (e.g., Gin's `ShouldBindJSON` + `binding` tags); avoid manual request body parsing
2. Middleware chain order: logging → recovery → auth → rate limiting → Handler
3. Pass request-scoped data (user ID, request ID) via `context.Context`, not global variables or `sync.Map`
4. Handle errors with custom types implementing the `error` interface; the Handler layer uniformly converts them to HTTP responses
5. If the project already has web framework conventions, follow them first

## 5. Prohibitions

- Operating the database directly inside Handlers
- Repeating error-handling logic in every Handler (encapsulate it as middleware or utility functions)
- Writing HTTP responses inside goroutines (ResponseWriter is not concurrency-safe)
- Using `interface{}` instead of explicit request/response structs
- Ignoring `context` timeouts so request handling waits indefinitely

## 6. Recommended Checks

- Request binding includes validation rules (required, min, max, etc.)
- Middleware order is reasonable
- Context values are passed with custom key types rather than strings
- Error response format is unified
- Go-specific API recommendations align with the project's framework ecosystem
