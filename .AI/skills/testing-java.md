---
title: Testing Java
doc_type: skill
status: active
scope: ALDS
updated: 2026-08-24
summary: Java-specific testing guidance for common JVM testing patterns, mocking, and framework-aligned verification.
based_on: "skills/testing.md"
languages: ["java"]
---

# Testing - Java Specialization

> This skill is a Java specialization of `skills/testing.md`, for test design and implementation recommendations in the JVM ecosystem.

## 1. Applicability

- Java unit testing
- Java integration testing
- JUnit-based test structure recommendations
- Mockito-based test double strategies
- Common Java test naming and assertion habits

## 2. Trigger Conditions

Load in addition to `skills/testing.md` when:

- `languages` in `.AI/project/project.yaml` contains `java`
- Or the current task is explicitly bound to a Java / JVM testing implementation

## 3. Output Constraints

Inheriting `skills/testing.md`, may additionally include:

1. Test framework selection recommendations
2. Java test class structure recommendations
3. Usage boundaries of Mockito or equivalents
4. Java naming and assertion style recommendations

## 4. Core Rules

1. Prefer a clear Arrange / Act / Assert structure
2. Use mocks only to isolate external dependencies or uncontrollable factors
3. Test names should express method, scenario, and expected result
4. Keep the boundary between integration tests and unit tests clear
5. If the project already has test framework conventions, follow them first

## 5. Prohibitions

- Pulling unnecessary framework weight into unit tests
- Over-mocking, distorting what the tests verify
- Papering over async or timing issues with sleep
- Mistaking framework defaults for project contracts
- Pushing a specific framework without project context

## 6. Recommended Checks

- Test class structure is clear
- Names express scenario and expectation
- External dependencies are reasonably isolated
- Assertions focus on behavior
- Java-specific tooling aligns with the project ecosystem
