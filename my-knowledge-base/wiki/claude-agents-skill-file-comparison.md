---
title: SKILL.md vs CLAUDE.md vs AGENTS.md 完整比較與分層策略
date: 2026-06-05
tags: [ai/claude, ai/agent, dev/architecture]
source: https://www.termdock.com/blog/skill-md-vs-claude-md-vs-agents-md/zh
type: article
status: processed
asset_value: 82
---

三種 AI Agent 設定檔各自解決不同問題，關鍵差異是「永遠載入」vs「按需載入」。AGENTS.md 是 2026 年最接近通用標準的跨工具 context 檔，CLAUDE.md 是 Claude Code 專屬補充，SKILL.md 是條件式載入的任務能力。三者應同時使用，不是選一個。

## T（核心論點）

**CLAUDE.md 和 AGENTS.md 是永遠載入（advisory），SKILL.md 是按需載入（conditional）。** 這個差異決定了什麼內容放哪裡：任務專屬的深度 workflow（migration、PR review、deploy）放 SKILL.md，只在相關時才消耗 context；跨任務通用的專案規則放 AGENTS.md 或 CLAUDE.md。

ETH Zurich 2026 研究發現讓很多人意外：

| 情況 | 任務成功率變化 | 推理成本 |
|------|-------------|---------|
| LLM 生成的 context 檔案 | **-3%**（比沒有更差）| +20% |
| 人工撰寫的 context 檔案 | +4% | +20% |

核心問題：詳盡的 codebase 概述跟既有文件高度重複，過度限制的指令反而讓 agent 更難解決實際任務。

**最重要的一行測試**：`「拿掉這行，agent 會犯一個自己無法恢復的錯誤嗎？」` — 通不過就刪掉。

## B（適用場景）

**適用：**
- 管理多個 AI coding 工具（Claude Code + Codex CLI + Copilot CLI + Gemini CLI）
- 需要決定「這條規則該放哪個檔案」
- 想要精簡 CLAUDE.md 但不知道從何刪起

**不適用：**
- 純 PKM / 知識庫場景（ETH Zurich 研究針對 code repo；知識庫的 CLAUDE.md 邏輯不同）
- 只用 Claude Code 一個工具的極簡配置（AGENTS.md 優先的策略收益遞減）

## R（三檔案對照表）

| | SKILL.md | CLAUDE.md | AGENTS.md |
|-|----------|-----------|-----------|
| **用途** | 任務能力（按需） | 專案 context（Claude 專屬）| 專案 context（跨工具）|
| **載入時機** | 符合 description 時才載入 | 每次 session 永遠載入 | 每次 session 永遠載入 |
| **誰讀** | Claude Code、Codex、Copilot、Gemini | Claude Code | 所有主流工具 |
| **建議大小** | 每個 skill < 500 行（按需所以可詳細）| **< 100 行**（單工具）或 **< 20 行**（搭配 AGENTS.md）| < 100 行 |
| **可執行 script** | ✅ | ❌ | ❌ |

### 三層分層策略

```
第一層：AGENTS.md（永遠載入、跨工具）
  → 專案架構、程式碼慣例、硬性限制、build 指令
  → < 100 行，所有 AI 工具的共同基礎

第二層：CLAUDE.md（永遠載入、Claude 專屬）
  → 最小化。只放 Claude Code 專屬指令
  → 搭配 AGENTS.md 時只需 20 行以下
  → 其他全部引用 AGENTS.md

第三層：SKILL.md（按需載入、任務專屬）
  → 專業 workflow（migration、PR review、deploy）
  → 每個 skill 可以很詳細（< 500 行），因為只在需要時才進 context
```

**結果**：基礎 context 精簡（CLAUDE.md + AGENTS.md ≤ 120 行），按需獲得深度任務能力。

### 決策流程（5 問）

1. **這跟每一個任務都有關？** 是 → AGENTS.md（或 CLAUDE.md 若只用 Claude Code）
2. **有多個步驟的專業 workflow？** 是 → SKILL.md
3. **需要執行 script？** 是 → SKILL.md（唯一支援）
4. **用多種 AI 工具？** 是 → 共用 context 放 AGENTS.md，專屬放 CLAUDE.md，任務 workflow 放 SKILL.md
5. **中間地帶不確定？** → 預設放 AGENTS.md（覆蓋面最廣）

## C（關鍵行動）

- [ ] 用「拿掉這行，agent 會犯一個自己無法恢復的錯誤嗎？」測試 CLAUDE.md 每一行
- [ ] 把目前 CLAUDE.md 的任務 workflow（多步驟流程）移到 SKILL.md
- [ ] 若有多工具需求，建立 AGENTS.md 作為單一真相來源，CLAUDE.md 改為只寫 `Read AGENTS.md for project context`
- [ ] NEVER 用 LLM 生成整份 context 檔案（ETH Zurich 數據：比不放更差）

## 補充：SKILL.md 三個獨特能力

1. **Script 執行**：可包含 Claude Code 執行的 script，CLAUDE.md / AGENTS.md 做不到
2. **自動觸發**：frontmatter 的 `description` 決定何時載入，description 寫強勢一點（Claude Code 傾向少觸發）
3. **存取控制**：`disable-model-invocation: true` / `user-invocable: false` / `allowed-tools` — 細粒度控制

## 相關連結

- [[claude-md-optimization-techniques]] — CLAUDE.md 精簡化六大技巧（< 200 行、NEVER/ALWAYS 格式、任務路由表）
- [[claude-md-guide]] — Boris Cherny 六大核心引擎 + Karpathy 12 條規則
- [[agents-md-guide]] — AGENTS.md 設計原則（Linux Foundation Agentic AI Foundation 開放標準）
- [[claude-skills-guide]] — Claude Skills 完整指南：能力包型 + 軟編排型
- [[zerospec]] — ZeroSpec 以 AGENTS.md 為核心的 Layer 0 上下文整備框架
- [[context-engineering-fundamentals]] — 多 Agent 拓撲與 context 管理理論基礎
