---
title: Claude Code Git 工作流：commit / review / PR / merge conflict 全流程參與
date: 2026-05-22
tags: [ai/claude, dev/ci-cd, ai/workflow]
source: https://medium.com/@n913239/git-工作流-讓-claude-code-真正融入你的版本控制-b5511d80fc7d
type: article
status: processed
asset_value: 60
---

## T（核心論點）

多數人只讓 Claude 改程式碼，`git add`／`commit`／開 PR 全靠自己——**Claude 淪為聰明的程式碼產生器**。但 Claude 對 codebase 有完整上下文（知道改了什麼、為什麼改、影響哪裡），而 **git 流程裡到處都是 context**（改動原因、背景、影響）。把它帶進 git 流程，能做的遠不只是「幫你打 commit message」。

## B（適用場景）

**適用：** 日常 git 操作（commit／review／PR／解衝突／查歷史）；接手不熟悉的 repo 要理解歷史

**不適用 / 注意：**
- 讓 Claude 自己 commit 前**養成看一眼的習慣**——它通常會先顯示內容讓你確認，但別完全放手
- 敏感檔案（`.env`、credential）**要靠 `.gitignore` 把關**，不是靠 Claude 自覺
- 解 merge conflict 建議**先讓它解釋兩邊意圖，再決定怎麼合**，比直接叫它解安全

## R（七個應用點）

### 1. Commit Message——Claude 知道的比你多

你打 message 時只想著「剛剛做了什麼」；Claude 同時知道：改了哪些檔案、**改動的目的**（因為它幫你改的）、解決哪個問題、**有沒有潛在副作用**。

```
幫我寫 commit message
幫我寫 commit message，這次是修復登入頁在 iPad 橫向時 keyboard 擋住輸入框的問題  ← 給 context 品質更好
```

**`/commit` skill 的完整五步流程**：
① `git status` + `git diff` 顯示變更 → ② code review 找潛在問題 → ③ 確認要排除的檔案 → ④ 詢問工單號 → ⑤ 寫 message 並 commit

> 差別：`git commit -m "fix"` vs 說明清楚「什麼情境下出問題 + 用什麼方法解 + 為什麼這樣解」的多行 message

### 2. Code Review——push 前多一道安全網

```
幫我 review 這次的 git diff，找出潛在的 bug 或問題
```
Claude 讀 `git diff --staged` 從四個角度分析：

| 角度 | 檢查什麼 |
|---|---|
| **Correctness** | 邏輯對不對、有沒有 edge case 沒處理 |
| **Security** | SQL injection、XSS、不安全的資料處理 |
| **Performance** | N+1 query、不必要的 loop |
| **Readability** | 命名清不清楚、複雜邏輯要不要補 comment |

**針對性 review 更有效**：
```
這次的 diff 裡面，API token 有沒有可能被 log 出去？
review 這個 Core Data migration，有沒有資料遺失的風險？
```
配合 GitHub MCP 也能 review 別人的 PR：`幫我 review PR #142，特別看 authentication 的部分`

### 3. Branch 管理

```
我要開始做登入頁面的 iPad 修復，幫我建一個 branch
→ git checkout -b fix/login-ipad-keyboard-overlap（依 CLAUDE.md 的命名規則或常見慣例）

幫我列出所有已經 merge 進 main 的 branch，然後問我哪些可以刪掉
這些 branch 都做了什麼？幫我整理一下  ← 逐一跑 git log 分析後給概覽
```

### 4. PR 描述

好的 PR description 四要素：**解決什麼問題 / 怎麼解決（技術決策）/ 怎麼測試（test plan）/ 需特別注意的地方**。

配合 GitHub MCP 可直接開 PR：`幫我根據目前的 commits，開一個 PR 到 main，base branch 是 develop`

### 5. 查詢 Git 歷史（用對話代替指令）

```
幫我解釋 commit a3f9c2b 做了什麼，為什麼要這樣改   ← 用人話解釋，不是只列檔案清單
LoginView 的這個問題可能是什麼時候引入的？          ← 搭配 git log --all -p / git bisect 概念縮小範圍
過去這兩週 auth 模組改了什麼？幫我整理一下
這個檔案是誰寫的、為什麼這樣設計？從 git blame 找線索
```

### 6. 解 Merge Conflict——Claude 能理解兩邊的「意圖」

這是 Claude 特別有用的地方：它讀衝突標記後**分析兩邊各自在做什麼**再決定怎麼合。

> **比手動解衝突快，比只選一邊更正確**——特別適合「不是你寫的那邊，你根本不知道對方想做什麼」的情況。

⚠️ 但建議先問：`這個 conflict 是怎麼發生的？兩邊分別改了什麼？` 理解後再決定怎麼合，比直接叫它解更安全。

### 7. 特殊格式要求寫進 CLAUDE.md

```
記住：commit message 格式是 Conventional Commits，例如 feat(login): fix keyboard overlap on iPad
```
之後每次都會遵守。

## C（關鍵行動）

- [ ] 下次 commit 前先讓 Claude review diff（成本低、抓到問題的期望值高）
- [ ] 設定 `/commit` skill 走完整五步流程，取代手打 `git commit -m "fix"`
- [ ] commit message 格式規範（如 Conventional Commits）寫進 CLAUDE.md
- [ ] 確認 `.env`／credential 都在 `.gitignore`——**別靠 Claude 自覺**
- [ ] 解衝突時先問「兩邊分別在做什麼」，再決定怎麼合

## 原始摘要

MikeWang 的 Claude Code 系列連載第 6 篇（Medium，2026-05-22）。系列前幾篇談內部機制（CLAUDE.md、Hooks、Memory）與外部整合（MCP），本篇處理「Claude 改完程式碼之後呢？」。作者結語：**Claude 的強項是理解 context，而 git 工作流裡到處都是 context——讓它參與，而不是只讓它改程式碼。**

## 相關連結
- [[cursor-origin-git-forge]] — Cursor Origin：為 agent 節奏重新設計 git forge，對照本篇在傳統 git 流程上的做法
- [[ai-code-review-safeguards]]／[[code-review-graph]] — Code review 工具與護欄；本篇是**對話式 review**（review 前置於 commit），與工具化 review 互補
- [[claude-md-optimization-techniques]] — commit 格式等專案慣例該寫進 CLAUDE.md（屬「模型無法自行推測的專案特有前提」）
- [[mcp-stateless-architecture]] — GitHub MCP 是本篇「直接開 PR」「review PR #142」的前提
- [[claude-code-skill-survivors-20-picks]] — `/commit` 這類日常 skill 的選用參考
- [[obligations-as-code-tedium-is-stability]] — 「每條義務接可判定檢查」；本篇的 review 四角度可視為 commit 前的義務清單
