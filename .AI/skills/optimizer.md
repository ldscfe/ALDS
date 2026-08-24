---
title: Optimizer
doc_type: skill
status: active
scope: ALDS
updated: 2026-04-22
summary: Reusable guidance for query and execution-path optimization with evidence-based bottleneck analysis.
---

# Optimizer Skill

For performance bottleneck analysis, query optimization suggestions, and execution path improvement evaluation.

## 1. Applicability

- SQL Optimization
- Query Performance Analysis
- Execution Plan Review
- Index Optimization Suggestions
- Evidence-Based Performance Bottleneck Localization

## 2. Trigger Conditions

Load when task contains:

- Optimization
- Performance
- Slow Query
- Explain
- Execution Plan
- Index Optimization
- Query Slowdown

## 3. Output Constraints

Output organized as:

1. Original Object or Current State Description
2. Problem Diagnosis
3. Optimization Suggestions
4. Expected Gains
5. Risks & Boundaries

## 4. Core Rules

1. Diagnose bottleneck first, then propose optimizations
2. Index and rewrite suggestions must bind reasons and expected gains
3. If missing table structures, data volumes, or execution plans, explicitly state uncertainty
4. Must not silently change business semantics for performance
5. Optimization suggestions should distinguish low-risk and high-risk items

## 5. Prohibitions

- Asserting bottleneck location without evidence
- Just saying "add index" without explaining why
- Suggesting high-risk forceful measures without explaining costs
- Producing rewrite solutions inconsistent with original semantics
- Skipping verification and directly declaring optimization effective

## 6. Recommended Checks

- Execution plan or equivalent evidence available
- Primary bottleneck identified
- Suggestions match access patterns
- Expected gains and side effects explained
- Original business semantics preserved