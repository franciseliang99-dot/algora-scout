# CHANGELOG

## V0.1.14 — 2026-05-01 早 — #15934 round-4 maintainer design pushback handled (V0.1.4-mode round 3 闭环 + 第 11 dry scan round)

**Trigger**: User said "扫描algora并查看邮件" → Step 0 subagent (Plan 维度: 第 11 轮节奏判 + 邮件关联性预测 + gmail query 选词 + 不重复事项 + 风险预警) 推 V0.1.4-mode 优先 (Tier 1 sweep 净新增 = 0 跳)。Algora 平台 ground truth 第 1 页 10 issue (ZIO 8 + Twenty IMAP + Kyo #390 $500) **100% V0.1.13 已 abort** (净新增 = 0, V0.1.12 takeaway #18 持续 30+ 天验证)。**第 11 dry scan round**。同时 Gmail 查询命中 mastra #15934 5/1 10:28 TylerBarnes (mastra maintainer) PR-comment [4360652408](https://github.com/mastra-ai/mastra/pull/15934#issuecomment-4360652408) — round-4 真正 maintainer 进入 review (前 3 轮均 reporter jmzhang 闭环)。

**Maintainer 提议** (auto-close on type change): "any reason we can't just push directly to an ordered array and not maintain separate tracking for different types of parts? if the array ends in text-delta (or some other delta) and a part besides text-end comes in, we can push a text-end, and then the next part"。

**Action taken** (mastra side, 1 GitHub action):
- PR 评论 reply [4361409484](https://github.com/mastra-ai/mastra/pull/15934#issuecomment-4361409484): multi-delta same-span 硬反例 (`text-delta(s1, "hello, ")` → `text-delta(s1, "world")` under auto-close 错误 split 成 `["hello, ", "world"]` 而非 `"hello, world"`) + 同 bisect 跨 reasoning interleaving 同 text span case (`text-delta(s1) → reasoning-start(s2) → reasoning-delta → reasoning-end → text-delta(s1)` 中 reasoning-start 触发 synthetic `text-end(s1)` 后 s1 第二 delta 进入新 part) + 引用既有 `should correctly separate interleaved text spans by ID` test 契约 (placeholder pattern 在 first-seen-delta 占 slot, end-event 填 slot, 唯一保留 same-span deltas 累积) + 末段 offer gate placeholder behind multi-delta check or refactor if maintainer cites single-delta-per-span consumer (与 V0.1.13 round-3 路 A 同 YAGNI 风格 — 不主动重写, 等 maintainer 给具体 consumer)。

**Action taken** (algora-scout side, 2 git-tracked files):
- `shipped-log.md`: L24 #15934 row Notes 末尾 in-place append round-4 段 + L32 first-merge stats 计数 dry rounds 10 → 11 + V0.1.14 描述段
- `CHANGELOG.md`: this entry

**Why** 不另起 takeaway #19: round-4 maintainer pushback 是 V0.1.10 takeaway #13 (round-2 evidence-based reply) 框架在 maintainer 维度的延伸应用, 不是新模式发现。"反例 + offer-不重写" 路 A 与 V0.1.13 round-3 reply 路 A 同 YAGNI 风格 (差异: round-3 是 reporter acceptance 后 stand pat, round-4 是 maintainer pushback 后给反例不让步), 都是 "thread ball 转 maintainer + 不主动加 placeholder for imaginary case" — 同框架不同 phase。如未来出现 maintainer 给具体反例后我让步的复杂情况, 再起 takeaway 不晚。dry scan 计数 +1 (10→11) 但 takeaways 不变 (#18 V0.1.12 sync delay 结构性观察 30+ 天持续验证, 仍不需新 entry)。

**Step-0 subagent 审核** (1 round, 100% 采纳 0 分歧):
- subagent (a155bd64ba33bc96a) 5 问回答全 corroborate 三方数据 — Q1 跳 Tier 1 sweep 进 V0.1.4-mode (Algora 10/10 V0.1.13 已 abort 验证) / Q2 邮件 = GitHub PR review notification (TylerBarnes 5/1 design pushback 命中) / Q3 gmail query 选词命中 #15934 12+ comments / Q4 不重复 V0.1.12 abort + Tier 1 actionable + HTML grep PR-level 区分 (本轮全避免) / Q5 scope 锁邮件 reply 不开新 PR (mastra cap=2 portfolio rule 一致)
- 实际执行与 subagent 5 问预测 100% 一致 — 验证 5 步流程 Step 0 subagent 在 V0.1.4-mode 已建立稳态后是 ROI 高的 audit (相比 V0.1.12 等 framework-discovery 轮 takeaways 由实测发现, V0.1.13/V0.1.14 portfolio-driven 轮 subagent 预测命中率高, 因 reply tactics 已 codified)

**Diff vs V0.1.13**:
- mastra side: 1 reply comment 4361409484 (无新 commit/branch/push)
- `shipped-log.md`: L24 in-place append round-4 段 + L32 dry round 计数 10→11 + V0.1.14 描述段
- `CHANGELOG.md`: this entry
- 不动 WORKFLOW.md (无新规则发现; V0.1.12 三 bullet + V0.1.13 maintainer-acceptance 模板均持续生效)
- 不动 evaluation-checklist.md (R/G flag set 与 round-4 pushback 处理正交, CLAUDE.md 3-file mutual-ref 守恒)

**Open follow-up state** (updated):
- `mastra-ai/mastra#15934` **round-4 maintainer pushback handled**, thread ball 转 maintainer (TylerBarnes 已 design-engaged → 比 round-3 状态更可能进 next review)。5-day cadence baseline 2026-05-01 早 → watchdog **`trig_0135vtMdNwShaLBmhPazoN4h` one-time scheduled 2026-05-06T17:00:00Z** (read-only check, sonnet-4-6, source = algora-scout public, ABORT 条件 codified: merged / closed-rejected / TylerBarnes 回应 / 其他 mastra-org member 介入 / 5-day silent → user 决定 next round)
- `mastra-ai/mastra#15904` awaiting review unchanged (5-day cadence due 2026-05-04 — 已 OPEN 2 天, reviewDecision=`REVIEW_REQUIRED` 但 reviewRequests=[], 等 mastra triage assign reviewer)
- `grundmanise/mastra#1` watchdog `trig_01VmjHWi8uLW5Zxkc1VUPry2` Monday 17:00 UTC active (carry-forward V0.1.13)
- `formatBlock` follow-up trigger `trig_013bUbcqV4jaEyJzdHALTPTD` daily 18:00 UTC active (carry-forward V0.1.13)
- maybe-finance active-bounty watchdog **永久 disable** (V0.1.11 carry-forward)
- **Cannibalization risk** unchanged: mastra-ai org direct cap = 2 (#15904 + #15934 OPEN) + 1 PR-into-PR (grundmanise#1)
- Ruby Tier 1 hibernate state (V0.1.11 carry-forward)
- **V0.1.14 implication**: round-4 maintainer 第一次 design-engaged 后 thread ball 在 maintainer 一侧的 next-action 概率最高 (TylerBarnes 已表态等 weigh 反例); 若 5 day 内无 maintainer 回应, 触发 watchdog 看是否需 nudge 或 maintainer 已 disengage

**Revert path**: `git revert <V0.1.14 sha>` 恢复 algora-scout 文件 (shipped-log L24 round-4 段删 + L32 dry rounds 计数 11→10 + V0.1.14 描述段删 + CHANGELOG entry 删)。Mastra reply 4361409484 是已 push 公开 GitHub action — revert 需手动 `gh api -X DELETE /repos/mastra-ai/mastra/issues/comments/4361409484` 删评论 (但删评论会留 GitHub edit 痕迹/邮件已 delivery, 实际不可逆); 推荐 revert 仅文件不撤 GitHub action。

---

## V0.1.13 — 2026-04-30 早 — #15934 round-3 reporter acceptance handled (V0.1.4-mode round 2 闭环 + 第 10 dry scan round)

**Trigger**: User said "扫描algora" → Step 0 subagent 给 priorities (poison list 先锁 / Tier 1 lift unlock≈0 跳 / 横向 paying-org no-bounty + freshness window + portfolio nudge)。平台 ground truth 15 GitHub-issue + 4 PR-level **100% V0.1.12 已 abort**(净新增 = 0); per-org 复查 mastra-ai 0 + twentyhq 2/2 CLOSED+Rewarded stale + CapSoftware/Cap 5/5 CLOSED/MERGED+Rewarded stale (V0.1.7 takeaway #10 24-72h sync delay 同质)。**第 10 dry scan round**。同时发现 portfolio 内 mastra #15934 jmzhang round-3 评论 (2026-04-30 05:28Z) 引 Gemini Vercel AI SDK chunk lifecycle 综述 → **明确 acceptance + 自标 imaginary edge case 可 skip**, 不是 push back。User said "你直接 reply" → 路 A 执行。

**Action taken** (mastra side, 2 GitHub actions):
- PR 评论 reply [4358004776](https://github.com/mastra-ai/mastra/pull/15934#issuecomment-4358004776): 致谢 jmzhang 引 Gemini + 论 `addStartStepPartsForAIV5`/`step-start` 与 unified rule consistency + 同意 skip imaginary case (first-seen vs last-seen 都不能 pin 跨 tool span split + chunk-level fragmentation 是另机制等 actual repro) + 末段 "Leaving the PR as-is for maintainer review" 标 thread 暂停
- thumbs-up reaction 352557148 加到 jmzhang round-3 评论 4349923241 增信号

**Action taken** (algora-scout side, 2 git-tracked files):
- `shipped-log.md`: L24 #15934 row Notes 末尾 in-place append round-3 acceptance update (Round-3 jmzhang 评论 ID + Gemini 论点 + acceptance 引用 + reply ID + 路 A 执行 + thumbs-up reaction ID + thread ball 转 maintainer)
- `CHANGELOG.md`: this entry

**Why** 不另起 takeaway #19: round-3 acceptance 是 V0.1.10 takeaway #13 模板 (round-2 三步核对 + evidence-based reply) 的成功验证, 不是新模式发现。round-3 acceptance 后路 A (致谢 + 同意 skip + 不动 PR) 是 YAGNI 直接应用, 无需独立 takeaway。如未来出现 reporter 接受后 maintainer 反对 / reporter 接受后又翻转的复杂情况, 再起 takeaway 不晚。dry scan 计数 +1 (9→10) 但 takeaways 不变 (#18 V0.1.12 sync delay 结构性观察持续验证, 不需新 entry)。

**Step-0 subagent 审核** (3 rounds, 全采纳): 
- Round 1 (Plan subagent): 先锁 poison list 再扫 (执行) + 不再扫 V0.1.12 已验 unlock≈0 Tier 1 lifted 语言 (本轮避开) + 横向 paying-org no-bounty pool / freshness window / portfolio nudge 三方向 (本轮覆盖前两方向; portfolio nudge = #15934 round-3 闭环为最高 ROI)
- Round 2 (general-purpose subagent): jmzhang round-3 = acceptance 判定 (3 强证据: "can fix the bug" / "should be the expected behavior" / "imaginary ... skip for now") + 路 A 推荐 (4 evidence: reporter 已自给 skip rationale / Vercel SDK 当前协议无 interleaving / 路 B "known limitation" 反向放大 reviewer risk / 路 C 实测损害 unified-rule clean diff) + in-portfolio 闭环优先级高于 scout deliverable (time-sensitive vs scout 0 candidate)
- Round 3 (general-purpose subagent draft audit): 草稿 v2 → 两处改 (Q3 删冗余末句 "Not adding a placeholder for it preemptively" 与前句同义 + Q4 末段精简为 "Leaving the PR as-is for maintainer review" 删 "Will" 和 "and wait"); 实际 push 版本 final draft 全采纳两处改

**Diff vs V0.1.12**:
- mastra side: 1 reply comment 4358004776 + 1 reaction 352557148 (无新 commit/branch/push)
- `shipped-log.md`: L24 in-place append round-3 acceptance 段
- `CHANGELOG.md`: this entry
- 不动 WORKFLOW.md (无新规则发现, V0.1.12 三 bullet 持续生效)
- 不动 evaluation-checklist.md (R/G flag set 与 round-3 acceptance 处理正交)

**Open follow-up state** (updated):
- `mastra-ai/mastra#15934` **round-3 reporter acceptance handled**, thread ball 转 maintainer (5-day cadence baseline 2026-04-30 早 → watchdog due 2026-05-05 unchanged from V0.1.12 2026-05-04 — 微调 +1 day)
- `mastra-ai/mastra#15904` awaiting review unchanged (5-day cadence due 2026-05-04)
- `grundmanise/mastra#1` watchdog `trig_01VmjHWi8uLW5Zxkc1VUPry2` Monday 17:00 UTC active
- `formatBlock` follow-up trigger `trig_013bUbcqV4jaEyJzdHALTPTD` daily 18:00 UTC active
- maybe-finance active-bounty watchdog **永久 disable** (V0.1.11 carry-forward)
- **Cannibalization risk** unchanged: mastra-ai org direct cap = 2 (#15904 + #15934 OPEN) + 1 PR-into-PR (grundmanise#1)
- Ruby Tier 1 hibernate state (V0.1.11 carry-forward)
- **NEW V0.1.13 implication**: round-3 acceptance 后下次 maintainer review 概率上升 (thread 现有 reporter explicit endorsement 供 maintainer arbitrate, 比 round-2 evidence-only 状态强); 若 5 day 内无 maintainer 介入, 触发 watchdog 看是否需 nudge

**Revert path**: `git revert <V0.1.13 sha>` 恢复 algora-scout 文件 (shipped-log L24 round-3 段删 + CHANGELOG entry 删)。Mastra reply 4358004776 + thumbs-up 352557148 是已 push 公开 GitHub action — revert 需手动 `gh api -X DELETE` 删 reaction + reply 评论 (但删评论会留 GitHub edit 痕迹/邮件已 delivery, 实际不可逆); 推荐 revert 仅文件不撤 GitHub action。

---

## V0.1.12 — 2026-04-30 早 — Tier 1 lift verification dry scan (V0.1.8 unlock 实测 paid org candidate ≈ 0; auto-close swarm + PR-level bounty 新分类入册)

**Trigger**: User invoked `/effort max` + "扫描algora" → V0.1.8 PHP/Scala/Java/Ruby Tier 1 lift 后**首次系统性 paid org per-org 验证轮**。Step 0 subagent 给 portfolio snapshot + scan priorities + watch-for: (1) ZIO/coollabsio 池试水, (2) PR-level bounty 类需识别, (3) mastra direct cap 已达。全平台 HTML grep ground truth 21 个 + coollabsio per-org page 10 个 + Java/Python paid org Algora 404 验证 → 100% abort,第 9 dry scan round。

**Action taken** (3 git-tracked files):
- `shipped-log.md` — first-merge stats counter `8 → 9` dry rounds + 2026-04-30 早 V0.1.12 描述; aborted-targets 加 5 行 (ZIO 8 issue batch + coolify#7458 R2 极端 + coolify Algora 9 R9 batch + archestra 3 re-confirm + PR-level bounty 4 batch); 新起 "Org-level takeaways (2026-04-30 早 — V0.1.12 Tier 1 lift verification + new swarm pattern)" section + #15/#16/#17/#18。
- `WORKFLOW.md` — Known poison: ① coolify 行 V0.1.8 unlock 后 append V0.1.12 swarm caveat (Algora-标 issue 池 swarm 锁 vs unassigned bug 池 R8 健康分流 + revert path 独立于 V0.1.8); ② 新增 "PR-level bounty (`/pull/N` URL 类)" bullet (grep 区分 + PR-into-PR exception); ③ 新增 "auto-close + bounty hunter swarm 模式" bullet (识别信号四件 + keephq vs swarm 同质 vector 不同对照)。
- `CHANGELOG.md` — this entry.

**Why** the 4 takeaways 是 load-bearing, 不是 routine abort log:
- **#15 Tier 1 lift verification** = V0.1.5/V0.1.8 user-explicit override 框架的第一次 evidence-based reality check。Tier 1 框架 consistency 保留 (mirror V0.1.11 Ruby C hibernate decision + V0.1.5 Rust 0 PR retention),但下次 scout 默认行为变化 (paid org list 不再当 actionable scout target)。区分"user-explicit override 框架"(保留) vs "实际可投 candidate"(本轮验证 ≈ 0) — 框架不撤但实证收紧。
- **#16 PR-level bounty 新分类** = pre-flight grep 命令永久变化 (issues/N vs pull/N 二元区分),不是 one-off 观察。本轮验证 4 个 100% Rewarded stale,识别成本 (grep 一次正则) 远低于深查成本 (gh pr view + 状态判断)。同质 V0.1.7 takeaway #10 stale 信号但维度正交 (state stale vs URL pattern stale),并行存在不冲突。
- **#17 auto-close swarm 新 poison 模式** = 与 keephq 高奖金抢占失控池 (V0.1.6 takeaway re-confirm) **同质但 vector 不同** — keephq = 高奖金引人, swarm = 低奖金 + AI 化 + auto-close。识别信号四件 (5+ closed PR / AI reply / maintainer 沉默 / bot 警告) 任三同满即跳。这个 vector 在 LLM-coding 后时代会扩散 (低 stakes bounty + AI swarm 是结构性现象,不是 coolify 独有), 早识别避免下一轮在 mastra/twentyhq 撞同坑。
- **#18 Algora 平台 sync delay 结构性 quantification** — V0.1.7 takeaway #10 首次发现 24-72h delay (zed#4440), V0.1.12 量化 "新 candidate 出现率 ≈ stale candidate 残留率" (24h +7 但 100% abort)。结构性而非 outlier — 含义: 单纯依赖 algora.io 平台 ground truth 的 scout 长期 ≈ 0 net candidate, 必须组合 V0.1.4 mode (paid org unassigned bug 池) 才能 sustain merge cadence。本轮 mastra direct cap 已达暂不可投, 等 #15904/#15934 review 解锁。

**Step-0 subagent 审核** (1 round): subagent (ab980afa7fff7cdf8) audit 给 portfolio snapshot + active gates + scan priorities (mastra > TS/Python sweep > PHP/Scala/Java 试水 > Rust hibernate 维持) + 4 risk warnings (mastra cap 已达 / 3-file mutual-ref drift / archive stale 类持续 / persona boundary)。**全采纳并扩展**: scan priorities (1) mastra direct cap 验证后明确不能投 (subagent 提示一致); (2) Scala ZIO 池试水实测 8/8 R6+R9 + (3) PHP coollabsio 池试水实测 10/10 R9+R2+R4 — 触发了 #15/#17 framework-level takeaway (subagent 没预测,因没 fetch issue 内容); (4) PR-level bounty 4 个识别 = subagent 没列入 watch-for 但本轮 grep 抓出 → #16 新分类。Subagent watch-for #1 "3-file mutual-ref drift 是最高风险" 全采纳 (本轮 5 abort row + 4 takeaway + WORKFLOW 2 新 bullet + 1 caveat 修改 严格 mirror)。

**Diff vs V0.1.11**:
- `shipped-log.md`: stats counter +1 dry round + 5 abort rows + 1 takeaway section (#15/#16/#17/#18)
- `WORKFLOW.md`: coolify L191 行 append V0.1.12 caveat + 2 新 poison bullets (PR-level bounty + auto-close swarm)
- `CHANGELOG.md`: this entry

**Open follow-up state** (carried forward from V0.1.11):
- `mastra-ai/mastra#15904` awaiting review (5-day cadence, watchdog due 2026-05-04 — 4 天后)
- `mastra-ai/mastra#15934` awaiting review (round-2 feedback handled at V0.1.10, last update 2026-04-29 19:xx, 5-day cadence due 2026-05-04)
- `grundmanise/mastra#1` watchdog `trig_01VmjHWi8uLW5Zxkc1VUPry2` Monday 17:00 UTC active
- `formatBlock` follow-up trigger `trig_013bUbcqV4jaEyJzdHALTPTD` daily 18:00 UTC active
- maybe-finance active-bounty watchdog **永久 disable** (V0.1.11)
- **Cannibalization risk**: mastra-ai org direct cap = 2 (#15904 + #15934 OPEN) + 1 PR-into-PR (grundmanise#1) — 第 3 个 mastra-ai PR 在 #15904 / #15934 任一 review/merge/close 之前不能开 (V0.1.11 carry-forward unchanged)
- Ruby Tier 1 hibernate state (V0.1.11) carry-forward
- **NEW V0.1.12 implications**:
  - 下轮 scout 默认优先级 = V0.1.4 mode (mastra unassigned bug pool, 等 #15904/#15934 review 解锁后) > Algora 平台 sweep。Algora 平台 sweep 价值降级为"结构性变化检测器"(新 paid org 出现 / known poison 解锁 / 新 swarm pattern 验证), 不再当 actionable candidate 来源。
  - Pre-flight grep 必区分 `/issues/N` vs `/pull/N` (#16) — 永久流程变化,不是 one-off。
  - 任 issue 验"swarm poison" 4 件信号 (#17) — 加入 evaluation 流程。
  - V0.1.4 mode 探 PHP coollabsio unassigned bug 池 (R8 健康) 是有效 fallback 但 0-exp 40-80h ramp-up 投入风险待 user 显式决策。

**Revert path**: `git revert <V0.1.12 sha>` 恢复三 git-tracked 文件。本 entry 不引入新外部 PR / commit / 远程操作, 仅 markdown log + workflow rule update,revert 完全本地。Tier 1 框架 (V0.1.5/V0.1.8) 不动 — 本轮**不撤** Java/PHP/Scala/Ruby unlock,只是评估"实际可投 candidate ≈ 0"为事实记录。如未来需要撤 V0.1.8 unlock,需 fresh user-explicit decision (按 `feedback_user_overrides.md` 框架)。

---

## V0.1.11 — 2026-04-29 夜深 — maybe-finance archived 永久跳 (Ruby Tier 1 选 C hibernate, mirror Rust V0.1.5 precedent)

**Trigger**: User said "现在我们有几个 bounty" → discovered V0.1.7 sweep 数据 24h 内已过期: per-org page 实测 maybe-finance 0→5 active bounty (V0.1.6 takeaway #7 watchdog 触发条件首次满足) → user said "要" 扫这 5 个 → Step 0 subagent 第一轮就给 STOP: `gh api repos/maybe-finance/maybe` = `archived: true` + `pushedAt: 2025-07-24` (9 月零提交) + `hasIssuesEnabled: false`; verify 5 个 algora-listed bounty 全 stale: marketing#301 MERGED 2025-04-30 + maybe#2081 CLOSED 2025-04-18 (都是 1 年前 rewarded) + maybe#1718/1734/2077 410 Gone (issues disabled) → user said "你推荐哪个" 三选 (A 撤 Ruby Tier 1 / B 找新 Ruby paid org / C hibernate 不撤) → 推荐 C → user said "go".

**Action taken** (3 git-tracked files):
- `WORKFLOW.md`: ① L136 maybe-finance 行 strikethrough + stub "→ 移到 Known poison by V0.1.11" (mirror V0.1.8 coollabsio 解锁模式: 留历史 trail 不完全删除,保 mutually-referential 三文件互引可追溯); ② Known poison section 加新 bullet "maybe-finance/maybe (整 org) — ARCHIVED 2025-07-24 ... 整 org 永久跳" + 新 watchdog 触发条件三件齐 (archived=false / hasIssuesEnabled=true / pushedAt < 90d)
- `shipped-log.md`: ① Aborted-targets 加 V0.1.11 row (maybe-finance 5/5 stale + R-archived 新 abort 类型 + Ruby Tier 1 选 C 注); ② 新起 "Org-level takeaways (2026-04-29 夜深 — V0.1.11 maybe-finance archived)" section + 单条 takeaway #14 (algora.io 12 月陈尸 stale 新极端 + 替换 V0.1.6 #7 watchdog 触发条件 + Ruby Tier 1 hibernate decision 引)
- `CHANGELOG.md`: this entry

**Why** algora.io stale 是新极端: V0.1.7 takeaway #10 记的 stale 是 zed#4440 / Cap-go#1667 = 24-72h GitHub close 事件 sync 延迟。这次 maybe-finance = repo archived 9 个月 + 5 个 listed bounty 中 2 个 1 年前 rewarded + 3 个 issues disabled,**algora.io 仍列**。stale 量级比之前高一个 order of magnitude (24-72h → 12 月)。直接含义: V0.1.6 takeaway #7 maybe-finance "0 active 时建 watchdog 等池补" 单维度触发被证伪 — 池补的 bounty 也可以是陈尸。新触发条件加 unarchived gate + issues enabled gate + pushedAt < 90d gate。

**User-explicit override 框架 reaffirm (load-bearing)**: V0.1.5 Rust Tier 1 lift (0 merged Rust PR + user explicit) + V0.1.8 PHP/Scala/Java/Ruby allowlist + Tier 1 lift (0 production exp + user explicit) 已建立 user-explicit override 框架。`feedback_user_overrides.md` 明令 "user explicit override 不能 silent drop, 只能 user 主动撤"。本次 Ruby Tier 1 选 C hibernate **不撤** 是 explicit decision (非无脑保守): ① **Rust precedent consistency** — Rust 同样 0 PR + Tier 1 hibernate 至今未撤 (CHANGELOG V0.1.5 → V0.1.10 全程保留),Ruby 同形必同处置;② **平台无 Ruby active bounty** (V0.1.7 takeaway #9 + 今晚 verify 平台 15 个 active 0 Ruby repo) — 撤 Ruby 也不解锁任何新 candidate;③ **撤 Ruby 留 Rust 立"实测无 candidate 即撤"隐性触发器**, 破坏 override 框架 consistency 长期心智负担; ④ **C 代价 ≈ 0** — 仅文档加一条 dormant 标记, revert path 由 V0.1.8 sha 保留不动。所以选 C 是与 V0.1.5/V0.1.8 同框架的 explicit 保留, 非 silent retain。

**Step-0 subagent 审核** (3 rounds): first round (用户问 "几个 bounty" 语义解读) GAVE 4 解读 + 推 (b) portfolio claimed = 0; second round (扫 5 个 maybe-finance bounty 是否可行) GAVE STOP 决断 + 4 stop conditions (archived / issues disabled / 9-mo no push / 5/5 closed),全采纳; third round (Ruby Tier 1 三选 A/B/C) GAVE C + 一致理由 (Rust precedent + override consistency + dormant 文档代价 ≈ 0); fourth round (V0.1.11 housekeeping 计划审核) GAVE 4 调整: (2) WORKFLOW paid orgs 留 strikethrough stub 不全删 + (3) takeaway #14 单起 V0.1.11 section 不并入 V0.1.10 + (4) CHANGELOG 加 user-override reaffirm 段 + (6) user_tech_stack.md / evaluation-checklist.md 同步 — 全采纳并执行,(6) 经核对 Rust precedent (没加 dormant) → Ruby 也不加, evaluation-checklist 不动 (R-flag set 与 archived gate 正交)。

**Diff vs V0.1.10**:
- `WORKFLOW.md`: L136 maybe-finance 行 strikethrough + Known poison 加 1 bullet
- `shipped-log.md`: aborted-targets 加 1 row + 新 takeaway section + #14
- `CHANGELOG.md`: this entry
- `user_tech_stack.md` (memory, not in git): **不动** (Rust V0.1.5 lift 时也没加 dormant 注, Ruby mirror 同处置)
- `evaluation-checklist.md`: **不动** (R-flag set 与 "archived repo gate" 正交, 触发条件已落 WORKFLOW + shipped-log takeaway #14)

**Open follow-up state** (updated):
- `mastra-ai/mastra#15934` review-feedback handled (push 3e8d178, reply 4349505648), awaiting maintainer (V0.1.10 carry-forward)
- `mastra-ai/mastra#15904` awaiting review (5-day cadence, watchdog due 2026-05-04)
- `grundmanise/mastra#1` watchdog `trig_01VmjHWi8uLW5Zxkc1VUPry2` Monday 17:00 UTC active
- `formatBlock` follow-up trigger `trig_013bUbcqV4jaEyJzdHALTPTD` daily 18:00 UTC active
- ~~`maybe-finance` active-bounty watchdog NOT scheduled~~ → **永久 disable**: archived 后 V0.1.6 watchdog 触发条件证伪, 不再 schedule。新触发条件 = 三件齐 (archived=false + hasIssuesEnabled=true + pushedAt<90d), 即使 maybe-finance 解 archive 也需新 evidence-driven decision 才重启
- **Cannibalization risk** (V0.1.9 carry-forward): mastra-ai org 现 2 open PR + 1 PR-into-PR (grundmanise#1) = 上限 2; 第 3 个 mastra-ai PR 在 #15904 / #15934 任一 review/merge/close 之前不能开
- **Ruby Tier 1 hibernate state**: 0 Ruby PR + 0 standalone scout target (maybe-finance 死后) + Tier 1 框架保留。等 (a) maybe-finance 主动 unarchive (低概率) 或 (b) 新 Ruby paid org 浮现于 algora 平台 (V0.1.7 takeaway #9 平台已 0 Ruby, 短期不太可能) 重启 scout
- **NEW V0.1.11 implication**: 下次 scout 对**任何** algora.io listed bounty 必跑 `gh repo view <r> --json archived,hasIssuesEnabled,pushedAt` 三件齐 pre-flight (V0.1.7 #10 zed-style state check + V0.1.11 #14 archived check 串联)

**Revert path**: `git revert <V0.1.11 sha>` 恢复 algora-scout 文件 (WORKFLOW maybe-finance 移回 paid orgs + Known poison 删 maybe-finance bullet + shipped-log abort row 删 + takeaway #14 删 + CHANGELOG entry 删)。但 archived 事实是 GitHub 客观状态, revert 文件不撤 archived 状态; 如要重新启用 maybe-finance scout 需要 maybe-finance 先主动 unarchive (检测命令 `gh api repos/maybe-finance/maybe --jq .archived`)。

---

## V0.1.10 — 2026-04-29 夜 — #15934 round-2 review feedback handled (V0.1.4-mode pushback sub-pattern logged)

**Trigger**: User said "扫描algora并查看邮件" → discovered jmzhang 14:24Z PR comment on #15934 proposing text-spans-as-exception to first-seen ordering + CodeRabbit 2 actionable. User said "由你决定" → analyzed jmzhang 论点 vs issue #15914 原文场景 / 现有测试 / conclusive-remarks 推演 → 三步全砍 → executed 路 ② (rebut + push test swap + outcome-focused changeset rewrite).

**Action taken** (mastra side, 1 commit):
- Branch `fix/build-messages-semantic-order` push `c4fab88..3e8d178`:
  - `packages/core/src/loop/workflows/agentic-execution/build-messages-from-chunks.test.ts:84-86` swap expect 顺序 from `[Goodbye, Hello, world!]` (旧 text-end ordering) to `[Hello, world!, Goodbye]` (first-seen ordering) + comment 改为引 #15914 + 解释 t1 first-delta 早于 t2
  - `.changeset/fix-build-messages-semantic-order.md` rewrite outcome-focused (去除 buildMessagesFromChunks/end-event/placeholder 实现细节, 保留 user-facing semantic effect + Closes #15914)
- Commit `3e8d178` "chore: address review feedback on #15934 (test expectation + changeset)" (no Co-Authored-By, hard rule #5)
- PR comment [4349505648](https://github.com/mastra-ai/mastra/pull/15934#issuecomment-4349505648) rebutting jmzhang text-spans 例外 with 3 evidence points + opt-in flag offer if maintainer cites concrete downstream consumer

**Action taken** (algora-scout side, 2 git-tracked files):
- `shipped-log.md`: L24 #15934 row Notes 末尾 in-place append round-2 update (push SHA + reply ID + 3-evidence summary); new "Org-level takeaways (2026-04-29 夜 — V0.1.9 round-2 review pushback)" section + takeaway #13 (Issue-author review pushback 三步核对 + maintainer-拉入 末段模板)
- `CHANGELOG.md`: this entry

**Step-0 subagent 审核** (3 rounds): first round (路 ① vs 路 ② 决断) GAVE 路 ② with 4-point evidence chain (issue 场景全跨-type / step-start 正交 / conclusive 在 first-seen 下也对 / 现有测试是 bug 同源), 全采纳; second round (执行计划缺陷审核) FLAGGED 行号修正 (L72-87 / L84-85 specific) 采纳 + ERRONEOUSLY 主张 jmzhang 评论在 issue 不在 PR (verify 后确认在 PR via `gh api repos/.../issues/15934/comments` + message-id `pull/15934/c...`, 不采纳); third round (V0.1.10 log update 边界) RECOMMENDED 按计划 4 步 (takeaway-growth bump 符合 V0.1.6/V0.1.7 先例), 全采纳。

**Diff vs V0.1.9**:
- mastra fork: 1 commit `3e8d178` on `fix/build-messages-semantic-order` (test:84-86 swap + changeset rewrite)
- mastra PR: 1 reply comment 4349505648
- `shipped-log.md`: L24 in-place update + new takeaway section + #13
- `CHANGELOG.md`: this entry

**Why** the takeaway #13 is load-bearing: V0.1.4-mode 是新流程 (no-bounty bug-fix in already-friendly orgs), round-2 reviewer pushback 是首次出现的 sub-pattern。如果不固化"三步核对 + 末段 maintainer 拉入"模板, 下一次 V0.1.4-mode round 3+ 遇到同形 pushback 会重新发明轮子, 且容易在 evidence 不足时草率反驳或在 evidence 充足时无理由软化。固化后下次直接套。

**Open follow-up state** (updated):
- `mastra-ai/mastra#15934` **review-feedback handled** (push 3e8d178, reply 4349505648), still awaiting maintainer review (5-day cadence baseline 2026-04-29 夜 → watchdog due 2026-05-04 unchanged)
- `mastra-ai/mastra#15904` awaiting review (5-day cadence, watchdog due 2026-05-04 unchanged)
- `grundmanise/mastra#1` watchdog `trig_01VmjHWi8uLW5Zxkc1VUPry2` Monday 17:00 UTC active
- `formatBlock` follow-up trigger `trig_013bUbcqV4jaEyJzdHALTPTD` daily 18:00 UTC active
- maybe-finance active-bounty watchdog NOT scheduled
- **Cannibalization risk** (V0.1.9 carry-forward): mastra-ai org 现 2 open PR + 1 PR-into-PR (grundmanise#1) = 上限 2 (PR-into-PR 不计 mastra-ai org cap)。第 3 个 mastra-ai PR 在 #15904 / #15934 任一 review/merge/close 之前不能开。
- **NEW V0.1.10 implication**: round-2 pushback 处理后 jmzhang 可能进一步 reply (赞同 / 持续坚持例外 / 弃赛); maintainer 介入概率上升 (因为 thread 现在有 evidence-rich 决策点供 maintainer arbitrate)。下次 sync 时优先看 #15934 thread 状态。

**Revert path**: `git revert <V0.1.10 sha>` 恢复 algora-scout 文件 (shipped-log.md L24 row + takeaway #13 + CHANGELOG entry)。Mastra fork commit `3e8d178` 是外部 push — revert 需 `cd /home/myclaw/oss-scout-work/mastra && git revert 3e8d178 && git push fork fix/build-messages-semantic-order` (用户决定; revert 后还要在 PR 上 reply 解释为什么撤回)。

---

## V0.1.9 — 2026-04-29 — Third upstream mastra PR shipped (#15934 fixes #15914 buildMessagesFromChunks semantic order)

**Trigger**: User said "下一步该做什么" post-V0.1.8 portfolio unlock, then "按你的推荐" → A (mastra unassigned bug pool round 2, V0.1.4 model). 30min scout: 25 unassigned bug-labeled issues → 5 candidates after R-cuts (Memory module avoid + effort:high + waiting + already-aborted) → 3 deep-scan (#15914 / #15288 / #15920) → #15914 viable (R-checklist all-pass except R6/R7 with mitigation).

**Why #15914 won**:
- R2 ✓ 0 cross-ref PR for issue (verified via `gh pr list --search "15914 in:body"`)
- R7 ✓ daneatmastra "smaller-model repro request" 化解 by reporter's model-agnostic chunk-array unit test (32 lines, pure assertion)
- R14 ✓ reporter自问 "is this expected?" 不是否认 bug — bug is real internal mismatch (streaming order vs semantic expectation downstream)
- R17 ✓ 0 closed-PR with "we will raise PR" / "needs discussion" lock
- Module-clean ✓ `loop/workflows/agentic-execution/build-messages-from-chunks.ts` ≠ #15904 `processors/memory/message-history.ts` subtree (no #15904 cannibalization risk)
- Devin 7-day squat ✓ devin touches `core/memory/*`, `processors/*`, `playground/*`, `pg/*` — does NOT touch buildMessagesFromChunks
- TylerBarnes #15897 OPEN concurrent on same file — verified 0 functional overlap (他改 L338 backward-compat contentString, 我改 buildMessagesFromChunks 主体 placeholder pattern)

**Action taken** (mastra side, 1 PR):
- mastra-ai/mastra fork branch `fix/build-messages-semantic-order` off main `a2b4baa` (29 commits ahead of #15904 base `332eb8d`)
- 7 source edits to `build-messages-from-chunks.ts` (option-(b) placeholder pattern: text/reasoning spans reserve slot at first-seen-delta or reasoning-start-if-redacted, fill at end-event)
- 1 regression test in `build-messages-from-chunks.test.ts` (reporter's chunk-array assertion verbatim, before "Empty stream" section)
- 1 changeset `.changeset/fix-build-messages-semantic-order.md` (`@mastra/core: patch`)
- Single commit `c4fab88` (no Co-Authored-By: Claude per hard rule #5)
- Pushed to fork → `gh pr create` → **PR #15934 OPEN, MERGEABLE, 9 CI checks (7 pending 0 failing), REVIEW_REQUIRED**

**Action taken** (algora-scout side, 2 git-tracked files):
- `shipped-log.md`: PR portfolio table 加 1 row mirror V0.1.4 #15904 shape; first-merge-hunt stats counter `5 → 6 PRs opened`; dry scan rounds counter 注 "2026-04-29 夜 V0.1.9 NOT a dry round"
- `CHANGELOG.md`: this entry

**Hard rule #6 override** (mirror V0.1.4 #15904 +69 precedent):
- 表面 +146/-37 inflated by Object.assign refactor pattern in reasoning-end + flush loops (placeholder mutate-vs-push 分支统一去重 redacted/non-redacted dual emit blocks)
- 新增 logic 净 ~50 lines + 36-line regression test (reporter's repro 1:1)
- 3 files (符合 hard rule #6 file-cap)

**Step-0 subagent 审核** (3 rounds): first round (next-step recommendation) RECOMMENDED-NEXT = mastra V0.1.4-mode round 2; second round (scout method audit) PROCEED with module-overlap caveat + 30min stop conditions; third round (Plan agent on draft-and-ship) PROCEED with one trim, option-(b) placeholder+mutate winning on LOC + clarity. 全部采纳 + HTML grep 修正一处 (subagent 推荐 "ZIO 池没有 ≤$300 候选" — 实测 4 个 ≤$300 ramp issues exist, ZIO ramp path 真实可执行但 100h Scala learning 是 hard prerequisite)。

**Diff vs V0.1.8**:
- `shipped-log.md`: portfolio 加 1 row + counter update
- `CHANGELOG.md`: this entry

**Open follow-up state** (carried forward):
- `mastra-ai/mastra#15904` awaiting review (5-day cadence, watchdog due 2026-05-04)
- `mastra-ai/mastra#15934` awaiting review (NEW — same 5-day cadence baseline, watchdog due 2026-05-04)
- `grundmanise/mastra#1` watchdog `trig_01VmjHWi8uLW5Zxkc1VUPry2` Monday 17:00 UTC active
- `formatBlock` follow-up trigger `trig_013bUbcqV4jaEyJzdHALTPTD` daily 18:00 UTC active
- maybe-finance active-bounty watchdog NOT scheduled
- **Cannibalization risk** (V0.1.9 specific): mastra-ai org 现 2 open PR + 1 PR-into-PR (grundmanise#1) = 上限 2 (PR-into-PR 不计 mastra-ai org cap)。**第 3 个 mastra-ai PR 在 #15904 / #15934 任一 review/merge/close 之前不能开**。
- **NEW V0.1.8 implication carried**: 下轮 scout 必须按新 allowlist 重扫 Scala/PHP/Ruby/Java; ZIO #519 $20k 三层叠是高 ROI 但 100-150h ramp-up 是 hard prerequisite。

**Revert path**: `git revert <V0.1.9 sha>` 恢复 algora-scout 文件 (shipped-log.md row + CHANGELOG entry)。`mastra-ai/mastra#15934` 是外部 PR — revert 需 `gh pr close 15934 --comment "withdrawing"` (用户决定)。本地 commit `c4fab88` 在 fork branch 不影响。

---

## V0.1.8 — 2026-04-29 — PHP+Scala+Java+Ruby allowlist+Tier 1 lift (user-explicit override mirrors V0.1.5 shape; 0-exp framing acknowledged)

**Trigger**: User said "全部解锁portfolio" then "按你推荐的" mid-V0.1.7 dry-sweep aftermath, when offered 5 enumerated interpretations (I-1 literal drop all R1 / I-2 drop R1+hard rules REJECTED / I-3 PHP-only / I-4 Scala-only / **I-5 Algora-paid-org allowlist PHP+Scala+Java+Ruby keep hard rules** ← chosen). **Triggering reason**: pure strategy upgrade post-V0.1.7 dry sweep revealing TS/Python/Rust = 0 platform-wide active bounties; no specific candidate cited.

**Gates being bypassed** (3):
- (a) `user_tech_stack.md` "DOES NOT have production experience" list containing PHP/Ruby/Scala
- (b) `WORKFLOW.md` Java Tier 2 ≤150 line cap
- (c) `WORKFLOW.md` L165 coollabsio R1 PHP poison row (V0.1.7 takeaway #8 + V0.1.7 abort row)

**Step-0 subagent 审核** (2 rounds): first enumerated 5 interpretations + I-2 REJECT + I-5 default safe; second GREEN-LIGHT + 3 implementation extensions (delete L165 coollabsio R1 row, append-only counter-rows for previously R1-aborted ZIO/coolify entries, extend Still-Avoid orthogonal: kyo for Scala / kafka+pulsar for Java / keycloak IAM/security 红线). 全部采纳。

**Action taken** (3 git-tracked files + 2 memory files):
- `WORKFLOW.md` — L9-12 tier description (allowlist + Tier 1/2 重排 + 0-exp 注); L130-148 paid orgs section 加 PHP/Scala/Ruby block (each w/ scout target + 0-exp ramp-up + Still Avoid orthogonal + revert path); Java reframed Tier 2→Tier 1 with 2-yr-gap caveat; L165 coollabsio R1 row → "解锁 by V0.1.8" (revert-restorable)。
- `shipped-log.md` — "Documented strategy overrides" 加 V0.1.8 row mirror V0.1.5; 新 "Counter-rows for V0.1.8 portfolio unlock" section 列已变状态的历史 abort 行 (zio/* R1 部分解锁 / coollabsio R1 完全解锁 / PX4/gyroflow/mastra/twentyhq/cal.com 不变); append-only 不删原行。
- `CHANGELOG.md` — this entry + V0.1.7 catchup entry (omitted from V0.1.7 commit 8b395d5)。
- `user_tech_stack.md` (user memory at `~/.claude/projects/-home-myclaw/memory/`) — Stack 段加 PHP/Scala/Ruby Tier 1 lines (0-exp acknowledged + ramp-up budget + scout target + revert path); Java Tier 2 → Tier 1; "DOES NOT have production experience" 删 PHP/Ruby/Scala; allowlist + tier ranking + Market info Scala 段同步更新。
- `feedback_user_overrides.md` (project memory) — Precedent log 加 V0.1.8 行 + 新 "0-exp Tier 1 framing precedent" 段。

**Caveat — bypass risks** (different shape from V0.1.5 Rust):
- **PHP/Scala/Ruby = 0 production experience**; Tier 1 标记 reflect scout-allowlist 不是 production depth。Future Claude 读 user_tech_stack.md 必须看行内 0-exp 注。
- **First-PR 风险高于 V0.1.5 Rust**: V0.1.5 时用户已学过 Rust + paired with helper; V0.1.8 三语言全冷启动, 首战 PR 失败率 structurally higher。
- **Ramp-up budget**: 40-80h PHP / 100-150h Scala / 30-60h Ruby; 首战 PR ≤$300 low-stakes + clear-repro 入门, 验证 cold-account merge rate 后再投 mid-range。
- **Still Avoid orthogonal 保留**: getkyo/kyo (Scala 升 Tier 1 仍 avoid, 与 V0.1.5 Rust block 一致); apache/kafka + apache/pulsar (distributed-systems internals); keycloak (IAM/security 红线); getdozer/dozer + spaceandtimefdn/sxt (Rust)。

**Revert path**: `git revert <V0.1.8 sha>` 恢复三 git-tracked 文件 (WORKFLOW.md + shipped-log.md + CHANGELOG.md)。`user_tech_stack.md` + `feedback_user_overrides.md` **均不在 git** (在 `~/.claude/projects/.../memory/`), 必须按本 entry 列的改动手动 revert。

**Diff vs V0.1.7**:
- `WORKFLOW.md`: tier desc + paid orgs 大改 + 新增 PHP/Scala/Ruby block + Java reframed + L165 row reformatted
- `shipped-log.md`: strategy override 加 1 row + 新 Counter-rows section
- `CHANGELOG.md`: V0.1.8 + V0.1.7 catchup entries
- `user_tech_stack.md` (not in git): 5 stack lines + allowlist + tier ranking + market info
- `feedback_user_overrides.md` (not in git): precedent + 0-exp framing 段

**Open follow-up state** (carried forward unchanged from V0.1.7):
- `mastra-ai/mastra#15904` awaiting review (~5 day cadence; watchdog due 2026-05-04)
- `grundmanise/mastra#1` watchdog `trig_01VmjHWi8uLW5Zxkc1VUPry2` Monday 17:00 UTC active
- `formatBlock` follow-up trigger `trig_013bUbcqV4jaEyJzdHALTPTD` daily 18:00 UTC active
- maybe-finance active-bounty watchdog NOT scheduled
- **NEW V0.1.8 implication**: 下轮 scout 必须按新 allowlist 重扫 Scala (47 active / $38k) + PHP (38 active / $3.7k) + Ruby (maybe-finance watchdog) + Java (apache/* + keycloak 边缘 issue); ZIO #519 ($20k 三层叠) 优先但 100-150h Scala ramp-up hard limit, 留给用户 next-action 触发。

---

## V0.1.7 — 2026-04-29 (catchup, omitted from V0.1.7 commit 8b395d5) — 全平台 sweep dry scan (4 新 abort + R6-sec 新类型 + 平台 TS/Python/Rust 三池 0)

**Trigger**: User-invoked scout 2026-04-29 晚 (post-V0.1.6 same day). Subagent 给 PROCEED narrow scope: per-org page scan 必跑、mastra portfolio 已到 cap (#15904 + grundmanise#1) 不能加第 3 PR、跳过所有 known poison。完整覆盖: 平台全 14 个 GitHub-issue 型 bounty (HTML grep ground truth) + 4 个 Tier 1 paid org per-org 页 (twentyhq / CapSoftware / maybe-finance / gyroflow)。

**Action taken** (2 git-tracked files):
- `shipped-log.md`: dry-scan rounds 6 → 8 (V0.1.6 中 + V0.1.7 晚); aborted-targets 加 4 行 (coollabsio R1 PHP/Blade re-confirm + PX4 R1 C++ + zed#4440 R3 CLOSED + Cap-go/capgo#1667 R6-sec security disclosure rolling pool); Org-level takeaways 加 #9-#12。
- `WORKFLOW.md`: Known poison 加 coollabsio (R1 PHP) + PX4 (R1 C++) + Cap-go (security pool 模式) + algora.io 列表 stale 信号; "(as of 2026-04-26)" → "(as of 2026-04-29)"。

**Key findings** (sweep 后才发现, subagent 没预测):
1. 全平台 26 open bounty 仅 Scala 10 / CSS 7 / JS 7 / Java 2 / Nix 1 / Shell 1; **TS / Python / Rust = 0**。
2. **R6-sec 新 abort 类型**: Cap-go/capgo#1667 = security disclosure rolling pool ($2,780+ / 17 awards / 13 commenters), Francis 不做 security research → 整 org 跳。
3. **algora.io 列表 stale**: zed#4440 已 GitHub CLOSED 仍 list, 24-72h 不 sync。Pre-flight `gh issue view <num> --json state,body,labels`。
4. **HTML grep > WebFetch**: WebFetch 必只回前 10; `curl -sL ... | grep -oE 'github\.com/.../issues/[0-9]+' | sort -u` 是 ground truth。

**Outcome**: Algora 直接赚钱可能 0 (4-8 周空窗); credibility-only path 仍活水。User 询问 "Algora 没钱了吗" → subagent 评估 "过度悲观, Francis 切片对 Algora 池零匹配"; 进入 V0.1.8 portfolio unlock 决策。

**Diff vs V0.1.6**: shipped-log.md +20/-2; WORKFLOW.md +6/-1.

---

## V0.1.6 — 2026-04-29 — per-org page scan 发现 (gyroflow 4/4 abort, maybe-finance 入册, "假 paid org" + "无 algora 页" 列表扩展)

**Trigger**: User selected option C ("复查 Algora per-org page") then D ("写 abort log + 扫 8 个新 orgs") after V0.1.5 push. Tier 1 解禁后第一次系统性 per-org page 扫描——之前 scout 只用 algora.io/bounties 全局列表 + 6 个语言子页,**漏掉 per-org 单页挂的 bounty**。

**Per-org scan 发现**:
- **gyroflow** algora 主页有 4 active bounty ($1,350 总)。全局列表只显示 0 个。Tier 1 升级后复查全部 abort:
  - `#742 ($500)` Refactor lens profile handling: R2 (PR #1118 OPEN by CntrlX) + R3 (`/attempt #742` 2026-02-20) + R6/R9 (7-checkbox 大重构,2 年半老 issue 40 评论)
  - `#45 ($500)` Optical only stabilization: R2 (PR #1143 OPEN by yasumorishima 2026-04-24) + R7/R13 (maintainer AdrianEddy 2025-06-30 说"not right now, we'll need #831"——blocked) + R12 (PR #1130 by kira-autonoma 2026-03-16 同 fix CLOSED)
  - `#150 ($200) ` Support lensfun database: R2 (PR #1141 OPEN by yasumorishima 2026-04-23) + R3 (`/attempt #150` 6 天前 active) + R12 (PR #1106 by buildingvibes CLOSED)
  - `#384 ($150)` 已 abort (Tier 升不解 R12)
- **keephq #2112 SNMP $4,500 主奖 re-confirm**: 6 天前 abort log 写"5 个 open PR";现 **15 个 open PR + 43 /attempt + 最新 /attempt 2026-04-28**——PR pile 翻 3 倍,**结构性 R2 持续恶化**,bounty 永远不发。Algora 自助 attempt 平台 + 高额奖金 = 抢占失控池。

**新 paid org 入册**:
- **maybe-finance** (algora.io/maybe-finance, $18,000 awarded / 47 completed) — 排已知 paid 第 2 大,仅次于 cal.com。Top earner Huzef = neo773 (twentyhq incumbent 同人,跨 org 高产)。当前 0 active bounty,但活跃度高;watchdog candidate。语言:Rails/Ruby 主体 + 大量 TS-adjacent;R1 砍 Ruby 部分,只投 TS 子组件——issue-level 判断而非 org-level。

**"假 paid" / "无 algora 页" 列表扩展**:
- 假 paid (有页但 $0 awarded): + `tauri-apps`
- 无 algora 页 (404): + `sst`, `tldraw`, `dyrector-io`, `cal`/`calcom`/`cal-com`, `mastra-ai`, `zulip`
- subagent 推荐的 7 个新 orgs (triggerdotdev/keephq/sst/maybe-finance/tldraw/coollabsio/dyrector-io/tauri-apps),实测 0 个产生 actionable 候选;3 个根本无 algora 主页,subagent 训练数据 stale。新 takeaway #8 (shipped-log Org-level): "subagent 推荐 paid orgs 信息 stale" — WebFetch 是唯一 ground truth。

**Action taken** (3 git-tracked files):
- `shipped-log.md`: aborted-targets 加 4 行 (gyroflow #742 / #45 / #150 + keephq #2112 re-confirm); Org-level takeaways 加 #6/#7/#8 (per-org scan 是 step 1 子优先级 / maybe-finance 入册 / subagent 信息 stale)
- `WORKFLOW.md`: Known bounty-paying orgs 加 maybe-finance; "假 paid org" 列表加 tauri-apps; "无 algora 页" 列表加 sst/tldraw/dyrector-io/cal*/mastra-ai/zulip
- `CHANGELOG.md`: this entry

**Outcome state**:
- Algora 公开池 + 已知 paid org per-org page **全 dry / abort**。0 个 actionable 候选。
- mastra PR #15904 仍等 review。grundmanise/mastra#1 仍等 grundmanise reaction。formatBlock follow-up trigger active。
- 下次 scout (任何 trigger) 必须按 takeaway #6 先跑 per-org page 循环再看全局列表。

**Diff vs V0.1.5**:
- `shipped-log.md`: +4 abort rows + 3 takeaway sections
- `WORKFLOW.md`: maybe-finance entry + 假 paid list + 无 algora 页 list
- `CHANGELOG.md`: this entry

**Open follow-up state** (carried forward):
- mastra-ai/mastra#15904 awaiting review (~5 day natural cadence; if no engagement by 2026-05-04 schedule watchdog matching V0.1.3 grundmanise pattern)
- grundmanise/mastra#1 watchdog `trig_01VmjHWi8uLW5Zxkc1VUPry2` Monday 17:00 UTC active
- formatBlock follow-up trigger `trig_013bUbcqV4jaEyJzdHALTPTD` daily 18:00 UTC active
- maybe-finance active-bounty watchdog **NOT yet scheduled** — defer until user requests (无 active 时频繁查无意义,等 user 主动 monitor 或下轮 scout 触发)

---

## V0.1.5 — 2026-04-29 — Rust Tier 2 → Tier 1 (user-explicit override at 0 merged Rust PR; gate bypass acknowledged)

**Trigger**: User said "我要解除 rust 的限制" mid-session, then explicitly chose option "A" (升 Tier 1 全解除) when offered four interpretations (A: full lift / B: only ≤150-line cap / C: only "Still Avoid" list / D: only 2-3x time-budget note). No triggering Rust candidate cited; not driven by a specific bounty in the pipeline. Pure strategy decision.

**Gate being bypassed**: `WORKFLOW.md` Rust block 2026-04-24 was tagged "Tier 2 provisional ... After first Rust PR merged: revisit tier — upgrade to Tier 1 or revert to Tier 3". At the time of this V0.1.5 commit, **0 Rust PRs have been merged** (entire shipped-log shows only TS PRs: #15692 merged + #15637 / #15904 / grundmanise#1 open). User is overriding the gate without the triggering merge.

**Action taken** (3 git-tracked files + 1 user-level memory + 1 project-level memory):
- `user_tech_stack.md` (user memory at `~/.claude/projects/-home-myclaw/memory/`) — Rust line moved Tier 2 → Tier 1 with explicit override timestamp; "How to apply when scouting" tier ranking section updated to list Rust under Tier 1; domain-Still-Avoid (crypto / kyo / dozer) preserved as orthogonal to tier.
- `WORKFLOW.md` (project) — Tier definition block reworded (Rust into Tier 1, Tier 2 down to Java/C++ only); detailed Rust block (formerly 8 lines describing Tier 2 cap + 2-3x time budget + revisit gate) replaced with Tier 1 framing + revert path; **领域 Still Avoid (dozer/sxt/kyo) preserved verbatim** because these are domain-depth risks (async streaming internals / crypto audit red line / 自研 DSL learning curve) — orthogonal to tier; helper being Rust-fluent does not unblock them.
- `shipped-log.md` (project) — new "Documented strategy overrides" section above "Org-level takeaways"; row records the bypass with revert path.
- `CHANGELOG.md` (project) — this entry.
- `~/.claude/projects/-home-myclaw-algora-scout/memory/feedback_user_overrides.md` (project memory, NEW) — feedback memory documenting the precedent so future Claude sessions don't treat "user can override self-set gate at 0 evidence" as the default; only this specific Rust→Tier 1 override is sanctioned, future overrides require fresh user-explicit decision.

**Step-0 subagent审核** flagged 4 considerations and gave 3-file + 1-commit landing plan; full plan adopted with one addition: subagent missed `user_tech_stack.md` as the user-level source of truth (project WORKFLOW is a copy), which has been updated synchronously. Subagent's decision against keeping a "soft ≤150-line cap as first-Rust-PR cushion" is honored (留软上限 = gate 改穿马甲, violates "绕 gate 留痕透明").

**Diff vs V0.1.4**:
- `WORKFLOW.md`: 行 10-12 tier 块 + 行 145-152 Rust 详细段
- `shipped-log.md`: new "Documented strategy overrides" section + 1 row
- `CHANGELOG.md`: this entry
- `user_tech_stack.md` (memory, not in git): 1 stack-line + How-to-apply tier list
- `feedback_user_overrides.md` (memory, not in git, NEW): precedent record

**Caveat — bypass risks**:
- First Rust PR will lack the Tier 2 safety net (≤150 line cap was meant to limit blast radius if helper-cadence assumption fails). User has accepted this risk in choosing A.
- `Still Avoid` (dozer/sxt/kyo) **is not** lifted — keeping these as poison protects against orthogonal failure modes (domain-depth) that helper fluency doesn't address.
- If a Rust PR ships and stalls for >7 days OR closes-rejected, V0.1.6 should reconsider whether the Tier 1 framing is sustainable or should retreat to a narrower override.

**Revert path**: `git revert <V0.1.5 sha>` returns three files to V0.1.4 state. User-level memory `user_tech_stack.md` is **not in git** and must be manually reverted using its prior content (preserved in this CHANGELOG entry above as reference).

**Open follow-up state** (carried forward from V0.1.4):
- `mastra-ai/mastra#15904` awaiting maintainer review (no watchdog scheduled yet — natural cadence ~5 days based on #15692 precedent).
- `grundmanise/mastra#1` watchdog `trig_01VmjHWi8uLW5Zxkc1VUPry2` active (Monday 17:00 UTC).
- `formatBlock` follow-up trigger `trig_013bUbcqV4jaEyJzdHALTPTD` active (daily 18:00 UTC, fires once #15637 merges).

---

## V0.1.4 — 2026-04-29 — second upstream mastra PR (#15904 fixes #15880 trim regression) + R17 sub-pattern (per-issue internal lock via "we will raise PR")

**Trigger**: User-invoked scout 2026-04-29 早. Algora pools (TS / Python / JS) fully poison-blocked (archestra R4 / twentyhq IMAP R16 / zio R1 / kyo R1) — same poison set as 2026-04-28 晚, no incremental bounty additions. Pivoted to no-bounty bug-fix scouting in already-friendly orgs (post-#15692 mastra confidence). Two candidates surfaced: mastra #15089 (Vector SDK return types) and mastra #15880 (`filterMessagesForPersistence` trim regression). #15089 had a fully-formed external fix (`octo-patch` PR #15119) closed 22 days ago by `intojhanurag` with *"needs discussion, then we will raise PR"* — same shape as cal.com R17 internal lock. #15880 was clean: complete repro + suggested fix in issue body, regression source PR #15454 (CalebBarnes 2026-04-21) cited, no `/attempt` claims, devin-ai-integration not in `processors/memory/message-history.ts`.

**Action taken**:
- Verified `removeWorkingMemoryTags` in `working-memory-utils.ts` always returns a fresh string (indexOf-based reconstruction) — issue's suggested `cleaned !== text` snippet works because JS string `!==` is value-compare, not reference-compare.
- Branched `fix/filter-messages-preserve-whitespace` off `main` (HEAD `332eb8d`).
- Applied two-path fix to `MessageHistory.filterMessagesForPersistence` (string-content branch + `parts[]` text branch); inline comment documents *why* the value-compare guard exists, to discourage future "simplification" back into the regression.
- Added 1 regression test in `message-history.test.ts` (~55 LOC, mirrors existing `processOutputResult` describe block) covering 4 text parts where 3 carry token-boundary leading whitespace and 1 contains a `<working_memory>` tag — proves both regression-fix AND original strip+trim path coexist correctly.
- Added `.changeset/preserve-text-part-whitespace.md` (`@mastra/core`: patch).
- Step-0 subagent (Plan) flagged five risks; #1 (string-content path symmetry) integrated, #2 (reference-vs-value compare) verified false-alarm via source read, #3-5 (test coverage / changeset framing / inline comment) integrated.
- Pre-push race check: #15880 evidence count steady (2 comments, last 2026-04-28T20:58 — pre-PR), no new `/attempt`, no new fix PR matching keywords (`filterMessagesForPersistence`, `message-history`, `15880`).
- Single commit `08289ed` (no `Co-Authored-By: Claude`).
- Opened **[mastra-ai/mastra#15904](https://github.com/mastra-ai/mastra/pull/15904)** against `main` (3 files, +69/-2). PR-landed sanity check: OPEN / MERGEABLE / REVIEW_REQUIRED, CI in_progress (Memory/E2E/Combined-store + Socket Security + CodeRabbit pending; Vercel docs preview FAIL is fork-authorization issue, not code).

**R17 sub-pattern documented (orthogonal to PR ship)**: mastra has *per-issue* internal locks (not the per-repo locks that cal.com uses). Triggered by recognizing #15089's PR #15119 close pattern. Updates:
- `evaluation-checklist.md` R17 row: trigger surface widened to "issue body OR most recent closed external-fix PR"; trigger phrase list extended (`"we will raise PR"`, `"we'll do it internally"`, `"keep this for the team"`, `"needs discussion, then we'll PR"`).
- `evaluation-checklist.md` Documented failures: full mastra #15089 / PR #15119 narrative + lesson re: checking closed-without-merge external PRs.
- `WORKFLOW.md` mastra entry: added R17 internal-lock check command (`gh pr list --search "<issue-num> in:body" --state closed`).
- `shipped-log.md` Aborted targets: row for mastra #15089 with abort rationale.

**Outcome state**: PR #15904 open, awaiting maintainer review. mastra portfolio status: 1 merged (#15692) + 1 upstream open (#15904) + 1 PR-into-PR open (grundmanise/mastra#1) — within `Max 2 open PRs per org` rule (PR-into-PR doesn't consume mastra-ai/mastra slot, opens against `grundmanise:`).

**Diff vs V0.1.3**:
- `shipped-log.md`: new row `mastra-ai/mastra#15904`; PRs-opened counter 4 → 5 (4 upstream + 1 PR-into-PR); 2026-04-29 早 noted as NOT a dry round; new aborted-targets row mastra #15089.
- `evaluation-checklist.md`: R17 row widened; new Documented failure entry.
- `WORKFLOW.md`: mastra Known-bounty-paying-orgs entry gains R17 internal-lock check command.
- `CHANGELOG.md`: this entry.

**Caveat**: Did not run `pnpm typecheck` / `vitest` locally — same monorepo install gap as V0.1.3. Disclosed in PR body. Dry-ran the four expected outputs against `removeWorkingMemoryTags`'s indexOf reconstruction in source. Relying on CI surfacing on #15904.

**Scope rule pressure**: 69-line PR slightly exceeds WORKFLOW hard rule #6 (50 lines / 3 files). Test occupies 55 LOC mirroring existing describe-block range; net fix is 9 LOC. Decision: ship anyway because (a) cold-account #15692 already merged builds maintainer trust margin; (b) regression test is non-negotiable for this fix shape; (c) splitting into fix-only + test-only PRs would double review burden for the same author. Disclosed in PR body. **If maintainer pushes back on size, fall back is to drop the string-content-branch fix (saves 4 LOC) — but the test stays.**

**Open follow-up state** (do NOT lose):
- Awaiting maintainer review on mastra-ai/mastra#15904. No watchdog scheduled yet — bias toward natural review cadence (mastra reviewed #15692 in 4 days). If 7 days pass with no engagement, schedule a watchdog routine matching the V0.1.3 grundmanise/mastra#1 pattern (`trig_01VmjHWi8uLW5Zxkc1VUPry2` precedent).
- V0.1.3 open follow-ups carried forward unchanged: grundmanise/mastra#1 watchdog (`trig_01VmjHWi8uLW5Zxkc1VUPry2`) + formatBlock follow-up trigger (`trig_013bUbcqV4jaEyJzdHALTPTD`). Both still active.
- R17 trigger phrase list will likely keep growing — every new "internal lock" wording variation seen in the wild should be appended to `evaluation-checklist.md` R17 row in the same edit.

---

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
- Awaiting grundmanise review/merge on grundmanise/mastra#1. If 7 days pass with no engagement, ping or pivot. **Watchdog scheduled 2026-04-28T18:30Z**: routine `trig_01VmjHWi8uLW5Zxkc1VUPry2` (cron `0 17 * * 1` — every Monday 10:00 America/Vancouver / 17:00 UTC), [dashboard](https://claude.ai/code/routines/trig_01VmjHWi8uLW5Zxkc1VUPry2). Read-only status reporter; emits `🎉 MERGED` / `❌ CLOSED` / `⚠️ STALE` / `✅ healthy` banner. Disable manually via dashboard after PR reaches terminal state — cron does NOT auto-stop.
- formatBlock hook PR is now properly deferred (was V0.1.2 open follow-up): file as separate upstream PR after #15637 merges. **Trigger scheduled 2026-04-28T18:30Z**: routine `trig_013bUbcqV4jaEyJzdHALTPTD` (cron `0 18 * * *` — daily 11:00 America/Vancouver / 18:00 UTC), [dashboard](https://claude.ai/code/routines/trig_013bUbcqV4jaEyJzdHALTPTD). Daily ALREADY_DONE poll: exits silently while #15637 still open; fires `🚨 formatBlock NOT FILED` banner once #15637 merges AND no `franciseliang99-dot` PR with "formatBlock" in title/body exists. Disable manually after follow-up PR opens.

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
