---
title: Performance SQL
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: SQL-specific performance guidance for query tuning, execution plan analysis, and database-level optimisation.
based_on: "skills/performance.md"
languages: ["sql"]
---

# Performance - SQL Specialization

> This skill is an SQL specialization of `skills/performance.md`, for query performance analysis and optimization at the database level.

## 1. Applicability

- SQL query execution plan interpretation
- Query rewriting and equivalent transformations
- Materialized view and query cache strategies
- Evaluating partitioned tables and sharding schemes
- Batch operation optimization

## 2. Trigger Conditions

Load in addition to `skills/performance.md` when:

- `languages` in `.AI/project/project.yaml` contains `sql`
- Or the current task is explicitly bound to SQL database performance optimization

## 3. Output Constraints

Inheriting `skills/performance.md`, may additionally include:

1. Interpretation of key execution plan metrics (seq scan vs index scan, rows vs actual rows)
2. Query rewriting recommendations (subquery flattening, JOIN order adjustment)
3. Best practices for batch operations

## 4. Core Rules

1. Use `EXPLAIN (ANALYZE, BUFFERS)` for actual execution plans; compare estimated rows vs actual rows deviation
2. Slow query optimization priority path: large table scan → missing index → wrong JOIN order → skewed data distribution
3. For batch INSERT, use `COPY` or multi-row VALUES; keep row counts per statement reasonable
4. Design partitioned tables around query filter conditions so partition pruning takes effect
5. If the project already has a database performance baseline, reference it first

## 5. Prohibitions

- Making optimization decisions from estimated rows without looking at actual rows
- Enabling `pg_stat_statements` in production without assessing performance impact
- Using `NOT IN` instead of `NOT EXISTS` (`NOT IN` handles NULL differently)
- Using complex window functions or recursive CTEs on hot query paths without assessing cost
- Focusing only on single-query latency while ignoring connection pool wait statistics

## 6. Recommended Checks

- No Sequential Scan on large tables in execution plans
- No order-of-magnitude deviation between actual rows and estimated rows
- Query time is not dominated by I/O waits
- A suitable index exists to eliminate sort operations
- SQL-specific recommendations align with the concrete database engine
