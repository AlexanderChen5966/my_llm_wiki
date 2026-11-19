---
title: "Agency Agents 教學：讓 Claude Code 與 Codex 自動切換 AI 專家角色"
source: "https://www.letswrite.tw/agency-agents/"
author:
  - "[[Let's Write]]"
published: 2026-08-12
created: 2026-11-11
description: "本文介紹 Agency Agents 工具與 Router Skill 設定，教你如何讓 Claude Code 與 Codex 根據開發需求自動切換前端、UI/UX、架構師等 AI 專家角色，大幅提升 AI Coding 效率與產出品質！"
tags:
  - "clippings"
status: "processed"
---
![Agency Agents 教學：讓 Claude Code 與 Codex 自動切換 AI 專家角色](https://www.letswrite.tw/_astro/agency-agents.YxPoypS2_Z1XppGM.webp)

Agency Agents 教學：讓 Claude Code 與 Codex 自動切換 AI 專家角色

## 本篇要解決的問題

現在工作上幾乎都用 Claude Code、Codex 來進行開發，只是常常會遇到以下狀況：

要讓 AI 做 UI，會希望它像 UI 設計師一樣。

要 Code Review，希望它變成 Code Reviewer。

下一個專案要處理 Docker，又希望它不要一直卡在前端的事情上。

雖然我們可以每次都在 Prompt 裡打一大串：

```text
你現在是一位資深前端工程師，
熟悉 Vue、React、Accessibility、Core Web Vitals……
```

再雖然我們可以把自己習慣的 Prompt 存起來複製貼上，但基本上人類都是懶惰的，能少打一個字就少打一個字，可以的話最好是 AI 能自己懂現在要扮演什麼樣的專業角色，這就是為什麼貓咪們看到人類都會覺得：

![愚蠢的人類](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/t8vr8lqjt42bkyutnh62.jpg)

*愚蠢的人類*

最近看到一個 GitHub 專案，發現可以解決上面很愛演的困擾： [Agency Agents](https://github.com/msitarzewski/agency-agents)

簡單來說，這專案就是：

> 幫 AI 準備好一大票不同領域的專家角色，需要誰時，就叫誰出來 ~~當奴才~~ 工作。

官方目前已經有 230+ 個不同領域的 Agent，而且除了 Claude Code，也支援 Codex、Cursor、GitHub Copilot、Gemini CLI …… 等工具。

本篇除了筆記 Agency Agents 的安裝方式，也會多做一步：

**建立一個 Router Skill，讓 Claude Code、Codex 自己判斷這次應該找哪個 Agent。**

這樣平常只要正常描述需求：

```text
幫我重新設計這個 Dashboard，完成後用 Vue 實作。
```

AI 就可以自己判斷要用 UI Designer，或是 Frontend Developer~~，或是鏟屎的~~。

---

## Agency Agents 是什麼？

Agency Agents 不是新的 AI 模型，也不是另一套 Claude Code 或 Codex。

它比較像是一套「 **AI 專家角色設定檔** 」。

每一個 Agent 裡都已經寫好角色、工作方式、核心任務、交付內容、檢查項目等設定。

官方對它的定位也是「specialized AI agent personalities」，每個角色除了專業領域之外，還會包含自己的工作流程與交付方式。

例如 Frontend Developer，就不是只有：

```text
You are a Frontend Developer.
```

然後就句點。

它裡面還會定義前端開發時要注意的東西，例如：

```text
Vue / React / Angular
Responsive Web Design
Accessibility
Core Web Vitals
Component architecture
Performance
Testing
```

所以原本我們跟 Codex 說：

```text
幫我做這個 Vue 頁面
```

裝完 Agency Agents 後，就可以直接指定要哪個角色，該角色會自動補足我們遺忘的專業度：

```text
Use the Frontend Developer agent to implement this Vue page.
```

---

## 安裝 Agency Agents App

官方做了一套 Agency Agents App，它是 Agency Agents 的管理工具，可以瀏覽有哪些角色，再把 Agent 安裝進我們使用的 Coding Agent。

### Windows

Windows 建議直接用 Agency Agents App。

到 Agency Agents App 的 [GitHub Release](https://github.com/msitarzewski/agency-agents-app/releases/latest) 下載 Windows Installer 後安裝即可。

目前官方提供 x64 與 ARM64 的 Windows installer；官方也有特別註明 Windows 版本目前尚未 Code Sign，所以如果跳出 Windows SmartScreen，可以從「其他資訊」繼續執行。

裝完開啟 Agency Agents。

接著就可以選想使用的 Agent，以及要安裝到哪一套工具。

### Mac

Mac 可以直接使用 Homebrew：

```shell
brew install --cask msitarzewski/agency-agents/agency-agents
```

這也是 Agency Agents 主專案目前提供的快速安裝方式。

裝完後開啟 Agency Agents App。

不論是 Windows 還是 Mac，Agency Agents App 的介面都長得相同：

![Agency Agents App 介面](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/t3jae9bgoem2goik5677.png)

*Agency Agents App 介面*

---

## 前端工程師可以先裝哪些 Agent？

Agency Agents 的角色很多，不建議看到什麼就全部裝，最後搞得像寶可夢一樣不全部收集完就睡不著。

以平常前端開發來說，可以先從這幾個開始：

| Agent | 適合工作 |
| --- | --- |
| Frontend Developer | Vue、React、JavaScript、CSS、RWD |
| UI Designer | UI、Dashboard、Design System、版面設計 |
| UX Researcher | UX、流程、資訊架構、易用性 |
| Software Architect | 系統架構、大型重構 |
| Code Reviewer | PR、Code Review、Bug、Regression |
| Senior Developer | 比較複雜的開發工作 |
| DevOps Automator | Docker、CI/CD、Jenkins、部署 |
| Minimal Change Engineer | 修 Bug，而且不要順手就重構整個宇宙 |

安裝角色時，就可以直接選擇要裝在哪些 AI 上：

![選擇裝在全域的哪個 AI 上](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/o3g7khbieonwx92zv0yx.png)

*選擇裝在全域的哪個 AI 上*

---

## 讓 Claude Code、Codex 自動選 Agent

Agency Agents 官方提供的是「角色」。

接下來這段則是自己另外加上的功能：

**Agency Agents Router。**

概念很簡單，我們再建立一個 Skill，專門負責判斷以下情境：

> 分析使用者需求 → 需要專家嗎？ 需要專家 → 找最適合的 Agency Agent。 不需要專家 → 自己直接做。

就不會看到「Vue」三個字，就一定叫 Frontend Developer。

所以 Router 要判斷的是： **這個工作有沒有必要使用專家。**

例如：

「把 margin-top 20px 改成 24px」→ 不用 Agent。

「重新設計整個後台，考慮 RWD 與無障礙」→ 用 Agent。

---

## Claude Code、Codex 建立 agency-agents Skill

Claude Code、Codex 的 Skill 放置位置不同。

### Claude Code

Mac：

終端機輸入：

```shell
open ~/.claude/skills
```

Windows：

開啟任一個資料夾，檔案路徑輸入：

```text
%USERPROFILE%.claudeskills
```

接著新增以下資料夾跟檔案：

```text
agency-agents
└── SKILL.md
```

### Codex

Mac：

```shell
open ~/.agents/skills
```

Windows：

```text
%USERPROFILE%.agentsskills
```

同樣建立：

```text
agency-agents
└── [SKILL.md](http://skill.md/)
```

接著 SKILL.md 的內容，Claude Code、Codex 有一點不同，可以各自複製貼上。

### Claude Code

[查看 GitHub 檔案](https://github.com/letswritetw/agency-agents-skill/blob/main/claude/SKILL.md)

### Codex

[查看 GitHub 檔案](https://github.com/letswritetw/agency-agents-skill/blob/main/codex/SKILL.md)

---

## 讓 Claude Code 自動使用 Router

Skill 建好後其實就可以用了，Claude Code 會依照 description 判斷什麼時候載入。

不過如果要讓 AI 更積極一點想到 Agency Agents Router，需要再到全域 `CLAUDE.md` 補一段規則。

打開 CLAUDE.md 檔的方式如下。

Mac：

終端機輸入：

```shell
open ~/.claude/CLAUDE.md
```

Windows：

開啟任一個資料夾，檔案路徑輸入：

```text
%USERPROFILE%.claudeCLAUDE.md
```

在打開的 CLAUDE.md 中加入：

```markdown
## Agency Agents

For development tasks that would materially benefit from specialist expertise, use the \`agency-agents\` skill to select and delegate to the most appropriate installed Agency Agent.

Prefer using the \`agency-agents\` skill automatically when the current task clearly matches its purpose. The user should not need to explicitly request the skill.

Use the minimum number of agents necessary.

Prefer direct execution for simple or mechanical tasks.

Do not delegate unless specialist expertise is likely to materially improve correctness, design quality, architecture, maintainability, reliability, performance, accessibility, or review quality.

Do not expand the requested scope merely because additional agents are available.

The user's original requirements, constraints, and project conventions always take priority over agent routing.
```

之後平常就不用輸入：

```text
Use agency-agents.
```

直接講需求即可。

---

## Codex 的 AGENTS.md

Codex 也可以在自己的 `AGENTS.md` 補上 Router 使用規則。

Mac：

終端機輸入：

```shell
open ~/.codex/AGENTS.md
```

Windows：

開啟任一個資料夾，檔案路徑輸入：

```text
%USERPROFILE%.codexAGENTS.md
```

在打開的 AGENTS.md 中加入：

```markdown
<!-- AGENCY_AGENTS_START -->

## Agency Agents

For software development tasks that would materially benefit from specialist expertise, use the \`agency-agents\` skill to select and delegate to the most appropriate installed custom agent.

When the current task clearly matches the \`agency-agents\` skill description, prefer implicit invocation instead of requiring the user to explicitly request \`$agency-agents\`.

Use the minimum number of agents necessary.

Prefer direct execution for simple or mechanical tasks.

Do not delegate unless specialist expertise is likely to materially improve correctness, design quality, architecture, maintainability, reliability, performance, accessibility, or review quality.

For tasks spanning multiple disciplines, let the \`agency-agents\` skill determine whether sequential delegation is useful.

Do not expand the requested scope merely because additional agents are available.

The user's original requirements, constraints, project conventions, and requested scope remain the source of truth.

<!-- AGENCY_AGENTS_END -->
```

---

## 總結

Agency Agents 裡面有一堆角色可以使用，目前 August 還在實驗中，看是不是在開發上會產生更精緻的結果。

記得幾年前 ChatGPT 剛出來時，網路上有很多人在分享怎麼寫 Prompt，很多開頭的第一句都是寫「請你扮演 XXX」。

這件事情現在已經不用了，因為已經可以讓 AI 自己選擇適合的角色來扮演。

這個 Agency Agents 專案目前有 143k 的星星數，看來懶惰的人類是很多的（誤），只好再放一次貓咪怎麼看我們人類的圖：

![愚蠢的人類](https://dev-to-uploads.s3.us-east-2.amazonaws.com/uploads/articles/t8vr8lqjt42bkyutnh62.jpg)

*愚蠢的人類*