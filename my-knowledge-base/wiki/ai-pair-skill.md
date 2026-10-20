---
title: AI-Pair：異構 AI 團隊協作 Skill（Claude + GPT + Gemini）
date: 2026-05-14
tags: [ai/agent, ai/claude]
source: https://github.com/axtonliu/ai-pair
type: article
status: processed
asset_value: 68
---

AI-Pair 把多個 AI 模型組成審查團隊：Claude 創作、Codex（GPT）做分析型審查、Gemini 做編輯型審查。三者審查同一份工作時，發現的問題完全不重疊——這才是多模型協作的正確姿勢，而非重複問同一問題。

## T（核心論點）
不同模型天然關注不同維度：Claude 抓論點過度解讀、GPT 拆論證鏈和邏輯跳躍、Gemini 評受眾適配和可讀性。把模型差異變成「互補視角」而非「冗餘備份」。

## B（適用場景）
**適用：** 程式碼 review（dev-team 模式）；內容審查（content-team 模式）；需要多維度品質把關的輸出
**不適用：** 一次性草稿（Overhead 不值得）；需要快速迭代的任務

## R（工作流）
```
Team Lead (Claude Code) 
  ├── creator → 創作/執行
  ├── codex-reviewer → 分析型審查（邏輯/事實/架構）
  └── gemini-reviewer → 編輯型審查（可讀性/風格/受眾）
```

半自動工作流：每步都保留人工控制點。審查在並行模式下運行，最後匯總報告。

觸發指令：
- 開發團隊：`/ai-pair dev-team ProjectName`
- 內容團隊：`/ai-pair content-team TopicName`

## C（關鍵行動）
- [ ] 前置條件：需同時安裝 Claude Code + Codex CLI + Gemini CLI 並完成認證
- [ ] 確認輸出中有 `Source: Codex CLI` 標籤，否則 Sub-agent 只是在角色扮演而非真正呼叫 CLI
- [ ] 安裝：`git clone https://github.com/axtonliu/ai-pair.git ~/.claude/skills/ai-pair`

## 相關連結
- [[claude-skills-guide]] — Skills 的軟編排型設計（同類架構）
- [[dual-tool-strategy]] — Claude + Gemini 的成本分工策略
- [[concepts/agent-workflow-taxonomy]] — 屬「⑥ Skills 工作流（人為軟編排）」；對照學習式編排 Sakana Fugu
- [[concepts/agent-handoff-memory-patterns]] — 並行協作的 REVIEW_FILE token 機制（v4.3 實例）

## 補充：本地 v4.3 演進（由 OSS 版自改）

> 以下記錄個人 fork 從公開 OSS 版延伸的差異，**底層機制改動但角色名與架構骨架不變**。完整重構紀錄（依時序）：`b2b-manager/ai-pair-main/重構建議-v3.md`、`archive/重構建議-v4.md`、`archive/重構建議-v4.1.md`、`archive/重構建議-v4.2.5.md`、`archive/重構建議-v4.3-agy遷移.md`；前瞻設計（v4.4-opencode 評估 / v4.5 成本優化 / v4.5.1）見 `~/.claude/skills/ai-pair/reference/`（2026-07-08 批次 B 健檢後之唯一真相源），尚未建立獨立 wiki 條目。

### 0. 版本演進史（v3 → v4.3）

| 版本 | 日期 | 核心演進 |
|------|------|---------|
| **v3** | 2026-04-28 | **架構奠基**：加入第三個 gemini-reviewer（spec compliance 視角，與另兩者不重疊）、Inter-Agent Protocol v1 壓縮通訊、分層 review（L1/L2/L3）、**diff-only + reviewer 不讀 source**、單任務 dispatch、Review 快取、額度降級鏈。每輪 token 估省 ~84% |
| **v4** | 2026-05-08 | **動態模型分配**：套 Pilot Suite tier 邏輯，developer／reviewer 依任務複雜度＋review level 選最便宜模型；發現 Copilot Pro 的 GPT-5 mini／GPT-4.1 免費 |
| **v4.1** | 2026-05-14 | **v4 校正**：移除兩個錯誤擔憂（~~Gemini 雙倍 token~~ 已被 diff-only 解決、~~違反向下委派~~ 異構生態不適用）；修正缺口（移除 Haiku 4.5 fallback 改 GPT-4.1、補齊 settings 權限、L1 靜默 1→2 reviewer）|
| **v4.2.5** | 2026-06-01 | **Copilot Credit 計費適應**：gpt-4.1 預計移除 → copilot-reviewer 改 `gpt-5.4-mini`（見下方 2.）|
| **v4.3** | 2026-06-18 截止 | **gemini → agy 遷移**（見下方 1.）|

