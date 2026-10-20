---
title: Wiki 內容對齊檢查報告 — 2026-05-21（第 2 批）
date: 2026-05-21
tags: [pkm/workflow]
type: note
status: processed
---

# Wiki 內容對齊檢查報告 — 2026-05-21（第 2 批）

## 整體狀況

- 掃描條目：30 篇（第 1 批已掃描 7 篇，此為完整庫第二輪）
- 含補充資料段落：30 篇（共 62 個段落）
- 對齊：60 個 ✅ / 邊緣：2 個 ⚠️ / 不對齊：0 個 ❌

---

## ⚠️ 邊緣段落（建議人工判斷）

### 1. agent-skill-design-patterns.md → 補充段落「Hermes 四層記憶系統（Skill 設計的參考框架）」

**T 層主題：** Google 5 大 Skill 設計模式（Tool Wrapper / Generator / Reviewer / Inversion / Pipeline）——核心在於「Skill 裡的內容邏輯該怎麼設計」。

**補充段落主題：** 此補充段落整體標題為「第六種模式——自我成長型（Self-Evolution）」，內含三個子節：
1. `### 第 6 種設計模式：自動生成型 Skill（Learning Loop）` — Hermes 提出的從任務自動生成 Skill 的第 6 種模式 → **✅ 對齊**（正當地補充第 6 種模式）
2. `### Hermes 四層記憶系統（Skill 設計的參考框架）` — Hermes 的四層記憶架構表格 → **⚠️ 邊緣**
3. `### OpenClaw vs Hermes 的 Skill 設計哲學差異` — 靜態配置 vs 自動生成的對比表 → **✅ 對齊**（比較兩種 Skill 設計哲學）

**邊緣原因（子節 2）：**
- 「Hermes 四層記憶系統」的核心主題是 Hermes 的記憶架構本身，而非 Skill 設計模式
- 雖然文末有「設計洞察」連結到 Claude Skills Progressive Disclosure，這個連結有說服力，但主表格（4層×3欄）的主角是 Hermes 的記憶層級，不是 Skill 設計
- 相同的四層記憶系統內容已在 `[[hermes-agent]]` 條目中有完整說明

**選項 A：保留**（理由：「設計洞察」段直接連結到 Progressive Disclosure，提供跨工具設計比較視角，有獨特參考價值；且整個補充段落標題已定位為「第 6 種模式」，此子節是機制說明）

**選項 B：精簡**（將表格縮短，只保留「設計洞察」的 1-2 句話 + wikilink `[[hermes-agent]]`，避免與 hermes-agent.md 重複）

---

### 2. dual-tool-strategy.md → 補充段落「跨工具 AI 協作：AGENTS.md 作為單一維護點」

**T 層主題：** 雙工具策略：Gemini CLI 探索（免費）+ Claude Code 執行，省 60-70% 費用——聚焦於兩工具的分工與成本效益。

**補充段落主題：** 跨工具 AI 協作架構——以 AGENTS.md 作為 6 種 AI 工具（Claude Code / Gemini CLI / Codex CLI / VS Code Copilot / Cursor / Windsurf）的單一維護點，含各工具讀取的 Context 檔案對照表、@import 機制、.claude/rules/ 路徑規則、驗收自我測試。

**邊緣原因：**
- 補充段落的核心服務是「如何管理多工具 Context 檔案」，主角是 AGENTS.md 設計原則，而非 Gemini + Claude 的成本分工策略
- 補充段落覆蓋 6 種工具，已超出「雙工具」策略的範疇
- 來源（`cross-tool-ai-setup.md`）與第 1 批 alignment-check ⚠️ 項目 #4 相同，當時在 `agents-md-guide.md` 也發現跨工具 Context 對照內容（用戶判斷：保留，無重複）

**選項 A：保留**（理由：使用雙工具策略的用戶同樣需要跨工具 Context 管理；B2B Manager 實測數據（315行→250行）和「單一維護點」原則對雙工具使用者有直接實務價值）

**選項 B：搬移**（將此補充移至 `claude-code-file-management.md` 或 `agents-md-guide.md`，dual-tool-strategy.md 留一句摘要 + wikilink）

---

## ✅ 對齊良好（60 個段落）

### 1. agent-skill-design-patterns.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| Tool Wrapper 官方範例程式碼（數位時代）| ✅ | 直接深化 T 層第一種設計模式的具體實作 |
| 第 6 種設計模式：Learning Loop | ✅ | 以 Hermes 為例正當地補充第 6 種設計模式 |
| OpenClaw vs Hermes 的 Skill 設計哲學差異 | ✅ | 對比兩種 Skill 設計哲學（靜態配置 vs 自我成長），延伸 T 層設計模式視角 |

### 2. agentic-engineering-sop.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| CEO + 員工模式與複雜任務挑戰（三大挑戰表）| ✅ | 直接延伸 T 層「監督 AI 員工的工程師」核心概念，以大型任務三大挑戰深化實戰框架 |

### 3. ai-pair-skill.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 技術架構 + 已知問題 + 功能邊界 + 真實案例（ai-pair GitHub README）| ✅ | 全面延伸 T 層「異構審查」的技術實作與邊界 |

