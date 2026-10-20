---
title: Twinkle Hub：台灣公開資料 MCP 服務
date: 2026-05-21
tags: [ai/mcp, pkm/workflow]
source: https://github.com/ai-twinkle/Hub
source_2: https://cyberq.tw/2026/05/12/twinkle-ai-taiwan-mcp-hub/
source_3: https://www.bnext.com.tw/article/90971/twinkle-hub
type: article
status: processed
asset_value: 40
---

Twinkle Hub 是 Twinkle AI 開發的 MCP 服務，把台灣政府資料開放平臺上 49,343 筆資料集整合成單一 MCP 端點，讓 Claude、Cursor 等支援 MCP 的 AI 客戶端直接查詢台灣在地公開資料，解決「AI 只能叫你去官方網站查」的痛點。目前 Alpha 免費，無使用上限。

## T（核心論點）

台灣開放資料雖然豐富（近 5 萬筆），但分散在上百個入口、格式五花八門，AI 無從取用。Twinkle Hub 用 MCP 協定統一整合，讓自然語言查詢取代手動搜尋，同時提供 32 個台灣在地工具（地址格式、機關名稱等）修正開源模型在本土任務的常見錯誤。

## B（適用場景）

**適用：** 需要台灣政府公開資料的研究者、記者、開發者；不動產（實價登錄）、環境（空氣品質）、政府採購案、觀光旅宿合法性查詢；Claude Code 或桌面版 Claude 用戶
**不適用 / 注意：** 資料即時性受限於 API 同步週期（如採購資料可能滯後數週）；民宿查詢可能有多筆相似名稱、無模糊提示；Alpha 階段功能穩定性仍在測試中

## R（操作步驟）

### 安裝方式一：Claude Code（最快）

```bash
# 1. 登入 Twinkle Hub 取得 API key（Google / GitHub 帳號一鍵登入）
# 2. 在 Claude Code 執行：
claude mcp add --transport http twinkle-hub https://api.twinkleai.tw/mcp/ \
  --header "Authorization: Bearer sk-..."
```

### 安裝方式二：桌面版 Claude

將以下安裝提示詞貼入 Claude 對話（替換 API key）：

```
幫我把 Twinkle Hub 加到 Claude Desktop（編輯
~/Library/Application Support/Claude/claude_desktop_config.json）。
加入這段：
{
  "mcpServers": {
    "twinkle-hub": {
      "type": "http",
      "url": "https://api.twinkleai.tw/mcp/",
      "headers": {
        "Authorization": "Bearer 我等下從 https://hub.twinkleai.tw/dashboard 給你貼"
      }
    }
  }
}
```

### 驗證串接

```
/mcp  # 在 Claude Code 確認 twinkle-hub 已連線
```

### 資料涵蓋範圍

| 領域 | 範例資料 |
|------|---------|
| 環境 | PM2.5、空氣品質監測 |
| 不動產 | 實價登錄 |
| 交通 | — |
| 醫療 | — |
| 食品 / 藥物 | — |
| 政府採購 | 招標案、決標金額、得標廠商 |
| 觀光旅宿 | 民宿合法登記狀態 |

共 20 個領域，每天與政府同步更新（各類別同步頻率不同）。

## C（關鍵行動）

- [ ] 登入 hub.twinkleai.tw 取得 API key，串接 Claude Code
- [ ] 使用自然語言詢問台灣在地資料（「顯示台北近 30 天 PM2.5 趨勢」、「今年金額最高的前 10 筆政府採購案」）
- [ ] 注意資料即時性限制：重要決策仍須驗證最新官方來源

## 實測結果摘要

《未來商務》三項實測（2026-05-18）：
1. **台北 PM2.5 趨勢**：成功從環境資料集拉取監測站數據，給出趨勢分析，但資料更新至 5/08
2. **政府採購前 10 大合約**：回傳含機關名稱、標案名稱、得標廠商、決標金額的清單，與採購網吻合，但資料截至 3 月中旬
3. **民宿合法登記查詢**：成功比對登記狀態與核准字號，但對模糊店名無「可能有多筆」提示

## 補充資料

### 數位時代串接教學與使用情境（2026-06-02）

數位時代的圖卡教學提供更簡潔的三步驟串接流程：
1. **取得 API 金鑰**：前往 hub.twinkleai.tw，用 Google/GitHub 登入，系統自動發送 API key
2. **串接 Claude**：Claude Code 用一行指令安裝；Chat 分頁貼提示詞跟著操作
3. **重啟確認**：重啟 Claude → `/mcp` 確認 twinkle-hub 出現在清單

### 新增資料範圍

- **國考考古題**：近 14 年、6.4 萬份考卷、32 萬道高考/地特/司法官試題
  - 使用方式：「從考古題出 5 題行政法讓我練習」→ AI 撈出真題與答案 → 答錯可請求進一步解釋
- **公司登記資料、大學各科系錄取率**

### 實用問法範例

- 生活查詢：「最近大安區的房子成交價？」「某間食品廠有違規記錄嗎？」
- 政策追蹤：「某位立委在這個法案投贊成還是反對？」
- 政府採購：「2026 年政府採購金額超過 10 億的標案有哪些？」
- 國考備考：「從考古題出 5 題行政法讓我練習」

### 注意事項

建議提問時順便請 Claude 說明「最新資料收錄到哪個日期」，不同資料集更新頻率不同，重要決策仍須與官方最新公告交叉比對。

## 相關連結

- [[agent-skills-vs-mcp]] — Skills vs MCP 選擇邏輯
