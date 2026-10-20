---
title: Claude Code 寫入 Figma：CLAUDE.md/DESIGN.md 規則檔與元件庫整理實作指南
date: 2026-09-14
tags: [ai/claude, ai/mcp, type/how-to]
source: https://rar.design/posts/figma-mcp-write-to-canvas-setup-guide
type: article
status: processed
asset_value: 64
---

Claude Code 透過 Figma MCP 的 `use_figma` 工具，可以用既有元件庫與 Variables 直接在 Figma 畫布上生成原生圖層（2026-03-24 上線，beta 免費）。本篇是**深入實作教學**：CLAUDE.md/DESIGN.md 規則檔全文、元件命名策略、seat/額度門檻、self-healing loop 工作流程五步驟。

## T（核心論點）

> **差別從來都在 AI 動筆前讀到了什麼。** 同一句 prompt 丟給 Claude Code 兩次，會拿到兩個都能看、但都不像你家產品的畫面——解法不是換更好的 prompt，是把脈絡準備好：規則檔（怎麼工作）+ 元件庫（讀得懂的命名與 Variables）+ 範本畫面（照抄的對象）。

agent 找元件的方式是**搜尋名字**（`search_design_system`），命名即 prompt 的一部分；Variables 沒綁等於「給 AI 一盒蠟筆」，只能寫死色碼。

## B（適用場景）

**適用：**
- 已用 Figma MCP 讀取端功能，想進一步讓 AI 直接寫入畫布生成圖層的團隊/個人
- 需要一套可複製的 CLAUDE.md/DESIGN.md 規則檔起手版
- 想知道元件庫要整理到什麼程度，AI 才畫得對

**不適用 / 注意：**
- 寫入需要 Full seat + 檔案編輯權；Starter 免費方案每月僅 20 次讀取額度，只夠測試
- 目前 beta 免費，官方明說之後會用量計費
- 現存限制：無法置入圖片素材（僅佔位方塊）、自訂字型仍列限制清單、輸出品質為 beta 等級（同一 prompt 兩次結果會不同）、寫入功能僅遠端 server 支援（桌面版僅讀取）

## R（設定流程與規則檔）

### 十分鐘接上 Figma

```
claude plugin install figma@claude-plugins-official
```
裝好後 `/plugin` 授權、`/mcp` 確認 `figma` 顯示 connected。手動路線：`claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp`（此路線 skills 需另外從 `figma/mcp-server-guide` 抓取）。

### 開一個資料夾，讓 AI 每次從同一份脈絡出發

**CLAUDE.md（入口，短，只寫工作方式）**：
```
# 專案：產品名稱 設計工作區
這個資料夾是 Figma 設計工作的脈絡來源。動 Figma 之前先讀完 @DESIGN.md。

## 工作方式
- 每次任務先讀 DESIGN.md 指定的檔案，用 get_metadata 確認現有頁面與結構，再動手。
- 元件一律先用 search_design_system 查，找不到就回報，不要自己畫替代品。
- 一次只做一個畫面，做完用 get_screenshot 比對，回報沒對到 variable 的地方。
- 只在 DESIGN.md 指定的頁面建立東西，其他頁一律不碰。
```

**DESIGN.md（主戰場，回答五個問題：檔案在哪、哪一頁能動、怎麼命名、元件變數怎麼用、任務流程）**：
```
# DESIGN.md：Figma 規則

## 檔案入口
- 主檔（工作用）：https://www.figma.com/design/主檔ID/產品名稱
- 元件庫（唯讀，只查不改）：https://www.figma.com/design/元件庫ID/DS
- 工作頁：AI Sandbox。所有新畫面只能建在這一頁。

## 頁面結構
- Cover：封面與版本紀錄，不動。
- Flows：正式流程，只有設計師會改。
- Components：本檔專用元件，不動。
- AI Sandbox：AI 的工作區。每個任務開一個 section，section 名用任務名。

## 命名規則
- 畫面 frame 命名：flow/screen/state，例：checkout/payment/error
- 圖層名用內容命名：Header、Price、CTA，不留 Frame 123、Rectangle 45。
- 不改任何既有元件或圖層的名字。

## 元件與變數
- 顏色、間距、圓角、字級一律綁 variables。集合：Color、Space、Radius、Type。不寫死任何色碼或數字。
- 元件只用元件庫已發佈的版本，不 detach、不建新的主元件、不改變體屬性。
- 找不到對應元件時先回報缺什麼，不要用矩形拼一個假的。

## 排版
- 全部 auto layout。間距只用 4 的倍數：4、8、12、16、24、32、48。
- 手機畫面寬 390，桌面 1440。畫面之間左右間隔 160。

## 每次任務的流程
1. 讀完這份檔案。
2. get_metadata 看 AI Sandbox 頁現況，確認沒有同名 section。
3. search_design_system 列出這個畫面會用到的元件，先貼清單給我看。
4. 建一個畫面。一次呼叫的輸出上限 20KB，畫面大就分區塊建。
5. get_screenshot 截圖，跟 references/ 裡的範本比對。
6. 回報：用了哪些元件、哪些值沒對到 variable、哪裡需要我接手。
```

