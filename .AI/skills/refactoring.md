---
title: Refactoring
doc_type: skill
status: active
scope: ALDS
updated: 2026-04-22
summary: Reusable guidance for safe refactoring with bounded scope, risk control, and verification planning.
---

# Refactoring Skill

For identifying code smells, designing safe refactoring steps, and controlling refactoring risks.

## 1. Applicability

- Code Smell Analysis
- Small-Step Refactoring Planning
- Maintainability Improvement
- Refactoring Risk Assessment

## 2. Trigger Conditions

Load when task contains:

- Refactoring
- Clean Code
- Code Quality
- Code Smell
- Maintainability Improvement
- Duplicate Code

## 3. Output Constraints

Output organized as:

1. Code Smell Identification
2. Refactoring Plan
3. Risk Assessment
4. Verification Methods

## 4. Core Rules

1. Refactoring must preserve external behavior
2. Prioritize small-step, rollbackable, verifiable refactoring paths
3. Confirm test and verification foundation first, then advance structural adjustments
4. Refactoring plan must explain gains and risks
5. If touching public contracts, should first escalate to higher-level flow for assessment

## 5. Prohibitions

- Suggesting large-scale refactoring without verification foundation
- Mixing refactoring with new feature development
- Simultaneously changing multiple unrelated areas
- Changing public APIs without explaining impact
- Creating extra complexity with complex patterns

## 6. Recommended Checks

- Primary smell types identified
- Steps ordered by minimum risk
- Rollback and verification strategies exist
- Avoided crossing into architecture evolution
- Expected gains explained