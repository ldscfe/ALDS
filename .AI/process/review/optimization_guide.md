---
version: 1.0.0
status: active
scope: ALDS
updated: 2026-04-21
---

# Optimization Guide

Optimization tasks carry higher burden of proof than ordinary changes.

## Required Rules

1. Establish baseline first
2. Define target metrics clearly
3. Implementation must be atomic and bounded
4. Re-run equivalent verification after changes
5. No measurable improvement = do not claim success

## Upgrade Rules

If optimization proposal weakens invariants, safety boundaries, ordering guarantees, or review gates, must upgrade to architecture evolution.