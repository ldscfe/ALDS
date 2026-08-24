---
title: Testing SQL
doc_type: skill
status: active
scope: ALDS
updated: 2026-06-03
summary: SQL-specific testing guidance for stored procedures, schema migrations, query correctness, and database integration testing with containers.
based_on: "skills/testing.md"
languages: ["sql"]
---

# Testing - SQL Specialization

> This skill is an SQL specialization of `skills/testing.md`, for test organization, containerized integration testing, and query correctness verification in SQL projects.

## 1. Applicability

- Schema change testing and migration script verification
- Unit testing stored procedures, functions, and triggers
- Query correctness and performance regression testing
- Integration testing with Testcontainers / H2 / SQLite
- Transaction boundary and data consistency testing

## 2. Trigger Conditions

Load in addition to `skills/testing.md` when:

- `languages` in `.AI/project/project.yaml` contains `sql`
- Or the current task is explicitly bound to an SQL-related implementation

## 3. Output Constraints

Inheriting `skills/testing.md`, may additionally include:

1. Schema version management and migration script verification strategies
2. Design and management of containerized test environments
3. Query plan and performance benchmarking methods

## 4. Core Rules

1. **Schema testing**: every schema change (DDL) must pass verification scripts for `CREATE TABLE` / `ALTER TABLE` / `CREATE INDEX`, testing consistency before and after migration
2. **Integration over unit**: SQL logic depends heavily on the execution environment; prefer integration-level testing with H2, SQLite, or Testcontainers (PostgreSQL/MySQL) over pure mocks
3. **Transaction isolation**: explicitly use `BEGIN TRANSACTION; ...; ROLLBACK;` or dedicated isolated test transactions so test side effects never pollute persistent storage
4. **Query regression**: for `SELECT` and DML statements, maintain golden datasets and verify outputs against expected result sets (using `dbt-tests` or plain `UNION EXCEPT` approaches)
5. **Parameterized assertions**: use tools such as `sqlfluff` and `pgTAP` for SQL syntax checks and static analysis
6. **Never touch production tables in tests**: ensure test data runs in an isolated schema or database instance

## 5. Prohibitions

- Running any test data inserts or schema changes against the production database
- Using unqualified table names (always specify `schema.table` explicitly)
- `SELECT *` in test assertions (use explicit column names for stable assertions)
- Skipping verification of `FOREIGN KEY` and `NOT NULL` constraints
- Ending tests without cleaning up test data or rolling back transactions

## 6. Recommended Checks

- Query plans verified with `EXPLAIN (ANALYZE, BUFFERS)` to catch regressions
- Abnormal inputs covered (NULL, empty strings, special characters)
- Stored procedures and triggers have independent unit tests
- Data integrity and consistency checked after transaction rollback
- Parameterized queries tested for SQL injection (negative tests)
- SQL-specific testing recommendations align with the project's database ecosystem
