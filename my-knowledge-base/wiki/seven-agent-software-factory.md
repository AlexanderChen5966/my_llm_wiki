---
title: 七代理人軟體工廠：從 Vibe Coding 升級為專家級開發流程
date: 2026-06-17
tags: [ai/agent, ai/claude, ai/prompt-engineering]
source: https://www.blocktempo.com/claude-code-software-factory-7-agents/
type: article
status: processed
asset_value: 65
---

## T（核心論點）

Vibe Coding 的根本問題不是 AI 不夠聰明，而是**工作流失敗**——把產品分析師、架構師、後端/前端工程師、測試、審查員全部塞進一個對話，錯誤的假設會從規格蔓延到資料庫、API、UI。解法是把工作拆給七個專責代理人，每個擁有單一職責、乾淨的 context 視窗、嚴格的修改半徑限制。

## B（適用場景）

**適用：**
- 已掌握 Claude Code 基本操作，想大幅減少 AI 監督成本的開發者
- 有複雜垂直切片需求（後端+前端+測試+驗證）的個人/小團隊
- 想把專家知識（支付/認證/CI 規則）沉澱到代理人配置中

**不適用：**
- 一次性小功能（工廠 setup 成本高於收益）
- 完全不熟悉 CLAUDE.md 或 Claude Code 的新手

## R（七個代理人設計）

### 完整代理人架構

| 代理人 | 職責 | 工具限制 | 關鍵規則 |
|--------|------|----------|---------|
| **研究員（Researcher）** | 掃程式碼庫、標出風險、找類似功能 | Read/Grep/Glob（**唯讀**）| 永遠先跑；**只問不猜** |
| **故事撰寫者（Story Writer）** | 把構想變使用者故事 + 驗收標準 | Read（**唯讀**）| **人類審核點 1**：你核准才往下 |
| **規格撰寫者（Spec Writer）** | 把故事變技術簡報（資料模型/API/前端/測試） | Read/Grep/Glob（**唯讀**）| **人類審核點 2**：你核准才動任何檔案 |
| **後端建造者（Backend Builder）** | API 路由/Services/DB/Tasks + 單元測試 | Read/Edit/Write/Bash（**僅後端資料夾**）| 不碰 React；跑完 typecheck+lint+tests |
| **前端建造者（Frontend Builder）** | 元件/頁面/Hooks/UI 測試 | Read/Edit/Write/Bash（**僅前端資料夾**）| 先讀後端摘要再動工；不發明端點 |
| **測試驗證者（Test Verifier）** | 為每條驗收標準寫驗收測試 | Read/Edit/Write（**僅測試檔**）/Bash | 不修程式碼；失敗→報告→丟回建造者 |
| **實作驗證員（Validator）** | 實作 vs 故事/簡報的落差分析 | Read/Grep/Glob（**唯讀**）| 輸出按 Critical/Important/Minor 分組；不會發明問題 |

### 完整流程（三個人類審核點）

```
研究員掃程式碼庫
  ↓
故事撰寫者產出使用者故事
  ⏸ [人類審核點 1：核准故事]
  ↓
規格撰寫者產出技術簡報
  ⏸ [人類審核點 2：核准技術簡報（此時抓出「把 ID 存在記憶體」等紅旗）]
  ↓
後端建造者實作 → 前端建造者讀後端摘要後實作（平行）
  ↓
測試驗證者寫驗收測試 → 若失敗丟回對應建造者修復
  ↓
實作驗證員最終比對落差
  ⏸ [人類審核點 3：審查並開 PR]
```

### CLAUDE.md：跨 session 的永久記憶

每次開啟 Claude Code 都是從零記憶開始。CLAUDE.md 是修正這個問題的機制：

- 放在 repo 根目錄，每個對話自動載入
- 內容：技術棧、常用指令、架構規則、**不要做的清單**（「不要加 cron——用 BullMQ」）
- 建議長度：100-300 行
- **每次 AI 犯了讓你驚訝的錯誤，問自己：如果 CLAUDE.md 有這條規則，能避免嗎？加進去。**

### 上下文漂移的處理原則

對話中的錯誤假設不能靠「補丁」修復——會讓舊假設和新假設並行飄在四處。

- **小錯字**：直接 inline 修正
- **架構假設錯了**：**丟棄整個對話，從頭開始**，把正確假設烙進第一個 prompt

> 一個有正確心智模型的乾淨對話，永遠勝過一個打了補丁的對話。

### 8 步週末設定清單

```
1. 安裝 Claude Code
2. 建立目錄結構：
   .claude/agents/
   .claude/skills/feature-factory/
   .claude/skills/build-with-tests/
   .claude/hooks/
3. 寫 CLAUDE.md（100-300 行）
4. 用 /agents 指令建立 7 個代理人
5. 建立 feature-factory orchestrator skill
6. 建立 build-with-tests skill
7. 加 pre-commit hook（阻擋 .env/.key/.pem）
8. 跑一個真實功能走完整條鏈
```

## C（關鍵行動）

- [ ] 比較 superpowers 七步 Mandatory Workflow vs 軟體工廠七代理人的適用情境：前者強制 TDD 紀律，後者強調角色隔離
- [ ] 評估 CLAUDE.md 目前是否足夠完整（有沒有記錄讓 AI 犯過的假設？）
- [ ] 先用單一小功能跑完整鏈，觀察哪個環節卡住，再加規則

## 相關連結

- [[vibe-to-agentic-engineering]] — PEV Loop + AGENTS.md 升級三步驟（同主題不同角度）
- [[superpowers-framework]] — Superpowers：15 個 Skill 強制 TDD 工程紀律（互補）
- [[agentic-engineering-sop]] — Agentic Engineering 六階段 SOP（spec/plan/tasks 三層落檔）
- [[claude-code-context-management]] — Context 管控全攻略（與 CLAUDE.md 機制互補）
- [[synthesis-agentic-engineering]] — 核心：角色轉型（執行者→監督者），Understanding 不可外包
