---
title: "gstack 安裝教學 — Claude Code 23 個技能一行指令裝好（2026）｜黃志弘"
source: "https://leoaido.com/gstack/"
author:
  - "[[Leo Huang (kanisleo328)]]"
published: 2026-03-31
created: 2026-05-14
description: "gstack 安裝教學：一行指令裝好 Claude Code 23 個技能 + 8 個工具。Y Combinator CEO Garry Tan 開源技能包，含安裝步驟、常用技能介紹與常見問題。"
tags:
  - "clippings"
status: processed
---
### 30 秒搞懂

**gstack = Claude Code 的技能擴充包**

[gstack](https://github.com/garrytan/gstack) 是 Y Combinator 現任 CEO **Garry Tan** 開源的專案。它幫 Claude Code 裝上 23 個專業技能和 8 個工具 — 從自動 QA 測試、AI Code Review 到一鍵發 PR，全部一行指令搞定。

想像一下：Claude Code 是一支新手機，gstack 就是一次幫你裝好 23 個最實用的 App。

58K+

GitHub Stars（截至 2026/3）

23

個技能

8

個工具

1 行

指令安裝

前置條件

### 開始之前確認這些都有

- Claude Code 已安裝
- Git 已安裝
- Bun v1.0+（沒裝的話下面有指令）

還沒裝 Bun？

### 先安裝 Bun

curl -fsSL https://bun.sh/install | bash

裝完後輸入 `bun --version` 確認有出現版本號就 OK。

核心指令

### 一行安裝 gstack

打開 Terminal，貼上這行：

git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack && cd ~/.claude/skills/gstack &&./setup

這行做了三件事：1. 從 GitHub 下載 gstack 2. 放到 Claude Code 的技能目錄 3. 執行安裝腳本。全自動，不需要其他操作。

驗證安裝

### 確認裝好了

打開 Claude Code，輸入：

/qa

如果 Claude Code 認得這個指令（不是顯示「找不到」），代表 gstack 安裝成功。

/qa

自動 QA 測試

給它網址，它自己打開瀏覽器找 bug，找到直接修。不用你手動測試每個按鈕、每個表單，AI 全自動跑一遍，發現問題就幫你修好。

/browse

給 AI 一雙眼睛

讓 AI 看到網頁畫面 — 截圖、點擊、填表單。以前 AI 只能看 code 看不到畫面，現在它能像真人一樣操作瀏覽器，看到你的使用者看到的東西。

/ship

一鍵發 PR

跑測試、審 code、開 PR，一個指令搞定。以前要手動跑測試、寫 PR 描述、等 review，現在一個 `/ship` 全部自動完成。

/review

AI Code Review

像資深工程師一樣審你的 code。它會看架構、看邏輯、看安全性，給你具體的修改建議。一個人開發最缺的就是 code review，這個技能補上了。

/investigate

自動 Debug

丟錯誤訊息給它，它會追根到底。不是只告訴你「可能是什麼問題」，而是真的去翻 code、找原因、給你修復方案。

Think 階段

### 規劃與思考

| 技能 | 說明 |
| --- | --- |
| `/office-hours` | 模擬 Office Hours，幫你釐清想法 |
| `/plan-ceo-review` | 用 CEO 視角審查你的計劃 |
| `/plan-eng-review` | 用工程主管視角審查技術方案 |
| `/plan-design-review` | 用設計主管視角審查 UX/UI |

Design 階段

### 設計

| 技能 | 說明 |
| --- | --- |
| `/design-consultation` | 設計諮詢，討論方向和風格 |
| `/design-review` | 審查設計稿，給改善建議 |

Build 階段

### 開發

| 技能 | 說明 |
| --- | --- |
| `/review` | AI Code Review，審查程式碼品質 |
| `/investigate` | 自動追查 bug 根本原因 |
| `/ship` | 一鍵跑測試 + 審 code + 開 PR |

Test 階段

### 測試

| 技能 | 說明 |
| --- | --- |
| `/qa` | 自動 QA，開瀏覽器找 bug 並修復 |
| `/qa-only` | 只測試不修復，產出報告 |
| `/cso` | 安全審查，找安全漏洞 |
| `/benchmark` | 效能測試，找出瓶頸 |
| `/canary` | 金絲雀測試，小範圍驗證 |

Release 階段

### 發布

| 技能 | 說明 |
| --- | --- |
| `/land-and-deploy` | 合併 PR 並部署 |
| `/document-release` | 自動產生 release notes |
| `/retro` | 回顧檢討，記錄學到的事 |

Browse

### 瀏覽器操作

| 技能 | 說明 |
| --- | --- |
| `/browse` | 開瀏覽器瀏覽網頁、截圖、互動 |
| `/connect-chrome` | 連接已開啟的 Chrome 瀏覽器 |
| `/setup-browser-cookies` | 設定瀏覽器 cookies |

安全工具

### 保護你的專案

| 技能 | 說明 |
| --- | --- |
| `/careful` | 進入謹慎模式，每步都確認 |
| `/freeze` | 凍結檔案，防止 AI 誤改 |
| `/guard` | 守護模式，監控變更 |
| `/unfreeze` | 解除凍結 |

### 免費嗎？

完全免費，MIT 開源授權。任何人都可以免費使用、修改、分享。gstack 本身免費，但你還是需要有 Claude Code 的使用權（Anthropic API key 或 Claude Max 訂閱）。

### Windows 能用嗎？

可以，用 Git Bash 或 WSL（Windows Subsystem for Linux）執行安裝指令就行。建議用 WSL2 會最順，跟 Mac/Linux 體驗一樣。

### 跟 Claude Code 有什麼不同？

gstack 是 Claude Code 的 **擴充包** ，就像手機裝 App 一樣。Claude Code 是基礎系統，gstack 在上面加了 23 個專業技能。沒有 Claude Code 就沒辦法用 gstack，但有了 Claude Code 不裝 gstack 也能正常使用。

### 安裝後怎麼更新？

在 Claude Code 裡輸入以下指令就能更新到最新版：

/gstack-upgrade

### 會影響現有專案嗎？

不會。gstack 裝在 `~/.claude/skills/gstack/` 目錄裡，不會動到你的專案檔案。它只是在 Claude Code 裡新增技能，不會改變任何現有的設定或程式碼。

### 安全提醒：Telemetry（使用記錄）

gstack 會在本地記錄你用了哪些 skill，存在 `~/.gstack/analytics/skill-usage.jsonl` 。

**預設不會上傳。** 除非你在安裝時主動同意 telemetry，否則資料只留在你自己的電腦上。

**怎麼確認你的狀態：**

cat ~/.gstack/analytics/skill-usage.jsonl

如果有內容，代表有本地記錄（但沒有外傳）。

**如果不放心，刪掉記錄：**

rm -rf ~/.gstack/analytics/

gstack 的 skill 功能本身沒問題，只是 telemetry 這件事應該讓使用者知道。

覺得有用？引用這篇分享給更多人