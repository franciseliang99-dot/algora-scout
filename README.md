# algora-scout

A markdown-only workflow for scouting [Algora](https://algora.io) bounties and scoping no-bounty bug-fix PRs to build OSS contribution track record. There is no CLI and no automation — the workflow is executed by Claude Code on demand.

## Files

- `WORKFLOW.md` — scouting workflow + cross-reference checks + known bounty-paying orgs.
- `evaluation-checklist.md` — per-issue scoring rubric (red/green flags + scope estimate).
- `shipped-log.md` — append-only PR portfolio with outcomes.

## Usage

This is a documentation project. To use it, point Claude Code at this directory and ask it to scout, evaluate a candidate, or draft a PR. The hard rules in `WORKFLOW.md` (no auto-submit, never push to upstream main, scope cap 50 lines / 3 files) constrain Claude's actions.

## License

MIT — see [LICENSE](LICENSE).
