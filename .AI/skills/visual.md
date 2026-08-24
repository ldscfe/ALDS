---
title: Visual Testing
doc_type: skill
status: active
scope: ALDS
updated: 2026-06-04
summary: Reusable guidance for visual regression testing, screen comparison testing, and UI consistency verification across Web front-end projects.
---

# Visual Testing Skill

For Web frontend visual regression test strategy, screenshot comparison process management, and UI consistency verification solution design.

## 1. Applicability

- Web Frontend UI Visual Regression Testing
- Component Library / UI Library Visual Consistency Checks
- Design System Deliverable Acceptance
- Multi-Browser/Multi-Resolution UI Consistency Verification

## 2. Trigger Conditions

Load when task contains:

- Visual Testing
- Screenshot Comparison
- UI Regression
- Pixel Difference
- Page Comparison
- Visual Consistency
- Screenshot Testing

## 3. Output Constraints

Output organized as:

1. Visual Testing Strategy
2. Test Level Definitions (Component, Page, Interaction)
3. Baseline Management Methods
4. Difference Judgment Standards
5. Uncovered Risk Explanations

## 4. Core Rules

1. Visual regression testing must decouple from functional testing — verify functional correctness first, then visual consistency
2. Baseline images must be human-confirmed — automated updates only assist, cannot replace human review
3. Set difference tolerance (Threshold) not zero-tolerance — font rendering micro-differences, OS/browser version differences should be allowed
4. Organize tests by level: Component (atomic) → Page (integration) → Interaction (state snapshots)
5. Tests must capture stable static states — snapshots after animations/transitions fully complete, never capture intermediate frames
6. Cross-browser visual differences are not defects — different browsers rendering same CSS differently is expected behavior, not regression basis

## 5. Prohibitions

- Using dynamic data (timestamps, random numbers, auto-increment IDs) for screenshots — should mock external dependencies for snapshot consistency
- Asserting pixel-perfect layout dimensions in visual tests — relative values (percentages, ratios) are key
- Using visual regression testing as sole test type — it cannot replace functional testing
- Ignoring dark mode / high contrast / theme switching accessibility theme changes
- Running visual tests in non-deterministic environments (different GPU drivers, system fonts, screen DPI)

## 6. Recommended Checks

- Visual regression test coverage boundaries established (which components/pages/states included)
- Baseline image version management standardized (naming conventions, approval process, branch isolation)
- External resource loading stability handled (third-party images, CDN fonts, icon libraries)
- Difference accept/reject workflow and notification mechanism defined
- Visual testing CI integration clear (blocking vs non-blocking, baseline auto-update strategy)