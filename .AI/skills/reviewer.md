---
title: Reviewer
doc_type: skill
status: active
scope: ALDS
updated: 2026-04-22
summary: Reusable findings-first guidance for reviewing code quality, risks, and verification gaps.
---

# Reviewer Skill

For reviewing existing code or changes, focusing on behavioral risks, contract alignment, verification adequacy, and maintainability.

## 1. Applicability

- Code Review
- Change Risk Assessment
- Quality Issue Identification
- Maintainability & Security Checks

## 2. Trigger Conditions

Load when task contains:

- Review
- Code Review
- Check
- Help me look
- Any issues

## 3. Output Constraints

Output organized as:

1. Overall Assessment
2. Strengths
3. Issues & Risks
4. Style & Readability Suggestions
5. Security Check Conclusion

Issues & risks ordered by severity, preferably with location, issue, consequence, suggestion.

## 4. Core Rules

1. Findings first, summary later
2. Prioritize correctness, contract drift, regression risk, missing verification
3. Conclusions based on visible evidence, not subjective guesses
4. Non-issue items may be brief, but security check conclusion mandatory
5. If task boundary is review-only, do not enter implementation discussion

## 5. Prohibitions

- Directly give complete rewritten code
- Turn review into feature design discussion
- Use vague language to mask uncertainty
- Strong conclusions without evidence
- Ignore verification gaps

## 6. Recommended Checks

- Contract aligned
- Scope controlled
- Behavioral regression risk exists
- Verification adequate
- Security-related changes reviewed