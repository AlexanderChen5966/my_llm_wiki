---
title: Claude Skills 新手入門：設定教學與五層 AI 熟練度
date: 2026-06-15
tags: [ai/claude, pkm/workflow]
source: https://www.bnext.com.tw/article/90870/claude-skills-guidebook
type: article
status: processed
asset_value: 65
---

Claude Skills 是「讓 AI 記住你的工作流程」的核心工具；解決每次重貼提示詞和 Skill 設好沒反應兩大痛點的關鍵在於：description 必須同時說明「何時用」和「何時不用（負向觸發詞）」。

## T（核心論點）

**Skills vs Projects vs MCP 的本質分工**：

| 工具 | 解決什麼 |
|------|---------|
| Skills | **怎麼做**（SOP / 工作流程 / 方法論）|
| Projects | **知道什麼**（背景知識 / 專案資料）|
| MCP | **連到活資料**（即時外部服務）|

AI 熟練度五層級（L1-L5）：

| 層級 | 特徵 |
|------|------|
| L1 對話 | 用 Claude 回答問題 |
| L2 進階對話 | 上傳檔案、生成內容 |
| L3 Workflow 建構 | 設定 Skills、建立可重複工作流 |
| L4 檔案協作 | Projects + CLAUDE.md 長效記憶 |
| L5 Agent 操作 | 自動化執行、多步驟委派 |

## B（適用場景）

- 第一次設定 Claude Skills
- Skill 設好了但 AI 沒有自動觸發
- 想建立可重複使用的 SOP 工作流程
- 從 Profile Instructions 遷移到 CLAUDE.md

## R（完整內容）

### 建立 Skill 的步驟

Anthropic 官方 `skill-creator` Skill 入口：`Customize → Skills → + → Browse → Anthropic & Partners`

### SKILL.md 四段結構

1. **前置作業** — 執行前需準備的資料或前提
2. **執行步驟** — 具體步驟說明
3. **輸出格式** — 期望的輸出格式（含範例）
4. **自查清單** — AI 輸出後自我檢核的項目

### 修復「Skill 沒觸發」的關鍵

description 必須同時說明**何時用**（正向觸發）和**何時不用**（負向觸發）：

```
# 正確示例（description 欄位）
當使用者要整理 raw/ 文件時使用此 Skill。
請勿用於已有現成格式的文件、重複使用的內容、或非知識整理任務。
```

缺少負向觸發詞是 Skill 自動觸發失敗的最常見原因。

### Profile Instructions 設定（500字以內）

入口：`Settings → General → Instructions for Claude`

建議格式：4-6 段，包含：
- 我是誰 / 主要工作
- 我希望你如何稱呼我和溝通風格
- 我的工作重點領域
- 我不希望你做的事

撰寫方式：先進行 20 問訪談再整理（提示詞：「請問我 20 個問題以了解我的工作和需求，再幫我寫 Profile Instructions」）

### Profile Instructions → CLAUDE.md 遷移路徑

Claude.ai Profile Instructions（跨所有專案）→ Project 的 CLAUDE.md（專案特定）→ 本地 CLAUDE.md（最細粒度）

## C（關鍵行動）

- [ ] 確認 SKILL.md 的 description 同時包含正向和負向觸發詞
- [ ] 在 `Anthropic & Partners` 找到 `skill-creator` 安裝
- [ ] 撰寫 Profile Instructions（≤500字，4-6段）
- [ ] 漸進遷移：全域設定用 Profile Instructions，專案特定用 CLAUDE.md

## 相關連結

- [[claude-skills-guide]] — Claude Skills 完整指南（能力包型 + 軟編排型）
- [[skill-md-writing-techniques]] — SKILL.md 撰寫技巧
- [[claude-cowork-setup-guide]] — Claude Cowork 設定（Global Instructions vs CLAUDE.md 分工）
- [[ai-skills-mcp-plugins-agents-comparison]] — Skills / MCP / Plugins / Agents 四概念比較
- [[sop-to-skill-4-steps]] — 把部門 SOP 變 AI 技能的 4 步法
