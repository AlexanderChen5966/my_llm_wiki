---
title: dotbabel：模型無關的 AI 編碼代理治理工具包
date: 2026-06-02
tags: [ai/agent, ai/workflow]
source: https://github.com/kaiohenricunha/dotbabel/tree/main
type: note
status: processed
asset_value: 50
---

## T（核心論點）

dotbabel 是一個模型無關的 AI 編碼代理治理工具包，提供兩個獨立面向：(1) 全域規則底線 + skill/command 函式庫，跨 Claude Code / Codex / Gemini CLI / Copilot 同步；(2) 可選的 spec-driven development 治理閘門，在 PR 層級執行規範驗證與漂移偵測。核心理念是將工程規範從「人腦記住」提升為「工具強制執行」。

## B（適用場景）

**適用：**
- 使用多個 AI CLI 工具的開發者（Claude Code + Codex + Gemini + Copilot）
- 需要統一 CLAUDE.md / 規則 / skills 跨工具同步
- 團隊希望在 PR 層級加入規格驗證閘門
- 想要現成的 skill + command 函式庫（/ground-first、/fix-with-evidence、/pre-pr 等）

**不適用 / 注意：**
- CLI 功能需 Node ≥ 20
- bootstrap（純 skill 同步）和 CLI（治理閘門）是獨立路徑，可只用其一
- 不 commit 真實 API key 到設定檔

## R（操作步驟）

### 路徑一：Skills 函式庫（30 秒，無需 npm）

1. `git clone https://github.com/kaiohenricunha/dotbabel.git ~/projects/dotbabel`
2. `./bootstrap.sh` — 將 commands/ + skills/ + CLAUDE.md symlink 到 `~/.claude/`
3. 更新：`./sync.sh pull`

### 路徑二：治理 CLI

1. `npm install -g @dotbabel/dotbabel`
2. `dotbabel bootstrap` — 設定 ~/.claude/
3. `dotbabel init` — 在 repo 中建立 specs、hooks、manifest
4. `dotbabel doctor` — 自我診斷
5. PR 閘門：`dotbabel check-spec-coverage`、`dotbabel check-instruction-drift`

### 跨 CLI 同步

- `dotbabel project-sync` — 將 .claude/ 的 skills 和 commands fan-out 到 .codex/、.gemini/、.github/
- 使用 symlink，.claude/ 為唯一真相來源

### 個人規則疊加

- `~/.config/dotbabel/local-rules.md` — 在全域規則底線之上疊加個人規則

## C（關鍵行動）

- [ ] 評估 dotbabel bootstrap 是否適合納入現有工具鏈
- [ ] 檢視其 skill 函式庫中是否有可直接複用的 workflow

## 原始摘要

dotbabel 是一個模型無關的 AI 編碼代理治理工具包，提供全域規則底線、skill/command 函式庫、跨 CLI（Claude Code/Codex/Gemini/Copilot）同步，以及可選的 spec-driven development 治理閘門（PR 層級的規範驗證與漂移偵測）。

## 相關連結

- [[impeccable-style-ai-coding-guide]] — AI-first Coding Style Guide，與 dotbabel 規則底線互補
- [[claude-code-file-management]] — Claude Code 設定檔管理的四層架構
- [[spec-driven-ai-workflow]] — spec-driven 工作流的完整論述
- [[multi-agent-cli-bridge]] — CCB 多 agent 工作區，dotbabel 可提供跨 CLI 的規則同步
- [[research/dev-workflow-optimization-tools]] — 開發流程優化工具評估中有 dotbabel
