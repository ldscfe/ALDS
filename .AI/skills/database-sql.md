---
title: Database SQL
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: SQL-specific database guidance for query optimisation, indexing strategy, schema design, and transaction management.
based_on: "skills/database.md"
languages: ["sql"]
---

# Database - SQL Specialization

> This skill is an SQL specialization of `skills/database.md`, for query optimization, index design, and transaction management in relational database projects.

## 1. Applicability

- SQL query performance optimization (execution plan analysis)
- Index strategy design (B-tree / Hash / GIN / GiST)
- Table structure and constraint design
- Transaction isolation levels and locking mechanisms
- Safe data migration and DDL operations

## 2. Trigger Conditions

Load in addition to `skills/database.md` when:

- `languages` in `.AI/project/project.yaml` contains `sql`
- Or the current task is explicitly bound to an SQL database implementation

## 3. Output Constraints

Inheriting `skills/database.md`, may additionally include:

1. Execution plan analysis of SQL queries
2. Covering index and index condition pushdown strategies
3. Isolation level selection and lock conflict analysis

## 4. Core Rules

1. Understand query intent and data distribution before optimizing SQL and indexes
2. Index recommendations must be based on query patterns; verify effects with `EXPLAIN ANALYZE`
3. For DDL on large tables (ALTER TABLE, CREATE INDEX), prefer `CONCURRENTLY` options to avoid table locks
4. Keep transactions atomic; choose isolation levels sensibly (if READ COMMITTED suffices, do not use SERIALIZABLE)
5. If the project already has database conventions (naming, migration strategy), follow them first

## 5. Prohibitions

- Wrapping indexed columns in functions inside WHERE clauses (`WHERE DATE(col) = ...`)
- Ignoring the impact of JOIN order on query performance
- Using table locks or overly high isolation levels in OLTP scenarios
- Using `SELECT *` instead of explicit column lists
- Mixing DDL and DML in one transaction, causing implicit commits

## 6. Recommended Checks

- Queries use appropriate indexes (avoid full table scans)
- JOIN conditions are all backed by indexes
- Transaction boundaries are clear; no long-transaction risk
- Migration scripts are rollback-able
- SQL-specific recommendations align with the concrete database (PostgreSQL / MySQL, etc.)
