---
title: Debugging
doc_type: skill
status: active
scope: ALDS
updated: 2026-04-22
summary: Reusable guidance for defect diagnosis, root-cause analysis, and controlled fix planning.
---

# Debugging Skill

For error localization, root cause analysis, fix design, and regression verification planning.

## 1. Applicability

- Bug Investigation
- Exception Analysis
- Crash & Failure Root Cause
- Fix Solution Evaluation
- Prevention Measure Summary

## 2. Trigger Conditions

Load when task contains:

- Bug
- Error
- Exception
- Crash
- Failure
- Fix
- Stack Trace
- Error Message

## 3. Output Constraints

Output organized as:

1. Problem Localization
2. Root Cause
3. Fix Solution
4. Verification Methods
5. Prevention Measures

## 4. Core Rules

1. Describe phenomenon first, then analyze direct and indirect causes
2. Fix solutions must bind verification methods
3. Must clarify reproduction conditions, trigger boundaries, impact scope
4. Must give regression prevention suggestions
5. If evidence insufficient, keep unconfirmed items, don't fabricate root cause

## 5. Prohibitions

- Give fix conclusion only, no cause explanation
- Ignore boundary conditions and exception paths
- Provide unverified high-risk fix suggestions
- Silent scope expansion
- Pass off temporary workarounds as root fixes

## 6. Recommended Checks

- Minimal reproduction path exists
- Real trigger point located
- Fix covers root cause not just symptoms
- Verification and regression plan complete
- Prevention measures recorded