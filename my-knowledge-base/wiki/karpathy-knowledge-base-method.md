---
title: Karpathy AI 知識庫方法
date: 2026-05-13
tags: [pkm/workflow, pkm/obsidian]
source: https://www.anduril.tw/karpathy-knowledge-base/
type: article
status: processed
asset_value: 75
---

## T（核心論點）
把 AI 從「用完即忘的問答工具」升級為「會自己長大的個人圖書館」——人只負責丟資料，AI 全權負責整理與維護 wiki。

## B（適用場景）
**適用：** 有大量碎片素材需要整理、長期研究某個主題、希望 AI 越用越精準的場景
**不適用：** 一次性查詢、素材量少（< 10 篇）時效益不明顯；個人想法與 AI 整理的內容需分開存放

## R（操作步驟）
1. 建立 `raw/` `wiki/` `outputs/` 三個資料夾
2. 所有素材丟入 `raw/`，不分類（用 Web Clipper 一鍵存入）
   - 進階版：用 `agent-browser`（Vercel Labs CLI，26K⭐）自動抓網頁存入 raw/，比 Playwright MCP 少用 82% tokens
3. 對 Claude Code 下指令：「讀取 raw/ 按 CLAUDE.md 規則編譯成 wiki/」
4. 每次查詢的結果回存到 `outputs/`（觸發知識複利）
5. 每月執行健康檢查：「掃描整個 wiki/，找矛盾、補缺漏、發現關聯」

## C（關鍵行動）
- [ ] raw/ 和 wiki/ 必須分開存放（避免 AI 內容淹沒個人筆記）
- [ ] 每次 Claude 查詢後把有價值的回答回存 outputs/
- [ ] wiki 新增條目後確認 INDEX.md 已更新

## 原始摘要

Karpathy 的比喻：大多數人用 AI 像去便利商店（用完即忘），他的做法是讓 AI 建一座圖書館（越用越厚）。六步驟的關鍵是第二步——AI 自動把 raw/ 編譯成結構化 wiki，人幾乎不手動整理。Lex Fridman 的延伸做法：跑步時用語音模式對 wiki 提問（互動式 Podcast）。

一個重要提醒（Obsidian 創辦人 Steph Ango）：自己的筆記和 AI 產出的知識庫應分開存放，混在一起會讓搜尋結果被 AI 生成內容淹沒。

## 相關連結
- [[obsidian-web-clipper]] — 配合使用的擷取工具，是 raw/ 輸入端
- [[obsidian-knowledge-base-planning]] — 本知識庫的整體規劃文件
- [[claude-code-obsidian-setup]] — Claude Code + Obsidian 完整建構指南

## 補充來源
- DeTools 工具翼零（https://tools.wingzero.tw/article/sn/3865）：同一方法的中文拆解版，新增 agent-browser 自動化工具介紹

## 補充資料（來源：Claude + Obsidian 打造 AI 第二大腦，Karpathy 的知識管理 LLM Wiki 教學｜科技翰林院）

### 為什麼 Obsidian 是 AI 知識管理最佳載體

三個技術原因讓 Claude Code 和 Obsidian 天生相容、零整合成本：

1. **純 Markdown**：Claude Code 讀寫 `.md` 不需要任何轉換層（Notion 需要 API 拉取轉格式）
2. **雙向連結語法**：Obsidian 的 `[[wikilink]]` 讓 Claude Code 可以解析並自動建立關聯
3. **YAML frontmatter**：每篇筆記的 metadata 可批次讀取、篩選、更新

**Local-first 的長期保障**：Obsidian 的筆記就是檔案，就算 Obsidian 公司消失了，知識庫一個字都不會少。

### Ingest / Query / Lint 三核心動作詳解

**Ingest（攝入）**：新資料進來不只是「存起來等查詢」，而是被消化、整合到既有知識網路。Karpathy 指出一次 ingest 可能動到 10-15 個 wiki 頁面。

