# algora-scout — workflow

Purpose: find and ship small OSS contributions that (a) win Algora bounties directly, or (b) build merged-PR track record in repos that pay Algora bounties, so later bounty claims are accepted faster.

Project hard rules in `CLAUDE.md`. Trigger words are routed through the shim at `~/.claude/skills/algora-scout/SKILL.md`; this file is the canonical workflow.

Context is in `~/.claude/projects/-home-myclaw/memory/`:

- `user_tech_stack.md` — language allowlist with **ship-readiness tiers**. Allowlist: Python, TS/JS, Java, C++, Rust, **PHP, Scala, Ruby (since V0.1.8 2026-04-29)**. Reject others from recommendations (still show in market-view section C).
  - Tier 1 (Python / TS / JS / **Rust** since V0.1.5 2026-04-29; **Java / PHP / Scala / Ruby** since V0.1.8 2026-04-29): any scope (hard rules ≤50 lines / 3 files still apply).
  - Tier 2 (C++ only since V0.1.8): bug fixes + small features ≤150 lines.
  - Tier 3: (empty)
  - **0-exp 注**: PHP/Scala/Ruby 实际生产经验 = 0;Tier 1 标记 reflect scout-allowlist policy 不是 production depth;首战 PR budget 2-3x 时间 + low-stakes bounty 优先。
- `user_english_constraint.md` — async-only platforms; skip anything with live review calls.
- `feedback_direct_next_step.md` — after analysis, state the one next action as a directive; don't offer options.

## Workflow (strict order)

### 1. Scout

Query in priority order:

1. `https://algora.io/bounties/typescript` + `/python` + `/java` + `/cpp` (aggregate, language-filtered)
2. Per-repo pages of orgs with Algora bounty history (see "Known bounty-paying orgs" below)
3. If Algora pool is dry: switch to **no-bounty bug-fix scouting** in the same orgs (GitHub issues with `label:bug` + `no:assignee` + recent maintainer activity)

For each candidate, run `evaluation-checklist.md` scoring. Drop any candidate that fails a **red flag** row.

### 2. Cross-reference check (non-negotiable before drafting)

For each candidate that passes scoring, run all three:

```bash
# (a) cross-referenced PRs/issues from the timeline
gh api "repos/<org>/<repo>/issues/<N>/timeline" --jq '.[] | select(.event=="cross-referenced") | {src: .source.issue.html_url, state: .source.issue.state}'

# (b) keyword search across PRs (catches fix PRs whose title matches the bug)
gh pr list --repo <org>/<repo> --search "<keyword> in:title,body" --state all

# (c) read the last 3-5 issue comments — LLM triage bots (Dosu, @dosubot[bot], similar) frequently post "fixed in PR #X / v1.Y.Z".
#     These are NOT surfaced by (a) or (b) when the fix PR title doesn't match the bug keywords (see R14 + better-auth #8904 in evaluation-checklist).
gh issue view <N> --repo <org>/<repo> --json comments --jq '.comments[-5:] | .[] | {author: .author.login, body: .body}'

# (d) for long-open bounties with a feature keyword (e.g. "IMAP", "CalDAV", "Gmail"), map incumbent activity — an unassigned flagship can hide
#     a domain incumbent squatting via sibling issues (see R16 + twentyhq IMAP $2500).
gh api -X GET search/issues -f "q=repo:<org>/<repo> <keyword> is:issue" --jq '.items[] | {num: .number, assignees: [.assignees[].login], state: .state}' | head -20
```

If (a) or (b) find an open/merged PR → drop. If (c) uncovers a bot-credible "already fixed" — verify the referenced PR, then drop. If (d) shows 5+ recent issues all assigned to one user who's merging PRs in the same domain → drop, the bounty is de-facto claimed.

### 2b. Fix-target verification (R15)

Before drafting, confirm the edit target is a file in the repo:

```bash
gh api "repos/<org>/<repo>/contents/<suspected-path>" --jq '.size' 2>&1
```

