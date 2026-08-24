---
title: Troubleshooting
doc_type: skill
status: active
scope: ALDS
updated: 2026-04-22
summary: Reusable guidance for structured incident investigation, evidence collection, and remediation planning.
---

# Troubleshooting Skill

For systematic fault localization, diagnostic evidence organization, root cause analysis, and solution planning.

## 1. Applicability

- Troubleshooting
- Startup Failure & Runtime Anomaly Analysis
- Service Unavailability Problem Localization
- Temporary & Permanent Solution Design

## 2. Trigger Conditions

Load when task contains:

- Failure
- Troubleshooting
- Diagnosis
- Service Anomaly
- Startup Failure
- Connection Failure
- Runtime Error

## 3. Output Constraints

Output organized as:

1. Problem Definition
2. Information Collection
3. Troubleshooting Steps
4. Root Cause Analysis
5. Solutions
6. Prevention Measures

## 4. Core Rules

1. Collect evidence first, then converge conclusions
2. Troubleshooting steps should give expected results and judgment criteria
3. Root causes ordered by likelihood, with verification methods
4. Solutions must distinguish temporary and permanent
5. Fault handling prioritizes business availability and rollbackability

## 5. Prohibitions

- Concluding directly without sufficient evidence
- Simultaneously advancing multiple high-risk fixes
- Ignoring logs, monitoring, environment differences
- Changing production config without impact assessment
- Not recording troubleshooting process

## 6. Recommended Checks

- Impact scope and fault severity defined
- Logs, monitoring, environment info collected
- Troubleshooting chain reproducible
- Root cause evidence-backed
- Follow-up prevention actions formed