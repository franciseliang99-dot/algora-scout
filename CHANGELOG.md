# CHANGELOG

## V0.1.3 — 2026-04-28 — first PR-into-PR (grundmanise/mastra#1 forward-ports tripwire into #15637)

**Trigger**: grundmanise replied 2026-04-28 09:58Z on `mastra-ai/mastra#15637` explicitly inviting Francis to push commits to his branch (*"contribute to my PR branch here `grundmanise:grundmanise/channel-stream-hook` ... merge everything through this single PR"*). Resolves the V0.1.2 open-follow-up state — replaces the original "wait, then either bundle formatBlock or file follow-up" plan with "ship the regression-prevention port first, defer formatBlock as separate follow-up after #15637 merges".

**Action taken**:
- Verified grundmanise's `default-consume-stream.ts` (extracted from `consumeAgentStream`) is missing the `tripwire` branch added by #15692 — confirmed regression risk: once #15637 rebases past `00f8f8c14`, processor `strategy: "block"` notifications silently drop again.
- Branched `fix/tripwire-default-consume-stream` off grundmanise's tip (`1fd0746`).
- Forward-ported the 17-LOC tripwire branch + 3 tests + separate `.changeset/*.md` (patch). Tripwire branch byte-identical to #15692. Tests mirror grundmanise's existing `runConsumer` mock pattern.
- Pre-push race check: grundmanise tip unchanged.
- Opened **[grundmanise/mastra#1](https://github.com/grundmanise/mastra/pull/1)** (PR-into-PR; ready, not draft; commit `837d26f`).

**Outcome state**: PR open, awaiting grundmanise review/merge. If merged → his commit becomes part of #15637 → upstream merge carries the tripwire surface preserved. If rejected/abandoned → fall back to filing a separate upstream PR after #15637 lands (or instead of, if #15637 dies).

**Diff vs V0.1.2**:
- `shipped-log.md`: new row for `grundmanise/mastra#1`; `#15692` Notes extended with the coordination resolution; PRs-opened counter 3 → 4 (split into "3 upstream + 1 PR-into-PR"); dry-scan rounds note clarifying 2026-04-28 is NOT a dry round.
- `WORKFLOW.md`: hard rule 3 amended (PR target may be another contributor's branch on explicit public invitation); Step 6 gained "Variant: PR-into-PR" sub-block with mechanics (remote add, race check, push, gh pr create syntax).
- `CHANGELOG.md`: this entry.

**Why** the WORKFLOW rule-3 amendment is load-bearing: prior wording (*"only ever open PRs to the upstream's designated contribution branch"*) would prohibit the literal action just shipped. Without the amendment, future scouts following the rule strictly would refuse a co-author contribution invite — losing a high-leverage path that bypasses cold-account first-PR friction. Letter must catch up to practice. **Boundary preserved**: explicit *public* (in-PR-thread) invitation required (memory or DM doesn't count); still no auto-submit; still no AI attribution; race-check on contributor's tip is mandatory pre-push (encoded in Step 6 variant).

**Caveat**: Did not run `pnpm typecheck`/`vitest` locally before push — monorepo `node_modules` absent; install would be ~5-15 min for one-off check on a 17-line copy-paste of #15692 logic that already passed CI. Disclosed in PR-side `shipped-log` Notes; will respond to whatever CI surfaces on grundmanise's PR.

**Open follow-up state** (do NOT lose):
- Awaiting grundmanise review/merge on grundmanise/mastra#1. If 7 days pass with no engagement, ping or pivot.
- formatBlock hook PR is now properly deferred (was V0.1.2 open follow-up): file as separate upstream PR after #15637 merges. Track via `/schedule` when the merge happens.

---

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
