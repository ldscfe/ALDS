---
title: Security Go
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-16
summary: Go-specific security guidance for middleware auth, crypto standards, input sanitization, and HTTP hardening.
based_on: "skills/security.md"
languages: ["go"]
---

# Security - Go Specialization

> This skill is a Go specialization of `skills/security.md`, for HTTP security middleware, cryptographic usage, and input boundary checks in Go services.

## 1. Applicability

- Go HTTP middleware security configuration (CORS, CSRF, HSTS)
- Correct use of the `crypto` standard library
- Input validation and boundary sanitization
- Checking JWT authentication implementations in Go
- Dependency security auditing (govulncheck)

## 2. Trigger Conditions

Load in addition to `skills/security.md` when:

- `languages` in `.AI/project/project.yaml` contains `go`
- Or the current task is explicitly bound to a Go security implementation

## 3. Output Constraints

Inheriting `skills/security.md`, may additionally include:

1. Security middleware configuration checks for Go HTTP frameworks
2. Correct usage patterns for cryptographic libraries
3. Go dependency security scanning recommendations

## 4. Core Rules

1. Hash passwords with `golang.org/x/crypto/bcrypt`, cost factor no less than 10
2. Sign JWTs with `github.com/golang-jwt/jwt/v5`; never use `github.com/dgrijalva/jwt-go` (unmaintained)
3. Set HTTP security headers uniformly through middleware (CSP, X-Frame-Options, X-Content-Type-Options)
4. CORS configuration must restrict origins and methods precisely; never use `Access-Control-Allow-Origin: *` with credentials
5. If the project already has security framework conventions, follow them first

## 5. Prohibitions

- Using `crypto/md5` or `crypto/sha1` for password hashing
- Generating security tokens with `math/rand` when `crypto/rand` is available
- Receiving request bodies as `any` or `interface{}` and using them without type assertion
- Ignoring SQL injection risk from CGO dependencies like `go-sqlite3` (CGO bypasses Go's security boundary)
- Placing the panic recovery middleware before the auth middleware, swallowing auth-error panics

## 6. Recommended Checks

- Password hashing uses bcrypt with cost ≥ 10
- The JWT library is a maintained version
- HTTP security headers are set uniformly at the middleware layer
- No known vulnerabilities in dependencies (`govulncheck ./...`)
- Go-specific security recommendations align with the project's framework ecosystem
