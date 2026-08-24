---
title: Security Java
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: Java-specific security guidance for Spring Security, servlet filters, authentication flows, and JVM web defenses.
based_on: "skills/security.md"
languages: ["java"]
---

# Security - Java Specialization

> This skill is a Java specialization of `skills/security.md`, for authentication and authorization, filter chains, and common web defense checks in Java/Spring services.

## 1. Applicability

- Spring Security configuration review
- Servlet filter and interceptor chain security analysis
- JWT authentication in Spring Boot
- Checking method-level security annotations
- Input validation and XSS/CSRF protection

## 2. Trigger Conditions

Load in addition to `skills/security.md` when:

- `languages` in `.AI/project/project.yaml` contains `java`
- Or the current task is explicitly bound to a Java security implementation

## 3. Output Constraints

Inheriting `skills/security.md`, may additionally include:

1. Spring Security filter chain configuration checks
2. Java-specific serialization security notes
3. JVM security policy and third-party dependency audit recommendations

## 4. Core Rules

1. In Spring Security, prefer the lambda DSL configuration; avoid the deprecated `HttpSecurity` chained style
2. Validate Controller inputs with `@Validated` + `jakarta.validation` annotations; do not validate manually in Handlers
3. Never write sensitive information (passwords, tokens) to logs via `toString()`
4. CORS configuration must restrict origins precisely; never use `allowedOrigins("*")`
5. If the project already has security framework conventions, follow them first

## 5. Prohibitions

- Returning exception stack traces to clients from Controllers
- Using `@CrossOrigin(origins = "*")` or equivalent fully-open CORS policies
- Exposing framework version numbers in HTTP response headers
- Insecure deserialization (e.g., `ObjectInputStream` on untrusted data)
- Leaving Spring Boot Actuator endpoints unprotected

## 6. Recommended Checks

- The order of authentication and authorization filters in the chain is correct
- CSRF protection is configured (non-STATELESS sessions)
- No sensitive endpoints accidentally exposed by `permitAll()`
- No known vulnerabilities in dependencies (OWASP Dependency Check)
- Java-specific security recommendations align with the project's framework ecosystem
