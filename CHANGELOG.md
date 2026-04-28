# CHANGELOG

## V0.1.0 — 2026-04-27 — scaffolded from skill upgrade trigger

**Trigger**: 2026-04-27 mastra-ai/mastra PR #15692 merged (1st OSS merge for cold account, 4 days from open to merge with APPROVED review). SKILL upgrade gate "1 merged PR → local repo" satisfied; user explicitly requested upgrade.

**Migrated from** `~/.claude/skills/algora-scout/`:
- `SKILL.md` → `WORKFLOW.md` (renamed, content unchanged)
- `evaluation-checklist.md` (moved, content unchanged)
- `shipped-log.md` (moved, content unchanged)

**New files in this project**:
- `CLAUDE.md` — 4 hard rules: markdown-only / append-only log / 3-file mutual-ref sync / Francis-persona boundary
- `CHANGELOG.md` — this file

**Skill folder retained** as a thin shim: `~/.claude/skills/algora-scout/SKILL.md` (5 lines, redirects trigger words "扫 Algora" / "找 bounty" / "Algora 赏金" / "open source bounty" to this project's `WORKFLOW.md`). Trigger entry point preserved; no duplicate workflow maintenance.

**Design decisions** (per Step-0 subagent meta-audit):
- (a) markdown-only, no Python/TS scout CLI — SKILL self-locked "no automation" at 1-merge tier; sample size doesn't justify code
- (b) Strict adherence to SKILL's "no agent in the name" — directory is `algora-scout`, not `algora-agent`
- (c) Thin SKILL shim chosen over full skill deletion — preserves global trigger-word routing without splitting workflow across two locations
- (d) User prerogative to override SKILL gate timing accepted — scaffold cost is near-zero, no irreversible side effects
