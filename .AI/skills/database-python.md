---
title: Database Python
doc_type: skill
status: active
scope: ALDS
updated: 2026-06-03
summary: Python-specific database guidance for SQLAlchemy, peewee, asyncpg and other ORM/ODM patterns in Python projects.
based_on: "skills/database.md"
languages: ["python"]
---

# Database - Python Specialization

> This skill is a Python specialization of `skills/database.md`, for ORM selection, connection pool management, and async query patterns in Python projects.

## 1. Applicability

- Selecting and applying SQLAlchemy / peewee / Django ORM
- Async database drivers such as asyncpg / aiomysql / databases
- Connection pool configuration (pool_size, max_overflow, pool_recycle)
- Version management with Alembic / Django migrations
- Multi-database routing and read/write splitting

## 2. Trigger Conditions

Load in addition to `skills/database.md` when:

- `languages` in `.AI/project/project.yaml` contains `python`
- Or the current task is explicitly bound to a Python data layer implementation

## 3. Output Constraints

Inheriting `skills/database.md`, may additionally include:

1. Comparison of recommended ORM solutions (SQLAlchemy vs peewee vs Django ORM)
2. Selection recommendations for async drivers
3. Usage conventions for migration tools (Alembic / Django Migrations)

## 4. Core Rules

1. **Prefer SQLAlchemy 2.0+ for ORM selection**: new projects should use the declarative ORM and the new query API; avoid the legacy Query interface
2. **Configure connection pools explicitly**: `create_engine` must explicitly set `pool_size`, `max_overflow`, `pool_recycle` to avoid connection leaks
3. **Separate async from sync**: use async drivers for I/O-intensive operations (asyncpg + async SQLAlchemy); compute-heavy operations may stay synchronous; never call the sync ORM inside an async context
4. **Alembic migration file naming**: name as `revision_id_short_description`; each migration makes exactly one schema change; split large changes into multiple migrations
5. **Use bulk interfaces for batch operations**: replace row-by-row inserts with `session.bulk_insert_mappings()` or `bulk_update_mappings()` to reduce round trips

## 5. Prohibitions

- Concatenating raw SQL strings inside `session.query()` (use SQLAlchemy `text()` or the ORM abstraction layer)
- Calling sync database drivers in async context, blocking the event loop
- Triggering N+1 queries via SQLAlchemy lazy loading (use `selectinload`, `joinedload` explicitly)
- Ignoring `pool_recycle`, causing long-lived connections to fail retrying after the database drops them
- Calling business logic code directly inside migration files

## 6. Recommended Checks

- ORM queries generate efficient SQL (check with SQLAlchemy echo or SQL logging)
- Async session lifecycle is correct (`begin/commit/rollback` on `AsyncSession`)
- Alembic migrations are backward compatible and support rollback
- Connection pool configuration meets concurrency requirements
- Python database solution aligns with the project ecosystem