If the "fix" would require editing a GitHub release/tag body, a pinned-issue PSA, an org-level wiki, or anything maintained via the GitHub UI rather than in-tree — there is no PR to open. Drop, or post a comment on the issue instead.

### 3. Present the ranked list

Give the user a short table: title, repo, scope sentence, language, risk, **one recommendation** with reasoning. No menu. The user says go / change target / abort.

### 4. Draft PR locally

- Clone shallow: `git clone --depth=50 <repo>`
- **Verify target branch**: some repos require PRs against `dev` (e.g., open-webui auto-closes PRs to `main`). Check `CONTRIBUTING.md` + recent merged PRs.
- Create feature branch off the correct base (e.g., `git fetch origin dev:dev && git checkout -b fix/<slug> dev`).
- Apply minimal fix. Add sync syntax check (`python3 -m py_compile` / `tsc --noEmit` / `node --check`).
- Commit with a conventional message matching the repo's style. **Do not add `Co-Authored-By: Claude`** for third-party OSS PRs — some maintainers close AI-attributed PRs on sight. Save the attribution for user's own repos.

### 5. Present preview, get explicit GO

Hard gate before any remote action. Present:

- Three network actions (fork if needed, push, pr create)
- PR title
- Full PR body including repo-required CLA checkbox
- Diff stats
- Base branch confirmation
- Any design-choice decisions worth flagging (e.g., "used sync DB variant instead of asyncio juggling, prepared to defend if maintainer pushes back")

User says GO / ABORT / 改 X.

### 6. Ship

On GO:

```bash
gh repo fork <org>/<repo> --clone=false   # only if first PR to this org
git remote add fork <fork-url>             # only if not already added
git push -u fork <branch>
gh pr create --repo <org>/<repo> --base <base-branch> --head <user>:<branch> --title "..." --body "..."
```

Use a HEREDOC for the PR body to keep markdown intact.

**Variant: PR-into-PR (co-author contribution to another contributor's open PR)**. Only when the other contributor has issued an *explicit public invitation* in the upstream PR thread (the words "contribute to my branch" / "push commits here" — DM or memory-only invitations don't qualify). Mechanics:

```bash
git remote add <contributor> https://github.com/<contributor>/<repo>.git   # only first time
git fetch <contributor> <their-pr-branch>
git checkout -b <your-branch> <contributor>/<their-pr-branch>
# ... edit + commit ...
git fetch <contributor> <their-pr-branch>   # last-second race check
git rev-parse <contributor>/<their-pr-branch>   # abort + rebase if tip moved
git push -u fork <your-branch>
gh pr create --repo <contributor>/<repo> --base <their-pr-branch> --head <user>:<your-branch> --title "..." --body "..."
```

The PR opens against the contributor's fork (not upstream); when they merge it, their upstream PR auto-includes your work. Track in `shipped-log.md` with the contributor-fork URL in the Repo column and a Notes line referencing the upstream PR + their invitation comment.

### 7. Log

Append a row to `shipped-log.md`. Update when state changes (CI, review, merge, close, bounty).

## Hard rules

1. **Never auto-submit PRs**. Human approval gate at step 5 is mandatory, regardless of how clean the diff looks.
2. **Never batch PRs**. One candidate at a time. User holds the throttle.
3. **Never push to a repo's main branch remote**. Only ever push to user's fork. PR target is one of: (a) upstream's designated contribution branch, or (b) another contributor's open-PR branch when they have *explicitly publicly invited* a co-author commit (PR-into-PR — see Step 6 variant).
4. **Never invent a fix not grounded in the issue body + maintainer-visible code**. If the issue lacks a repro or suggested fix, drop it — maintainer review will drag, which is the worst outcome for a cold account.
5. **Never include AI attribution in third-party PR commits or bodies**. User owns authorship, Claude is internal tooling.
6. **Scope cap**: 50 lines changed, 3 files touched. Over that → drop or split. Cold accounts don't ship multi-file refactors.

## Known bounty-paying orgs (as of 2026-04-29)

Verified via Algora pages. Priority order for scouting = cold-account friendliness × stack match. **Pre-flight:** `WebFetch algora.io/<slug>` to confirm `total awarded > 0` before investing — paid status decays.

