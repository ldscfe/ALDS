---
title: ALDS Session Log
doc_type: session_log
status: active
scope: ALDS
updated: 2026-08-13
---

# Instruction Ledger

Records each **substantive user instruction** + **AI response summary** + metadata (environment/model/session/repository status, etc.). Specification in `.AI/start.md` §12; column definitions and metadata collection in `.AI/templates/session_log_template.md`.

- Newest on top (new entries inserted at first row below header).
- Only substantive instructions; pure confirmations/clarifications/greetings not recorded separately.
- AI only appends to disk, **does not auto-commit**; when to commit is user's decision.

| # | Time | Environment | Model·Session | Repo | User Input | AI Response | Artifact·Path | Context |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|  |  |  |  |  |  |  |  |  |