---
title: Writing
doc_type: skill
status: active
scope: ALDS
updated: 2026-04-22
summary: Reusable guidance for structured technical writing with clear scope, audience, and document conventions.
---

# Writing Skill

This skill produces structured technical documents with clear scope, audience, and conventions.

## 1. Applicability

- README
- User Manuals
- API Documentation
- Architecture Descriptions
- Design Decision Records (ADR)
- Technical Reports

## 2. Trigger Conditions

Load this skill when task contains any:

- Write documentation
- README
- User Manual
- API Documentation
- Architecture Description
- ADR
- Technical Report

## 3. Output Constraints

Output should follow:

1. First clarify target audience, scope, and purpose
2. Use hierarchical headings to organize information
3. Prefer tables for parameters, status codes, options
4. Use explicit warning formats for cautions
5. Example content must be clearly separated from body

## 4. Core Rules

1. Language formal, objective, traceable
2. Document structure serves reader tasks, not information stacking
3. Terms, constraints, preconditions clearly stated
4. Explanatory docs must not mix unconfirmed implementation promises
5. Adjust structure per document type, not single template

## 5. Prohibitions

- Colloquial expressions replacing formal descriptions
- Mixing unconfirmed implementation details in explanatory docs
- Omitting scope, preconditions, or limitations
- Hiding key conclusions in long paragraphs
- Producing non-executable or non-maintainable document structures

## 6. Recommended Checks

- Target audience clear
- Document structure matches task
- Terms and definitions consistent
- Limitations, risks, cautions complete
- Document convenient for future maintenance and updates