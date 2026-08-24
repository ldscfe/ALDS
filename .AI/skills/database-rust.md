---
title: Database Rust
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: Rust-specific database guidance for SQLx, Diesel, connection pooling, and type-safe queries.
based_on: "skills/database.md"
languages: ["rust"]
---

# Database - Rust Specialization

> This skill is a Rust specialization of `skills/database.md`, for ORM/query builder selection and type-safe database access in Rust projects.

## 1. Applicability

- Using and choosing patterns for SQLx / Diesel
- Mapping between Rust's type system and database types
- Connection pool configuration and management
- Transaction management and error handling
- Usage conventions for migration tools

## 2. Trigger Conditions

Load in addition to `skills/database.md` when:

- `languages` in `.AI/project/project.yaml` contains `rust`
- Or the current task is explicitly bound to a Rust database implementation

## 3. Output Constraints

Inheriting `skills/database.md`, may additionally include:

1. Rust ORM framework selection recommendations (SQLx vs Diesel vs sea-orm)
2. Type-safety recommendations for compile-time query checking
3. Matching connection pool parameters to the concurrency model

## 4. Core Rules

1. Prefer compile-time checked queries (SQLx `query!` macros or Diesel schema mapping)
2. Match pool size to the async runtime's thread count; do not exceed 2-4× CPU cores
3. Avoid holding a database connection across `.await` inside transactions, to prevent pool exhaustion
4. Distinguish connection errors from query errors; handle recoverable and unrecoverable errors separately
5. If the project already has ORM conventions, follow them first

## 5. Prohibitions

- Using `unwrap()` on database query results
- Performing non-database async I/O inside a transaction block
- Concatenating SQL strings manually instead of parameterized queries
- Ignoring N+1 query problems
- Hardcoding connection pool configuration in code

## 6. Recommended Checks

- Queries use parameter binding rather than string concatenation
- Pool size matches the concurrency model
- Transaction boundaries are clear
- Migrations are rollback-able
- Rust-specific recommendations align with the project's ORM ecosystem
