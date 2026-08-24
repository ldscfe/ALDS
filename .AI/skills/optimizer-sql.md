---
title: Optimizer SQL
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: SQL-specific optimisation guidance for query rewriting, index strategy, and execution plan analysis.
based_on: "skills/optimizer.md"
languages: ["sql"]
---

# Optimizer - SQL Specialization

> This skill is an SQL specialization of `skills/optimizer.md`, for database query optimization, index adjustment, and execution plan review.

## 1. Applicability

- SQL query rewriting and equivalent transformations
- Composite index design strategies (column order, covering indexes)
- In-depth execution plan analysis
- Statistics updates and vacuum strategy
- Query cache and prepared statement optimization

## 2. Trigger Conditions

Load in addition to `skills/optimizer.md` when:

- `languages` in `.AI/project/project.yaml` contains `sql`
- Or the current task is explicitly bound to SQL optimization

## 3. Output Constraints

Inheriting `skills/optimizer.md`, may additionally include:

1. Best column order for index design (equality columns first, range columns after)
2. Equivalent transformation rules for query rewriting
3. Best practices for statistics maintenance

## 4. Core Rules

1. In composite indexes, place equality-filtered columns on the left and range-filtered columns on the right, ensuring leading-column selectivity
2. Covering indexes include all columns a query needs, avoiding table lookbacks
3. Use `pg_stat_user_tables` to check whether table statistics are stale; run `ANALYZE` when necessary
4. Subquery flattening and CTE materialization behavior varies by database; verify with `EXPLAIN`
5. If the project already has database optimization conventions, follow them first

## 5. Prohibitions

- Adding indexes without assessing the write-performance impact
- Using `IN (subquery)` without checking the subquery's execution plan
- Ignoring nullability's effect on query plans (NULL columns may not use indexes)
- Creating too many indexes in OLTP systems (INSERT/UPDATE performance suffers)
- Locking a wider range with `FOR UPDATE` than the rows actually needed

## 6. Recommended Checks

- Composite index column order matches query patterns
- Queries can benefit from covering indexes
- Statistics are up to date
- No redundant or never-used indexes
- SQL-specific recommendations align with the concrete database (PostgreSQL / MySQL, etc.)
