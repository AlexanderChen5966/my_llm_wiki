---
title: Wiki 內容對齊檢查報告 — 2026-07-03
date: 2026-07-03
tags: [pkm/workflow, project/knowledge-base]
type: reference
status: processed
---

# Wiki 內容對齊檢查報告 — 2026-07-03

## 整體狀況

- **掃描條目**：234（頂層 wiki）
- **含補充資料段落**：44 篇（約 90+ 個 `## 補充資料` 段落）
- **對齊 ✅**：約 87 個段落（絕大多數）
- **邊緣 ⚠️**：4 個段落（建議人工判斷）
- **不對齊 ❌**：0 個（無需立即搬移）

> **本批（Batch 22–24）新增 25 篇條目全部為獨立條目、未附加任何「補充資料」段落**（中英重複來源是合併成一篇新條目，非補充至既有條目），故本批對齊風險為零。以下邊緣案例皆屬既有條目。

---

## ⚠️ 邊緣段落（建議人工判斷）

### [[karpathy-knowledge-base-method]]
**邊緣段落**：`## 補充資料：PKM 適合誰？建立前的投報率判斷框架` > `三不記原則 / 四維決策矩陣 / 真正需要 PKM 的三種角色`
**邊緣原因**：此段是「該不該建 PKM」的**通用決策框架**，主題比「Karpathy 的知識庫方法」更廣，不專屬 Karpathy。
- **選項 A（保留）**：讀者在學 Karpathy 方法時，順帶判斷自己適不適合，情境連貫。
- **選項 B（搬移/昇華）**：抽成獨立條目 `pkm-adoption-decision-framework.md` 或併入 [[obsidian-knowledge-base-planning]]，因其可被多篇 PKM 條目共用。

### [[agent-skill-design-patterns]]
**邊緣段落**：`Hermes 四層記憶系統（Skill 設計的參考框架）` / `OpenClaw vs Hermes 的 Skill 設計哲學差異`
**邊緣原因**：核心主體是 Hermes/OpenClaw，但以「Skill 設計參考」框架切入。
- **選項 A（保留）**：作為 Skill 設計模式的延伸對照，扣合 T 層。
- **選項 B（搬移）**：Hermes 記憶系統細節搬至 [[hermes-agent]]，此處僅留一句 wikilink 指引。

### [[claude-code-obsidian-setup]]
**邊緣段落**：`obsidian-second-brain：Karpathy LLM Wiki 的進化版`
**邊緣原因**：obsidian-second-brain 是獨立工具（也被 notebooklm-workflow 等引用），此處作為「建構選項」補充。
- **選項 A（保留）**：作為 Obsidian 建構的進階選項，情境合理。
- **選項 B（昇華）**：若未來再被 2 篇以上引用，抽成獨立條目 `obsidian-second-brain.md`。

### [[cognitive-hub]]
**邊緣段落**：`## 補充資料：知識囤積 vs 知識內化的根本差距`
**邊緣原因**：談「第二大腦核心誤區」的通用 PKM 觀念，但末段「對 Cognitive Hub 設計的啟示」已錨回主題。
- **選項 A（保留，建議）**：末段已扣回 T 層，屬合理延伸。
- **選項 B**：通用部分可與 karpathy 篇的 PKM 決策框架一起昇華成 PKM 方法論概念頁。

---

## ✅ 對齊良好

其餘約 87 個補充段落確認對齊，主要為三類（皆符合對齊標準）：
1. **同工具官方 README 深化**：agents-md-guide 跨工具表、ai-pair-skill 技術細節、superpowers/gstack 官方安裝、impeccable 23 命令、llm-wiki-karpathy-vs-nashsu 的 nashsu 技術細節等。
2. **同主題版本/功能更新**：claude-code-commands 版本對照、claude-skills-deep-dive 官方最新欄位、codex-advanced-tips 新功能等。
3. **T 層論點的延伸/案例/數據**：harness-engineering 量化數據、llm-cost-optimization 的 17 倍節省案例、claude-md-guide 的多來源 CLAUDE.md 寫法等。

---

## 建議修正優先順序

1. （可選）[[karpathy-knowledge-base-method]] 的「PKM 投報率判斷框架」→ 評估是否昇華為獨立 PKM 決策概念頁（跨篇共用價值最高）
2. （可選）[[agent-skill-design-patterns]] 的 Hermes 記憶細節 → 搬至 [[hermes-agent]]，此處留指引
3. 其餘邊緣案例維持現狀即可

> 三項皆為「可選優化」，非錯誤。目前 wiki 內容歸屬健康，無強制搬移需求。

---

*檢查方法：逐條目提取 T 層 vs 各「## 補充資料」段落主題比對（對齊/邊緣/不對齊三級）。本 skill 只產出報告，不自動修改；如需執行搬移請下明確指令。*
