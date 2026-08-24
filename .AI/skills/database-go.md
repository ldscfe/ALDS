---
title: Database Go
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: Go-specific database guidance for sqlx, GORM, connection pooling, and transaction management.
based_on: "skills/database.md"
languages: ["go"]
---

# Database - Go Specialization

> This skill is a Go specialization of `skills/database.md`, for database access library selection, connection pool management, and transaction patterns in Go projects.

## 1. Applicability

- Selection and use of sqlx / GORM / ent
- `database/sql` connection pool parameter tuning
- Transaction management and nested transactions
- Batch operations and prepared statement caching
- Usage conventions for migration tools

## 2. Trigger Conditions

Load in addition to `skills/database.md` when:

- `languages` in `.AI/project/project.yaml` contains `go`
- Or the current task is explicitly bound to a Go database implementation

## 3. Output Constraints

Inheriting `skills/database.md`, may additionally include:

1. Connection pool configuration recommendations for Go databases
2. Transaction boundary patterns for `database/sql`
3. Criteria for choosing ORM vs handwritten SQL

## 4. Core Rules

1. Pool `SetMaxOpenConns` must not exceed the database max_connections; `SetMaxIdleConns` is typically set to CPU cores × 2
2. Use `sql.Tx` or GORM's `Transaction` callback; keep Commit/Rollback within the same function scope
3. For batch writes, prefer `pgx.CopyFrom` (PostgreSQL) or multi-row VALUES statements
4. When scanning query results into structs, handle NULLs: nullable database fields should use `*type` or `sql.NullType`
5. If the project already has ORM conventions, follow them first

## 5. Prohibitions

- Creating a new `sql.DB` per request (use a global connection pool)
- Executing writes outside a transaction and leaving partial success unrolled
- Leaking connections by ignoring `rows.Close()`
- Concatenating SQL with `fmt.Sprintf` instead of parameterized queries
- Using the same `sql.Tx` concurrently from multiple goroutines

## 6. Recommended Checks

- Connection pool parameters match the service load
- Transaction boundaries are clear and independent
- Queries correctly use `$1`, `$2` or `?` placeholders
- Struct scanning handles NULL fields
- Go-specific database recommendations align with the project ecosystem