### 4. claude-code-commands-2026.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 版本對照 + /ultrareview計費 + /goal注意 + /batch深度 + .claude/commands/ vs Skills + Plugin Marketplace + 學習清單 | ✅ | 全面深化 T 層 12 個新命令的操作細節與邊界 |

### 5. claude-code-commands-reference.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 官方完整命令清單（2026-05 版）| ✅ | 直接補充 T 層速查表的最新版本 |
| 13 大類完整指令（數位時代整理版）| ✅ | 延伸速查視角，補充「為什麼要學指令」的動機 |
| 三層命令體系說明 + 常見混淆 + 新手必記（DeTools v2.1.74）| ✅ | 補充具體版本號 + 常見誤解釐清，服務 T 層「速查不用背」目標 |

### 6. claude-code-context-management.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| Auto Memory 完整機制（儲存位置/載入機制/管理/版本要求/自訂位置）| ✅ | 直接深化 T 層「Auto Memory 機制詳解」部分，補充操作細節 |

### 7. claude-code-file-management.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| CLAUDE.md 完整讀取順序 + Rules 系統 + Skills commit 策略 + 完整決策表 | ✅ | 直接延伸 T 層「哪些設定檔該 commit」的四層架構判斷 |

### 8. claude-code-glossary.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 超級餐廳比喻完整對照 + 術語演進時間軸 | ✅ | 比喻強化術語的直觀理解；時間軸提供術語的演進背景，服務 T 層速查目標 |

### 9. claude-code-obsidian-setup.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| obsidian-second-brain：Karpathy LLM Wiki 的進化版 | ✅ | 延伸 T 層「Claude Code + Obsidian 整合方案」，提供社群進化版參考 |

### 10. claude-code-plugins-guide.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| oh-my-claudecode（OMC）Teams-first 多代理編排框架 | ✅ | OMC 是建立在 Plugin 系統上的具體框架，作為 T 層「Plugin 生態」的代表性範例 |

### 11. claude-code-tools.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 完整工具清單 + 為什麼要搞懂 Tools + Bash vs Monitor + LSP + AskUserQuestion | ✅ | 直接深化 T 層各工具的選擇邏輯與邊緣行為 |

### 12. claude-md-guide.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| Boris Cherny 六大核心引擎 + 任務管理 + 核心原則 + 複利效應 | ✅ | 直接實現 T 層「六大核心引擎」的完整呈現 |
| 三種常見錯誤寫法 + 跨工具通用性 | ✅ | 以反例鞏固 T 層「撰寫技巧」的操作邊界 |
| Karpathy 12 條規則由來 + 原版盲點 + 完整清單 | ✅ | 直接實現 T 層「Karpathy 12 條規則」的完整內容 |
| 數位時代整理版重點 | ✅ | 補充整理視角，服務不同閱讀習慣 |
| HKT 實驗室：負面清單優於正面清單哲學 | ✅ | 深化 T 層撰寫技巧的設計哲學 |

### 13. claude-skills-asset-building.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| MAPS 四維度拆解 + 軟編排四條工程規則 + 資源庫管理 | ✅ | 直接延伸 T 層「MAPS 框架 + 漸進式披露機制」 |

### 14. claude-skills-ecosystem.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| Skills 生態元年背景 + 安裝演進 + 六種角色組合 + 警告 + Cheat Sheet | ✅ | 全面深化 T 層四檔分類 + 安裝演進的生態全景 |
| Research & Knowledge Super-Skill（知識工作者導向大型套件）| ✅ | 屬於「第四檔：領域利基」的代表性案例，直接符合 T 層四檔分類框架 |
| Scientific Agent Skills（科學研究天花板套件）| ✅ | 同上，另一個領域利基代表 |

### 15. cognitive-hub.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 分層架構 + Hub Index + 治理層級 + 核心工作模式 + 五條設計原則 + 維護成本 + OpenClaw 關係 + 快速開始 | ✅ | 完整實作 T 層「外部化活記憶」的技術架構 |
| 知識囤積 vs 知識內化的根本差距（Kevin Shih）| ✅ | 提供 T 層「記憶先於行動」的批判性對話：工具設計再好，思考不足就會淪為囤積場所——「對 Cognitive Hub 設計的啟示」段落明確連回 T 層 |

### 16. gemini-cli-subagent.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 實測心路歷程：Claude 派 Gemini 當小弟 | ✅ | 以真實使用者視角佐證 T 層設定方案的實際效果 |

### 17. gstack-framework.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 安裝與常見問題 | ✅ | 直接支援 R 層使用操作 |
| 31 項技能六大分類（vocus 版完整解析）| ✅ | 補充 T 層 Sprint 流程的完整技能清單 |
| v0.19+ 新增功能（/pair-agent / checkpoint / 多平台 / 並行 Sprint）| ✅ | 官方版本更新，直接延伸 T 層技能框架 |

