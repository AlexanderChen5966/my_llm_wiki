---
title: AI 時代 Harness 工程在後台管理系統中的實踐
date: 2026-07-16
tags: [ai/agent, pkm/framework, dev/architecture]
source: https://codelove.tw/@tony/post/anb7Qa
type: article
status: processed
asset_value: 68
---

V3 Admin Vite（開源 Vue3 後台模板）作者分享第二期「AI 友善模板」實踐：在既有 Rules 基礎上新增專案級約定（AGENTS.md + CLAUDE.md）與專案 Skills，並用軟連結讓多個 AI Coding 工具（Claude Code / Codex / Cursor / Trae / Antigravity）共用同一份設定。

## T（核心論點）

CLAUDE.md / Rules / Skill 三者應依「載入時機」分工——CLAUDE.md 每次會話都載入（核心約定/建構命令）、Rules 按檔案路徑匹配才載入（節省 context）、Skill 按需被呼叫（可重複使用的工作流程參考）。透過 `AGENTS.md` 作為通用層、`CLAUDE.md` 關聯它、再用軟連結（`ln -s`）讓 `.claude/skills/` 與更通用的 `.agents/skills/` 共用同一份 Skill 檔案，可以讓多個 AI Coding 工具同時吃到一致的專案上下文，而不必維護多份重複文件。

## B（適用場景）

**適用：** 團隊/開源專案需要同時支援多種 AI Coding 工具（Claude Code、Codex、Cursor、Trae、Antigravity）、後台管理系統類專案有大量重複性 CRUD/Store/Route 建立需求。
**不適用 / 注意：** 單人單工具的小型專案不需要 AGENTS.md + CLAUDE.md 雙檔關聯的複雜度，直接寫 CLAUDE.md 即可。

## R（操作步驟）

1. 新增 `AGENTS.md`（通用格式，供 Cursor/Antigravity/Trae/Codex 讀取）+ `CLAUDE.md`（供 Claude Code 讀取），內容保持在 200 行以內，模組化分成命令／架構／約定三塊
2. 用官方 `skill-creator` 工具建立專案級 Skill（而非從零手寫），每個 Skill 內容控制在 500 行以內
3. 把高頻需求（CRUD 頁面、全域狀態、路由表、內建工具用法）各自抽成一個 Skill
4. 用 `ln -s` 把 `.claude/skills/` 軟連結到更通用的 `.agents/skills/`，避免同一份 Skill 內容要在多個工具目錄各存一份
5. 安裝社群第三方 Skill（如 antfu/skills 提供的 pinia/vite/vue-best-practices 等），透過 `skills-lock.json` 記錄、`npx skills experimental_install` 安裝

## C（關鍵行動）

- [ ] 檢查專案是否已有 AGENTS.md，若無則優先建立（比 CLAUDE.md 通用）
- [ ] 盤點目前重複發生 3 次以上的開發需求，逐一抽成 Skill
- [ ] 若同時使用多個 AI Coding 工具，改用軟連結而非複製貼上維護設定檔

## 原始摘要

V3 Admin Vite 開源專案作者 pany 的第二期 AI Coding 體驗提升實踐。第一期（一年前）只用 Rules，隨 AI 能力提升，此次新增專案級約定檔案與 Skills 兩層。文中引用 Claude Code 官方文件對 CLAUDE.md / Rules / Skill 的定位區分，並以 andrej-karpathy-skills 開源專案的 CLAUDE.md 四條原則為基礎完善約定內容。

## 相關連結

- [[harness-engineering]] — Harness Engineering 完整框架（Guides×Sensors），本篇是其在單一開源專案的落地案例
- [[claude-code-plugins-guide]] — Plugin 六元件系統，Skills/Rules/Hooks 的官方生態全貌
- [[ai-agent-skills-popular-2026]] — 熱門 Agent Skills 生態，含 antfu/skills 同類社群 Skill 儲存庫
- [[synthesis-ai-dev-workflow-frameworks]] — AI 開發工作流框架精粹，本篇的三層分工可對照其架構治理層論點
