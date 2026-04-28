# CHANGELOG

## V0.1.1 — 2026-04-27 — 5th dry scan + devin-ai-integration squat takeaway

**Trigger**: 2026-04-27 evening scout pass found 0 viable Tier-1 candidates (5th consecutive dry round). Aggregate `algora.io/bounties/{typescript,python,javascript}` returns only archestra (poison) + zio/kyo (Scala out of allowlist) + twentyhq IMAP (R16 neo773-locked). Per-org pages: mastra-ai/Zulip 404, CapSoftware drained, twentyhq 1 bounty.

**Mastra unassigned bug pool — 7 candidates fully scored, 7 dropped**:
- #15229 (R2+R3): PR #15769 OPEN same fix + `@Magicray1217` soft-claim (same person as #15692 ambient risk)
- #15729 (R2): `app/devin-ai-integration` already posted root-cause + Reproduction Steps in comments → de-facto claim
- #15823/#15734/#15481 (R6 effort:high)
- #15799 (R12 status:wontfix)
- #15509 (R14 partial fix already landed)

**twentyhq pivot blocked**: `@neo773` is repo-wide incumbent (42 assigned issues, not just IMAP/CalDAV/Gmail domain). Zero `good first issue` unassigned. Original R16 scope (IMAP-only) was too narrow — neo773 is the whole-repo incumbent.

**Diff vs V0.1.0**:
- `shipped-log.md`: dry counter 4 → 5; resolved stale "pending user GO" line (scaffold complete, commit 22a0d28); 5 abort rows appended (mastra batch + twentyhq pivot); org-level takeaway #2 (mastra) extended with `app/devin-ai-integration` evidence.
- `WORKFLOW.md`: mastra-ai/mastra "Known bounty-paying orgs" row gained pre-draft squat-check command + comment-thread devin signal.

**Why** the takeaway #2 update is load-bearing: original takeaway only named `kagura-agent` (all-CLOSED pattern, low immediate threat). 2026-04-27 data shows `app/devin-ai-integration` is the dominant squatter — 15 PRs in 24h, mix of MERGED and OPEN, and uses comment-thread root-cause posts as soft claim mechanism. Cold account 2nd mastra PR cannot be drafted blind to this; pre-draft `gh pr list --author app/devin-ai-integration` is now mandatory before mastra scope selection.

---

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
