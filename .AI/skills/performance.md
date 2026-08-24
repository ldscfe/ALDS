---
title: Performance
doc_type: skill
status: active
scope: ALDS
updated: 2026-04-22
summary: Reusable guidance for evidence-based performance analysis, bottleneck isolation, and optimization planning.
---

# Performance Skill

For performance analysis, bottleneck localization, optimization direction evaluation, and verification planning.

## 1. Applicability

- Slow Response Analysis
- Throughput & Latency Problem Localization
- Resource Contention Analysis
- Optimization Direction Evaluation
- Performance Verification Plan Design

## 2. Trigger Conditions

Load when task contains:

- Performance
- Bottleneck
- Speedup
- Latency
- Throughput
- Optimization
- Slow Response

## 3. Output Constraints

Output organized as:

1. Performance Current State
2. Bottleneck Diagnosis
3. Optimization Suggestions
4. Verification Plan
5. Risks & Trade-offs

## 4. Core Rules

1. Localize bottleneck first, then propose optimizations
2. Optimization suggestions should bind specific evidence or reasonable assumption boundaries
3. Must explain expected gains and potential side effects
4. Prioritize low-risk, high-gain optimization items
5. If missing key metrics, first supplement observability, not jump to conclusions

## 5. Prohibitions

- Asserting bottlenecks without observability data
- Trading correctness for performance
- Disguising structural refactoring as performance optimization
- Ignoring verification and regression risks
- Passing framework-specific tricks as general conclusions

## 6. Recommended Checks

- Key metrics and samples available
- CPU, I/O, lock, network, or storage bottlenecks identified
- Pre/post optimization verification criteria explained
- Side effects and rollback paths evaluated
- Avoided crossing into architecture evolution