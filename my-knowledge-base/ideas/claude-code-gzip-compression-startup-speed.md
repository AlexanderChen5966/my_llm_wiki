---
title: Claude Code 加入 gzip 壓縮與雲端 session 提早啟動：改善的是「開始回應前」的等待
date: 2026-10-30
tags: [ai/claude]
source: https://www.aiposthub.com/claude-code-gzip-compression-cloud-startup-speed/
type: idea
status: processed
asset_value: 8
---

## T（核心論點）

Claude Code 於 2026-10-17（台灣時間）經 ClaudeDevs 公告兩項改動：①每輪送出的對話先以 **gzip（無損傳輸壓縮）** 再傳；②**雲端 session 在使用者開始打字時就啟動**，與輸入時間重疊。改善的是「送出到開始回應」的傳輸與環境準備等待，**不是模型思考或輸出速度**；公告沒有統一提速比例，也沒有宣布費用或額度減免。

> ⚠️ **時效與性質**：aiposthub（Philo，2026-10-18）二手整理，引 ClaudeDevs（X）公告與 CHANGELOG **2.1.300**；行為隨版本可能變動。官方延遲比較圖是 2026-08～10 的整體改善，**不能當 gzip 單項實測**。傳輸 byte 與 token 是不同指標，不能換算成帳單折扣。

## 重點

- gzip 屬可還原的無損壓縮，與 `/compact` 這類「摘要式」對話壓縮是不同概念（後者可能丟細節），勿混為一談；見 [[claude-code-billing-usage]] 談用量的脈絡
- **企業 proxy 排錯**：CHANGELOG 2.1.300 載明大型請求送往 `api.anthropic.com` 時，即使關閉遙測也會 gzip；可用環境變數 `CLAUDE_CODE_GZIP_REQUEST_BODIES=0` **停用**，作者建議僅在排錯對照時用（沒有相容性問題就不必關）；範圍只限該端點，不可延伸到所有第三方入口
- 排錯對照：保留同版本、同入口、同類任務，改設定前後各留一筆成功與失敗紀錄；分享錯誤代碼與版本即可，不要貼完整對話或憑證
- 雲端 session 提早啟動不會讓缺漏的套件、權限或程式庫存取自動補齊；卡在準備階段先看環境紀錄
- 作者建議的量測法（**本文自己的建議，非官方實測**）：分開記錄「送出→第一個回應」「工具執行」「整項任務完成」，並記版本／模型／執行位置；以可核對的完成證據為準

## 相關連結

- [[claude-code-billing-usage]] — Claude Code 用量與計費（本更新未宣布額度變動）
