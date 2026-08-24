---
title: Session Log Template
doc_type: session_log
status: active
scope: ALDS
updated: 2026-08-13
---

# Session Log (session_log)

Records each **substantive user instruction** + **AI response summary** + **rich metadata** as traceable human-AI interaction ledger. Spec in `.AI/start.md` §12.

- **Trigger**: Only substantive instructions (inputs driving actual work). Pure confirmations/clarifications/greetings ("OK", "continue", etc. no new work semantics) not recorded separately; if confirmation accompanies substantive action, still record.
- **Write**: After processing a substantive instruction, append one row at **first row below header** in `reports/session_log.md` (newest on top).
- **No Auto-Commit**: Only append to disk, file becomes dirty, **no** auto `git commit`; commit timing user's decision.
- **No Fabrication**: Time/repo state from actual command output; response summary & artifacts must trace to facts occurred this round.
- **No Secrets**: Credentials/keys must not appear in "User Input" or "AI Response" (consistent with `skills/git.md` §5).

## Metadata Collection

Before appending, collect host/time/repo fields with one read-only command (model & session id from conversation context):

```
date '+%Y-%m-%d %H:%M %z'; hostname; uname -srm; git rev-parse --abbrev-ref HEAD; git rev-parse --short HEAD; git status --porcelain | wc -l
```

## Column Definitions (9 columns, one row/instruction, newest on top)

| Column | Content | Notation |
| :--- | :--- | :--- |
| `#` | Entry sequence (auto-increment) | Integer |
| `Time` | Local time + timezone | `YYYY-MM-DD HH:MM +ZZZZ` |
| `Environment` | Host·OS/kernel/arch·shell·user (merged) | `macbook·Darwin 25.5/arm64·zsh·ldscf` |
| `Model·Session` | Model·session id prefix (merged) | `glm-5.2·d78dad62` |
| `Repo` | Branch@HEAD·dirty file count | `main@496f768·2Δ` |
| `User Input` | Original or compact paraphrase | `"commit"` |
| `AI Response` | One-line summary of what done | `amend commit to English, remove Co-Authored-By` |
| `Artifact·Path` | Output·task path (simple/engineering) | `commit 496f768·simple task` |
| `Context` | Workflow·READ_SET count·matched skills | `feature_development·READ_SET 7·git` |

> Merge related fields to carry rich info while controlling table width. No value fill `—`.

## Table (real instances start at first row below header; this row is example, real instances don't keep example row)

| # | Time | Environment | Model·Session | Repo | User Input | AI Response | Artifact·Path | Context |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| _example_ | `2026-08-13 09:43 +0800` | `macbook·Darwin 25.5/arm64·zsh·ldscf` | `glm-5.2·d78dad62` | `main@496f768·2Δ` | `"change commit to English..."` | `amend commit to English, remove Co-Authored-By` | `commit 496f768·simple task` | `—·READ_SET 6·git` |