**Query（查詢）**：Claude Code 從你自己累積的知識（不是 AI 訓練資料）合成答案。關鍵洞察：**好的回答應該被存回 wiki 變成新頁面**，讓你的探索也產生複利。

**Lint（健檢）**：掃描整個 vault 找出矛盾資訊、過時內容、孤立頁面、缺失的交叉引用。Karpathy 點出：人類會放棄維護 wiki 是因為維護成本的增長比價值還快，LLM 不會無聊、一次能動 15 個檔案。

### Claude Code + Obsidian 五個實戰工作流

1. **每日筆記**：設定 `/today` slash command，自動檢查 daily note 是否存在、用範本建立、搬入未完成待辦
2. **研究筆記**：說「Research X 存到 resources/」，Claude Code 先搜 vault 避免重複，再綜合外部資訊產出有 wikilink 的結構化筆記
3. **批次加反向連結**：掃描一個月 daily notes，找出所有提到的人名/專案/書名，自動加 `[[wikilink]]`，沒有對應筆記就自動建立
4. **批次加標籤**：根據內容自動分類，參考 CLAUDE.md 的標籤規範，補上 YAML frontmatter 中的 tags
5. **每週回顧**：設定 `/weekly-review`，統計完成/未完成 tasks、辨識重複主題、找出停滯專案，產出可被未來 AI 查詢的回顧筆記

### 推薦 Vault 結構

| 資料夾 | 用途 | Claude Code 用法 |
|--------|------|-----------------|
| `CLAUDE.md` | AI 助理工作手冊 | 每次啟動自動讀取 |
| `.claude/commands/` | 自訂 slash commands | 存放 `/today`、`/weekly-review` |
| `daily/` | 每日筆記（YYYY-MM-DD.md）| 自動建立、搬入未完成待辦 |
| `projects/` | 進行中的專案筆記 | 追蹤進度和待辦 |
| `resources/` | 參考資料和研究筆記 | ingest 新資料的目標目錄 |
| `_inbox/` | 待處理收件匣 | 定期清空、歸檔到正確位置 |

CLAUDE.md 建議控制在 100 行以內。核心內容：身份和工作領域、資料夾結構邏輯、筆記格式規範、Claude Code 行為邊界（NEVER 刪除任何檔案）。

### 設置方式（30 秒）

最簡單的方式：打開終端機 → `cd` 到 Obsidian vault 目錄 → 輸入 `claude`。Claude Code 會自動掃描目錄結構，讀取 CLAUDE.md。第一個指令可以打：「幫我列出這個 vault 裡所有的資料夾結構和筆記數量」。

Karpathy 的核心名言：「Obsidian 是 IDE，LLM 是工程師，wiki 是程式碼」——分工很自然：你負責閱讀、思考、產出洞察，Claude Code 負責整理、連結、維護。

## 補充資料

### 六步驟複製流程（一般人可操作版）

來源：《AI 大神 Karpathy 的 AI 筆記流，一般人也能做到八成》（Fox Hsiao，2026-04-03）

將 Karpathy 的系統拆解為非工程師也能操作的六步驟：

1. **把資料全部丟進 raw/ 資料夾**：用 Obsidian Web Clipper 一鍵存為本地 Markdown，不分類、先存再說
2. **叫 AI 把亂七八糟的資料「編譯」成百科全書**：不自己整理筆記，讓 AI 讀完 raw/ 後自動產出結構化 wiki，AI 全權維護
3. **直接對百科全書提問**：知識庫到達一定規模（Karpathy：某主題約 100 篇、40 萬字）後，提問即可，不需要 RAG
4. **答案回存**：查詢結果回存到知識庫，知識庫越用越厚、越厚越好用（知識複利）
5. **定期健康檢查**：讓 AI 掃描整本百科全書，找矛盾、補缺漏、發現跨主題關聯
6. **自己做小工具**（進階，可跳過）：vibe coding 做搜尋介面

**不寫程式也能做到八成的關鍵**：步驟 1-5 不需要寫程式，只需要 Obsidian + Claude Code + Web Clipper。

