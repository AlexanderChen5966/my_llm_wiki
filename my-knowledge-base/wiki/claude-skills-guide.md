---
title: Claude Skills 完整指南（能力包 + 軟編排）
date: 2026-05-13
tags: [ai/claude, ai/agent]
source: https://www.axtonliu.ai/newsletters/ai-2/posts/claude-skills-capability-package-soft-orchestration-guide
type: article
status: processed
asset_value: 85
---

## T（核心論點）
Claude Skills 有兩種本質不同的用法：**能力包型**封裝判斷邏輯（5 分鐘做完 3 小時的事），**軟編排型**調度多個 Agent 協作複雜流程——兩者都比 MCP 更省 Token、比直接 Prompt 更可復用。

## B（適用場景）
**適用：** 重複執行同樣判斷邏輯；需要多 Agent 協作的複雜流程；想把隱性知識資產化
**不適用：** 一次性任務；需要即時外部資料（用 MCP）

## R（操作步驟）
1. **能力包型**：把判斷邏輯寫進 SKILL.md → 定義輸入/輸出模板 → 加 trigger 關鍵字
2. **軟編排型**：設計 Agent 分工（A→B→C）→ 每個 Agent 有獨立職責 → Team Lead 統籌
3. 用 5A+ 迭代：先跑 MVP → 觀察品質 → ADJUST → 穩定後 APPLY

## C（關鍵行動）
- [ ] 能力包型：SKILL.md 的判斷邏輯要具體，不能只寫「整理筆記」
- [ ] 軟編排型：Agent 之間用中間檔案（/tmp/）傳遞，不要用對話
- [ ] 每次使用後記錄問題，更新 Changelog

## 原始摘要
Axton Liu 三週實測：discussion-organizer（能力包）5 分鐘完成 50 條筆記整理；srt-workflow（軟編排）2 分鐘字幕轉文章。Skills vs MCP：Skills 是靜態指令包，省 Token；MCP 是動態工具，需要即時連線。

## 相關連結
- [[claude-skills-asset-building]] — Skills 系統化構建的架構設計
- [[agent-skill-design-patterns]] — Google 5 大 Skill 設計模式
- [[agent-skills-vs-mcp]] — Skills 與 MCP 的選擇判斷
- [[claude-code-five-layer-architecture]] — Skills 在五層架構中的定位（第二層：知識層）

## 補充資料（來源：Claude Code Skills：讓 AI 變身專業工匠｜高見龍）

### SKILL.md 完整結構規範

最小結構：一個資料夾 + 一個 `SKILL.md`。name 欄位規則：1-64 字元、只能小寫字母/數字/`-`、不能以 `-` 開頭或結尾、必須與目錄名稱一致。description 欄位：1-1024 字元，應描述「做什麼」和「什麼時候用」兩個維度。

可選資料夾（需在 SKILL.md 中明確引用 Claude 才知道存在）：

```
skill-name/
├── SKILL.md      # 必要
├── scripts/      # 可執行腳本（Python、Bash、JavaScript）
├── references/   # 額外參考文件
└── assets/       # 靜態資源（範本、圖片、資料檔）
```

### Progressive Disclosure 三層機制

| 層次 | 內容 | Token 量 | 載入時機 |
|------|------|---------|---------|
| Layer 1：Metadata | `name` + `description` | ~100 tokens | 啟動時預載所有 Skill |
| Layer 2：Instructions | `SKILL.md` 主體（建議 ≤5000 tokens）| 依 Skill 大小 | Claude 判斷任務相關時載入 |
| Layer 3：Resources | `scripts/`、`references/`、`assets/` | 依需要 | 按需讀取，Claude 需要時才讀 |

**設計精髓**：「amount of context that can be bundled into a skill is effectively unbounded」——因為 Layer 3 是按需載入，理論上可以打包無限量的知識。

### Skills vs 其他機制完整比較

