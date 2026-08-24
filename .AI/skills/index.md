---
title: Skills Governance
doc_type: index
status: active
scope: ALDS
updated: 2026-06-01
---

# Skills Governance

`.AI/skills/` is an optional extension layer for storing governed reusable skill descriptions. It is not a default entry point, nor a pre-requisite for task execution.

## 1. Usage Principles

1. Complete entry routing first: read `.AI/index.md`, check `.AI/project/project.yaml` instance fact source status; if `initialized` read project fact source, then enter `.AI/start.md` or `.AI/project/init.md`
2. Only load `.AI/skills/` on demand when task type, target module, or project language explicitly matches
3. `skills` cannot replace `.AI/project/` project facts, nor `.AI/workflows/` and `.AI/process/` process constraints
4. `skills` only provide supplementary capability profiles, language conventions, and professional output specifications

## 2. Governance Rules

1. Skill files must remove project-specific coupling content, avoid writing single-project experience as general skill
2. Skill files must serve reusable capabilities, not record one-off process materials
3. Every skill file must specify applicability, trigger conditions, prohibitions, and output constraints
4. If skill has language-specific versions, must follow language skill naming spec

## 3. Language Skill Specification

Language skills use "general skill + language-specific skill" two-layer naming.

### 3.1 Naming Rules

1. Files without language suffix = general skills, e.g.:
   - `performance.md`
   - `optimizer.md`
2. Files with language suffix = language-specific skills, e.g.:
   - `performance-python.md`
   - `performance-rust.md`
   - `optimizer-go.md`
3. Base names should be unified, concise, reusable; prefer capability names over role descriptions. E.g.:
   - Recommended: `performance`, `optimizer`, `reviewer`
   - Not recommended: `performance-tuner`, `advanced-code-optimizer`, `code-review-specialist`

### 3.2 Matching Rules

1. First read `languages` from `.AI/project/project.yaml`
2. First load general skill matching task intent
3. If task has determined target language, then overlay load same-base-name language-specific skill
4. In multi-language projects, only load language-specific skills for languages directly involved in current task
5. If language undetermined, must not reverse-infer project language just because language-specific file exists

Example:

1. Task matches `performance`, default load `performance.md`
2. If project language or current task language is Python, additionally load `performance-python.md`
3. If no corresponding language-specific file, only use general skill

## 4. Project-Specific Skills

If skill still depends on a specific project's architecture, framework, module naming, or organizational conventions, it should not enter `.AI/skills/`. Such content belongs in project knowledge layer.

## 5. Current Structure

`.AI/skills/` currently uses:

- General skills: base capability name files, e.g., `api.md`, `optimizer.md`
- Language-specific skills: base capability name + language suffix, e.g., `performance-rust.md`
- Domain-specific skills: domain capability name files, e.g., `ui-design-system.md`, `operational.md`
- Skill governance: this file

## 6. Skill Categories

> Single Source of Truth: Skill catalog per §9 as authoritative. Adding/changing skills: first register in §9, then sync §7 language coverage matrix. §6 only structural, §6.1 consistent with §9.1, §6.2 points to §9.2 no longer maintains specific file copies to avoid drift from duplicate maintenance.

Current skills organized as:

### 6.1 General Skills

These files don't depend on single language, directly matchable by base capability name. Complete catalog (authoritative) in §9.1:

- `api.md`
- `database.md`
- `debugging.md`
- `git.md`
- `optimizer.md`
- `performance.md`
- `refactoring.md`
- `reviewer.md`
- `security.md`
- `testing.md`
- `troubleshooting.md`
- `visual.md`
- `writing.md`

### 6.2 Language-Specific Skills

These files overlay language-specific constraints after hitting general skill. Complete authoritative catalog in §9.2 (this section no longer maintains specific file copies to avoid drift). Naming examples: `security-rust.md`, `performance-python.md`, `testing-java.md`.

