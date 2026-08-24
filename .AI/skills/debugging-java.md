---
title: Debugging Java
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: Java-specific debugging guidance for JVM diagnostics, thread dumps, heap analysis, and exception patterns.
based_on: "skills/debugging.md"
languages: ["java"]
---

# Debugging - Java Specialization

> This skill is a Java specialization of `skills/debugging.md`, for performance diagnostics, thread problem analysis, and OOM localization in JVM projects.

## 1. Applicability

- JVM thread dump analysis
- Heap memory leak localization (heap dump analysis)
- Hard problem troubleshooting in production (Arthas / JMC)
- ClassLoader and dependency conflict diagnostics
- Slow query and JIT hotspot localization

## 2. Trigger Conditions

Load in addition to `skills/debugging.md` when:

- `languages` in `.AI/project/project.yaml` contains `java`
- Or the current task is explicitly bound to Java debugging or troubleshooting

## 3. Output Constraints

Inheriting `skills/debugging.md`, may additionally include:

1. JVM parameter tuning recommendations
2. Diagnostic methods for thread deadlocks and races
3. Step-by-step localization of memory leaks

## 4. Core Rules

1. First separate CPU bottlenecks, memory bottlenecks, and IO bottlenecks; collect baseline data with `top -H` / `jstack` / `jstat`
2. For thread problems, first check lock contention and pool exhaustion; analyze BLOCKED states in the thread dump
3. For OOM analysis, obtain a heap dump first via `-XX:+HeapDumpOnOutOfMemoryError`
4. Locate dependency conflicts with `mvn dependency:tree` or `gradle dependencies`
5. If the project already has JVM parameter conventions, follow them first

## 5. Prohibitions

- Running `jmap -dump` directly in production without first assessing heap size and performance impact
- Ignoring business thread states contained in the thread dump
- Solving memory leaks by only increasing heap size
- Tuning GC parameters without understanding the GC algorithm
- Mistaking logging framework configuration problems for application code errors

## 6. Recommended Checks

- Enough diagnostic data collected (heap dump, thread dump, GC logs)
- Thread pool parameters match the application load
- No memory leaks caused by circular references
- Framework and library versions are not known to have memory leak defects
- Java-specific troubleshooting recommendations align with the project's JVM version and frameworks
