---
title: Reviewer TypeScript
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: TypeScript-specific code review checklist for Vue components, Pinia stores, type safety, and async patterns.
based_on: "skills/reviewer.md"
languages: ["typescript"]
---

# Reviewer - TypeScript Specialization

> This skill is a TypeScript specialization of `skills/reviewer.md`, for checking common issues in Vue 3 frontend project code reviews.

## 1. Applicability

- Reviewing Vue component structure and Composition API usage
- Reviewing Pinia store design and state management
- Checking TypeScript type definitions and generic usage
- Reviewing async handling and error boundaries
- Checking frontend performance and security issues

## 2. Trigger Conditions

Load in addition to `skills/reviewer.md` when:

- `active_language` is `typescript`
- Or the current task is explicitly bound to a TypeScript frontend code review

## 3. Output Constraints

Inheriting `skills/reviewer.md`, may add these TypeScript-specific checks:

1. Vue component lifecycle and reactivity usage
2. TypeScript type safety review
3. Frontend exception handling patterns

## 4. Core Rules

1. Vue components use `<script setup lang="ts">` Composition API; never mix in Options API
2. Pinia store state uses the `() => ({})` factory function; actions access state via `this`
3. Async requests in components are wrapped in `try/catch`; error states are shown in the UI, never silently swallowed
4. TypeScript type definitions prefer `interface` (object types) and `type` (unions/utility types)
5. If the project already has coding conventions, follow them first

## 5. Prohibitions

- Mutating Pinia store state directly in components instead of through actions
- Watching primitive values without `deep: true` or `immediate` when needed
- Ignoring TypeScript strict mode checks (`strict: true` in `tsconfig.json`)
- Complex expressions in templates (extract them into computed properties)
- Failing to clear timers and event listeners in `onUnmounted`

## 6. Recommended Checks

- Components use the `<script setup>` Composition API
- Type imports are correct (`import type` vs `import`)
- Async requests handle the loading/error/data tri-state
- The store contains no computed values that could be derived directly
- TypeScript-specific review recommendations align with the project's Vue version
