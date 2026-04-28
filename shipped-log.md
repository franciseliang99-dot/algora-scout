# Shipped PR log

Append-only. One row per PR. Update status column when state changes.

## Status codes

- `open` — PR open, awaiting CI / review
- `ci-red` — CI failed, investigating
- `review-requested` — maintainer asked for changes
- `merged` — merged into upstream
- `closed-rejected` — maintainer closed without merge
- `closed-duplicate` — superseded by another PR
- `bounty-claimed` — Algora bounty paid out

## Log

| Date opened | Repo | PR | Closes | Lang | +/− | Status | Notes |
|---|---|---|---|---|---|---|---|
| 2026-04-23 | open-webui/open-webui | [#24045](https://github.com/open-webui/open-webui/pull/24045) | #24038 | Python | +11 / −1 | closed-rejected | MCP resource content type handler. No bounty. Target = dev. Bot review: 1 warning (non-dict resource), addressed in follow-up commit 2a9128fd. **Silent-closed by `tjbck` 2026-04-24 09:46Z, no comment.** |
| 2026-04-23 | open-webui/open-webui | [#24046](https://github.com/open-webui/open-webui/pull/24046) | #23936, #23938 | Python | +35 / −8 | closed-rejected | OTEL metric callbacks sync conversion. No bounty. Adds 3 sync query variants to UsersTable. Bot review clean. **Silent-closed by `tjbck` 2026-04-24 06:01Z, no comment.** |
| 2026-04-23 | mastra-ai/mastra | [#15692](https://github.com/mastra-ai/mastra/pull/15692) | #15344 | TS | +22 / 0 | merged | AgentChannels.consumeAgentStream tripwire branch so Slack/Discord users see block reason instead of silence. No bounty. Target = main. Changeset added. **Merged 2026-04-27 with APPROVED review — first merge for cold account.** Ambient risk @Magicray1217 never resurfaced (12-day lapse). |

## First-merge hunt stats (track until broken)

- PRs opened (all time): 3
- PRs merged: **1 (mastra #15692, 2026-04-27)**
- PRs closed-rejected: 2 (both open-webui, silent-closed by maintainer 2026-04-24)
- Bounties claimed: 0
- Dry scan rounds (0 candidates shipped): 5 (2026-04-24, 2026-04-25 早, 2026-04-25 晚, 2026-04-27 早, 2026-04-27 晚)
- Days from first-PR-opened to first-merge: 4 (2026-04-23 → 2026-04-27)

**Skill upgrade complete (2026-04-27): repo scaffolded at `/home/myclaw/algora-scout/` (commit 22a0d28 V0.1.0). Skill shim at `~/.claude/skills/algora-scout/SKILL.md` retained as trigger entry only.**

Update these counters after each status change.

## Aborted targets (learning log)

Keep brief. Each row documents *why* we walked away so future scouts don't re-investigate.

| Date | Candidate | Why aborted |
|---|---|---|
| 2026-04-23 | cal.com #5756 Proton Calendar $200 | Proton has no CalDAV by design; cal.com Tech Lead explicitly abandoned 2023-06. |
| 2026-04-23 | activepieces #8135 Canva MCP $100 | Issue body locks bounty to PR #8143 pending Canva OAuth App Review. |
| 2026-04-23 | activepieces #8072 Gmail MCP $200 | Same lock pattern to PR #8083. |
| 2026-04-23 | onyx #2281 JSM connector $250 | Maintainer Weves silent 12+ months on this specific bounty; PR #3087 (complete, 413 lines) stale-bot-closed after author asked for review twice. See R12. |
| 2026-04-23 | gyroflow #384 dynamic zooming $? | Two prior fix PRs (#1132, #1133) closed without merge. Problem harder than described; Tier 3 Rust too risky. |
| 2026-04-23 | twentyhq/twenty IMAP $2500 | R16: `@neo773` is domain incumbent (10+ assigned sibling issues + merged `fix: IMAP ...` PRs). Bounty is unassigned by design but reward path is spoken for. |
| 2026-04-23 | better-auth #8904 phoneNumber units | R14: `@dosubot[bot]` comment disclosed fix had already landed in PR #8006 (2026-02-16). Cross-ref didn't catch it — fix PR title didn't match bug keyword. |
| 2026-04-23 | better-auth #8954 non-ASCII OAuth | R2: PR #9065 already open addressing the same fix. |
| 2026-04-23 | chainlit #2887 release-notes/data-layer | R15: "fix release notes" option targeted the 2.9.4 GitHub tag body (UI-edited, no repo file). Full-integration option crossed 2 repos / 3+ files. |
| 2026-04-23 | chainlit #2897 File(content=) crash | R2: existing open PR #2799 + two tracking issues #2827 #2851. Crowded. |
| 2026-04-24 | mastra #15576 / #15717 / #15706 / #15677 | R2+R14+R6 mixed. `kagura-agent` (autonomous fix agent) 4 天 7 PR 全 CLOSED 抢占整个 fix 赛道 (#15577/#15718/#15709/#15622/#15575/#15571/#15511). 冷账号同 org 叠第 2 个人工 PR 撞时间窗风险极高。 |
| 2026-04-24 | formbricks #7629 editor HTML crash | R7: reporter @slonopot 最后一条自述 "the idea of the issue is no longer relevant". 另 `algora.io/formbricks` 显示 0 awarded / 0 completed — formbricks 已非 bounty org。 |
| 2026-04-24 | trigger.dev #3292 OTLP nanosecond overflow | R2: 5 个 fix PR 已 CLOSED (#3294/#3325/#3362/#3378/#3381/#3383). **+ Vouch gate**: repo 有 "Vouch Request" 制度 (#3384/#3385), 新 contributor PR 自动被 close, 需另开 vouch request issue 等 maintainer 点名。 |
| 2026-04-25 | better-auth #8122 phone-number OpenAPI requestBody | R14: Dosu bot 揭示 root cause 是 PR #7699 结构性回归;兄弟 issue #9298 (emailOTP) 同模式,单 plugin 修复会被要求改通用层。 |
| 2026-04-25 | better-auth #8898 ReDoS proxy host regex | R2 + R3: PR #9333 OPEN 同 fix;`skalkii` 2026-04-23 留 /attempt 评论附文件行号 (< 48h soft claim active)。 |
| 2026-04-25 | payloadcms #15989 s3 getFilePrefix transaction | R2: PR #16172 OPEN "fix(plugin-cloud-storage): pass req to find in getFilePrefix"。 |
| 2026-04-25 | payloadcms #15837 CMD/Shift+Click new tab | R12: 三个 fix PR 全 CLOSED (#15873/#15920/#16001),最后一个附完整 root-cause 仍 silent close,maintainer 静默拒绝整条 issue。 |
| 2026-04-25 | Unstructured #4235 XLSX row boundaries | R2: PR #4299 OPEN "fix: restore double-newline row boundaries"。 |
| 2026-04-25 | Unstructured #4260 partition_pdf fast empty | R12 + R7: PR #4280 "skip complexity check when fast" 已 CLOSED;issue body 本身无可复现样本。 |
| 2026-04-25 | cal.com #18992 no-show zapier $50 | **R17 (new)**: issue body 顶部 "this issue needs to be handled internally. please do not create a PR for this issue, this will handled by the core team."  内部锁。 |
| 2026-04-25 | cal.com #18947 optional guest $25 | 3 个 /attempt 抢占 (chengyixu/vivekkguptaa/duaamjad019-lang) + R6 (UI+DB+booking+email scope 必超)。 |
| 2026-04-25 | cal.com #13532 invite emails $20 | R5: assigned to `anikdhabal` + PR #26957 in progress。 |
| 2026-04-25 | keephq #2112 SNMP provider $200 | R2 极端: 5 个 open PR 同 fix (#6258/#6267/#6275/#6288/#6289) + 2026-04-24/25 仍在堆 /attempt 评论。Cold account 零机会。 |
| 2026-04-25晚 | keephq #3960 Nagios $30 | R2+R3+R5: assigned `Ayush9026` + PR #6218/#6278 已 open + /attempt 抢占。Provider bounty 24h 内必有 2-3 个 /attempt,与 SNMP 同构。 |
| 2026-04-25晚 | keephq #3526 Solarwinds $50 | R2+R3+R5: assigned `DMMutua` + PR #6221/#6277。同上 /attempt 抢占模式。 |
| 2026-04-25晚 | CapSoftware/Cap #1540 Deeplinks+Raycast $200 | R6+R9: 2-stage feature (deeplinks 核心 + Raycast 桥),103 评论,3 月老。 |
| 2026-04-25晚 | trigger.dev zod #2654 $25 | Vouch Request gate (org-wide,新 contributor PR 自动 close)。 |
| 2026-04-25晚 | archestra-ai #3851 OpenAI proxy $900 | R4: 仍是 "Reserved for SE interview" poison org。 |
| 2026-04-25晚 | zio/* 全部 + getkyo/kyo #390 | R1: Scala 不在 allowlist;kyo gRPC = Rust DSL feature request scope >>50 行 (SKILL.md 明确避雷)。 |
| 2026-04-27晚 | mastra #15229 AgentConfig.tools typing | R2 + R3: PR #15769 OPEN 同 fix ("reject plain functions as individual tool entries");`@Magicray1217` 评论 "I'd like to work on this" 软抢占,与 #15692 的 ambient risk 同人。 |
| 2026-04-27晚 | mastra #15729 cross-provider tool_use IDs leak | R2: PR #15745 已 CLOSED 但 `app/devin-ai-integration` 已在评论贴 root-cause + Reproduction Steps,会接管 next-fix; 5+ 历史 PR 同 provider-executed-tool 域 high churn。 |
| 2026-04-27晚 | mastra #15823/#15734/#15481/#15799/#15509 batch | R6 (effort:high label) / R12 (status:wontfix) / R14 (partial fix already landed) 混合;mastra unassigned bug pool 这轮 7 个全 fail。 |
| 2026-04-27晚 | twentyhq 非 IMAP/CalDAV/Gmail 域 pivot | `@neo773` 跨 repo assigned 42 issue (远超 IMAP 域,是 twentyhq 通用 incumbent);`good first issue` 标签 0 unassigned;外部 author 比例 4/20 边缘。整 repo 不止 IMAP 域被锁。 |

## Org-level takeaways (2026-04-24)

更新 `~/.claude/skills/algora-scout/SKILL.md` "Known bounty-paying orgs" / "Known poison" 列表时参考以下。

1. **open-webui → 降级到 poison**. 2026-04-24 同一天内 `tjbck` (founder/top maintainer) silent-close 我的 #24045 + #24046, 无评论无理由, bot review 干净也砍. 模式非 CLA / stale / policy, 是 maintainer 不欢迎外部贡献. 证据链: shipped-log 行 19-20 + close actor query `gh api repos/open-webui/open-webui/issues/<N>/timeline`.
2. **mastra → 双 autonomous-agent 抢占态 (2026-04-27 更新)**. 原 `kagura-agent` (4 天 7 PR 全 CLOSED) 仍在,但 `app/devin-ai-integration` 才是真威胁:24h 内开 15 个 PR,混 fix/feat/refactor,**有 MERGE** (#15794, #15793 今日同时间窗合入). devin 模式: 在 issue 评论里贴 "Reproduction Steps + 代码分析",抢占 fix 路径 (见 #15729 抢占样本). 冷账号同 org 第 2 个 PR 必须避开 devin 已评论/已 PR 触及的 module + 文件;且因 mastra 接受 AI-agent fix,纯靠 "我是人类不是 bot" 没差异化优势,要靠 issue 选择 + 修改路径选择 + commit message 自然度。守住 #15692 再考虑下一个;下次扫前 `gh pr list --repo mastra-ai/mastra --author app/devin-ai-integration --state all --limit 20` 看 devin 这两天扫到哪。
3. **trigger.dev → Vouch Request gate**. 新 contributor PR 自动被 close, 要开 issue `Vouch Request: <name>` 等 maintainer 认证. 读 repo CONTRIBUTING.md 才知. 5 个 fix PR 踩同一坑 (#3292 系列). 介入前先 `gh issue list --repo triggerdotdev/trigger.dev --search "Vouch Request"` 摸清流程.
4. **"Known bounty-paying orgs" 列表 stale**. SKILL.md 列 formbricks + resend 为 TS bounty org, 但 `algora.io/formbricks` 和 `algora.io/resend` 均 **0 total awarded / 0 completed**. 扫 org 前必先 WebFetch `algora.io/<org>` 确认 paid count > 0, 否则即使 PR 合并也拿不到钱.
5. **Pre-flight checklist (下轮扫描先跑)**:
   - WebFetch `algora.io/<org>` → 确认 paid count > 0
   - `gh pr list --repo <org>/<repo> --author <bot-candidate> --limit 20` → 检查是否有 autonomous fix agent (名字含 `-agent`/`-ai`/`kagura`/`devin` 等)
   - 读 CONTRIBUTING.md 前 50 行 → 抓 Vouch / DCO / CLA gate
   - `gh pr list --state merged --limit 20 --json author` → 确认外部 contributor 最近 20 merge 里 >= 3 (trigger.dev 栽在这里, 10 merge 全内部)
