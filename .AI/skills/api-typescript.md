---
title: API TypeScript
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: TypeScript-specific API client guidance for Axios, fetch patterns, type-safe requests, and error handling.
based_on: "skills/api.md"
languages: ["typescript"]
---

# API - TypeScript Specialization

> This skill is a TypeScript specialization of `skills/api.md`, for HTTP client wrapping, type-safe requests, and API layer architecture in frontend projects.

## 1. Applicability

- Axios / fetch wrapping and interceptors
- TypeScript generics in the API layer
- Request state management (loading, error, data)
- API error handling and type assertions
- Integration of the frontend API layer with state management (Pinia)

## 2. Trigger Conditions

Load in addition to `skills/api.md` when:

- `active_language` is `typescript`
- Or the current task is explicitly bound to a TypeScript frontend API implementation

## 3. Output Constraints

Inheriting `skills/api.md`, may additionally include:

1. Organization structure of TypeScript API modules
2. Definition of generic response types
3. Type-safe handling in Axios interceptors

## 4. Core Rules

1. Organize API modules into files by business domain; each file exports typed functions
2. Constrain response types with generics: `ApiResponse<T>` wrapping `data`, `message`, `code`
3. Create the Axios instance centrally; configure baseURL and interceptors in the singleton
4. Convert errors uniformly at the interceptor layer; component layers handle only business errors
5. If the project already has API layer conventions, follow them first

## 5. Prohibitions

- Importing axios/fetch directly in components to make requests (go through API modules)
- Using `any` instead of explicit response type definitions
- Ignoring global 401 handling in interceptors (should redirect to the login page automatically)
- Mixing UI state management logic into API modules
- Using `try/catch` instead of unified error handling in Axios response interceptors

## 6. Recommended Checks

- Response types are wrapped with generics
- Errors are handled uniformly at the interceptor layer
- baseURL is read from environment variables
- API functions return explicit Promise types
- TypeScript-specific recommendations align with the project's framework ecosystem
