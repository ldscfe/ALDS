---
title: API
doc_type: skill
status: active
scope: ALDS
updated: 2026-04-22
summary: Reusable guidance for API design tasks with contract-first output structure.
---

# API Design Skill

For API design, interface definitions, request/response modeling, and contract review tasks.

## 1. Applicability

- RESTful API Design
- Interface Path, Parameter, Status Code Definitions
- Request/Response Body Structure Design
- API Contract Review & Completion

## 2. Trigger Conditions

Load when task contains:

- API Design
- Interface Design
- RESTful
- Endpoint
- Request/Response Spec
- Status Code Design
- Parameter Design

## 3. Output Constraints

Output organized as:

1. API Overview
2. Endpoint Definitions
3. Request Specification
4. Response Specification
5. Request & Response Examples
6. Notes

## 4. Core Rules

1. Resource-first, paths express resources not actions
2. HTTP method semantics must match operation intent
3. Explicit success and error response structures
4. Explicit auth, pagination, filtering, sorting, versioning strategies
5. Must not omit exception paths and error code definitions

## 5. Prohibitions

- Action-oriented naming in URLs, e.g., `/getUsers`
- Omitting error response structures without explanation
- Defining request body in GET requests
- Returning sensitive fields
- Substituting implementation details for interface contract

## 6. Recommended Checks

- Resource naming consistent
- Status codes match behaviors
- Field naming unified
- Pagination, filtering, sorting systematic
- Contract directly supports testing and documentation