Usage requirements:

1. Read general skill file first
2. Then overlay language-specific file per project language or current task language
3. When language undetermined, must not read only language-specific file

### 6.3 Non-Skill Layer Content

Following should not go in `.AI/skills/`:

- Project indexes
- Project-specific profiles
- One-off process descriptions
- Rule collections only valid for single project

## 7. Language Coverage Matrix

Marking currently supported language × skill combinations:

| Base Skill | rust | python | java | go | typescript | cpp | sql |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| performance | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ |
| security | ✅ | ✅ | ✅ | ✅ | ✅ | — | — |
| testing | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ |
| debugging | ✅ | ✅ | ✅ | ✅ | ✅ | — | — |
| database | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ |
| api | ✅ | ✅ | ✅ | ✅ | ✅ | — | — |
| refactoring | — | — | — | — | — | — | — |
| optimizer | — | — | — | — | — | — | ✅ |
| reviewer | ✅ | ✅ | ✅ | ✅ | ✅ | — | — |
| writing | — | — | — | — | — | — | — |
| troubleshooting | — | — | — | — | — | — | — |
| visual | — | — | — | — | ✅ | — | — |

✅ = language-specific file exists | — = not yet (contributions welcome)

---

## 8. Language-Specific Skill Operation Guide

### 8.1 When to Add

Consider adding language-specific skill file when:

1. Project uses a language with **unique coding conventions, pitfalls, or best practices**
2. General skill file insufficient for that language's specific constraints
3. Actual development tasks need that language-specific guidance

### 8.2 Naming Rules

```
skills/{base_name}-{language}.md
```

- `{base_name}`: Must exactly match general skill file base name
- `{language}`: Lowercase language identifier, common:

| Language | Identifier | Example |
| :--- | :--- | :--- |
| Rust | `rust` | `security-rust.md` |
| Python | `python` | `performance-python.md` |
| Java | `java` | `testing-java.md` |
| Go | `go` | `testing-go.md` |
| TypeScript/JavaScript | `typescript` | `api-typescript.md` |
| C++ | `cpp` | `debugging-cpp.md` |
| SQL | `sql` | `database-sql.md` |

### 8.3 File Structure

**Copy `skills/_template-lang.md` and modify as needed.** Every language-specific file must contain:

1. **Frontmatter** — `title`, `doc_type`, `status`, `scope`, `updated`, `summary`
2. **Title & Association** — Clearly state which general skill this specializes
3. **§1 Applicability** — Language-specific scenarios
4. **§2 Trigger Conditions** — How to judge this file should load
5. **§3 Output Constraints** — Additional/differentiated output requirements vs general skill
6. **§4 Core Rules** — Language-specific rules (minimum 3)
7. **§5 Prohibitions** — Language-specific anti-patterns (minimum 3)
8. **§6 Recommended Checks** — Actionable checklists

> ⚠️ **Prohibited** to write language-specific files as general skill translations. Content must be language-**unique** constraints, conventions, or pitfalls.

### 8.4 Trigger Conditions

Trigger conditions must satisfy both:

1. Corresponding general skill already hit (task intent matches)
2. Any of:
   - `project.yaml → languages` contains that language
   - Task explicitly specifies `active_language`

### 8.5 Submission Process

1. Copy `skills/_template-lang.md` to `skills/{base_name}-{language}.md`
2. Fill all sections, **especially prohibitions and recommended checks**
3. Register file in `index.md` §6 corresponding category
4. If language previously uncovered, add mark in §7 language coverage matrix
5. Ensure skill correctly referenced in workflow required documents table

### 8.6 Maintenance Requirements

- When base general skill updates, check all language-specific files for sync needs
- Language-specific files must not contradict general skill
- If language-specific content too thin (<3 unique rules), language has no specialization need, don't force create
- When the template structure changes, audit existing language-specific files and backfill the new structure

---

## 9. Current Skill Catalog

