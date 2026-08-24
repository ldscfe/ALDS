---
title: Database
doc_type: skill
status: active
scope: ALDS
updated: 2026-04-22
summary: Reusable guidance for database modeling, schema design, indexing, and storage-level tradeoff analysis.
---

# Database Skill

For data modeling, table structure design, index planning, and data layer solution evaluation.

## 1. Applicability

- Database Design
- Table Structure & Field Modeling
- Index Strategy Design
- Sharding & Partitioning Solution Evaluation
- Data Migration & Constraint Design

## 2. Trigger Conditions

Load when task contains:

- Database Design
- Table Structure
- Index Design
- Data Modeling
- ER Diagram
- Sharding
- Data Migration

## 3. Output Constraints

Output organized as:

1. Requirements Analysis
2. Data Model Design
3. Index Strategy
4. Optimization Recommendations
5. Risks & Notes

## 4. Core Rules

1. Identify entities, relationships, data volumes first, then design structure
2. Field types, primary keys, unique constraints, FK strategies explicit
3. Index recommendations must give reasons, not just conclusions
4. If sharding involved, must explain triggers and costs
5. Must not fabricate data relationships without business constraints

## 5. Prohibitions

- Blindly adding indexes without knowing access patterns
- Assuming data volumes and hotspot distributions without basis
- Overly complex structures replacing clear modeling
- Ignoring migration and rollback costs
- Mistaking DB implementation details for business contracts

## 6. Recommended Checks

- Entity relationships complete
- Field types and constraints reasonable
- Indexes support core queries
- Scaling strategy matches data scale
- Migration risks controllable