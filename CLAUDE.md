# algora-scout — rules that don't appear in the code

This is a markdown-only project. There is no `src/` and no scout CLI. The
entire workflow lives in `WORKFLOW.md` and is executed by Claude on demand,
not by automation. **Adding code requires updating `WORKFLOW.md`'s "Hard
rules" first** — the no-automation boundary was set when the skill was
upgraded to this project at 1 merged PR (2026-04-27).

- `shipped-log.md` is **append-only**. State changes update the row in place; rows are never deleted, reordered, or merged. The aborted-targets and org-takeaways sections grow only.
- `WORKFLOW.md`, `evaluation-checklist.md`, `shipped-log.md` are **mutually-referential** — any change to red/green flag definitions must mirror into `shipped-log.md` "Documented failures"; any addition to "Known poison" in `WORKFLOW.md` must mirror into `shipped-log.md` "Aborted targets". Drift between the three is the worst failure mode.
- **Persona boundary**: this project is a **Francis-identity asset**. All PRs, logs, drafts, and outputs are signed under Francis (`franciseliang99-dot` GitHub handle, real name). **Do not** mix Wenzel-persona content (`@wenzelborsos` X handle, xpost-bot drafts) into anything tracked here. Cross-persona content is rejected at scout time, not silently archived.
- `~/.claude/skills/algora-scout/SKILL.md` is a **thin shim** — it only redirects trigger words to this project. Never edit logs / checklist / workflow there. All state lives in this directory.
