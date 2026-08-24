---
title: Security TypeScript
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: TypeScript-specific frontend security guidance for XSS prevention, CSP, router guards, and token management.
based_on: "skills/security.md"
languages: ["typescript"]
---

# Security - TypeScript Specialization

> This skill is a TypeScript specialization of `skills/security.md`, for XSS defense, token management, and router guards in Vue frontend projects.

## 1. Applicability

- XSS defense in Vue templates (`v-html`, rendering user input)
- Content Security Policy configuration
- Authentication checks in Vue Router navigation guards
- Token storage and refresh strategies
- Axios interceptor security configuration

## 2. Trigger Conditions

Load in addition to `skills/security.md` when:

- `active_language` is `typescript`
- Or the current task is explicitly bound to a frontend security implementation

## 3. Output Constraints

Inheriting `skills/security.md`, may additionally include:

1. Secure coding patterns for Vue frontends
2. Token storage and lifecycle management on the frontend
3. Route-level access control implementation

## 4. Core Rules

1. Store tokens in `localStorage` and configure an Axios interceptor to attach the `Authorization` header automatically
2. Vue Router's global before-each guard checks token presence and validity; redirect unauthenticated users to `/login`
3. Never use `v-html` to render user input; when unavoidable, sanitize first with DOMPurify
4. Configure CSP headers via backend response headers or meta tags, restricting script-src and connect-src
5. If the project already has security conventions, follow them first

## 5. Prohibitions

- Storing passwords or sensitive credentials in `localStorage` or `sessionStorage`
- Persisting user passwords in Vuex/Pinia
- Executing dynamic code with `eval()`, `new Function()`, or `setTimeout(string)`
- Hardcoding API secrets or JWT keys in frontend code
- Ignoring XSS introduced by missing `@click.prevent` default event prevention

## 6. Recommended Checks

- Tokens are stored securely
- Router guards cover all routes requiring authentication
- `v-html` usage has passed security review
- The CSP policy is effective in production
- TypeScript-specific security recommendations align with the project's framework ecosystem
