---
title: AI 整理病歷：用 Claude 比對跨科別紀錄的 4 步驟
date: 2026-06-22
tags: [ai/claude]
source: https://www.bnext.com.tw/article/91275/ai-medical-record-summary-guide
type: article
status: processed
asset_value: 40
---

跨院所、跨科別的病歷難以人工整合。Claude 可以把分散的紀錄放在一起，找出潛在的關聯或矛盾（例如：三年前腦波檢測發現睡眠呼吸中止症但未被追蹤）。

**四步驟：**
1. **整理症狀時間軸**：上傳去除個資的病歷，讓 AI 整理發病時間、頻率、觸發點
2. **比對跨科別資料**：問「各科治療建議一致嗎？有哪些交互影響？」
3. **規劃下次就診**：「根據以上，我下次應該去哪個科、帶什麼問題？」
4. **人工確認**：AI 整理的結果必須給醫師確認，AI 不是診斷工具

**關鍵 prompt 設計：**
- 先去除姓名、身分證號、病歷號等個資才上傳
- 明確限制 AI 的角色：整理資訊 + 列提問，不是診斷
