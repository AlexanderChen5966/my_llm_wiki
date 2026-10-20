---
title: planning-with-files：Manus 風格持久化 Markdown 規劃 Skill
date: 2026-06-02
tags: [ai/claude, ai/agent]
source: https://github.com/OthmanAdi/planning-with-files
type: note
status: processed
asset_value: 75
---

## T（核心論點）

planning-with-files 實作 Manus 風格的持久化 Markdown 檔案規劃模式，核心原則是「Context Window = RAM（易失），Filesystem = Disk（持久）」。透過三檔案模式（task_plan.md / findings.md / progress.md）將 AI agent 的工作記憶從易失的 context window 轉移到檔案系統，解決目標漂移、錯誤遺忘、context 填滿重開等問題。目前已支援 17+ 個 IDE 平台，是 GitHub 上最高 star 的 Claude Code Skill 之一。

## B（適用場景）

**適用：**
- 多步驟任務（3+ 步驟）
- 研究型任務
- 跨多個 tool call 的建構/創建專案
- 需要 session 恢復的長任務

**不適用 / 注意：**
- 簡單問題、單檔編輯、快速查找不需要
- v2.43.0 已支援 Claude Code / Cursor / Codex / Gemini CLI / Kiro / Copilot 等 17+ 平台
- 有 hook 系統自動化：PreToolUse 重讀計畫、PostToolUse 提醒更新、Stop 驗證完成

## R（操作步驟）

1. **安裝**：`npx skills add OthmanAdi/planning-with-files --skill planning-with-files -g`
   - 或 Plugin 安裝：`/plugin marketplace add OthmanAdi/planning-with-files`
2. **啟動**：`/plan` 或 `/planning-with-files:plan`
3. **三檔案模式自動建立**：
   - `task_plan.md` — 追蹤階段與進度（checkboxes）
   - `findings.md` — 儲存研究與發現（不塞 context）
   - `progress.md` — session 日誌與測試結果
4. **核心規則**：
   - 永遠先建 Plan 再開始
   - 2-Action Rule：每 2 次檢視/瀏覽操作後存 findings
   - 記錄所有錯誤，永不重複失敗
5. **進階功能**：
   - `/plan-goal` — 結合 Claude Code `/goal`，從 plan 衍生終止條件
   - `/plan-loop` — 結合 `/loop`，10 分鐘 tick 重讀規劃檔
   - `/plan-attest` — SHA-256 鎖定 task_plan.md 防篡改
   - Parallel plan isolation — 多 session 各自獨立規劃目錄

### Benchmark（官方 skill-creator 框架）

| 指標 | 有 Skill | 無 Skill |
|------|---------|---------|
| Pass rate（30 assertions） | 96.7% | 6.7% |
| 3-file pattern 遵循 | 5/5 | 0/5 |
| Blind A/B wins | 3/3 | 0/3 |

## C（關鍵行動）

- [ ] 安裝 planning-with-files 並在下一個多步驟任務中試用
- [ ] 觀察 session 恢復（/clear 後自動 catchup）的實際效果
- [ ] 評估是否與現有 knowledge-organizer Skill 的規劃流程互補

## 原始摘要

一個 Claude Code skill，實作 Manus 風格的持久化 Markdown 檔案規劃模式（task_plan.md / findings.md / progress.md 三檔案模式），將 AI agent 的工作記憶從易失的 context window 轉移到檔案系統，解決目標漂移、錯誤遺忘等問題，已支援 17+ 個 IDE 平台。

## 相關連結

- [[harness-engineering]] — Harness Engineering 的 Guides×Sensors 框架，與持久化規劃互補
- [[cognitive-hub]] — 記憶先於行動的設計模式
- [[agentmemory]] — 另一種跨 session 持久記憶方案（MCP server）
- [[claude-code-goal-command]] — /goal 指令，planning-with-files 的 /plan-goal 與之整合
- [[claude-skills-ecosystem]] — Claude Skills 生態中的高 star 專案
