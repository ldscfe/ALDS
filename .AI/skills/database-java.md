---
title: Database Java
doc_type: skill
status: active
scope: ALDS
updated: 2026-06-03
summary: Java-specific database guidance for JPA, Hibernate, Spring Data JPA, JDBC, and connection pooling in Java projects.
based_on: "skills/database.md"
languages: ["java"]
---

# Database - Java Specialization

> This skill is a Java specialization of `skills/database.md`, for ORM selection, connection pool management, and persistence layer design in Java projects.

## 1. Applicability

- Applying and tuning JPA / Hibernate / Spring Data JPA
- Applicable scenarios for JDBC templates and native queries
- Connection pool configuration (HikariCP / Tomcat JDBC Pool)
- Second-level cache strategies (Redis / Caffeine / EhCache)
- Distributed transactions and the Saga pattern

## 2. Trigger Conditions

Load in addition to `skills/database.md` when:

- `languages` in `.AI/project/project.yaml` contains `java`
- Or the current task is explicitly bound to a Java data layer implementation

## 3. Output Constraints

Inheriting `skills/database.md`, may additionally include:

1. Selection recommendations among JPA vs MyBatis vs JDBC
2. Repository design patterns for Spring Data JPA
3. Hibernate lazy loading strategies and N+1 handling
4. Connection pool configuration and monitoring

## 4. Core Rules

1. **Prefer Spring Data JPA + HikariCP**: HikariCP is the first-choice pool; set `maximumPoolSize`, `connectionTimeout`, `idleTimeout`, `maxLifetime`
2. **N+1 handling**: use `EntityGraph`, `JOIN FETCH`, or Spring Data `@EntityGraph` instead of default lazy loading
3. **Paginate with Pageable**: avoid loading large datasets at once; use `Page<T>` and `Pageable`, and watch `COUNT(*)` performance
4. **Transaction boundaries in the Service layer**: declare transactions with `@Transactional` on Service methods; mark read-only transactions `readOnly = true` to enable Hibernate read-only optimizations
5. **Schema changes via Flyway / Liquibase**: manage database version changes, avoid manual DDL, keep migration scripts idempotent

## 5. Prohibitions

- Mixing JPA and native Hibernate API such as calling `Session.save()` on Entities
- Swallowing exceptions caught inside a `@Transactional` method (the transaction will not roll back; rethrow or explicitly set `rollbackFor`)
- Using `SELECT *` with full JPA entity mapping (use DTO / `@SqlResultSetMapping` or `Tuple` to reduce returned data)
- Executing SQL row by row in loops, causing excessive network round trips (use `saveAll` or batching)
- Ignoring `maxLifetime`, causing reused connections to fail after the database drops them

## 6. Recommended Checks

- Generated SQL verified via `EXPLAIN` or Hibernate `show_sql`
- `Pageable` used to avoid full-table loads
- Empty results handled effectively via `Optional` instead of `NoSuchElementException`
- Connection pool parameters meet concurrency requirements
- Java database solution aligns with the project ecosystem