**Steph Ango（Obsidian 創辦人）的重要提醒**：自己的筆記和 AI 產出的知識庫**必須分開存放**——混在一起會讓搜尋結果被 AI 生成內容淹沒，分不清哪些是自己的想法。

### 可直接作為 Prompt 的操作指引

來源：《Andrej Karpathy：完整 LLM wiki 建構提示詞》（鉅亨號，2026-04-05），Karpathy 本人直接給出的操作說明

**系統架構三層**：
1. **原始資料層（Raw sources）**：不可變（LLM 只能讀取，絕不修改）——事實真相源
2. **Wiki 層**：LLM 完全擁有，建立頁面、更新內容、維護交叉引用
3. **約束架構層（Schema）**：CLAUDE.md / AGENTS.md——告訴 LLM wiki 結構、命名約定、工作流

**適用場景（Karpathy 官方列舉）**：
- 個人管理：目標、健康、自我提升
- 深度研究：數周/數月的主題研究
- 閱讀書籍：邊讀邊歸檔，建立伴讀 wiki
- 商業/團隊：Slack 記錄 + 會議紀錄 + 專案文件
- 競品分析、旅行規劃、課程筆記、任何需要隨時間積累知識的場景

**使用方式**：把 Karpathy 的原文（包含 CLAUDE.md 規則）直接貼給 AI Agent，告訴它：「閱讀這篇文章，理解 LLM Wiki 的理念，以後你就是我的 Wiki 維護員了。」

> 類比：Obsidian 是開發工具（IDE），LLM 是程式設計師，你的 Wiki 就是程式碼庫。

## 補充資料（來源：Karpathy LLM Wiki 是什麼？一個卡片盒筆記法使用者的實測，WenHao Yu）

### Karpathy vs LYT 框架骨架對照

| Karpathy Pattern | LYT（WenHao Yu 的系統）|
|---|---|
| `raw/`（原始來源）| `Atlas/Sources/` |
| `wiki/`（LLM 維護的知識頁）| `Atlas/Dots/` + `Maps/` |
| schema 檔（CLAUDE.md 規則）| `CLAUDE.md` + 各 `SKILL.md` |
| `index.md`（索引）| `Maps/` MOC |

骨架幾乎一樣，根本分歧在哲學：**容器邊界問題**。

### 分歧核心：一張卡到底是什麼？

**LYT 的答案**：一個原子概念。一張卡一件事，邊界由概念本身決定。代價：掌握最新狀態需靠 MOC 和連結自己拼圖。

**Karpathy 的答案**：一個主題聚合。一張 wiki page 是那個主題目前的 best-of 總整理，10 個 sources 可能被 LLM 併進 1-2 張 page。問題：「主題邊界在哪？」——這是 folder 和 tag 時代的老問題換了個皮。

### Karpathy 方法有但現有系統缺少的三個功能

1. **Ingest 時的矛盾偵測**：新 source 進來，LLM 比對新資訊與舊 wiki page 衝突，標出矛盾留給人判斷
2. **跨 page 連鎖更新**：一個新 source 可能同時更新 10-15 張 wiki page
3. **Lint（健康檢查）**：定期掃整座 wiki 找矛盾、孤立 page、過期內容、概念缺口（「你在好幾個地方提到這個概念，但沒有獨立 page」）

### HN 上的兩大反對聲音

**Model Collapse（技術面）**：LLM 反覆讀自己寫的東西再改寫，長期下來細節被磨平、風格單一化（Nature 2024 論文論證）。

**Vibe Thinking（認知面）**：把「整理」外包等於把「思考」外包，wiki 看起來有組織但沒有內化。Karpathy 的防禦：「The human's job is to curate sources, direct the analysis, ask good questions.」

---

> **PKM 適合誰？** 「該不該建 PKM」的投報率判斷框架（三不記原則 / 四維決策矩陣 / 三種需要 PKM 的角色）已昇華為獨立條目：[[pkm-adoption-decision-framework]]（alignment-check 2026-07-03 抽出，因其為通用決策框架，非專屬 Karpathy 方法）。