**TypeScript-heavy**:
- mastra-ai/mastra (slug `mastra-ai`; check human reviewer assigned before drafting — `dane-ai-mastra` triage bot ≠ review). **Pre-draft squat check (2026-04-27)**: `gh pr list --repo mastra-ai/mastra --author app/devin-ai-integration --state all --limit 20` + `--author kagura-agent`. devin-ai-integration is the dominant squatter (15 PR/24h, MIX of merged + closed); kagura-agent secondary (all-closed pattern). Avoid scope already touched by devin in last 7 days. Also check issue comments for `devin-ai-integration` "Reproduction Steps" — that = de-facto claim. **R17 internal-lock check (2026-04-29)**: mastra has per-issue internal locks (not per-repo); for any candidate, also `gh pr list --repo mastra-ai/mastra --search "<issue-num> in:body" --state closed` and read the close comment — if a mastra-internal voice (e.g. `intojhanurag`) said *"we will raise PR"* / *"needs discussion"* and closed an external fix, the issue is silently claimed.
- twentyhq/twenty (slug `twentyhq/home`; large-scope bounties; R16 IMAP/CalDAV/Gmail incumbent in poison list)
- CapSoftware/Cap (slug `CapSoftware`; mostly Rust crates)
- cal.com (old, crowded; many R5/R17 locks — see abort log)
- maybe-finance (slug `maybe-finance`; **$18,000 awarded / 47 completed** as of 2026-04-29 — 2nd largest after cal.com; primarily Rails/Ruby with significant TS-adjacent issues; **R1 砍 Ruby 部分,只投 TS 子组件**; currently 0 active — watchdog candidate)

**Python-heavy**:
- Zulip/zulip

**Java (Tier 1 since V0.1.8 2026-04-29 — user-explicit override; was Tier 2)**:
- Scope cap: none (same as Python/TS/JS); hard rules ≤50/3 still apply.
- Acceptable: apache/kafka, apache/pulsar, PaperMC/Paper, keycloak/keycloak.
- **2-year gap caveat**: user 2018-2023 Alibaba enterprise Java, 2 yr 离开 budget familiarization time on first PR (modern Spring/Jakarta migration).
- **Still Avoid (orthogonal to tier — domain-risk)**:
  - keycloak/keycloak — IAM/security 红线 (即使会 Java,security-critical 改动 cold-account 不投)
  - apache/kafka + apache/pulsar 分布式系统 internals (broker/consensus 域不投,边缘 client 类 OK)

**C++** (Tier 2):
- godotengine/godot, ClickHouse/ClickHouse, envoyproxy/envoy — only take bugs with clear repro + stack trace; skip architecture changes.

**Rust (Tier 1 since V0.1.5 2026-04-29 — user-explicit override at 0 merged Rust PR; gate bypass acknowledged)**:
- Scope cap: none (same as Python/TS/JS).
- Acceptable: gyroflow/gyroflow (stabilization-parameter bugs), small Rust CLIs, Algora /rust bounties, larger Rust features. Previously-aborted gyroflow #384 stays aborted (prior fix PRs closed — historical reason, not tier-related).
- **Still Avoid (orthogonal to tier — domain-risk, not capacity-risk)**:
  - getdozer/dozer — async streaming internals
  - spaceandtimefdn/sxt-proof-of-sql — crypto audit red line
  - getkyo/kyo — 自研 effect system / DSL 学习曲线 (Scala 升 Tier 1 后仍 Still Avoid)
- Override revert path: `git revert` of the V0.1.5 commit returns Rust to Tier 2 with `≤150 lines / 3 files / $100-500` cap and the original "After first Rust PR merged: revisit tier" gate.

