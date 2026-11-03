---
title: Claude Code Review 正式開放：Team／Enterprise 的 Quick、Standard、Deep 三檔怎麼選
date: 2026-10-30
tags: [ai/claude, ai/workflow]
source: https://www.aiposthub.com/claude-code-review-general-availability-levels/
type: article
status: processed
asset_value: 18
---

> 註：asset_value 18，低於 skill 門檻，為使用者指定建立。
> 資料快照：2026-10-17（原文資料核對日）。方案、層級與限制可能之後變動，使用前請回官方文件核對。
> 來源性質：aiposthub（作者 Philo）對官方公告與官方文件的二手整理；其中「選層級的表格」是作者自己的導入建議，不是官方規則。

## T（核心論點）

Claude Code Review 是**雲端的 GitHub PR 審查服務**，於 2026-10-17 正式開放（GA）給 **Team 與 Enterprise** 方案。GA 帶來的新選擇是：管理員可**按組織或按儲存庫**選 Quick、Standard、Deep 三檔審查深度，用來在「檢查深度、等待時間、成本」之間分配資源。AI 只提供線索，**不批准、也不阻擋 PR**，合併決策仍由團隊負責。

> ⚠️ 這是與本機 `/code-review` **不同的功能**，請勿混用，詳見下方「與本機 /code-review 的差異」。

## B（適用場景）

**適用：**
- 已有 GitHub 開發流程、想降低人工審查負擔的 Team／Enterprise 組織
- 同一組織內專案風險不同，想分層配置審查深度

**不適用 / 注意：**
- 個人 Pro 訂閱不適用此正式服務（來源：「官方正式服務列為 Team 與 Enterprise」；個人工作階段中的審查指令屬其他使用方式）
- 不會建置或執行程式，**不能取代測試與建置**
- 「AI 沒留言」不等於程式沒錯，也不等於取得合併許可
- 啟用前要核對組織方案與資料要求（服務在 Anthropic 管理的環境執行，經管理員安裝的 GitHub App 取得存取），不是只看開發者有沒有 Claude 訂閱

## R（操作步驟）

### 運作方式（來源轉述官方文件）
多個代理讀取程式差異與相關程式，找邏輯錯誤、安全問題與邊界情況，再驗證、去重、依嚴重度回報；結果以相關程式行的留言、摘要與檢查項目呈現。

### 三檔差異

| 層級 | 官方定位（依來源） | 備註 |
|---|---|---|
| Quick | 最快、成本最低的輕量檢查 | 具體時間、費用：來源未提供 |
| Standard | 平衡深度、速度與成本；**預設值** | 具體時間、費用：來源未提供 |
| Deep | 最深入；等待時間與成本最高 | 具體時間、費用：來源未提供 |

### 選擇準則
原則：**依儲存庫的錯誤代價選，不要全部用同一檔**。以下為作者的導入建議（非官方規則）：

| 儲存庫情境 | 試點時可考慮 |
|---|---|
| 小型例行變更、回復容易 | 先觀察 Quick 的有效發現 |
| 一般產品功能 | 用 Standard 建立基準 |
| 錯誤影響較大的核心系統 | 評估 Deep，並保留人工專項審查 |

實際要依「漏掉哪些錯誤、多少留言成立、工程師處理留言的時間」再調整。

### 使用方式與限制
- 設定權限在管理員：可選組織層級，也可替個別儲存庫設不同層級
- 模型由服務選擇，官方管理設定沒有提供任意選模選項（依來源）
- 建議流程：先挑**一個儲存庫試點**；記錄有效問題、誤報、等待時間、審查費用，並對照回退修改、正式事故與人工工時；不要只看留言數量；知道哪類問題最常漏掉後再補審查指引，比一開始寫長規則好維護
- 來源提到的 Claude Code 團隊內部經驗（六到九月合併 PR 數量增加逾一倍、需回退比例降低逾 40%）是**該團隊多層流程（測試、安全審查、按風險決定人工參與、逐步發布）的整體結果**，不能當作此服務對所有客戶的保證

### 與本機 /code-review 的差異

| 項目 | Code Review 正式服務（本條目） | 本機 `/code-review`（見 [[claude-code-review-harness-principles]]，v2.1.147） |
|---|---|---|
| 性質 | 雲端服務，經管理員安裝的 GitHub App | Claude Code 內建指令，在本機工作階段執行 |
| 適用方案 | 僅 Team／Enterprise（2026-10-17 快照） | 所有方案 |
| 層級選項 | Quick／Standard／Deep（管理員設定） | 無此三檔 |
| 設定來源 | 管理員設定；既有條目記載 REVIEW.md 僅供此雲端服務讀取 | 只讀 CLAUDE.md，不讀 REVIEW.md |
| 輸出 | PR 行內留言、摘要、檢查項目 | 依該條目：以 `gh pr comment` 發布結果 |

兩者名稱相近但是**不同功能、不同設定面**：想調整雲端服務的審查標準看 REVIEW.md 與管理設定；想調整本機 `/code-review` 行為改 CLAUDE.md。

## C（關鍵行動）

- [ ] 確認組織方案是 Team／Enterprise，並請管理員核對資料要求後再啟用
- [ ] 選一個儲存庫以 Standard 建立基準，必要時對照 Quick／Deep
- [ ] 記錄有效問題、誤報、等待時間與費用，與回退、事故、人工工時一起評估
- [ ] 測試與建置流程維持不變，不以此服務取代

## 來源未提供的項目

- 定價與各層級的實際費用、用量上限
- 各層級的實際等待時間與審查範圍的量化差異
- 資料保留與隱私條款細節
- Quick／Deep 的內部運作差異（用了幾個代理、哪個模型）
- 如何自訂審查指引的具體語法（來源只說「補充審查指引」）

## 原始摘要

aiposthub 2026-10-17 文章：Claude Code Review 正式開放給 Team 與 Enterprise，管理員可按儲存庫選 Quick、Standard（預設）、Deep；服務讀差異與周邊程式找問題，不建置、不執行、不批准或阻擋 PR。文中選層級表格與試點指標為作者建議。已排除：電子報訂閱導流、延伸閱讀連結（GitHub ReviewBench、AI 程式驗收教學，未讀其內容，不收）。

## 相關連結

- [[claude-code-review-harness-principles]] — 本機 `/code-review` 的六大設計原則（不同功能，見上方差異表）
- [[ai-code-review-safeguards]] — AI 生成程式碼的 Code Review 防護層，人工審查不可省的補充觀點
- [[harness-engineering]] — 更高層的 review／harness 系統設計視角
- [[claude-code-team-layered-review-human-approval]] — Claude Code 團隊內部的組織層分層審查與人工核准（零／一／兩位）；與本條只共用「合併 PR 增加逾一倍、回退降逾 40%」一組數字，且為 Anthropic 內部結果
