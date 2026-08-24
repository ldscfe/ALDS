---
title: Operational Commands
doc_type: skill
status: active
scope: ALDS
updated: 2026-07-13
summary: Controlled operational intent command vocabulary and standardized output formats for summary, next steps·task, conclusion three meta-task categories.
related_docs:
  - .AI/index.md
  - .AI/start.md
  - .AI/standards/enums.md
  - .AI/standards/alds_standards.md
---

# Operational Commands Skill

This skill defines controlled operational intent command vocabulary and standardized output formats for three meta-task categories in conversation. These are session-level meta-tasks, not development tasks, do not enter project development task loop.

## 1. Applicability

- Session wrap-up of current work (summary)
- Produce follow-up items (next steps/task)
- Give final conclusion for review, analysis, or decision (conclusion)

## 2. Trigger Conditions

Load this skill when user instruction hits §3 operational intent vocabulary (summary / next steps·task / conclusion).

## 3. Controlled Operational Intent Vocabulary

| Command | Aliases | Meaning |
| :--- | :--- | :--- |
| `summary` | recap, summarize | Wrap up current work objectives, outcomes, status |
| `next steps` | todo, next | Produce follow-up items |
| `conclusion` | verdict | Give final conclusion with basis |

## 4. Output Formats

| Command | Standardized Output |
| :--- | :--- |
| `summary` | Three-part: **① Objective & Scope / ② Completed & Key Results / ③ Status & Verification Evidence**. No "next steps" (carried by "next steps" command). |
| `next steps` | **Follow-up items** list, each: item · priority · owning task brief/source · dependencies or blockers. |
| `conclusion` | **① Conclusion / ② Basis / ③ Applicable Scope & Risks**. |

## 5. Core Rules

1. Respond in user's current language
2. Outcomes, status, next steps must trace to facts occurred this session, no fabrication
3. If insufficient info for a section, explicitly mark "none" or "pending confirmation", no invention

## 6. Boundaries

- **Lifecycle**: These three commands are session-level meta-tasks, not in project development task loop; no persistent artifacts (file writes, git commits) unless user explicitly requests.
- **"Next Steps" ≠ "Next Steps" Section in Docs**: This command only produces next steps list in conversation, does not write to any project document fixed section.
- **"Conclusion" ≠ Analysis Report "Conclusion" Section**: Analysis report conclusion is document structure component; this command is session-level closing semantic, different scope.

## 7. Prohibitions

- Substitute §4 structured output with vague boilerplate like "completed", "all good"
- Disguise operational intent as development task, force into task loop
- Fabricate "outcomes" or "next steps" not verified this session
- Mix unconfirmed implementation commitments into output