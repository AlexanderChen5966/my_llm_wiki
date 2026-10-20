---
title: Model Boomerang：追蹤 Claude Code 掉出主模型並一鍵切回
date: 2026-07-22
tags: [ai/claude, ai/agent]
source: https://github.com/GWCCORP/.claude
type: article
status: processed
asset_value: 52
---

GWC 公開的 `~/.claude` 共享資料夾，主打 **Model Boomerang** skill——把 Claude Code 意外滯留在 fallback 模型（例如 Fable 5 觸發安全分類器改跑 Opus、之後整個 session 就留在 Opus）當成有生命週期的「excursion」追蹤，釘住觸發項並在修復後提醒你一鍵切回主模型。

## T（核心論點）

Claude Code 會悄悄把 session 擱在 fallback 模型：安全分類器把一次請求改跑 Opus、手動 `/model` 繞道、限額降級都會這樣，而且 **session 常常回不去**。Boomerang 的洞見是把每次 fallback 當成一個有明確生命週期的**偏移事件（excursion）**：

```
detected → open → resolved → closed
(觀察到切換)(釘住觸發項)(觸發項已修復)(切回主模型)
```

核心限制很誠實：**Claude Code 沒有程式化切換執行中 session 模型的手段**（沒有 hook / CLI / 設定能碰到執行中的 session），所以最後一個按鍵是你的；能自主路由的 harness 才能自動完成 boomerang，但生命週期模型可乾淨移植。

## B（適用場景）

**適用：**
- 主力用 Fable 5 / 特定模型，但常被安全分類器或限額踢到 fallback
- 想要一份「什麼一直把我踢出主模型」的真實帳本
- 多 agent orchestrator 想追蹤每個 agent 是否掉出主模型並復原

**不適用 / 注意：**
- 需要 Node.js；狀態存在本機 `~/.claude/boomerang/`（無網路、無相依）
- 無法真正自動切模型（Claude Code 限制），只能提醒 / 阻擋

## R（運作機制與 CLI）

**感測器 = statusline**：這是 Claude Code 唯一持續暴露即時 model ID 的地方。**四個 hooks**（SessionStart / UserPromptSubmit / PostToolUse / Stop）在回合中途 brief Claude 去辨識並記錄觸發項，然後嘮叨提醒你按一次 `/model` 切回——或在 opt-in 的 enforce 模式下，直接阻擋在 fallback 上送出新 prompt，直到觸發項被驗證修復。

安裝：clone repo → 複製 `skills/model-boomerang` 到 `~/.claude/skills/` → 在 `settings.json` 接上 statusLine + 四 hooks → 設定主模型前綴（預設 `claude-fable`）。

```
boomerang.js status              # 即時 excursion + 設定
boomerang.js open --item "..."   # 釘住觸發項
boomerang.js resolve             # 觸發項已修復，開始切回提醒
boomerang.js close               # 決定留在 fallback，結案
boomerang.js config --primary <prefix> --enforce on|off
```

**Orchestrator 延伸**：`maoe-report.js` 讀 MAOE run database，印出每個 agent 的 excursion（觸發任務 + 復原證明）；此模式可移植到任何會記錄 `(task, model, used_fallback)` 的 orchestrator。

## C（關鍵行動）

- [ ] 若常被踢出主模型，裝上 statusline + 四 hooks，先觀察 `boomerang.js status` 累積的帳本
- [ ] 用 `log` 檢視有哪些觸發項反覆把你踢出主模型，針對性處理
- [ ] 需要硬性把關時開 enforce 模式，避免整個 session 在貴/慢模型上空跑
- [ ] 多 agent 場景參考 maoe-report 模式，記錄每個 agent 的 model excursion

## 相關連結

- [[claude-model-selection-guide]] — Fable/Opus/Sonnet/Haiku 選型與 Fable 5 遇高風險自動改跑 Opus 的備援機制，正是 Boomerang 要追蹤的觸發來源
- [[fable5-subagent-model-routing-design]] — 用 agent frontmatter 做模型路由，與 Boomerang 的「回到主模型」互補
- [[claude-code-advanced-workflow]] — Claude Code hooks / statusline 等進階機制

## 原始摘要

GitHub GWCCORP/.claude。Model Boomerang 用 statusline 當感測器 + 四 hooks，把 session 意外滯留 fallback 模型當成有生命週期的 excursion 追蹤，釘住觸發項、修復後一鍵切回；誠實指出 Claude Code 無法程式化切換執行中 session 模型，最後按鍵在人，模式可延伸到 multi-agent orchestrator。
