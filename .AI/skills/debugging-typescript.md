---
title: Debugging TypeScript
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: TypeScript-specific debugging guidance for browser devtools, source maps, type errors, and runtime diagnostics.
based_on: "skills/debugging.md"
languages: ["typescript"]
---

# Debugging - TypeScript Specialization

> This skill is a TypeScript specialization of `skills/debugging.md`, for browser debugging, type error investigation, and build problem localization in frontend projects.

## 1. Applicability

- Browser DevTools debugging (Console, Network, Vue DevTools)
- Interpreting TypeScript compile errors
- Source Map configuration and breakpoint mapping
- Runtime error stack trace analysis
- Build and module loading problem troubleshooting

## 2. Trigger Conditions

Load in addition to `skills/debugging.md` when:

- `active_language` is `typescript`
- Or the current task is explicitly bound to TypeScript debugging

## 3. Output Constraints

Inheriting `skills/debugging.md`, may additionally include:

1. Quick localization methods for TypeScript type errors
2. Using the Vue-specific panels of browser DevTools
3. Source Map breakpoint mapping debugging recommendations

## 4. Core Rules

1. First separate build errors from runtime errors: read `tsc` or Vite output for build errors; browser stack traces for runtime errors
2. For TypeScript type errors, first check type definition files (`.d.ts`) and generic constraints
3. Use Vue DevTools to inspect component props, events, and Pinia store state
4. For breakpoint debugging, use the `debugger` statement or VSCode's JS Debug Terminal with correct sourceMap paths
5. If the project already has debugging tool conventions, follow them first

## 5. Prohibitions

- Committing `console.log` leftovers to version control (use `debugger` or VS Code breakpoints)
- Ignoring TypeScript `strict` mode errors (bypassing with `@ts-ignore`)
- Assuming backend errors without checking the network panel
- Fixing type errors with `as any` without understanding the root cause
- Debugging production builds with sourceMap disabled

## 6. Recommended Checks

- Compile error messages are read line by line
- Vue DevTools is installed and used during debugging
- sourceMap configuration is correct (`cheap-module-source-map` in development)
- No uncaught Promise rejections
- TypeScript-specific troubleshooting recommendations align with the project toolchain
