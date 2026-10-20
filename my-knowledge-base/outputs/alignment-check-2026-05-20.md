---
title: Wiki 內容對齊檢查報告 — 2026-05-20（第 1 批）
date: 2026-05-20
tags: [pkm/workflow]
type: note
status: processed
---

# Wiki 內容對齊檢查報告 — 2026-05-20（第 1 批）

## 整體狀況

- 掃描條目：7 篇
- 含補充資料段落：7 篇（共 14 個段落）
- 對齊：10 個 ✅ / 邊緣：3 個 ⚠️ / 不對齊：1 個 ❌

---

## ❌ 不對齊段落（需搬移）

### agent-skills-toolkit.md → 補充段落「研究知識超級 Skill（research-knowledge-super-skill）」

**T 層主題：** agent-skills 是 Addy Osmani 開源的生產級 AI 開發技能包（22 個 Skill，軟體工程生命週期導向），核心在於將 Google 工程文化編碼進可組合的 Skill 工作流。

**補充段落主題：** `get-zeked/research-knowledge-super-skill` — 一個整合 Perplexity Computer 研究技能與知識管理技能的大型合集 Skill，偏向知識工作者研究分析需求（知識圖譜、內容提取、統計分析、RAG）。

**判定：❌ 不對齊**

- T 層聚焦 Addy Osmani 的工程開發生命週期框架（DEFINE→PLAN→BUILD→VERIFY→REVIEW→SHIP）
- 此補充段落的主角是另一個完全不同的 Skill 套件，作者不同（get-zeked vs addyosmani）、定位不同（研究知識管理 vs 工程品質紀律）
- 補充段落末尾已自我說明：「相較於 agent-skills 的工程開發生命週期導向，research-knowledge-super-skill 更偏向知識工作者的研究分析需求」——這句話本身就是「主題不同」的明證

**建議歸屬目標：**
1. **最適合**：`claude-skills-ecosystem.md`——此條目負責 Skills 生態系全景，收錄不同類型的 Skill 套件最自然
2. **次選**：`notebooklm-skill-research-pipeline.md`——與研究知識 pipeline 主題接近
3. **若無適合條目**：獨立建立 `research-knowledge-super-skill.md`

---

## ⚠️ 邊緣段落（建議人工判斷）

### 1. agents-md-guide.md → 補充段落「跨工具 Context 檔案對照表 + 推薦多工具標準架構」（來源：cross-tool-ai-setup.md）

**T 層主題：** AGENTS.md 的價值在於「不可推斷的隱性知識密度」——只寫 AI 反覆犯錯的資訊（工具鏈指令、限制條件、任務路由）。

**補充段落主題：** 六大 AI Coding 工具的 Context 檔案對照表（Claude Code / Gemini CLI / Codex CLI / VS Code Copilot / Cursor / Windsurf），以及建立「AGENTS.md 作為單一維護點」的多工具架構方案。

**判定：⚠️ 邊緣**

- 理由（支持對齊）：AGENTS.md 的跨工具支援是其作為通用標準的重要論據，與 T 層「跨工具通用標準」的定位有直接關聯；對照表說明了 AGENTS.md 正成為事實標準（`趨勢：AGENTS.md 正成為跨工具事實標準`），直接支撐 T 層論點
- 理由（存疑）：此段落的核心服務是「如何設定多工具環境」，已超出 AGENTS.md 設計原則的範疇，與 `claude-code-file-management.md`（相關連結已指向）的主題高度重疊
- **建議**：若 `claude-code-file-management.md` 尚未涵蓋跨工具對照，此段落可保留在此；若已涵蓋，考慮搬移或建立雙向 wikilink 即可

### 2. agentic-ai-learning-roadmap.md → 補充段落「工具選擇：5 大 Agentic Coding 工具比較（2026）」（來源：WenHao Yu 文章）

**T 層主題：** 學 AI Agent 需要動手練習，7 階段學習路徑 + 兩條分流（Track A CLI Power User / Track B Agent Builder）。

