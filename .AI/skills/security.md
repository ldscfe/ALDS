---
title: Security
doc_type: skill
status: active
scope: ALDS
updated: 2026-04-22
summary: Reusable guidance for security review, unsafe pattern detection, and risk-oriented defensive checks.
---

# Security Skill

For security reviews, dangerous pattern identification, defensive boundary checks, and security risk explanations.

## 1. Applicability

- Security Audits
- Input & Command Boundary Checks
- Permission & Authentication Isolation Checks
- Web Security Protection Checks
- Dangerous Pattern Identification

## 2. Trigger Conditions

Load when task contains:

- Security
- Audit
- Vulnerability
- XSS
- CSRF
- Injection
- Permission Isolation

## 3. Output Constraints

Output organized as:

1. Review Scope
2. Risk Points
3. Exploitation Paths or Consequences
4. Mitigation Suggestions
5. Verification Methods

## 4. Core Rules

1. Prioritize input boundaries, execution boundaries, permission boundaries
2. Risk explanations must land on exploitable consequences where possible
3. Mitigation suggestions should distinguish immediate and long-term measures
4. If insufficient evidence, keep risk levels and pending confirmations
5. Security suggestions cannot replace project's actual permission model

## 5. Prohibitions

- Exaggerating or downplaying security risks without evidence
- Treating style issues as security issues
- Only giving abstract advice without concrete risks
- Ignoring verification methods
- Passing project-specific implementation details as general security rules

## 6. Recommended Checks

- Inputs validated and boundary-constrained
- Permission model has no privilege escalation paths
- Outputs have no injection or exposure risks
- Dangerous calls isolated
- Risk fixes verifiable