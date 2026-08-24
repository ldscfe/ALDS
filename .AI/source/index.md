# Source Materials

`.AI/source/` stores raw input materials needed for project initialization and subsequent governance judgments.

This directory only stores source materials, not AI-generated project governance context. AI generates or updates `.AI/project/`, `reports/`, and other controlled artifacts based on this directory's contents.

| Path | Purpose |
| :--- | :--- |
| `.AI/source/requirements/` | Raw requirements, PRDs, user stories, business rules, etc. |
| `.AI/source/design/` | Raw design documents, architecture designs, interaction designs, interface designs, etc. |
| `.AI/source/references/` | Supplementary materials, competitive analyses, meeting notes, screenshots, research materials, etc. |

Usage rules:

1. Raw materials placed in corresponding subdirectories by type.
2. AI reads source materials from this directory during project initialization, records traceable sources in generated artifacts.
3. `.AI/project/architecture/` only stores generated architecture context, not raw design documents.