# Issue evaluation checklist

Score each candidate issue against these rows. **Any red flag = drop.** Green-flag count is the rank tiebreaker.

## Red flags (any one → drop)

| # | Check | How |
|---|---|---|
| R1 | Language outside allowlist | Reject unless primary stack is Python / TS / JS / Java / C++ |
| R2 | Existing open PR addresses it | `gh api repos/<o>/<r>/issues/<N>/timeline` + `gh pr list --search "<kw>"` |
| R3 | `/attempt` comment < 48h old | Read issue comments; Algora treats this as soft claim |
| R4 | "Reserved for SE interview" / "Only for core team" / "invite only" | Scan issue body literally for these phrases |
| R5 | Assignee set | `gh api repos/<o>/<r>/issues/<N> --jq .assignees` non-empty |
| R6 | Scope > 50 lines or > 3 files | Read the fix hints in issue body; if vague, assume bigger |
| R7 | No repro steps, no stack trace, no suggested fix | Cold account + ambiguous scope = maintainer asks 5 questions, days lost |
| R8 | Repo's last external-contributor PR merge > 30 days ago | `gh pr list --state merged --limit 20` — median age check |
| R9 | Feature request (not a bug) | Title contains "add", "support", "enhance" without a concrete failure mode |
| R10 | Breaking API / schema change implied | Reject anything that requires migration, versioning, or deprecation dance |
| R11 | Requires live service / real account to test | e.g., needs a paid Proton account, real Slack workspace, production DB |
| R12 | Prior PR to this bounty closed by stale-bot after unanswered review request | Maintainer is silently refusing to engage; bounty is poisoned regardless of label |
| R13 | Issue body contains "finalized for this bounty" / "pending App Review" / "refrain from submitting" | Bounty is locked to an incumbent PR; reward will go to them when external approval arrives |
| R14 | LLM triage bot (Dosu, `@dosubot[bot]`, CodeRabbit summary, etc.) says "fixed in PR #X" / "was addressed in v1.Y.Z" | Read the comment thread — if a bot credibly claims the bug is already fixed, verify the referenced PR / tag and drop. Bot comments are not surfaced by `gh pr list --search`. |
| R15 | The "fix target" isn't a file in the repo (release/tag body edited via GitHub UI, pinned-issue PSA, org-level docs, wiki) | Before drafting, confirm the thing you'd need to edit lives in-tree. Release-tag bodies are manually maintained and not PR-able — there is no patch to open. Drop or comment on the issue instead. |
| R16 | Sibling issues in the same domain are all assigned to one active incumbent with merged PRs in that domain | Even when the flagship bounty issue shows no assignee, if `gh api search/issues` on the feature keyword returns 5+ recent issues all assigned to the same user and they've merged PRs in that area → bounty is de-facto claimed. Cold account loses. |
| R17 | Issue body OR most recent closed external-fix PR contains internal-lock phrasing | Phrases: `"needs to be handled internally"`, `"this will be handled by the core team"`, `"do not create a PR"`, `"we will raise PR"`, `"we'll do it internally"`, `"keep this for the team"`, `"needs discussion, then we'll PR"`. Same-shape regardless of wording. **Two surfaces** to check: (a) issue body literal grep (cal.com `[CAL-XXXX]` style); (b) most-recent closed-without-merge external PR for comments matching these phrases (mastra #15089 / PR #15119 — `intojhanurag` "It needs discussion , then we will raise PR" closed octo-patch's complete fix). Drop unconditionally. |

## Green flags (count them — higher is better)

| # | Check | Signal |
|---|---|---|
| G1 | Title names a specific error / exception / crash | Scope is bounded |
| G2 | Body has minimal repro snippet | Maintainer already triaged the root cause |
| G3 | Body proposes a fix (pseudo-diff or code block) | Scope + direction pre-agreed |
| G4 | `Closes #N` in PR will actually resolve it | Not a partial fix that needs follow-up |
| G5 | Similar-shape PR merged in this repo within last 14d | Maintainer review velocity + PR pattern exists |
| G6 | `good first issue` or `help wanted` label | Repo explicitly invites externals |
| G7 | Bug is a regression with commit SHA referenced | Revert-or-mirror approach is low-risk |
| G8 | Stack matches user's strongest language (Python / TS) | Lowest ramp-up cost |

## Scope estimate heuristic

| Estimate | Fix description |
|---|---|
| < 1h | Issue body has near-complete diff; change is 1 file, < 10 lines |
| 1-3h | Pattern clear from issue; 1-2 files; may need one new helper function |
| 3-8h | Needs reading 2-3 unfamiliar modules + writing a test |
| > 8h | **Drop.** Cold account shouldn't commit to scope > 1 evening; if PR stalls after 48h the claim goes stale |

## Repo-health sanity check (run once per target repo, not per issue)

```bash
# Merge velocity: median time from open to merge for last 20 PRs
gh pr list --repo <o>/<r> --state merged --limit 20 \
  --json createdAt,mergedAt \
  --jq 'map({d: (.mergedAt | fromdateiso8601) - (.createdAt | fromdateiso8601)}) | sort_by(.d) | .[10].d / 3600'
# < 48h is healthy; > 7 days = slow review = don't invest here first
```

## Documented failures (learn from these)

- **cal.com #5756 Proton Calendar**: 3.5 years old bounty, cal.com Tech Lead had already concluded integration was architecturally impossible (Proton has no CalDAV) in 2023-06. Failed R7 (no actionable fix path) retroactively. Lesson: when a bounty has been open > 12 months with 0 PRs, read the full comment thread before drafting — the answer is often "nobody shipped because it can't ship."
- **payload #16341 Node 24 loadEnv**: Had exact 3-line fix in body. But PR #16342 was opened the day before by another contributor. Failed R2. Lesson: R2 check is not optional even when the fix looks irresistible.
- **archestra-ai entire bounty batch**: Algora aggregation page hid the "Reserved for SE interview" label that was only on the GitHub issue body. Failed R4. Lesson: always read the raw GitHub issue body, not just the Algora summary.
- **activepieces #8135 (Canva MCP) / #8072 (Gmail MCP) — both locked**: Issue bodies contain `"PR #<X> has been finalized for this bounty and is currently pending App Review due to the required new scopes. Kindly refrain from submitting additional PRs for this bounty while we wait for the review to complete."` Aggregate Algora listings don't surface this lock. Lesson: **grep issue body for "finalized for this bounty" / "pending App Review" / "refrain from submitting"** — automatic drop if any match.
- **onyx #2281 (JSM connector) — maintainer silently abandoned**: "maintainer approved" label was set 2024-08-30. PR #3087 (413 lines, complete implementation) opened 2024-11-08, author pinged maintainer twice, maintainer never responded, PR stale-bot-closed 2025-08-25. 5+ /attempt comments since then, all ignored. Meanwhile other onyx bounties DID get merged (Gitbook, Outline, Coda, GitHub Pages) — so the silence is issue-specific, not repo-wide. Lesson: **before drafting, check the most recent closed-without-merge PR on the bounty issue.** If a complete implementation was closed by stale-bot with no maintainer response, the bounty is poisoned regardless of labels. Add red flag:
  - **R12 (new)**: ANY prior PR to this bounty closed by stale-bot after author requested review with no maintainer response → DROP.
- **better-auth #8904 (phoneNumber ms-vs-seconds)** — R14 first catch. Issue was real but `@dosubot[bot]` comment disclosed the fix had already landed in PR #8006 (2026-02-16). Cross-ref (`gh pr list --search`) didn't surface it because the fix PR title ("feat(phone-number): align API with email-otp plugin") did not contain the bug keywords. Lesson: **read the last 3-5 issue comments before adopting — LLM triage bots frequently pre-answer "already fixed in v1.X.Y."** Drop candidate without drafting.
- **chainlit #2887 (release-notes/data-layer mismatch)** — R15 first catch. Issue body offered two fix options: (a) correct the release notes, (b) wire the `modes` column into the data layer. Option (a) looked trivial but the release notes live in the GitHub tag body, edited manually in the UI — **not a file in the repo**. Option (b) crossed 2 repos (chainlit + unmaintained chainlit-datalayer) and 3+ files. Lesson: **before drafting, `gh api` the exact file the fix would touch.** If the "release notes", "changelog", "sticky post", "readme-front-matter" isn't backed by a file, there's no PR to open.
- **twentyhq/twenty IMAP $2500 (9 months open)** — R16 first catch. Listed as "open, no assignee" on Algora. Real state: `@neo773` has 10+ IMAP-domain issues assigned and multiple merged PRs in `fix: IMAP ...` space. The flagship bounty issue itself stays unassigned because Algora rewards the merged PR, not the claim — but every sibling issue confirms the incumbent. Lesson: **on any long-open bounty, run `gh api search/issues "<feature-keyword>"` to map incumbent activity. 5+ assigned-to-same-user siblings + merged PRs in-domain = de-facto claimed.**
- **mastra #15089 Vector SDK return-type mismatch** — R17 same-shape catch (sub-pattern: per-issue lock, not per-repo lock). Issue body looked clean (no "internal" / "do not PR" phrases). External contributor `octo-patch` shipped a complete fix in PR #15119 (4-7); a mastra internal (`intojhanurag`) replied *"It needs discussion , then we will raise PR"* and closed the PR. 22 days later no internal fix PR exists. **The same shape as cal.com `[CAL-XXXX]` "handled internally" — only the wording differs.** Lesson: **for any candidate, also check the most recent closed-without-merge PR.** If a mastra-internal voice closed an external fix with "we will raise PR" / "needs discussion, then we'll do it" wording, the issue is internally claimed regardless of issue-body label state. Add to R17 trigger phrases: `"we will raise"`, `"we'll do it internally"`, `"keep this for the team"`.
