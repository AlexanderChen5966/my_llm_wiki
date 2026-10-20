---
title: 使用 Claude Code 4 個月後的 10 個推薦指令與技能
date: 2026-06-30
tags: [ai/claude, type/how-to]
source: https://codelove.tw/@tony/post/37Z0jx
type: article
status: processed
asset_value: 62
---

作者實用 4 個月後精選的 10 個日常指令／技能，重點幾乎全集中在 **session 切換** 與 **Claude Code 本身的客製化**——因為實際寫程式多半交給 Claude，人的工作變成「把 Claude Code 調整好」。

## T（核心論點）

「程式由 Claude Code 寫，把 Claude Code 調整好則是人類的工作」。真正拉開體驗差距的，不是叫它做什麼，而是你會不會管理 session（切換、倒回、存檔）與持續客製化（權限、hook、skill 觸發）。

## B（適用場景）

- 已在用 Claude Code 但覺得沒用順手
- 想建立「越用越像為你量身打造」的個人化配置循環
- 需要在方向走偏時安全倒回、跨 session 交接

## R（10 個指令／技能）

**Session 管理（內建指令）**：
- **/clear**：清空上下文開新 session，舊 session 不會消失（可 /resume）。加名稱如 `/clear 認證bug` 方便日後尋找。作者偏好「產出交接提示詞轉新 session」勝過 /compact。
- **/resume**：從歷史 session 清單挑一個續作；重啟後回到工作現場。
- **/rewind**：倒回對話/程式碼（每次送 prompt 自動建 checkpoint），5 種操作（都倒/只倒對話/只倒程式碼/之後摘要化/之前摘要化）。空欄按兩下 Esc 開啟。注意：rm/mv 等 bash 變更不被追蹤，只回復 Claude 編輯工具的修改。

**客製化（最推薦）**：
- **/insights**：分析 session 產出 HTML 報告，含 friction points（區分 Claude 端 vs 使用者端問題）、未用功能建議。定期據此改善 CLAUDE.md/skill。
- **/update-config**：讓 Claude 自己改 settings.json（permissions/env/hooks）。注意 allow 常被「順手擴大」（要 `gh pr view *` 卻給 `gh *`），套 diff 前務必自己讀過。
- **/skill-creator**（官方外掛，需先裝）：建立/改善 skill、評估觸發；特別擅長調校 description（auto-trigger 的唯一依據）。
- **/fewer-permission-prompts**：掃描 session 找常用**唯讀** Bash/MCP 呼叫加進 allow；只限唯讀、依頻率排序、可逐一採用、預設專案層級（可指定 user scope）。

**其他**：
- **/copy**：複製最近回應到剪貼簿（避免終端拖曳帶入行首縮排），可 `/copy 2` 複製倒數第 2 則。
- **/loop 與 /schedule**（技能）：/loop 保持 session 重複執行同 prompt（`/loop 5m CI 跑過再告訴我`）；/schedule 建立 Anthropic 雲端定期任務，關機也能跑，適合每日資訊蒐集。

> 搭配拳：/insights 回顧 → /fewer-permission-prompts 整理權限 → /update-config 佈 hook，越用越貼身。

## C（對自身的啟發）

- [ ] 用 /insights 定期回顧，把 friction 寫回本知識庫的 CLAUDE.md
- [ ] /clear + 交接提示詞的做法，呼應 claude-code-handoff-memory-strategy
- [ ] /schedule 可用於每日簡報（daily-brief-generator）的雲端排程

## 相關連結

- [[claude-code-commands-2026]] — 2026 新命令解析
- [[claude-code-context-management]] — Context 管控（/clear 存檔策略）
- [[claude-code-handoff-memory-strategy]] — 跨 session 交接
- [[claude-code-advanced-workflow]] — /loop /schedule 進階
- [[claude-code-7-usage-mistakes]] — 7 個常見錯誤（養機制而非養對話）
