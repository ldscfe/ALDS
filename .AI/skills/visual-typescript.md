---
title: Visual Testing TypeScript
doc_type: skill
status: active
scope: ALDS
updated: 2026-06-04
summary: TypeScript-specific visual testing guidance for Playwright, Chromatic, Cypress, and component-level pixel-perfect verification.
based_on: "skills/visual.md"
languages: ["typescript"]
---

# Visual Testing - TypeScript Specialization

> This skill is a TypeScript specialization of `skills/visual.md`, for visual regression testing in frontend projects, covering Playwright, Chromatic, Cypress, and related toolchain practices.

## 1. Applicability

- Playwright Test's `toHaveScreenshot()` page-level visual regression testing
- Storybook + Chromatic component-level snapshot comparison and review
- Screenshot testing with Cypress plugins (such as cypress-image-diff)
- Component-level visual testing (pixel-perfect verification, scenarios JSDOM cannot cover)

## 2. Trigger Conditions

Load in addition to `skills/visual.md` when:

- `active_language` is `typescript`
- The project uses a frontend framework such as React, Vue, or Angular
- The task involves visual regression testing of frontend components or pages

## 3. Output Constraints

Inheriting `skills/visual.md`, may additionally include:

1. Visual testing toolchain selection recommendations for TypeScript projects (Playwright vs Chromatic vs Percy)
2. Component-level and page-level test code examples (with type definitions)
3. CI/CD integration configuration (GitHub Actions, GitLab CI)
4. A unified workflow for updating baseline images

## 4. Core Rules

1. Prefer Playwright's `page.screenshot()` + `toHaveScreenshot()` for page-level visual regression — cross-browser, officially maintained, supports threshold configuration
2. Prefer Chromatic / Storybook for component-level visual testing — deeply integrated with the component development workflow, provides change review UI and notifications
3. Always declare viewport size explicitly in Playwright tests — `test.use({ viewport: { width, height } })`, avoiding inconsistent results across device breakpoints
4. Mock all external data and network requests — visual tests must run on deterministic data; use Playwright `route.fulfill()` or Chromatic's built-in mocking
5. Specify diff tolerance with `maxDiffPixels` or `threshold` — avoid flaky tests from exact pixel matching; recommended: `{ maxDiffPixels: 100, threshold: 0.2 }`
6. Distinguish visual regression testing from accessibility (a11y) testing — the latter should run separately via `@axe-core/playwright` or Storybook's a11y addon
7. Leverage TypeScript type safety in component tests — custom type-driven snapshot helpers; avoid `any` types silently breaking ignore-region configuration

## 5. Prohibitions

- Asserting exact color values in visual tests — compare design system tokens (CSS variables, Tailwind classes) instead of absolute colors
- Waiting for resources with `cy.wait()` or `page.waitForTimeout()` — use the built-in waiting of `expect(page).toHaveScreenshot()` or combine `waitForSelector` + `waitForLoadState('networkidle')`
- Judging regressions by image pixel diff alone — always combine with semantic DOM assertions to prevent false positives
- Comparing raw image snapshots across different CI environments (Linux container vs macOS runner) — use a Dockerized consistent environment or a platform-agnostic diff service
- Covering every page with visual tests — cover only core pages and public components of the component library
- Treating functional bugs as visual regressions — functional errors (e.g., data not displaying) should be caught by functional tests first, not pixel comparison

## 6. Recommended Checks

- `playwright.config.ts` sets `update: process.env.CI === undefined` to prevent CI from auto-updating baselines
- Contingency for Google Fonts / CDN / icon font load failures (e.g., `dns-prefetch`, local fonts, or mocked assets)
- Docker dev containers or consistent CI images ensure all developers' screenshots match the CI environment
- Chromatic / Percy configure ignore regions for timestamps and dynamic data areas
- TypeScript types play their part in visual tests (custom Snapshot interfaces, generic constraints on helper functions)
- Snapshot files are managed in `.gitattributes` via LFS or binary rules
