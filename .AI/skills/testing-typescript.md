---
title: Testing TypeScript
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: TypeScript-specific testing guidance for Vitest, Vue Test Utils, component testing, and E2E patterns.
based_on: "skills/testing.md"
languages: ["typescript"]
---

# Testing - TypeScript Specialization

> This skill is a TypeScript specialization of `skills/testing.md`, for component testing, composable testing, and E2E test patterns in frontend projects.

## 1. Applicability

- Vitest / Jest TypeScript test configuration
- Vue component testing (Vue Test Utils / @testing-library/vue)
- Pinia store testing
- Composables testing
- Cypress / Playwright E2E testing

## 2. Trigger Conditions

Load in addition to `skills/testing.md` when:

- `active_language` is `typescript`
- Or the current task is explicitly bound to frontend TypeScript testing

## 3. Output Constraints

Inheriting `skills/testing.md`, may additionally include:

1. Vue component mounting and interaction testing patterns
2. Pinia store mocking strategies
3. Page Object patterns for E2E tests

## 4. Core Rules

1. Component tests prefer `@testing-library/vue`'s `render` and `screen` APIs; avoid testing component internals
2. Test Pinia stores with `setActivePinia(createPinia())` to create isolated store instances
3. Test composables by wrapping them in a host component or calling them directly
4. Use the Page Object pattern in E2E tests to encapsulate page selectors and actions
5. If the project already has testing conventions, follow them first

## 5. Prohibitions

- Testing Vue component internal state instead of user-visible behavior
- Mounting full Vue Router and Pinia instances in unit tests (use shallow rendering)
- Waiting for async rendering with `setTimeout` or `nextTick` (use `waitFor` or `findByText`)
- Ignoring differences in TypeScript type import paths within tests
- Hard-coded waits in E2E tests instead of `waitFor` or `intercept`

## 6. Recommended Checks

- Component tests trigger via user interaction rather than manipulating component instances
- Store tests are isolated (no dependence on other stores)
- E2E tests cover core user paths
- Test file names correspond to source files (`Component.spec.ts`)
- TypeScript-specific testing recommendations align with the project's framework ecosystem
