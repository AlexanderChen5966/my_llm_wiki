---
title: Godot＋Claude Code 做遊戲：純文字專案檔、提示詞三要素與兩個驗收坑
date: 2026-11-05
tags: [ai/claude, ai/workflow, ai/prompt-engineering]
source: https://aiqkangber.com/blog/godot-game-development（Q kangber 個人部落格，發布 2026-07-30，剪藏 2026-09-24）
type: article
status: processed
asset_value: 27
---

> 註：asset_value 27（頻率 3×耗時 3×複雜度 3），**保守估 18（頻率 2）**，屬邊界；主專案是 Flutter／Spring Boot，Godot 非主戰場，價值主要來自兩個坑可遷移到任何 AI 寫程式的驗收與環境宣告。
> 🔴 **來源性質**：**單一作者自述實測，無外部佐證，未驗證**。一手來源（Godot 官方文件、Claude Code 文件）本條未核對。
> ⏰ **快照 2026-07**：Claude Pro 價格（作者寫每月 20 美元）與 Unity Personal 年營收門檻（作者寫 20 萬美元）易過時，引用前回官方核對。
> 已排除：作者的接案與網站開發服務廣告段、電子報訂閱呼籲、遊戲本身細節（檔案大小等）、FAQ「AI 生成程式碼權利歸你」（作者斷言，非法律意見）。

## T（核心論點）

Godot 專案檔幾乎都是**純文字**，AI 讀得懂、改得動，所以是 Claude Code 搭引擎的新手首選。但「能改」不等於「改對」：兩個最容易踩的坑是 **AI 說改好了、實際沒接上**，以及 **AI 寫出 Godot 沒有的語法卻不報錯**。對策都是同一句話：**把驗收寫成機器能查的條件，把引擎名稱與版本寫進專案說明檔**。

## B（適用場景）

- 想用 Claude Code 從零做出可玩的 2D／3D 遊戲、又沒有既有引擎包袱的新手
- Unity 檔案多為二進位，AI 較難直接讀改；**已經會 Unity 的人不必為此搬家**（作者觀點）
- 接法：安裝 Godot、在專案資料夾裡開 `claude`、回編輯器按執行；**不需要外掛**（作者自述）

## R（關鍵細節）

### 1. 提示詞三要素（作者歸納）

1. **交代專案與規則放在哪裡**（哪個資料夾、哪份規則檔）
2. **判斷條件逐條列**，不要用「做得好看一點」這類無法判定的話
3. **限制本輪範圍，並要求回報改了哪些檔**

對照 [[codex-prompt-4-elements-agents-md]] 的提示詞要素，同一類做法。

### 2. 規則先寫成清單資料，再讓 AI 實作

戰棋、卡牌這類規則多的遊戲，先把規則寫成**清單式資料**（可逐條檢查），再請 AI 照清單實作，而不是一次口述整套玩法。

### 3. 素材要另外準備

圖片、音效 **AI 不會替你生**，需另外取得或製作；先講清楚素材放哪、命名規則，再叫 AI 引用。

### 4. 兩個坑（本條最可遷移的部分）

| 坑 | 現象 | 作者的對策 |
| --- | --- | --- |
| 說改好了，圖沒被引用 | AI 回報「已完成」，但新增的圖檔沒有被任何畫面引用，遊戲裡看不到 | 加**自動檢查：每張圖檔名都必須被引用**，不信完成訊息 |
| 寫出 Godot 沒有的語法 | AI 套用其他語言的迴圈語法，**Godot 不報錯，卻永遠不結束** | 把**引擎名稱與版本寫進專案說明檔**（CLAUDE.md／AGENTS.md） |

🔴 第二個坑的危險在「不報錯」：沒有錯誤訊息可以提醒你，只能靠版本宣告事前預防，或用執行逾時事後發現。

## C（關鍵行動）

- [ ] 要用 AI 寫任何引擎專案前，先在專案說明檔寫明**引擎名稱＋版本**（見 [[agents-md-guide]]）
- [ ] 驗收別看「已完成」，改寫一條機器可查的規則（例如資源是否被引用）；同樣的警訊見 [[agent-bug-fix-eval-green-is-not-correct]]、[[ai-collaboration-five-step-acceptance-criteria]]
- [ ] 每輪只給一個範圍，並要求列出改動檔案清單
- [ ] 引用價格與 Unity 門檻時標「2026-07 快照」

## 原始摘要

作者 Q kangber 以 Claude Code＋Godot 做出可連線對打的 3D 戰棋卡牌遊戲（瀏覽器版）。重點：Godot 專案檔為純文字、新手首選；接法不需外掛；提示詞三要素；規則先寫成清單；素材需另備；兩個坑（沒被引用的圖、Godot 沒有的迴圈語法不報錯但不結束）。全文為單一作者自述，附帶接案廣告與電子報呼籲（未收）。

## 相關連結

- [[awesome-gamedev-agent-skills-router-pattern]] — 遊戲引擎 skills 的路由模式；本條是「手動接法＋驗收坑」，兩者互補
- [[ideas/claude-code-game-studios-note]] — 多代理遊戲工作室筆記
- [[research/claude-code-game-studios-multi-agent]] — 同主題的多代理研究
- [[agents-md-guide]] — 引擎與版本寫進專案說明檔
- [[agent-bug-fix-eval-green-is-not-correct]] — 「說改好了」不等於真的好
- [[ai-collaboration-five-step-acceptance-criteria]] — 驗收條件寫法
- [[codex-prompt-4-elements-agents-md]] — 提示詞要素對照
