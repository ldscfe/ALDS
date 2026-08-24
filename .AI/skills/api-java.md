---
title: API Java
doc_type: skill
status: active
scope: ALDS
updated: 2026-06-03
summary: Java-specific API design guidance for Spring Boot, Jakarta EE, and RESTful services with type safety and exception handling.
based_on: "skills/api.md"
languages: ["java"]
---

# API - Java Specialization

> This skill is a Java specialization of `skills/api.md`, for API design, Spring Boot applications, exception handling, and type-safe contracts in Java projects.

## 1. Applicability

- API design for Spring Boot / Spring Web / Spring MVC / Spring WebFlux
- RESTful endpoint design and HTTP method mapping
- API development with Jakarta EE / JAX-RS
- OpenAPI / Swagger documentation generation
- Global exception handling and unified response formats

## 2. Trigger Conditions

Load in addition to `skills/api.md` when:

- `languages` in `.AI/project/project.yaml` contains `java`
- Or the current task is explicitly bound to a Java API implementation

## 3. Output Constraints

Inheriting `skills/api.md`, may additionally include:

1. Responsibility split between Spring Boot Controller / Service layers
2. Global exception handling (`@ControllerAdvice` / `RestControllerAdvice`)
3. Data validation (`javax.validation` / Spring Validation)
4. OpenAPI document auto-generation

## 4. Core Rules

1. **Clear layer responsibilities**: Controllers only receive requests and return responses (DTOs); business logic lives in the Service layer; data conversion uses Mappers (e.g., MapStruct)
2. **Unified response format**: All APIs return a unified wrapper (e.g., `ApiResponse<T>`) containing `code`, `data`, `message` fields; never throw raw exceptions to the outside
3. **Global exception handling**: Use `@RestControllerAdvice` + `@ExceptionHandler` to catch business exceptions and map them to the corresponding HTTP status codes and structured responses
4. **Validation up front**: Complete validation at the Controller layer with `@Valid` + `javax.validation` annotations; reject invalid requests with 400 before entering the Service
5. **DTO layer isolation**: Controllers receive Request DTOs and return Response DTOs; never use Entities directly as API inputs or return values

## 5. Prohibitions

- Calling Repositories or writing business logic directly in Controllers
- Returning bare strings or raw int/double values without wrapping (return standard response objects)
- Throwing `RuntimeException` directly instead of wrapping business exceptions (define a business exception base class)
- Returning HTTP 200 while the request actually failed
- Using a major version path like `/v1` without actual version control

## 6. Recommended Checks

- All exceptions handled uniformly via `@ControllerAdvice`
- DTO fields are safe (no sensitive fields such as passwords or keys leaked)
- Pagination parameters are reasonable (default page size, maximum page size limit)
- Documentation auto-generated via OpenAPI / Swagger
- Java API design aligns with the Spring ecosystem
