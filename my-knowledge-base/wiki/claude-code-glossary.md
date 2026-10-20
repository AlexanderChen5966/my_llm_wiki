---
title: Claude Code 術語大全：38 個 AI 開發關鍵詞
date: 2026-05-14
tags: [ai/claude, ai/agent]
source: https://hackmd.io/@BASHCAT/Hk5IjaLdbg
type: article
status: processed
asset_value: 65
---

Claude Code 世界的核心術語索引，以「超級餐廳」比喻串聯 38 個概念——從最底層的 LLM/Token 到最上層的 Harness Engineering。

## T（核心論點）
術語不是孤立的，而是層層堆疊的：Prompt → Context → Agent → Skill → Harness，每一層解決前一層的局限。看懂術語之間的關係，才能選對工具解決對的問題。

## B（適用場景）
**適用：** 剛接觸 Claude Code 的初學者；跨團隊溝通需要統一術語時

## R（核心術語速查）

### 基礎層（AI 基礎概念）
| 術語 | 一句話定義 | 比喻 |
|------|-----------|------|
| **LLM** | AI 的大腦，海量文字訓練的語言理解引擎 | 看過上萬本食譜的主廚 |
| **Token** | AI 處理文字的最小單位，也是計費單位 | 食材的份量單位 |
| **Prompt** | 你對 AI 說的話/下的指令 | 點餐內容 |
| **System Prompt** | 對話開始前塞給 AI 的「出廠設定」 | 廚房牆上的廚師守則 |
| **Context Window** | AI 一次能看到和記住的資訊總量上限 | 廚房工作台大小 |
| **Agentic Loop** | 思考→行動→觀察的循環直到任務完成 | 廚師接到派對任務後的工作循環 |
| **Tool Use** | AI 呼叫外部功能動手完成任務的能力 | 廚師使用烤箱、攪拌機等廚具 |

### Claude Code 架構
| 術語 | 定義 |
|------|------|
| **Claude Code** | Anthropic 官方 CLI AI 開發工具，200K/1M tokens Context Window |
| **CLAUDE.md** | 專案規則文件，Claude 啟動時自動讀取 |
| **AGENTS.md** | 跨工具通用規則標準（Copilot/Cursor/Codex 皆支援） |
| **Skills** | 封裝的判斷邏輯模組，靜態載入省 Token |
| **MCP** | Model Context Protocol，動態工具連線（即時外部資料） |
| **Subagent** | 獨立 Context Window 的子代理，完成後只回傳摘要 |
| **Hooks** | 特定事件觸發的 Shell 命令（如 pre-commit） |
| **Plan Mode** | Shift+Tab×2 切換，先規劃再動手，省 Token |

### 進階概念
| 術語 | 定義 |
|------|------|
| **Context Engineering** | 管理模型能看到什麼資訊（2025 代範式） |
| **Harness Engineering** | 設計 Agent 工作系統：Guides（前饋）× Sensors（回饋）（2026 代範式） |
| **Prompt Engineering** | 研究怎麼說（2022-24 代範式） |
| **RAG** | 檢索增強生成，讓 AI 讀取外部知識庫 |

## 相關連結
- [[harness-engineering]] — Harness Engineering 深度解析
- [[claude-code-tools]] — Claude Code 工具使用邏輯
- [[agent-skills-vs-mcp]] — Skills vs MCP 選擇判斷

## 補充資料（來源：Claude Code 術語大全：Agent、MCP、Skills、Hooks，一篇搞懂 38 個 AI 開發關鍵詞.md）

### 「超級餐廳」比喻完整對照

原文以超級餐廳串聯 38 個術語，分為五個維度：

**顧客層（使用者）**
- **User**：你，點餐的人
- **Human Turn / Assistant Turn**：顧客說話 vs 服務生回應，對話的基本節奏

**廚房層（模型基礎）**
- **Temperature**：控制回應隨機性（0 = 死板精確，1 = 有創意但有時出錯）
- **Sampling**：溫度的實現機制，從機率分布中抽取下一個 Token
- **Hallucination**：AI 自信但錯誤地輸出沒有依據的內容（「廚師亂說庫存有龍蝦」）
- **Grounding**：把 AI 回應錨定在可驗證的事實或文件上

**工具層（外部能力）**
- **Tool Call / Function Call**：AI 決定呼叫特定工具的動作
- **Bash Tool**：Claude Code 最核心的工具，直接執行終端命令
- **Read / Write / Edit**：檔案操作三件套，Claude 的日常動作
- **WebSearch / WebFetch**：上網搜尋 / 抓取網頁內容

**流程層（工作流設計）**
- **Checkpoint**：可以回滾的工作節點（`/rewind` 退回這裡）
- **Approval Flow**：需要人工確認才能繼續的關卡
- **Parallel Execution**：多個 Subagent 同時跑，`/batch` 的核心機制

**系統層（架構設計）**
- **Spec-Driven Development**：先寫規格（CLAUDE.md / ZeroSpec），AI 照規格執行
- **Memory Layer**：跨對話持久記憶（Auto Memory / CLAUDE.md / lessons.md）
- **Guardrails**：防護邊界——NEVER 區塊、`excludeTools`、權限白名單

### 術語演進時間軸

| 年代 | 範式 | 核心術語 |
|------|------|---------|
| 2022–24 | Prompt Engineering | Prompt、Temperature、Few-shot |
| 2025 | Context Engineering | Context Window、RAG、Grounding |
| 2026 | Harness Engineering | Hooks、Sensors、Harness、Spec-Driven |