| 機制 | 觸發方式 | 持久性 | 可包含程式碼 | 適合場景 |
|------|---------|--------|------------|---------|
| Skills | Claude 自動判斷（根據 description）| 跨對話/跨專案 | 是 | 重複性專業工作流程 |
| Custom Commands | 使用者輸入 `/command` | 專案或個人層級 | 否（單一 prompt 檔）| 固定格式的明確操作 |
| MCP | 工具呼叫 | 啟動時載入 | 是（server 端）| 連接外部 API、資料庫 |
| Subagents | Claude 自動委派或手動啟動 | 任務期間 | 是 | 需獨立 context、平行處理 |

類比：Skills 是「腦袋裡的知識」，MCP 是「手上的工具」——可以同時用 Skill 教 Claude 怎麼做翻譯、又用 MCP 讓它存取術語庫，兩者互補而非互斥。

### 安裝位置

- **使用者層**（所有專案可用）：`~/.claude/skills/<name>/SKILL.md`
- **專案層**（此專案所有人可用）：`<project>/.claude/skills/<name>/SKILL.md`
- 兩邊有同名 Skill，使用者層蓋過專案層

## 補充資料：Skill 的知識傳承與自我進化特性

來源：Ray 貓（ray-realms.com，2026-03-06）《Agent Skill 實戰攻略》

### Skill 的核心價值：知識傳承能力

傳統 prompt 每次使用都要重新貼上；Skill 帶 Description 目錄，讓 Agent 自動比對任務→調用對應 SOP，實現「全自動化」。

**自我進化機制（最獨特之處）：**
- 第一次執行任務：AI 探索摸索，花 2 小時，過程寫成 Skill
- 第二次：有 Skill 輔助，30 分鐘，遇到問題自行解決並記錄
- 第三次：10 分鐘，越做越順

> 類比：這就像人類建立文明的關鍵——知識傳承能力。AI 助手不再是每次重開機就失憶的金魚，而是能持續累積經驗、跟著你一起進化的超級員工。

**與 description 欄位的關係**：description 是「目錄」，讓 AI 在幾千個 Skill 中快速定位「我現在只需要這一個」，而不是載入所有 Skill 的完整內容。這就是 Progressive Disclosure 能擴展到無限 Skill 的原因。

### 撰寫 Description 的黃金法則

- **壞例**：`Helps with PDFs.`（太泛，Claude 不知道什麼時候用）
- **好例**：`Extracts text and tables from PDF files, fills PDF forms, and merges multiple PDFs. Use when working with PDF documents or when the user mentions PDFs, forms, or document extraction.`

description 要同時包含「做什麼」和「什麼時候用」兩個維度。如果 Skill 經常沒被啟用，或在不該啟用的時候被啟用，問題通常出在 description。

### Agent Skills 開放標準（2025-12-18）

