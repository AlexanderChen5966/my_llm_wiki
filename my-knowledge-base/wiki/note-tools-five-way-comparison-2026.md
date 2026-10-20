---
title: 五款筆記工具選型：知識庫型 vs 專案工作型（含 NotebookLM 改名）
date: 2026-08-13
tags: [pkm/workflow, pkm/obsidian]
source: https://www.bnext.com.tw/article/91831/ai-notes-tool-difference-2026（數位時代，2026-08-13）
type: article
status: processed
asset_value: 50
---

## T（核心論點）

> **五款工具的差別不在功能表，而在「它們如何處理你輸入的資訊」——分成知識庫型與專案工作型兩類，選錯類別再怎麼調設定都不對。**

- **知識庫型（Notion / Obsidian / Heptabase）** — 建立可長期擴張、跨主題累積的知識系統，範圍由自己決定，重點在記錄、組織與重新連結
- **專案工作型（Gemini Notebook / Kuse AI）** — 圍繞一組明確資料或特定任務工作，每個 notebook 或畫布有相對清楚的脈絡，目的是理解資料並完成研究或產出

判斷方式：問「該把筆記系統長期建在哪裡」→ 答案在前三者，差別只在你更看重團隊協作、資料自主權、還是釐清思緒的過程。問「只是想快速消化一批資料」→ 後兩者。

## 🔴 本篇最重要的單一事實：NotebookLM 已改名 Gemini Notebook

本文一律以 **Gemini Notebook（原 NotebookLM）** 稱之。本庫既有多篇條目沿用舊名（[[notebooklm-gemini-claude-research-workflow]]、[[notebooklm-py-cli-tool]]、[[notebooklm-skill-research-pipeline]]、[[google-workspace-notebooklm-gemini-admin-workflow]]、[[gemini-study-notebooks]]、[[ideas/notebooklm-22-article-guide]]、[[ideas/notebooklm-intro-bnext]]，以及 `notebooklm-kit` 相關規劃），**檢索與對外書寫時兩個名稱都要納入考慮**。⚠️ 改名時間點本文未載明，僅可確認 2026-08-13 的報導已採用新名。（2026-09-23 補：Google 於 2026 年 7 月更名〔aiposthub〈Gemini Notebook 互動式學習總覽怎麼用？〉FAQ〕；另一篇 bnext 報導〈Gemini Notebook新功能上線，10萬本電子書…〉載為 2026-07-16，附官方更名公告連結）

## B（適用場景）

**適用：**
- 替自己或他人做筆記工具選型時的第一層分流
- 判斷某個工具的**遷移風險**（本文對此的資訊最有價值）
- 確認各工具目前的 AI 串接方式（檔案層 vs MCP）

**不適用 / 注意：**
- ⚠️ 媒體選型文，各項限制（免費額度、離線範圍、人數上限）為 2026-08 快照，**這類數字折舊很快**
- 🔴 依本庫慣例，bnext 的業配段與延伸閱讀導流一律忽略
- 本文未涉及本庫實際採用的 Obsidian + Claude Code 整理引擎架構細節，那部分見 [[claude-code-obsidian-setup]]

## R（五款的核心用途與關鍵限制）

| 工具 | 核心用途 | 資料儲存 | 離線 | 協作 | 最推薦對象 |
|---|---|---|---|---|---|
| **Notion** | 記錄、分類、結構化 | 主要雲端 | 可手動下載頁面；**資料庫預設只下載第一個檢視的前 50 筆** | **強**（多人協作、權限管理、多檢視）| 需要團隊共用資料庫與排程的人 |
| **Obsidian** | 記錄、連結、長期保存 | **本機 Markdown 純文字** | **完整**（無網路也能讀寫）| **較弱**（無即時共編、無細緻權限，單一共享 vault 上限 20 人）| 重視資料自主權與長期個人知識管理的人 |
| **Heptabase** | 整理、連結、視覺化思考 | 雲端同步 + 本機保存 | 桌面 App 完整支援 | 中至強（即時協作、共享白板）| 需要視覺化拆解複雜議題者 |
| **Gemini Notebook** | 閱讀、研究、問答、產出 | Google 雲端 | 有限（僅語音摘要可下載離線播放）| 中（可邀檢視者／編輯者）| 需快速消化大量資料並產出成果的人 |
| **Kuse AI** | 研究、整理、產出、自動化 | 雲端持久化檔案系統 | 有限（主要靠瀏覽器，核心 AI 功能需連線）| 中（團隊方案、共享工作空間）| 想把研究、產出、自動化放在同一空間的人 |

### 遷移風險（本篇最實用的一節）

🔴 三個具體的「帶不走」：

- **Notion** — 資料庫做得越複雜，換平台成本越高：**關聯欄位在 CSV 匯出後會變成純文字網址，無法靠重新匯入恢復關聯**
- **Heptabase** — 白板上的空間佈局（卡片排列位置、分組方式）**很難搬到另一個純文字工具裡原樣恢復**
- **Gemini Notebook** — 資訊組織方式沒有 Obsidian／Notion 自由，**較難用來累積跨主題的長期知識系統**

對照 Obsidian：筆記就是本機 `.md`，就算不再使用 Obsidian，這些檔案依然可以用其他文字編輯器開啟。

