---
title: Performance TypeScript
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: TypeScript-specific performance guidance for bundle optimization, lazy loading, virtual scrolling, and render optimization.
based_on: "skills/performance.md"
languages: ["typescript"]
---

# Performance - TypeScript Specialization

> This skill is a TypeScript specialization of `skills/performance.md`, for build optimization, runtime performance, and render performance analysis in Vue frontend projects.

## 1. Applicability

- Vite build optimization (code splitting, tree shaking)
- Vue component lazy loading and route lazy loading
- Virtual scrolling (vue-virtual-scroller)
- Performance pitfalls of the reactivity system
- Bundle size analysis (rollup-plugin-visualizer)

## 2. Trigger Conditions

Load in addition to `skills/performance.md` when:

- `active_language` is `typescript`
- Or the current task is explicitly bound to frontend performance optimization

## 3. Output Constraints

Inheriting `skills/performance.md`, may additionally include:

1. Performance optimization recommendations for the Vue 3 reactivity system
2. Strategy choices for code splitting
3. Tools and methods for measuring frontend performance metrics

## 4. Core Rules

1. Route lazy loading: load each page's components dynamically via `defineAsyncComponent` or `() => import()`
2. List rendering optimization: use virtual scrolling for large lists; bind unique `key` in `v-for` to avoid unnecessary re-renders
3. Analyze bundle size with `rollup-plugin-visualizer`; optimize dependencies taking > 10% first
4. Avoid expensive computation in `watch` and `computed`; consider `shallowRef`/`shallowReactive` for large data
5. If the project already has a performance baseline, reference it first

## 5. Prohibitions

- Failing to clear timers and event listeners when components unmount (use `onUnmounted`)
- Using complex expressions directly in templates (extract them into computed properties)
- Ignoring tree shaking requirements (import precisely; do not import whole libraries)
- Omitting `key` in `v-for`, or using index as the key
- Importing entire third-party UI libraries (import on demand)

## 6. Recommended Checks

- Bundle size analysis shows no large modules left to optimize
- Routes are configured with lazy loading
- Long lists use virtual scrolling
- `v-for` binds the correct `key`
- TypeScript-specific performance recommendations align with the project's Vite/Vue versions
