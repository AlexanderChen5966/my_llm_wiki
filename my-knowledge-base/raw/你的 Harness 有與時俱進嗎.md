---
title: 你的 Harness 有與時俱進嗎
date: 2026-05-25
tags: [ai/agent, ai/claude]
source: Facebook 社群貼文
type: note
status: processed
---

你的 Harness 有與時俱進嗎， Anthropic 變動太快，如果沒有跟上，可能反而製造更多問題....😂 本週影響最大的指令就是 /code-review 指令（就是 2.1.147 把 /simplify 改名來的）— 在你本機 session 跑，回報 correctness bug，可加 --comment 把 findings 貼成 GitHub PR inline comment、可指定 effort（/code-review high）這個 code review 值得學習的 6 點
1. 🟣 Pre-existing 這個嚴重度層級。 它把「這個 PR 引入的 bug」和「codebase 本來就壞的 bug」分開。你現在的 Critical/Important/Minor 沒這個維度，結果是 reviewer 容易拿舊問題擋新 PR（scope creep）。加一層「pre-existing：記錄、不阻擋本 PR」很實用。
2. REVIEW.md 與 CLAUDE.md 分離。 Claude 把「review 專用校準」獨立成 repo 根目錄的 REVIEW.md，以最高優先注入每個 review agent；CLAUDE.md 只當一般 context。你的 repo 有很完整的 .claude/rules/，但那是給「寫 code 的 agent」看的。一個 REVIEW.md 可以集中放「本 repo 什麼算 Critical、Nit 上限、哪些路徑跳過、re-review 收斂規則」,讓 code-reviewer 等 4 個 subagent 開場先讀。附帶好處：哪天真的開託管服務，它會自動讀這個檔。
3. Re-review 收斂規則。 Claude 的建議寫法：「第一輪之後，suppress 新的 nit，只報 Important」——避免一行 fix 被風格意見拖到第 7 輪。你的 Step 5 re-review 沒有收斂條款，理論上會在新 Minor 上無限繞。值得加一句「re-review 只看 Critical/Important，新 Nit 記錄不阻擋」。
4. 顯式的 verification pass（過濾假陽性）。 Claude 的流程是「多 agent 平行 → 驗證候選對照真實 code 行為 → dedup → 排序」。我的 pr-review-cycle（簡易版）只有「彙整結果」，沒有去重也沒有反查驗證——4 個 agent 容易報重疊或臆測的 finding。pr-review-cycle-mob 的 R1/R2 debate 其實就是這個 verification，但簡易版缺。可以借「behavior 主張要附 file:line 引證，不能靠命名臆測」這條 verification bar（這跟我 rule 11「Cross-doc Cite 要 paste 原文」根本同一個精神）。
5. Nit 數量上限 + tally-first 摘要。 Claude 建議「最多報 5 個 nit，其餘用數量帶過」，且摘要開頭先給一行統計（2 factual, 4 style），沒 factual 問題就先講。你的 Minor 沒有上限、也沒 tally-first，reviewer 輸出容易讓人疲勞。
6. 偵測與「擋不擋」解耦。 Claude 的 check run 永遠 neutral，還吐一段 machine-readable 的 severity JSON 讓你自己的 CI 決定要不要 gate。像我的 skill 把「Critical = 擋 merge」寫死在 review 裡。對單人/小團隊寫死沒問題，但「偵測歸偵測、gating policy 歸 policy」這個分層思路，未來要做自動化時會省很多事。另一個很大的變更：/code-review 已經不做 cleanup-and-fix（舊 /simplify 會實際重構），所以之前的東西全都要修正了....😂未命名