### 跨工具串接：檔案層 vs MCP

- **Obsidian** — 資料就是本機 Markdown，**可直接把整個 vault 當成 Claude Code、Codex 等能讀本機檔案的 AI 工具工作目錄**，設定相對單純（＝本知識庫採用的架構）
- **Notion / Heptabase** — 主要透過 **MCP** 串接。Notion 官方已支援 Claude Code、Codex，可搜尋／讀取／新增／更新工作空間內容；Heptabase 也提供相同方式讓 Claude、ChatGPT 讀寫卡片
- **Gemini Notebook** — 報告可匯出到 Google 文件、表格匯出到 Google 試算表
- **Kuse AI** — 資料來源、AI 產出與工作流結果保留在同一工作空間，可繼續用於新任務

## C（關鍵行動）

- [ ] 先分類再比功能：這次是「長期累積知識」還是「消化一批資料」？
- [ ] 選之前先問**帶不走什麼**（Notion 關聯欄位、Heptabase 空間佈局）
- [ ] 搜尋 NotebookLM 相關資料時同時用新名 Gemini Notebook

## 補充資料

### Gemini Notebook 與 Kuse AI 的分野

兩者都能從一批資料做到成果交付，差別在重心：

- **Gemini Notebook** 強在**產出緊扣放進去的資料來源，且回答附引用位置可回去驗證**，降低「AI 從哪得出這個答案」的不確定感。可接受 PDF／網站／YouTube／音訊／Google Docs／Sheets／簡報／Word／Markdown，輸出可轉學習指南、資訊圖表、簡報、影片或音訊摘要
- **Kuse AI** 重心更往「完成任務」移動：文件、簡報、試算表、網頁都在同一工作空間產出，可保存前面成果延伸成新任務或自動化工作流。⚠️ 目前**沒有 iOS／Android 專屬 App**、**不支援即時多人協作**（官方列為未來規劃）、採點數制

原文對操作心態的區分很準：用 Gemini Notebook 時**先決定「這次要研究哪些資料」再開始問**；用知識庫工具時則從**「我要記下什麼」**開始。

### 三款知識庫型工具各自解決的不同問題

- **Notion** 的特色是頁面與資料庫在同一套系統：每筆資料能展開成完整頁面並帶狀態／日期／標籤／負責人，同一批資料可切表格／看板／日曆。價值在團隊共用時最明顯——編輯用日曆看排程、PM 用看板追進度、主管從表格篩狀態，**操作的是同一批資料**
- **Obsidian** 的核心是連結：不用一開始就決定筆記屬於哪個資料夾或專案，適合**知識會持續演變**的人（今天寫 AI Agent、幾個月後寫企業自動化，舊筆記仍可被新內容引用）。成本主要在同步（Obsidian Sync 或自行用 iCloud／Dropbox）與前期系統設計
- **Heptabase** 強調「看見」：筆記做成卡片放上白板，可移動位置、畫出關係。**這個差異看起來只是介面，其實會影響思考方式**——研究陌生產業時，一般筆記工具容易把市場數據、公司案例、技術名詞、專家觀點塞進一篇長文件，Heptabase 則可先拆卡片再重排「哪些屬同一組、哪些觀點互相矛盾、哪裡還缺資料」。特別適合「還不知道答案是什麼」的階段。⚠️ **沒有可直接申請的永久免費方案**（僅 7 天試用，或受付費用戶邀請成為協作者，但該帳號無法使用日記、個人卡片庫與 AI 功能）

### 學習曲線

最容易理解的是 Gemini Notebook（建 notebook → 加來源 → 開始問）。其餘四者的成本取決於想用多深：Notion 寫文件很快但開始建資料庫後會逐漸複雜；Obsidian 一開始只是 Markdown，但外掛、標籤、連結怎麼設計全都自己決定；Heptabase 要習慣卡片與白板的思考方式；Kuse 可從「丟資料給 AI」開始，但進階仍需建立自己的工作流程。

### 官方常見問題的實用結論

原文明確支持**混用**：用 Gemini Notebook 消化大量既有資料 → 用 Heptabase 或 Obsidian 釐清思路與長期保存 → 最終成果整理進 Notion 與團隊協作。免費可用：Notion、Obsidian、Gemini Notebook、Kuse AI（Heptabase 除外）。

## 相關連結

- [[claude-code-obsidian-setup]] — 本庫採用的 Obsidian + Claude Code 架構
- [[karpathy-knowledge-base-method]] — 三資料夾法：人負責輸入，AI 負責整理
- [[pkm-adoption-decision-framework]] — PKM 適合誰？建立前的投報率判斷
- [[llm-wiki-over-rag-claude-code]] — 用 Claude Code 建 LLM Wiki 取代 RAG
- [[notion-ai-workspace-analysis]] — Notion AI 工作空間分析
- [[lyt-framework-obsidian-ai-guide]] — LYT 框架：AI 相容性最高的 PKM 底層架構
- [[gemini-study-notebooks]] — Gemini Study Notebooks（同一產品線的學習模式）
- [[notebooklm-gemini-claude-research-workflow]] — 三 AI 接力研究流水線（沿用舊名 NotebookLM）