> 兩個被 v4.1 否決的擔憂值得記住：① **diff-only 原則**讓「先讀再傳」的雙倍 token 疑慮不成立——reviewer 共讀同一份 diff，無中間轉譯層；② **異構架構不套用「只向下委派」**——Team Lead 角色是定邊界＋驗收，實作者能力比指揮者強不是問題（如現實 Tech Lead）。

### 1. Gemini reviewer 底層遷移 `gemini` → `agy`（v4.3，截止 2026-06-18）

Google 停止 Gemini CLI 對免費／Google One 帳號服務，統一遷移至 Antigravity CLI（`agy`）。處理：

- 指令 `gemini` → `agy`；`--model` 旗標**確認支援**，Tier 系統保留只換模型名
- **角色名 `gemini-reviewer` 不改**（避免 `reviewer_set` 快取鍵失效，使用者識別不變）
- Tier 對應：LITE=`Gemini 3.5 Flash (Low)`、STANDARD=`Gemini 3.5 Flash (Medium)`、PREVIEW=`Gemini 3.1 Pro (High)`
- `SRC:` 欄改為 `SRC:agy/{model-slug}`
- 詳見 [[google-antigravity-platform]]

### 2. copilot reviewer 模型更新（v4.2.5，Credit 計費適應）

Copilot 改 AI Credits 計費後 `gpt-4.1` 預計移除 → copilot-reviewer 預設改 `gpt-5.4-mini`（fallback `gpt-5-mini`）。真正效益是**移除停用風險**，非省錢（reviewer 僅佔 ~8% credits，developer 佔 ~92%）。

### 3. Review Context Protocol —— 並行協作的 token 機制

三個 reviewer **共讀單一 REVIEW_FILE**（Team Lead 用 `mktemp` 建、範圍化 `git diff` 組成），**reviewers 不得自讀 source**。設計目的：

- 避免「每個 reviewer 各讀完整脈絡 = token ×3」陷阱
- **帳單分散**：copilot-reviewer 走 GPT Credits、gemini-reviewer 走 Google 額度（agy）、claude-reviewer 走 native subagent，三條帳單互不疊加
- Review Cache：`.ai-pair-cache/review-findings/{task_id}.md`

> 這正是 [[concepts/agent-handoff-memory-patterns]] 講的「並行協作 ≠ 序列接力」——並行時給精簡共用 context，不給完整 HANDOFF。

## 補充資料（來源：axtonliuai-pair GitHub README）

### 技術架構細節

三個 CLI 工具都是命令列工具（非桌面應用），需要在終端機中運行：

| 工具 | 用途 | 安裝 |
|------|------|------|
| Claude Code | Team Lead + agent runtime | `npm install -g @anthropic-ai/claude-code` |
| Codex CLI | GPT 分析型審查 | `npm install -g @openai/codex` |
| Gemini CLI | Gemini 編輯型審查 | `npm install -g @google/gemini-cli`（⚠️ 本地 v4.3 已改用 `agy`，見下方補充）|

安裝前確認：`claude --version`、`codex --version`、`gemini --version` 三個都能跑。

> 上表為公開 OSS 版（content-team 取向）。下方「本地 v4.3 演進」記錄由此版延伸自改的 dev-team 版本差異。

### 已知問題與排查

**問題：審查者沒有真正呼叫 Codex/Gemini CLI（Sub-agent 在角色扮演）**

驗證方式：檢查輸出中是否有 `**Source: Codex CLI**` 標籤和 `### CLI Raw Output` 段落。若缺少，表示 CLI 沒有被呼叫。

解決方案：v1.1.0 已加入強制 CLI 呼叫規則，更新 SKILL.md 至最新版本。

### 開放原始碼版本的功能邊界

- 開源版只包含 **Agent Teams 模式**
- 完整私有版額外有：Manual mode（兩個 CLI 透過共享檔案通信）、iTerm2 orchestration（自動化的創作/審查中繼）

### 真實案例（content-team 審查 Newsletter）

三個 AI 發現的問題完全不重疊，這才是多模型協作的意義：
- Claude（Team Lead）：發現對引用來源的過度解讀
- GPT（Codex）：拆解論證鏈，質疑邏輯跳躍
- Gemini：建議開頭對目標讀者來說太學術化