### 元件庫整理對照表（決定輸出是「可接手改」還是「重做比較快」）

| 項目 | AI 讀得懂 | AI 會亂猜 |
|---|---|---|
| 命名 | 類別/變體/狀態三層：`Button/Primary/Hover` | `Frame 128`、`btn_new_final2` |
| Variables | 顏色/間距/圓角/字級都綁 variable | 色碼直接填在圖層上 |
| Auto layout | 每個容器都是 auto layout | 元素用絕對位置排 |
| 元件 | 主元件有變體屬性，已發佈到元件庫 | 半數實例已 detach |
| 頁面與範本 | 有專用工作頁，放兩個做好的範本畫面 | 一頁塞所有東西，沒有可照抄對象 |

**範本畫面比規則文字更有效**：與其在 DESIGN.md 多寫十條，不如在工作頁放兩個親手做好的範本畫面，讓 AI 先看再畫，準確率遠高於憑文字描述。

### 工作流程五步（self-healing loop）

1. **讀規則**：CLAUDE.md 帶入 DESIGN.md
2. **查元件**：`get_metadata` / `search_design_system`
3. **建畫面**：`use_figma` 分區塊寫入（單次呼叫輸出上限 20KB）
4. **截圖自檢**：`get_screenshot` 比對範本，不符合的地方自己再改一輪——因為動的是真實結構，修正會落回元件與 variables
5. **設計師接手**：修細節、搬進 Flows。**預期是前 80% 它做、後 20% 你做。**

### Seat／額度門檻

寫入需 **Full seat**；Starter 免費方案每月僅 20 次讀取額度（只夠測試）；Professional 才夠日常使用；目前 beta 免費，之後用量計費。

## C（關鍵行動）

- [ ] 開一個規則資料夾：CLAUDE.md 當入口 + DESIGN.md 寫檔案連結/命名/元件變數/任務流程
- [ ] 元件命名改成「類別/變體/狀態」三層，把顏色/間距/圓角/字級全部綁 Variables
- [ ] 在工作頁放兩個親手做好的範本畫面，比多寫十條規則有效
- [ ] 確認帳號門檻：認真使用至少要 Professional 的 Full seat

## 原始摘要

設計師 Riven 整理 Claude Code 透過 Figma MCP `use_figma` 工具寫入畫布的完整實作流程：3 月 24 日上線、beta 免費，之後用量計費。核心是把工作脈絡固定成 CLAUDE.md（入口）+ DESIGN.md（Figma 怎麼用）兩份規則檔，搭配元件庫命名策略（agent 靠文字比對名稱找元件）與 Variables 綁定，最後用 self-healing loop（建畫面→截圖自檢→設計師接手）完成任務。

## 相關連結

- [[figma-ai-make-mcp-design-agent]] — Figma AI 生態總覽（已簡述 Claude Code 串接 Figma MCP 兩種方式）；本篇是深入實作版，補足規則檔全文、元件庫命名策略、seat/額度細節與 self-healing loop 五步
- [[grok-bot-figma-screenshot-voice-workflow]] — 同批「Grok Bot 工作流」是可跨工具遷移的下指令方法論（截圖定範圍+語音改稿），本篇是特定工具（Claude Code + Figma MCP）的規則檔/元件庫設定實作教學，層次不同，不重複
