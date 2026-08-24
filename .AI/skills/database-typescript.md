---
title: Database TypeScript
doc_type: skill
status: active
scope: ALDS
updated: 2026-06-03
summary: TypeScript-specific database guidance for Prisma, TypeORM, Sequelize and raw query patterns with type safety.
based_on: "skills/database.md"
languages: ["typescript"]
---

# Database - TypeScript Specialization

> This skill is a TypeScript specialization of `skills/database.md`, for ORM selection, type-safe queries, and migration management in TypeScript projects.

## 1. Applicability

- Selecting and applying Prisma / TypeORM / Sequelize / Drizzle
- Type-safe database queries (Prisma Client, Kysely)
- TypeScript migration tools (Prisma Migrate, TypeORM migrations)
- Connection pooling and concurrency control
- Serverless / Edge database access strategies

## 2. Trigger Conditions

Load in addition to `skills/database.md` when:

- `languages` in `.AI/project/project.yaml` contains `typescript`
- Or the current task is explicitly bound to a TypeScript data layer implementation

## 3. Output Constraints

Inheriting `skills/database.md`, may additionally include:

1. ORM selection comparison (Prisma vs TypeORM vs Drizzle)
2. Design patterns for type-safe queries
3. Migration and schema synchronization strategies
4. Database connection management under Serverless

## 4. Core Rules

1. **Prefer Prisma for new projects**: Prisma offers a declarative schema, type-safe queries, and automatic migrations, fitting most TypeScript backends; choose Drizzle when maximum control is needed
2. **Define schema and type models explicitly**: the Prisma `.prisma` file is the single schema source of truth; never maintain separate TypeScript interfaces in code as a substitute
3. **Version-control migration files**: Prisma Migrate generates deterministic migration files per run; they must be version controlled (Git); never `.gitignore` migration files
4. **Type-first query style**: with Prisma `$queryRaw`, use `{Prisma.raw()}` or the `Prisma.sql` template helpers so type inference stays correct and injection-safe
5. **Reuse connections in Serverless**: in Serverless environments (e.g., Vercel / Lambda), store the Prisma Client instance in a module-level variable and rely on module caching to avoid repeated instantiation

## 5. Prohibitions

- Concatenating raw SQL strings in Prisma raw queries (use `Prisma.sql` template literals)
- Bypassing Prisma type checks with `(data as any)`
- Editing schema inside migration files (schema changes should be generated via `prisma migrate dev`)
- Creating a new Prisma Client instance per request in Serverless functions
- Ignoring migration conflicts, letting dev and production schemas diverge

## 6. Recommended Checks

- Prisma schema matches the actual database structure (run `prisma validate`)
- Type-safe queries infer return types correctly
- Migration files can run from scratch on an empty database (`prisma migrate deploy`)
- Connections are reused in Serverless environments
- TypeScript database solution aligns with the project ecosystem
