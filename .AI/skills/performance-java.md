---
title: Performance Java
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: Java-specific performance guidance for Spring Boot optimization, JPA tuning, JVM GC, and connection pooling.
based_on: "skills/performance.md"
languages: ["java"]
---

# Performance - Java Specialization

> This skill is a Java specialization of `skills/performance.md`, for JPA performance, JVM tuning, and connection pool configuration in Spring Boot projects.

## 1. Applicability

- Spring Boot service layer performance analysis
- JPA/Hibernate N+1 query optimization
- JVM GC log analysis and parameter tuning
- Connection pool parameter configuration
- API response time profiling

## 2. Trigger Conditions

Load in addition to `skills/performance.md` when:

- `active_language` is `java`
- Or the current task is explicitly bound to Java/Spring Boot performance optimization

## 3. Output Constraints

Inheriting `skills/performance.md`, may additionally include:

1. JPA query optimization recommendations (Fetch Join, Entity Graph, Batch Size)
2. JVM heap memory and GC strategy recommendations
3. Analysis of the performance impact of Spring Boot auto-configuration

## 4. Core Rules

1. Solve JPA N+1 with `@EntityGraph` or `JOIN FETCH`, avoiding `n+1 select` statements
2. Size the HikariCP `maximumPoolSize` from the database max_connections and application concurrency
3. For JVM GC, prefer G1GC and set a target pause time `-XX:MaxGCPauseMillis=200`
4. Collect data for API performance issues with Spring Boot Actuator Metrics and Micrometer
5. If the project already has a performance baseline, reference it first

## 5. Prohibitions

- Calling JPA Repository methods in loops (causes N+1)
- Using `SELECT *` instead of explicit column selection
- Ignoring `@Transactional` scope, holding connections for a long time
- Resizing the JVM heap in production without assessment
- Using `Thread.sleep` instead of async processing or rate limiting

## 6. Recommended Checks

- The JPA query plan shows no multiple-SQL execution
- HikariCP active connections are not close to the maximum
- Full GC frequency and duration in GC logs
- No unneeded component scanning in Spring Boot auto-configuration
- Java-specific performance recommendations align with the project's Spring Boot version
