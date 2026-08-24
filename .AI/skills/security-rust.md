---
title: Security Rust
doc_type: skill
status: active
scope: ALDS
updated: 2026-08-24
summary: Rust-specific security guidance for unsafe usage, handler boundaries, auth flow isolation, and web-layer defenses.
based_on: "skills/security.md"
languages: ["rust"]
---

# Security - Rust Specialization

> This skill is a Rust specialization of `skills/security.md`, for handler boundaries, unsafe usage, and common web defense checks in Rust services.

## 1. Applicability

- Rust service security auditing
- Handler and parsing boundary checks
- Authentication and connection isolation checks
- Template and response output security checks
- unsafe usage review

## 2. Trigger Conditions

Load in addition to `skills/security.md` when:

- `languages` in `.AI/project/project.yaml` contains `rust`
- Or the current task is explicitly bound to a Rust security implementation

## 3. Output Constraints

Inheriting `skills/security.md`, may additionally include:

1. Boundary risks in handlers or parsers
2. Auth or connection management recommendations
3. Template rendering and output security recommendations
4. unsafe usage review opinions

## 4. Core Rules

1. Check handler, parser, and external-input boundaries first
2. Recommendations on auth, connection reuse, and resource isolation must state their security boundaries
3. unsafe usage must be examined separately with its necessity justified
4. Web-layer output security recommendations should focus on template and serialization boundaries
5. If the project already has security guardrails, follow the project conventions first

## 5. Prohibitions

- Treating Rust's memory safety as an overall security guarantee
- Ignoring template output and handler input boundaries
- Accepting unsafe paths without stating their necessity
- Mistaking framework default protections for protections the project actually has
- Pushing specific middleware strategies without project context

## 6. Recommended Checks

- Handler and parser boundaries cannot be abused
- Auth or connection management has no isolation gaps
- Template output carries no injection risk
- unsafe usage is minimal and bounded
- Rust-specific security recommendations align with the project ecosystem
