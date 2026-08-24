---
title: Reviewer Java
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: Java-specific code review checklist for Spring Boot patterns, exception handling, annotation usage, and dependency injection.
based_on: "skills/reviewer.md"
languages: ["java"]
---

# Reviewer - Java Specialization

> This skill is a Java specialization of `skills/reviewer.md`, for checking common issues in Spring Boot project code reviews.

## 1. Applicability

- Reviewing Spring Boot Controller/Service/Repository layer code
- Checking Java exception handling patterns
- Reviewing dependency injection and Bean lifecycle
- Checking annotation usage conventions
- Reviewing Stream API and Optional usage

## 2. Trigger Conditions

Load in addition to `skills/reviewer.md` when:

- `languages` in `.AI/project/project.yaml` contains `java`
- Or the current task is explicitly bound to a Java code review

## 3. Output Constraints

Inheriting `skills/reviewer.md`, may add these Java-specific checks:

1. Input validation and error handling at the Controller layer
2. Transaction boundaries and exception conversion at the Service layer
3. Query performance and safety at the Repository layer

## 4. Core Rules

1. Controller parameters use `@Valid` + `jakarta.validation` annotations; Handlers process exceptions uniformly with `@ExceptionHandler`
2. The Service layer throws business exceptions (custom RuntimeExceptions); ControllerAdvice converts them uniformly to HTTP status codes
3. `@Transactional` is placed on Service methods, not Controller methods
4. Dependency injection uses constructor injection, not `@Autowired` field injection (final fields + Lombok)
5. If the project already has coding conventions, follow them first

## 5. Prohibitions

- Catching exceptions in Controllers and returning `ResponseEntity` instead of using unified exception handling
- Catching and swallowing exceptions inside `@Transactional` methods (the transaction will not roll back)
- Running initialization logic in `@PostConstruct` without considering dependency order
- Modifying external variables inside Stream API forEach
- Calling `Optional.get()` without checking `isPresent()`

## 6. Recommended Checks

- Controller parameter validation annotations are complete
- Service layer transaction boundaries are clear
- Bean dependency injection style is consistent
- Exceptions are converted at the appropriate layer
- Java-specific review recommendations align with the project's framework versions
