# AI Agent Entry

This project uses ALDS to govern project behavior (the standard package lives in `.AI/`).
**At the start of every session/task, the first step is to read `.AI/index.md` and follow its routing rules.**

- ALDS version: 1.2.0 (subject to the actual content of `.AI/index.md`)
- Entry file: `.AI/index.md`
- Project fact source: `.AI/project/project.yaml`
- Three-state routing: `absent` / `empty_or_incomplete` → `.AI/project/init.md`; `initialized` or standard-package self-maintenance → `.AI/start.md`
- Routing details, directory responsibilities, and decision boundaries: defer to `.AI/index.md`