Anthropic 把 Agent Skills 發布為開放標準：規格 [agentskills.io](https://agentskills.io/)，原始碼 [agentskills/agentskills](https://github.com/agentskills/agentskills)。這讓在 Claude Code 建立的 Skills，理論上也能在其他支援此標準的 AI Agent 中使用。

### 安全注意事項

Skills 的 `scripts/` 目錄可以被執行，意味著惡意腳本可能在電腦上執行各種動作。安裝前務必：
1. 只從信任來源安裝（先看 GitHub 內容）
2. 檢查 `scripts/` 目錄，確認沒有呼叫外部 URL、修改系統檔案、存取敏感資料
3. 檢查 `SKILL.md` 是否引導 Claude 做危險操作（如傳送 `.env` 內容到外部 URL）

## 補充資料（來源：Claude Skills 教學：從零開始打造你的 AI 自動化工作流.md）

### MCP 是廚房，Skills 是食譜

WenHao Yu 的比喻精準區分兩者：
- MCP 讓 Claude 連到外部工具（解決「能不能做」）
- Skills 教它怎麼用這些工具（解決「怎麼做最好」）

以內容監控系統為例：MCP 負責從 Reddit、Hacker News 抓資料；Skill 負責篩選互動分數 4 以上的、分類成 5 種類型、推薦 2-3 個題材。

### Skill 的五種設計模式（Anthropic 歸納）

| 模式 | 描述 | 例子 |
|------|------|------|
| 按步驟執行 | 有順序的流程，步驟間有依賴 | 每日規劃：回顧→查週目標→建 daily note |
| 跨工具協作 | 串接多個工具，資料在系統間流動 | 建 GitHub repo → 設定 CI/CD → 通知 Slack |
| 迭代校稿 | 做一版、檢查、改、再檢查 | 內容校稿：初稿→簡繁轉換→品質檢查→不過關回去改 |
| 條件分支 | 同一目標，根據情況走不同路徑 | SEO 研究：依輸入決定查關鍵字還是分析競爭對手 |
| 嵌入專業知識 | 把領域知識寫進去，不只是工具操作 | Code review 標準：命名規則、測試覆蓋率要求 |

### Anthropic 官方撰寫哲學

> 「Try to explain to the model why things are important in lieu of heavy-handed musty MUSTs.」（用原因代替命令）

- **壞例**：`ALWAYS 在執行命令前顯示命令內容。NEVER 直接執行。`
  → Claude 只照做這兩條規則，遇到沒覆蓋的情境不知所措
- **好例**：`執行命令前先顯示內容，因為使用者需要確認安全性。未經確認就執行可能造成不可逆的損害。`
  → Claude 理解「為什麼」，能在灰色地帶舉一反三

唯一例外：格式規範（「輸出一定要用這個模板」）這種機械性要求，直接寫死即可。

Agent Skills 已成為跨平台開放標準（30+ 工具採用），詳見 [[claude-skills-deep-dive]]。

### Skill 迭代案例（WenHao 的 `/daily`）
WenHao 的每日規劃 Skill 已到 v6，每版都是使用中發現不足才改：
- v1：基本步驟
- v2：加入內容發現系統整合
- v3：修正週進度計算邏輯
- v4：加入自動觸發（週二提醒週會）
- v5：加入 iPhone 輕量模式（偵測環境跳過 Python 步驟）

「好的工作流是長出來的，不是設計出來的。」

---

## 補充資料

來源：《Agent Skill 實戰攻略，從入門到進階》（2026-03-06，Ray 貓）

### Description 驅動的情境觸發機制

Agent Skill 最核心的改變：替提示詞加上「說明（Description）」，告訴 Agent **只有在特定情境下才需要讀取這個 Skill**。

類比：查閱百科全書時先看目錄，不是把上萬頁全部翻完——Skill 替所有提示詞加上了說明目錄。你可以一次準備幾千個 Skill，當任務來臨時 AI 先看說明判斷：「在這幾千個技能裡，我現在只需要讀取這一個。」

**兩個顛覆性好處**：
1. **徹底全自動化**：不再需要手動找提示詞然後複製貼上，AI 自行比對目錄、調用對應 SOP
2. **自動維護與自我進化**：AI 可以根據任務過程中遇到的問題，自行維護並修改 Skill 內容，實現自我迭代

**Description 撰寫的黃金法則**（來源：官方文件 + 社群實踐）：
- **壞例**：`Helps with PDFs.`（太泛，Claude 不知道什麼時候用）
- **好例**：`Extracts text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.`（明確的「什麼時候用」）
- description 應該同時包含「做什麼」和「什麼時候用」兩個維度

### Skill 知識傳承的意義

自我進化機制讓 AI 具備「知識傳承能力」：
- 第一次探索新任務：2 小時 + 人類輔助，摸索後寫成 Skill
- 第二次執行：半小時，遇問題自己解決並記錄
- 第三次執行：10 分鐘，越做越順

**本質**：你的 AI 助手不再是每次重開機就失憶的金魚，而是能不斷累積專案經驗、跟著你一起進化的超級員工。