**補充段落主題：** Cursor、Antigravity、GitHub Copilot、Claude Code、Codex CLI 五大工具的功能對照表、月費比較，以及 Vibe Coding vs Agentic Coding 的產品成熟度框架。

**判定：⚠️ 邊緣**

- 理由（支持對齊）：學習路徑需要選工具，工具比較幫助學習者決策（Track A 需選 CLI 工具），與學習導向有間接連結
- 理由（存疑）：工具比較是靜態的「市場快照」，核心在選型決策而非學習方法；T 層強調的是「動手練習的學習論」，與工具比較的主題距離較遠；wiki 中已有 `dual-tool-strategy.md` 等工具選擇相關條目
- **建議**：此段落移至 `claude-code-commands-reference.md` 或獨立建立工具比較條目（如 `agentic-coding-tools-comparison.md`）更合適；或在此條目保留精簡摘要（僅一兩句話），詳細內容另建條目

### 3. claude-skills-guide.md → 補充段落「跨工具開放標準現況」（來源：Claude Skills 教學文章，WenHao Yu）

**T 層主題：** Claude Skills 的兩種用法：能力包型（封裝判斷邏輯）vs 軟編排型（調度多 Agent 協作），兩者都比 MCP 省 Token、比直接 Prompt 更可復用。

**補充段落主題：** Agent Skills 開放標準已被 30+ 工具採用（Cursor、VS Code Copilot、Gemini CLI、OpenAI Codex、JetBrains Junie），說明跨平台可用性。

**判定：⚠️ 邊緣**

- 理由（支持對齊）：開放標準的廣泛採用佐證了 Skills 相較 MCP/直接 Prompt 的可復用性優勢，與 T 層「更可復用」的論點有直接連結
- 理由（存疑）：此段內容與 `claude-skills-deep-dive.md` 的「跨平台開放標準（agentskills.io）」高度重疊，兩個條目都寫了相同的事實；在 `claude-skills-deep-dive.md` 的 T 層中，跨平台標準是核心主題，但在 `claude-skills-guide.md` 中只是側論
- **建議**：此段落可簡化為一句話 + wikilink 指向 `claude-skills-deep-dive.md`，避免重複

---

## ✅ 對齊良好

### 1. agents-md-guide.md

| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 補充視角：AI-Ready 倉庫設計三大支柱（支柱一/二/三）| ✅ | 直接深化 T 層的五原則與 AI-Ready 設計，提供具體範例 |
| 研究補充細節（測試條件、量化成本、LLM 謹慎機制、例外情況）| ✅ | 完整延伸 T 層「LLM 自動生成版本反而有害」的論點，補充論文量化數據 |
| AGENTS.md 最值得寫的內容 | ✅ | 具體說明「不可推斷的隱性知識」的範例，直接落實 T 層核心論點 |

### 2. claude-skills-guide.md

| 補充段落 | 判定 | 說明 |
|---------|------|------|
| SKILL.md 完整結構規範 + Progressive Disclosure 三層機制 + 安裝位置 + 撰寫 Description 黃金法則（來源：高見龍）| ✅ | 深化 R 層「能力包型：把判斷邏輯寫進 SKILL.md」的操作步驟，提供具體規格 |
| MCP 是廚房、Skills 是食譜 + 五種設計模式 + Anthropic 撰寫哲學 + 迭代案例（來源：WenHao Yu）| ✅ | 深化 T 層兩種用法的比較與實作哲學，案例佐證 B 層「比 MCP 省 Token、更可復用」 |
| Description 驅動情境觸發機制 + Skill 知識傳承意義（來源：Ray 貓）| ✅ | 從使用者認知角度強化 T 層「更可復用」的核心價值，自進化機制是能力包型的關鍵延伸 |

### 3. claude-skills-deep-dive.md

| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 官方 Frontmatter 完整欄位表 + `!` 動態注入語法 + Skill 內容生命週期 + 數量限制（來源：Anthropic 官方文件 code.claude.com）| ✅ | 核心機制的官方規格細節，直接深化 T 層「按需載入設計」的技術底層 |
| 官方定義精確架構 + 跨平台可用性 + 官方內建 Skills + 安全注意事項（來源：Anthropic 官方文件 platform.claude.com）| ✅ | 補充 T 層「跨平台開放標準」的官方說法與限制細節，與 T 層高度吻合 |
| 三週實測數據（能力包型/軟編排型）+ 官方 vs 軟編排關鍵差異 + 使用前置條件 + 常見問題（來源：Axton Liu）| ✅ | 實測數據佐證核心機制的實際效果，前置條件與限制是使用者必知資訊 |

### 4. agent-skills-toolkit.md

| 補充段落 | 判定 | 說明 |
|---------|------|------|
| AGENTS.md 說明（供其他 AI Coding Agent 使用）| ✅ | 說明 agent-skills 套件如何跨工具安裝與使用，直接延伸 B 層「支援多平台」的論點 |

### 5. agentic-ai-learning-roadmap.md

| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 等級評估：Agentic Engineering 的 8 個等級 | ✅ | 提供「我現在在哪裡」的定位工具，與學習路徑「要學什麼」互補，共同服務「系統性學習」的 T 層論點 |
| Hello-Agents 中文學習教程（Datawhale 開源）| ✅ | 同類型學習資源，提供與主幹路徑的對應關係，擴充學習者的選擇 |

### 6. karpathy-knowledge-base-method.md

| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 為什麼 Obsidian 是 AI 知識管理最佳載體 + Ingest/Query/Lint 三核心動作 + 五個實戰工作流 + Vault 結構 + 設置方式（來源：科技翰林院）| ✅ | 從技術角度深化 R 層操作步驟，特別是 Ingest→Query→Lint 三核心與 T 層論點直接對應 |
| 六步驟複製流程（一般人版）+ 可直接作為 Prompt 的操作指引（來源：Fox Hsiao / 鉅亨號）| ✅ | 降低門檻的實作指引，佐證 T 層「人只負責丟資料，AI 全權負責整理」的可行性 |
| Karpathy vs LYT 框架骨架對照 + 分歧核心 + 三個功能缺口 + HN 兩大反對聲音（來源：WenHao Yu）| ✅ | 批判性視角深化 T 層論點的邊界與侷限，符合知識條目的客觀性要求 |

### 7. spec-driven-ai-workflow.md

（此條目無獨立「補充資料」段落；補充視角段落均直接延伸 T 層與 R 層）

| 補充段落 | 判定 | 說明 |
|---------|------|------|
| 補充視角：角色定位比喻（OpenSpec=PM / Superpowers=Tech Lead）| ✅ | 強化 T 層「規格工具 + Superpowers 分別解決兩個問題」的直觀理解 |
| 補充視角：OpenSpec + Superpowers 完整五階段實踐（高見龍）| ✅ | 完整的 Step-by-step 操作，直接深化 R 層工作流，並補充搭配彈性說明 |

---

## 建議修正優先順序

### 高優先（明確不對齊，需搬移）

1. **agent-skills-toolkit.md → 研究知識超級 Skill 段落**
   - 動作：將此段落搬移至 `claude-skills-ecosystem.md`（最適合）或另建 `research-knowledge-super-skill.md`
   - 搬移後在 agent-skills-toolkit.md 留一句 wikilink 摘要即可（如：「相關：[[research-knowledge-super-skill]] — 知識工作者導向的大型研究 Skill 套件」）

### 中優先（邊緣，建議簡化或搬移）

2. **agentic-ai-learning-roadmap.md → 工具比較段落**
   - 動作：精簡為 2-3 句摘要 + wikilink，詳細表格移至更合適條目（或建立 `agentic-coding-tools-2026.md`）

3. **claude-skills-guide.md → 跨工具開放標準段落**
   - 動作：簡化為一句話 + `[[claude-skills-deep-dive]]` wikilink，避免與 deep-dive 條目重複

### 低優先（邊緣，人工確認後決定）

4. **agents-md-guide.md → 跨工具 Context 檔案對照段落**
   - 動作：確認 `claude-code-file-management.md` 是否已涵蓋；若有，加 wikilink 並精簡此段；若無，可保留
