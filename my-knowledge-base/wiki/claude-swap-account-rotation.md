---
title: claude-swap（cswap）：Claude Code 多帳號限額自動輪替 CLI
date: 2026-07-20
tags: [ai/claude, ai/workflow]
source: https://github.com/realiti4/claude-swap
type: reference
status: processed
asset_value: 51
---

`realiti4/claude-swap`（指令 `cswap`）：Claude Code 專用的多帳號切換 Python CLI。不需登出即可在多個 Claude 帳號間切換，或讓它在你快撞限額前自動幫你切；用 live dashboard 追蹤每個帳號用量，並可平行跑多帳號。相容 Claude Code CLI 與 VS Code 擴充。`uv tool install claude-swap`（Python 3.12+）。MIT 授權。

> ⚠️ **與 [[cc-switch-provider-manager]] 是完全不同的專案**。claude-swap＝單一 Claude Code 的多帳號限額輪替 CLI（換帳號/避限速）；cc-switch＝跨 8 種 CLI 的 provider 統合桌面 App（換供應商/管 MCP）。勿混淆。

## T（核心論點）

claude-swap 解的是「Claude 訂閱有 5 小時 + 7 天雙層限額、單帳號重度使用會撞牆」的痛點。它的價值不只是手動切帳，而是 **在撞限額前主動輪替 + 安全並行**：`cswap auto` 監控用量，達門檻（預設 90%）自動切到剩餘額度最多的帳號；切換時搶用 Claude Code 自己的 credential lock，確保 swap 不會與 token refresh 打架，可安全地在 Claude Code 運作中執行。

## B（適用場景）

**適用：** 持有多個 Claude 帳號、想避開限速中斷的重度用戶；想平行跑多帳號（不同 terminal/repo 用不同帳號）；需要 live 用量儀表板決定用哪個帳號。
**不適用 / 注意：** 只是想換 API provider 或跨多種 CLI 統一設定請用 cc-switch；僅限 Claude Code（需已安裝並登入）。

## R（重點整理）

- **帳號管理**：登入後 `cswap add` 加帳號；`cswap switch [N|email|alias]` 切換；`cswap list` 是儀表板（每帳號 5h/7d 用量與 reset 時間一覽）；`--strategy best`（剩最多）/`next-available`（跳過已限速）自動挑。
- **自動輪替 `cswap auto`**：前景迴圈每 60s 輪詢，達門檻切到剩最多的帳號；`--threshold 80` 提早切、`--model Fable` 把某模型週限額也納入判斷、`--once` 單次（供 cron，exit code 0 切/1 錯/2 無需/3 阻擋）、`--dry-run`。有 cooldown（預設 5 分）+ hysteresis margin 防在門檻附近 flip-flop；用量輪詢自適應（忙的帳號多看、閒的少看）保持 API 流量平穩；fail-safe（查詢出錯時信任上次數字）；死 refresh token 的帳號自動隔離。
- **並行 session mode `cswap run N`**：只在當前 terminal 以指定帳號啟動 Claude Code，其他 terminal 與 VS Code 維持預設帳號 → 兩帳號平行。共用 `~/.claude`（settings/CLAUDE.md/skills/MCP）但各帳號保留自己聊天歷史（`--share-history` 可共享）。`cswap map N ~/work/repo` 把目錄綁帳號，該目錄下 bare `cswap run` 自動用對應帳號。
- **憑證與遷移**：macOS 存 Keychain、Linux/Windows 存檔案；切換多數不需重啟（Linux/Windows 自動 re-read，macOS 約 30s Keychain cache 過期後生效）。`cswap export/import` 遷移（預設純 JSON 只帶各帳號登入，`--full` 含全部供同機備份）。`cswap add-token sk-ant-oat01.../sk-ant-api03...` 可從 raw token/API key 直接註冊（headless server 適用）。
- **介面**：`cswap tui`（全螢幕儀表板）、`cswap watch`（live monitor）、macOS menu bar app（`claude-swap[menubar]`，點一下切換）。`--json` 供腳本串接（含 pace 預測欄位 projectedExhaustionAt 等）。

## C（關鍵行動）

- [ ] 多 Claude 帳號重度用戶用 `cswap auto --threshold 80` 在撞限額前自動輪替
- [ ] 用 `cswap map` 把 work repo 綁工作帳號、其他用個人帳號
- [ ] 只需換 provider/跨工具管理請改用 cc-switch（不同工具）

## 原始摘要

GitHub README：claude-swap 單一 Claude Code 的多帳號限額輪替 Python CLI，含 live usage dashboard、達門檻自動切帳、session 平行（run/map）、TUI 與 macOS menu bar、cron 整合。

## 相關連結

- [[cc-switch-provider-manager]] — ⚠️ 不同工具：跨 8 種 CLI 的 provider 統合桌面 App
- [[claude-code-billing-usage]] — Claude Code 雙層限速（5h+7d）計費說明，claude-swap 正是為此設計
- [[deepclaude-cost-optimization]] — 另一種成本策略：換便宜後端（vs 本工具多帳號輪替）
