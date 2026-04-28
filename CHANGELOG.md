# CHANGELOG

## V0.1.2 — 2026-04-27 — email-channel scout + #15637 coordination comment + langfuse poison

**Trigger**: User flagged that scout missed an alternate channel (`gh api notifications` = GitHub email-equivalent). 26 unread notifications surfaced 3 actionable items the algora.io + GitHub-issue scout missed:

1. **Critical**: Francis's 2026-04-23 in-thread commitment on #15692 (`I'll track it and file the follow-up [formatBlock PR] once this merges`) had not been scheduled per WORKFLOW "Public in-PR commitments must be scheduled, not memory-d" rule. PR merged 2026-04-27 21:56 UTC, commitment unfulfilled at scout time.

2. **Regression risk on the merge**: PR #15637 (`feat(core): add consumeStream and formatOutboundText hooks to channels`, grundmanise, +775/-241 across 8 files, OPEN since 2026-04-22) refactors the exact `AgentChannels.consumeAgentStream` method Francis just patched. #15637's last main-merge was 2026-04-27 14:56 UTC — 7 hours BEFORE #15692 landed. Diff grep for "tripwire" in #15637 = 0 hits. Risk: maintainer merges #15637 without rebase awareness, silently regressing #15692's tripwire surfacing.

3. **Langfuse confirmed fake-paid org**: `algora.io/langfuse` returns $0 awarded / 0 completed despite Francis being subscribed to 8 langfuse issues (pre-existing investigation context). Adds to the V0.1.1 fake-paid pattern (formbricks/resend/novuhq/Unstructured already documented).

**Action taken** (this version):
- Posted coordination comment on #15637 ([issuecomment-4332190019](https://github.com/mastra-ai/mastra/pull/15637#issuecomment-4332190019)): rebase heads-up for tripwire branch + formatBlock placement question (`ConsumeStreamHelpers` member vs top-level `ChannelConfig`). User-approved at Step 5 hard gate. Replaces the originally-promised "file follow-up PR" with "coordinate first, then file or defer" — better path because #15637 changes the API shape that formatBlock would attach to.
- Updated `shipped-log.md` #15692 row Notes with follow-up commitment status + comment URL.

**Diff vs V0.1.1**:
- `shipped-log.md`: #15692 Notes column extended with formatBlock-commitment + #15637 coordination state.
- `CHANGELOG.md`: this entry.

**Why** the email-channel finding is load-bearing: WORKFLOW Step 1 only enumerates Algora pages + GitHub issues + per-repo bounty pages as data sources. After 1+ merge, post-merge state (CI fails, mentions, refactors that touch your fix) becomes a HIGHER-value scout signal than cold candidate hunting — but only `gh api notifications` surfaces it. This data source belongs in WORKFLOW Step 1 as a 4th query when `PRs merged: >= 1`. (Defer that WORKFLOW edit until pattern verified across 2nd+ merge — premature codification at n=1.)

**Open follow-up state** (do NOT lose):
- Awaiting grundmanise response on #15637 comment. Per WORKFLOW "Public in-PR commitments must be scheduled" — this should be checked weekly via `/schedule` until either (a) #15637 merges and formatBlock follow-up PR is filed, (b) grundmanise opts to bundle formatBlock into #15637, or (c) #15637 is closed/abandoned.

---

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
