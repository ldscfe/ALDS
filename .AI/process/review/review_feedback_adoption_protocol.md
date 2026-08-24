---
version: 1.0.0
status: active
scope: ALDS
updated: 2026-06-01
---

# Review Feedback Adoption Protocol

Review feedback is input, not automatic command.

## Required Steps

1. Categorize feedback scope: `architecture | spec | code | docs | performance | mixed`
2. Evaluate impact: `high | medium | low`
3. Choose disposition: `adopt_now`, `adopt_with_conditions`, `defer`, `reject`
4. Record reason and follow-up routing

## Upgrade Rules

If feedback touches architecture, invariants, workflow rules, or project contracts, must escalate to governance evolution flow.