### 18. harness-engineering.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 補充視角一：六層架構（WenHao Yu）| ✅ | 以六層架構深化 T 層 Guides × Sensors 框架，提供 LangChain 實測數據 |
| 補充視角二：第三個維度（溫煜鈞）| ✅ | 以「三代範式對比」的讀者視角強化 T 層「Harness 是第三維度」論點 |
| 補充視角三：多 Agent 對話的 Harness 技巧 | ✅ | 多 Agent 場景的 Harness 實作，直接延伸 T 層「多代理 Harness」應用 |
| 補充視角四：Harness Coding 的「第一讀者」洞察 | ✅ | 「第一讀者是 AI」的視角直接強化 T 層 Harness 設計理念 |

### 19. hermes-agent.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| OpenClaw vs Hermes 完整對比（設計哲學 + 安裝 + 場景 + 決策樹）| ✅ | 幫助用戶選擇工具，直接服務 B 層「適用場景」；決策樹讓 T 層「越用越懂你」的差異化更具體 |

### 20. impeccable-style-ai-coding-guide.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 23 命令清單 + 7 個領域參考文件 + 反模式規則 + CLI 掃描 + 11 種工具安裝 | ✅ | 完整實現 T 層「23 命令 + 規範轉換 AI 可理解的程式碼憲法」 |

### 21. llm-cost-optimization-strategies.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| Token 通膨現象 + 第五層：知識萃取（Claude 設計，腳本執行）| ✅ | 補充 T 層四層策略的第五層（邊際成本降至零），並提供 Token 通膨的結構性背景 |

### 22. llm-wiki-karpathy-vs-nashsu.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| nashsu 完整技術細節（4 訊號知識圖譜 + HTTP API + Vector Search + 驗證規模）| ✅ | 直接支援 T 層對 nashsu 「根本升級」的論點，提供技術規格佐證 |

### 23. maps-framework.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 四大框架關係 + 功能對照 + 場景速查 + 決策樹 + 核心原則 | ✅ | 以決策樹和場景表將 T 層四維羅盤轉化為可操作的選擇指南 |
| MAPS 四維度原廠定義 + 核心主張（原廠語言）| ✅ | 補充原廠一手資料，強化 T 層論點的權威性 |

### 24. mattpocock-claude-skills-real-engineers.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| Workflow 導向 vs 語言導向 + AI 開發三層架構中的定位（DeTools）| ✅ | 從更高視角（三層架構定位）深化 T 層四大失敗模式的根本原因 |

### 25. notebooklm-py-cli-tool.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| notebooklm-mcp（MCP 方案）定位對比 + 技術細節 + 安裝 | ✅ | 提供 MCP 替代方案比較，直接服務 T 層「NotebookLM 作知識庫 + Claude Code 作執行引擎」的選型決策 |
| Antigravity + NotebookLM MCP 組合（大腦+雙手）+ 兩個實戰案例 + Deep Research 優勢 | ✅ | 以具體案例（批量 Deep Research / 財報分析）深化 T 層核心論點 |

### 26. notebooklm-skill-research-pipeline.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| MCP Server 完整工具列表（13 個）+ 多客戶端支援設定 | ✅ | 補充 T 層 Pipeline 工作流的 MCP 接入技術細節 |

### 27. obsidian-visual-skills.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 三個 Skill 詳細功能 + 安裝方式 + 中文字體注意 | ✅ | 直接深化 T 層三種視覺 Skill（Canvas/Excalidraw/Mermaid）的操作規格 |

### 28. superpowers-framework.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 補充視角：設計哲學深度解析（brainstorming / verification / code-review / writing-skills TDD 對應）| ✅ | 深化 T 層各 Skill 的設計取捨，特別是反藉口機制的具體實作 |
| 官方安裝方式更新 + 七步驟確認 + Skills 完整庫 + 社群 | ✅ | 提供官方確認版本規格，補充 R 層安裝操作 |

### 29. zerospec.md
| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 作者第一手說明（ZeroSpec 誕生脈絡）| ✅ | 提供 T 層論點的作者原始動機，強化「AI 開工前的技術交接單」定位 |
| ZeroSpec 固定四類最小必要上下文 + 任務路由表 | ✅ | 直接實作 T 層「零依賴 Markdown 框架」的具體內容規格 |

---

## 建議修正優先順序

### 低優先（邊緣，建議人工判斷）

1. **agent-skill-design-patterns.md → Hermes 四層記憶系統子節**
   - 選項 A（保留）：維持現狀，因「設計洞察」的 Progressive Disclosure 類比有獨特價值
   - 選項 B（精簡）：將表格替換為 1-2 句話 + `[[hermes-agent]]` wikilink，避免與 hermes-agent.md 重複

2. **dual-tool-strategy.md → AGENTS.md 作為單一維護點補充**
   - 選項 A（保留）：維持現狀，因雙工具用戶確實需要跨工具 Context 管理的實作指引
   - 選項 B（搬移）：將此補充移至 `claude-code-file-management.md`，dual-tool-strategy.md 留一句摘要 + wikilink

---

*第 2 批掃描覆蓋所有剩餘含補充資料段落的 wiki 條目，與第 1 批（2026-05-20）合計完成全庫對齊掃描。*
