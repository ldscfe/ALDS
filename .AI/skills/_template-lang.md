---
title: "{Skill Name} - {Language}"
doc_type: skill
status: draft
scope: ALDS
updated: 2026-05-14
summary: "{Language} specialization supplement for {Skill Name}"
based_on: "skills/{base_name}.md"
languages: ["{language}"]
---

# {Skill Name} - {Language} Specialization

> This skill is a {Language} specialization supplement of `skills/{base_name}.md`, for {briefly describe language-specific scenarios}.

## 1. Applicability

- {Language-specific scenario 1}
- {Language-specific scenario 2}
- {Language-specific scenario 3}

## 2. Trigger Conditions

Overlay load after `skills/{base_name}.md` when:

- `.AI/project/project.yaml` `languages` contains `{language}`
- Or current task explicitly bound to {Language} implementation

## 3. Output Constraints

On top of inheriting `skills/{base_name}.md`, may supplement:

1. {Language-specific output requirement 1}
2. {Language-specific output requirement 2}
3. {Language-specific output requirement 3}

## 4. Core Rules

1. {Language-specific rule 1}
2. {Language-specific rule 2}
3. {Language-specific rule 3}
4. If project has {Language} conventions, prefer project conventions
5. Must not mistake framework features for {Language} general rules

## 5. Prohibitions

- {Language-specific anti-pattern 1}
- {Language-specific anti-pattern 2}
- {Language-specific anti-pattern 3}
- Mistaking {Language} memory/type safety for overall security guarantee
- Pushing specific {Language} middleware strategies without project context

## 6. Recommended Checks

- {Check item 1}
- {Check item 2}
- {Check item 3}
- {Language} specialization consistent with project ecosystem