**PHP (Tier 1 since V0.1.8 2026-04-29 — user-explicit override at 0 production experience)**:
- Scope cap: none; hard rules ≤50/3 still apply.
- Scout target: coollabsio/coolify (38 active / $3,693 + $1,382 历史 / 26 completed; PHP 6.8M + Blade 1.6M + JS 56KB; 54k stars; repo health 健康)。
- **0-exp ramp-up**: 40-80h Laravel + Eloquent + Blade + Livewire 学习预算;首战 PR 优先选 $100 以下 low-stakes 入门 issue,验证 cold-account merge rate 后再投 mid-range。
- Override revert path: `git revert <V0.1.8 sha>` 移 PHP 回 "DOES NOT have production experience" + 重新加 coollabsio R1 poison。

**Scala (Tier 1 since V0.1.8 2026-04-29 — user-explicit override at 0 production experience)**:
- Scope cap: none; hard rules ≤50/3 still apply.
- Scout target: zio ecosystem ($38,945 active / 47 open + **$124,025 历史 / 462 completed = Algora 平台最大单 org 之一**; ZIO #519 三层叠 $20k 单 issue;ZIO #9844 Queue shutdown $1,000)。
- **0-exp ramp-up**: 100-150h type system / implicit / for-comprehension / ZIO effect monad 学习 (Scala 学习曲线最陡);首战 PR 选 ≤$500 + clear-repro 入门。
- **Still Avoid (orthogonal to tier — domain-risk)**:
  - getkyo/kyo — 自研 effect system / DSL 学习曲线 (与 V0.1.5 Rust block 一致)
- Override revert path: `git revert <V0.1.8 sha>`。

**Ruby (Tier 1 since V0.1.8 2026-04-29 — user-explicit override at 0 production experience)**:
- Scope cap: none; hard rules ≤50/3 still apply.
- Scout target: maybe-finance ($18,000 历史 / 47 completed; 当前 0 active = watchdog mode 等池补)。
- **0-exp ramp-up**: 30-60h Rails magic / ActiveRecord / Sidekiq 学习。Rails 项目 magic 多但语法易,first-PR 选 backend bug-fix 不选 view/helper。
- Override revert path: `git revert <V0.1.8 sha>`。

**Known poison**:
- archestra-ai/archestra — bounty labels don't mention it, but issue bodies say "Reserved for SE interview". Cold account will ship unpaid.
- Tenstorrent tt-metal — three-stage review (proposal → mid → final), not viable for cold account.
- Any repo with `> 6 months` since last external-contributor PR merge.
- **twentyhq/twenty IMAP / CalDAV / Gmail bounties** — `@neo773` is the domain incumbent (10+ assigned sibling issues, multiple merged `fix: IMAP ...` PRs). The flagship bounty issue is unassigned on purpose (Algora rewards the merged PR, not the claim) but the reward is already spoken for. Pattern generalizes: whenever a repo has a **long-open bounty in a named feature area**, run the R16 check before adopting.
- **open-webui/open-webui** — top maintainer `tjbck` silent-closes external PRs without comment (验证 2026-04-24: #24045 + #24046 同日 silent-close, bot review 干净也砍). 不是 CLA / stale 制度,是 maintainer 不欢迎外部贡献. 不投.
- **triggerdotdev/trigger.dev** — Vouch Request gate. 新 contributor PR 自动被 close,需先开 `Vouch Request: <name>` issue 等 maintainer 认证. 5+ fix PR 踩同坑 (#3292 系列). 介入前 `gh issue list --search "Vouch Request"` 摸流程,或直接跳.
- **"假 paid org" 模式** — algora 页存在但 `$0 awarded / 0 completed` = 不是真 paid org. 已验:`novuhq`, `formbricks`, `resend`, `Unstructured-IO`, `tauri-apps` (2026-04-29 验,$0/0,推测过去通过非-algora 渠道发 bounty). 通用规则:扫前 WebFetch `algora.io/<slug>` 确认 awarded > $0,否则跳.
- **"无 algora 页"** — 部分名义 paid org 实际未在 algora.io 自助 host 任何 bounty. 已验 404:`better-auth`, `payloadcms`, `Chainlit`, `sst`, `tldraw`, `dyrector-io`, `cal`/`calcom`/`cal-com` (多 slug 变种均 404), `mastra-ai` (404 — bounties only via per-repo issue labels), `zulip` (404). 不要再为这几个 org 拉 algora 页.
- **outerbase/starbasedb** — R8 致命: 整 repo 13.5+ 个月零 PR merge (last merged 2025-03-13 by founder Brayden, screened 2026-04-28). bounty issues #59 / #72 各 3+ 人 /attempt + 完整 PR (#129/#138) OPEN 但 maintainer 全无 review。冷账号开了 PR 也 stale。整 repo 不投。
- ~~**coollabsio/coolify** — R1 PHP/Blade~~ **解锁 by V0.1.8** (2026-04-29 user-explicit PHP Tier 1 lift): PHP 6.8M + Blade 1.6M; Algora 38 open / $3,693 + $1,382 历史。现已是 PHP 主要 scout target (见 PHP block 上面)。Revert path: `git revert <V0.1.8 sha>` 恢复 R1 poison 状态。
- **PX4/PX4-Autopilot** — R1 C++ autopilot/无人机项目。Algora 平台直接挂 issue (#22464)。同 ClickHouse/envoy/godot 类 — Francis portfolio 无 C++,跳。
- **Cap-go/capgo (security disclosure rolling pool 模式)** — algora.io/Cap-go 直接 404,但 algora.io/bounties 主页列 #1667 "security issues retribution"。issue body 极短 + 60 评论 + 17 awards $2,780+ 散给 13 commenters → 是 **rolling security bounty pool** 而非 PR-fix bounty。性质 = security disclosure form,不写代码 PR。**Francis 不做 security research → Cap-go 整 org 永久跳** (V0.1.7 takeaway #11)。识别信号 (任三同满): body < 50 字 + 评论 > 30 + 单 issue 3+ awarded events + 关键词 "security"/"retribution"/"disclosure"。
- **algora.io 列表 stale 信号** — algora.io/bounties 不 sync GitHub close 事件 (24-72h 延迟)。verified 2026-04-29: zed-industries/zed #4440 vim Replace mode 已 GitHub CLOSED 但 algora 仍列。Pre-flight 必跑 `gh issue view <num> --repo <org>/<repo> --json state,body,labels` 验 ① state != CLOSED ② body 长度 > 50 字 (避免 R6-sec security pool) ③ labels 不含 "security/disclosure/retribution"。

## Portfolio rules (cold-account specific)

- **Max 2 open PRs per org at once.** After 2, do not draft a 3rd to the same org until one merges, gets a review, or closes. Reviewer-attention cannibalization is real: 3 unreviewed PRs from one cold contributor get batch-ignored or batch-"please consolidate"'d, and a single "we don't accept this kind of change" reply poisons all three threads. Validated 2026-04-23: Step-0 subagent flagged the risk on the mastra scout after 2 open-webui PRs; I diversified to mastra, which was correct.
- **Same-org gate re-triggers apply.** If PR #1 in an org hits a CLA bot / DCO sign-off / Coderabbit mandatory-address gate, PR #2 will hit the same gate. Check the CI pane on the earliest open PR in the org before drafting the next candidate there.
- **Public in-PR commitments must be scheduled, not memory-d.** If you comment "I'll open a follow-up PR once this merges" in a public thread, immediately schedule a weekly `RemoteTrigger` routine with a built-in stop condition (Step 2 checks "follow-up already exists" → ALREADY_DONE exit). Relying on TODO/memory is how commitments rot.

## Upgrade triggers

- **1 merged PR** → move this skill's content into a local repo `/home/myclaw/algora-scout/` (private) with a CLAUDE.md and CHANGELOG, following the other bot projects' pattern. Still no automation. Still no "agent" in the name.
- **3 merged PRs** → consider if a public OSS-contribution-methodology repo is worth publishing. If yes, rename to something neutral (`oss-contrib-log`, `good-first-pr-notes`) — never "algora" + "agent" in a public name.
- **First bounty paid** → save a `project_*.md` memory with payment path, tax implications for Canadian resident, and reliable org shortlist.

## References

- `evaluation-checklist.md` — per-issue scoring rubric
- `shipped-log.md` — PR history with outcomes
