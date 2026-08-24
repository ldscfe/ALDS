---
title: Testing
doc_type: skill
status: active
scope: ALDS
updated: 2026-04-22
summary: Reusable guidance for test strategy, test-case design, and verification planning across languages.
---

# Testing Skill

For test strategy design, test case planning, coverage analysis, and verification plan organization.

## 1. Applicability

- Unit Test Planning
- Integration Test Planning
- API Test Design
- Regression Test Design
- Coverage Analysis & Gap Remediation

## 2. Trigger Conditions

Load when task contains:

- Testing
- Test Cases
- Unit Testing
- Integration Testing
- Coverage
- Mock
- Stub
- TDD

## 3. Output Constraints

Output organized as:

1. Test Strategy
2. Test Scope
3. Test Scenario Design
4. Verification Methods
5. Coverage or Gap Analysis

## 4. Core Rules

1. Define test objectives and scope first, then design specific cases
2. At least cover normal, boundary, and exception scenarios
3. Tests should verify behavior, not bind implementation details
4. Mock, Stub, or double strategies should explain usage boundaries
5. Coverage targets only reference, cannot substitute behavior verification

## 5. Prohibitions

- Chasing coverage numbers ignoring key behaviors
- Mixing complex business logic into tests
- Brittle assertions binding internal implementation details
- Ignoring exception paths and boundary inputs
- Skipping verification criteria and directly declaring tests sufficient

## 6. Recommended Checks

- Main user paths covered
- Boundary and exception conditions covered
- External dependencies properly isolated
- Verification criteria explained
- Uncovered areas identified