---
title: Claude Code 計費與用量完整說明
date: 2026-06-15
tags: [ai/claude]
source: https://codelove.tw/@tony/post/3Kp5Ya
type: article
status: processed
asset_value: 50
---

Claude Code 計費分兩種路徑，容易混淆：訂閱制（Pro/Max）有包含用量但有速率限制；API Key 則按 token 計費無限制，但費用可能更高。

## T（核心論點）

**最重要的陷阱**：有 `ANTHROPIC_API_KEY` 環境變數存在時，Claude Code 會自動使用 API Key 計費，不管你有沒有 Max 訂閱。許多人設了 API Key 後忘記，結果 Max 的月費帳單外還有 API 帳單。

```bash
# 確認現在用哪種模式
echo $ANTHROPIC_API_KEY   # 有輸出 = API Key 模式
# 清除讓它用訂閱
unset ANTHROPIC_API_KEY
```

## B（適用場景）

**選訂閱（Pro/Max）：** 固定預算、日常開發、不確定用量  
**選 API Key：** 自動化腳本（Cowork 排程）、企業計費、需要完整 API 控制

## R（完整內容）

### 方案比較

| 方案 | 月費 | 含 Claude Code | 適合 |
|------|------|----------------|------|
| Pro | $20 | 有（限速） | 輕度使用 |
| Max 5x | $100 | 有（Pro 5 倍量） | 中度開發 |
| Max 20x | $200 | 有（Pro 20 倍量） | 重度開發 |
| API Key | 按用量 | 可用（另外計費） | 自動化 / 企業 |

### 雙層限速機制

**Layer 1 - 對話限速（每 5 小時）：** 輸出 token 計算，觸發時等 5 小時  
**Layer 2 - 週期配額（每週）：** 整週累計，觸發時配額重置後才恢復

兩層分開計算，都觸發時必須等外層（週）重置。

#### 撞到五小時上限時：優雅收尾與 9/22–23 額度調整（2026-09-29 補充）

- **優雅收尾**（ClaudeDevs 公告，aiposthub 2026-09-27 轉述）：撞到五小時上限時，Claude Code 會「**嘗試**」找一個安全的停止點；收尾用的是從**每週額度提撥的少量固定配額**，不是額外的免費額度，每週總量不變；官方**沒有承諾**會跑測試或提交變更。
  - ⚠️ 照實標註：原文**沒有版本號、沒有官方公告連結**（只附一段官方示範影片），無法確認從哪一版 Claude Code 開始。
- **9/22–23 額度調整**（rar.design 引官方，隨 Opus 5.5 發布）：Pro／Max／Team 的五小時上限調高，並送每位訂閱者一次可以自己選時間用的額度重置。來源沒有說明與優雅收尾是否為同一波政策，兩者並列。詳見 [[claude-opus-5-5-release-notes]]。
- 作者建議（非官方行為）：變更放在獨立分支；交接時分開列「已改／已測／待測」，測試狀態分三種（已跑且通過／已跑但失敗／因額度不夠沒跑）；不可逆操作保留人工確認。交接做法見 [[claude-code-session-handoff]]。

### 好用指令

```bash
/usage      # 看剩餘用量（5小時窗口 + 週期）
/context    # 看目前對話消耗了多少 token
/status     # 整體狀態（登入身份 / 計費模式）
/compact    # 壓縮對話 context（節省 token）
```

### API Key 模式設定

```bash
export ANTHROPIC_API_KEY=sk-ant-...
claude --model claude-sonnet-4-6   # 指定模型
```

API Key 模式優點：
- 無速率限制（只有 tier 上限）
- 可跑多個並行 session
- 適合 CI/CD 和 Cowork 自動化排程

API Key 模式缺點：
- 費用可能超出預期（特別是長 context 對話）
- 需要自行監控帳單

## C（關鍵行動）

- [ ] 檢查是否有 `ANTHROPIC_API_KEY` 環境變數意外啟用
- [ ] 確認自己的使用場景選對方案（日常用 Max 訂閱，Cowork 排程用 API Key）
- [ ] 用 `/usage` 養成定期查看剩餘用量的習慣
- [ ] 長任務接近五小時上限前先要求交接摘要，不要依賴優雅收尾

## 相關連結

- [[claude-cowork-setup-guide]] — Claude Cowork 設定指南（API Key 在 Cowork 的使用方式）
- [[loop-engineering-6-components]] — Loop Engineering（自動化流程設計）
- [[claude-swap-account-rotation]] — 多帳號限額輪替 CLI，正是為本篇 5h+7d 雙層限速而設計
- [[claude-opus-5-5-release-notes]] — Opus 5.5 發布時的 Pro／Max／Team 五小時上限調高與一次額度重置
- [[claude-code-session-handoff]] — 撞到額度上限前的交接做法（優雅收尾不保證跑測試或提交）
