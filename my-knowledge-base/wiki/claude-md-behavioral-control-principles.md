---
title: CLAUDE.md 行為控制四原則：從 andrej-karpathy-skills 學習
date: 2026-06-02
tags: [ai/claude, ai/prompt-engineering]
source: https://codelove.tw/@tony/post/37lvWa
type: article
status: processed
asset_value: 48
---

從 GitHub 16 萬+ 星的 andrej-karpathy-skills 專案學習 CLAUDE.md 行為約束設計。核心區分：**Skills/Plugin = 能力追加，CLAUDE.md = 行為控制**——能力變多但約束不夠，失控範圍反而更大。

## T（核心論點）

Claude Code 的問題已從「能不能寫程式」進入「能不能安全長時間運作」。andrej-karpathy-skills 整理出四個行為原則：Think Before Coding / Simplicity First / Surgical Changes / Goal-Driven Execution，核心是**抑制 AI 的典型失敗模式**（擅自重構、過度抽象、連不相關檔案也改）。關鍵洞察：CLAUDE.md 不是強制設定而是脈絡，寫得精簡具體比冗長模糊有效。

## B（適用場景）

**適用：**
- 在既有程式碼庫中長期使用 Claude Code 的開發者
- 需要產出可審查 diff 的場景
- 全域行為原則（放在 `~/.claude/CLAUDE.md`）

**不適用 / 注意：**
- CLAUDE.md 是脈絡而非強制，重要工作仍需在 prompt 重申約束
- 專案特定規則（建置指令、API 規格）應放專案 CLAUDE.md，不放全域

## R（四原則詳解）

### 1. Think Before Coding
實作前先說明：要達成什麼、修改哪些檔案、基於什麼前提、有哪些模糊處。防止 AI 自行解讀模糊需求就直接實作。

### 2. Simplicity First
以最小必要範圍實作。不新增未被要求的功能、不抽象只用一次的東西、配合既有寫法。抑制過度設計。

### 3. Surgical Changes
**最重要的一項。** 只動與需求直接相關的檔案。不做不相關重構、不擅自改格式/註解、不刪未被要求移除的程式碼。防止「順手修正」汙染 diff。

### 4. Goal-Driven Execution
把需求轉換成可驗證的目標：修正對象、預期結果、確認方法、可/不可修改範圍。有明確成功條件，AI 表現更穩定。

### Plugin vs CLAUDE.md 的角色區分

```
Plugin / Skills = 能力追加（擴充作業能力）
CLAUDE.md       = 行為控制（控制如何表現）
```

能力越強，約束設計越重要。最有力組合：CLAUDE.md 固定行為原則 + Skills 擴充作業能力。

### 建議的全域規則（作者追加）

```
Prefer minimal diffs.
Avoid unnecessary refactors.
Preserve existing architecture unless explicitly requested.
Do not modify unrelated files.
Ask before introducing new dependencies.
Do not change public API behavior unless required by the task.
Always explain verification steps after implementation.
```

## C（關鍵行動）

- [ ] 檢視自己的 `~/.claude/CLAUDE.md` 是否包含這四原則
- [ ] 與 [[claude-md-guide]] 的 Boris Cherny 六大引擎互補：本文偏「防失控」，Boris 偏「提效率」
- [ ] 與 [[claude-code-12-rules-error-reduction]] 的 Karpathy 4 條規則有重疊，但本文補充了「能力追加 vs 行為控制」的框架區分

## 原始摘要

codelove.tw 翻譯文章（原文：Qiita），從 andrej-karpathy-skills 的 CLAUDE.md 學習四個行為控制原則（Think Before Coding / Simplicity First / Surgical Changes / Goal-Driven Execution），強調 CLAUDE.md 是安全使用 Claude Code 的基礎。

## 相關連結

- [[synthesis-claude-md]] — CLAUDE.md 撰寫方法論知識精粹（本主題 4 篇整合）
- [[claude-md-guide]] — Boris Cherny 六大核心引擎 + Karpathy 12 條規則
- [[claude-code-12-rules-error-reduction]] — CLAUDE.md 12 條規則，錯誤率從 41% 降至 3%
- [[claude-code-file-management]] — CLAUDE.md 放置位置的四層架構
- [[impeccable-style-ai-coding-guide]] — AI-first Coding Style Guide，與 CLAUDE.md 行為控制互補