Current `.AI/skills/` contains following formal skill files:

### 9.1 General Skills

These files don't depend on single language, directly matchable by base capability name (consistent with §6.1):

- `api.md`
- `database.md`
- `debugging.md`
- `git.md`
- `optimizer.md`
- `performance.md`
- `refactoring.md`
- `reviewer.md`
- `security.md`
- `testing.md`
- `troubleshooting.md`
- `visual.md`
- `writing.md`

### 9.2 Language-Specific Skills

These files overlay language-specific constraints after hitting general skill:

- `api-go.md` — ✅ Go API specialization
- `api-java.md` — ✅ Java API specialization
- `api-python.md` — ✅ Python API specialization
- `api-rust.md` — ✅ Rust API specialization
- `api-typescript.md` — ✅ TypeScript API specialization
- `database-go.md` — ✅ Go Database specialization
- `database-java.md` — ✅ Java Database specialization
- `database-python.md` — ✅ Python Database specialization
- `database-rust.md` — ✅ Rust Database specialization
- `database-sql.md` — ✅ SQL Database specialization
- `database-typescript.md` — ✅ TypeScript Database specialization
- `debugging-go.md` — ✅ Go Debugging specialization
- `debugging-java.md` — ✅ Java Debugging specialization
- `debugging-python.md` — ✅ Python Debugging specialization
- `debugging-rust.md` — ✅ Rust Debugging specialization
- `debugging-typescript.md` — ✅ TypeScript Debugging specialization
- `optimizer-sql.md` — ✅ SQL Optimization specialization
- `performance-go.md` — ✅ Go Performance specialization
- `performance-java.md` — ✅ Java Performance specialization
- `performance-python.md` — ✅ Python Performance specialization
- `performance-rust.md` — ✅ Rust Performance specialization
- `performance-sql.md` — ✅ SQL Performance specialization
- `performance-typescript.md` — ✅ TypeScript Performance specialization
- `reviewer-go.md` — ✅ Go Review specialization
- `reviewer-java.md` — ✅ Java Review specialization
- `reviewer-python.md` — ✅ Python Review specialization
- `reviewer-rust.md` — ✅ Rust Review specialization
- `reviewer-typescript.md` — ✅ TypeScript Review specialization
- `security-go.md` — ✅ Go Security specialization
- `security-java.md` — ✅ Java Security specialization
- `security-python.md` — ✅ Python Security specialization
- `security-rust.md` — ✅ Rust Security specialization
- `security-typescript.md` — ✅ TypeScript Security specialization
- `testing-go.md` — ✅ Go Testing specialization
- `testing-java.md` — ✅ Java Testing specialization
- `testing-python.md` — ✅ Python Testing specialization
- `testing-rust.md` — ✅ Rust Testing specialization
- `testing-sql.md` — ✅ SQL Testing specialization
- `testing-typescript.md` — ✅ TypeScript Testing specialization
- `visual-typescript.md` — ✅ TypeScript Visual Testing specialization

### 9.3 Domain-Specific Skills

These files don't depend on single language, provide specific domain or output specification skill constraints:

- `operational.md` — ✅ Session meta-task output spec (summary/next steps/conclusion)
- `ui-design-system.md` — ✅ SaaS UI spec & alignment validation

### 9.4 Template Files

- `_template-lang.md` — Language-specific skill template

---

## 10. Naming Convergence Recommendations

Future skill additions/adjustments: prefer "base capability name" unified naming.

Recommended base name examples:

- `api`, `debugging`, `database`, `security`, `performance`
- `optimizer`, `reviewer`, `troubleshooting`, `refactoring`, `testing`, `writing`

Corresponding language-specific examples:

- `api-python.md`, `database-postgres.md`, `debugging-rust.md`
- `performance-python.md`, `security-rust.md`, `optimizer-rust.md`
- `testing-go.md`, `writing-zh.md`