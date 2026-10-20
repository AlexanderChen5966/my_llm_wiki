---
title: Felix Rieseberg 工作術：Claude 工程主管的 AI 協作原則
date: 2026-06-15
tags: [ai/claude, pkm/workflow]
source: https://www.koc.com.tw/archives/felix-rieseberg-workflow
type: article
status: processed
asset_value: 78
---

Felix Rieseberg（Claude Cowork & Claude Code Desktop 工程主管）揭露的私房工作術。核心論點：**問題不在工具，而在想像力的缺口（Imagination Gap）**——你能想像 Claude 做到什麼的上限，決定了你能得到的幫助有多大。

## T（核心論點）

**抽象層上移（往上提一層）**是最核心的思維轉換：遇到繁複任務時，不要繼續手動做、也不要半自動做，而是停下來問：
1. *Claude 能做這個嗎？*
2. *我能建立一個系統，讓我永遠不需要再做這個嗎？*

從「控制每一行程式碼」轉型到「相信結果、設計系統」。

## B（適用場景）

**適用：**
- 評估某項重複任務是否值得委託或自動化
- 選擇模型（Sonnet vs Opus）
- 決定何時切換到 live artifacts + connectors 工作模式

**不適用：**
- 探索未知領域的第一次嘗試（先用 Opus，不要過早系統化）

## R（詳細原則）

### 1. Imagination Gap（想像力缺口）

大多數人 Claude 用不好，不是 Claude 不夠強，而是**想像不到 Claude 能做到什麼**。解法不是學更多技巧，而是刻意練習「想更大」——在腦中把 Claude 的能力邊界推得更遠。

### 2. 抽象層上移（往上提一層）

```
舊思維：「這件事很繁瑣，我用 Claude 一步一步做」
新思維：「這件事很繁瑣 → 停。Claude 能整個接管嗎？ → 能的話建立系統，永遠不再手動做」
```

Felix 的 anti-to-do list：不是記「要做什麼」，而是記「不應該再由我親自做的事」。每次發現自己在手動做可自動化的事，就加進這個清單，然後建系統。

### 3. Reverse Interview Method（逆向訪談）

不是你問 Claude 問題，而是**讓 Claude 問你問題**。先說「我想做 X，在你開始之前，你需要了解什麼？請列出問題」。這能快速暴露你沒想到的盲點，比自己寫詳細 prompt 更省力。

### 4. 模型選擇原則

| 情境 | 選擇 | 原因 |
|------|------|------|
| 問題定義清楚，需要執行 | Sonnet | 快速、成本低 |
| 問題模糊、尚未定義好 | Opus | 更善於探索、釐清問題 |

**先用 Opus 釐清問題，再用 Sonnet 執行**——不要在問題還模糊時就用 Sonnet，因為它會直接執行你說的（可能是錯的方向）。

### 5. Live Artifacts + Connectors

與其讓 Claude 產出「靜態文件」，Felix 偏好 **live artifacts**：
- 連接 Gmail → 能讀你的 email、分析模式
- 連接 Spotify → 能看你的收聽習慣
- 連接 Notion → 能讀你的筆記庫

這讓 Claude 的輸出從「通用建議」變成「基於你真實資料的建議」，信噪比大幅提升。

### 6. 相信結果，不管過程

從「控制 Claude 怎麼寫每一行 code」轉型到「告訴 Claude 目標，讓它決定怎麼到達」。這需要：
- 清晰的驗收標準（怎樣算「成功」）
- 願意接受 Claude 的路徑和你預想的不同

## C（關鍵行動）

- [ ] 建立自己的 anti-to-do list：記錄「我不應該再手動做的事」
- [ ] 下次遇到繁瑣任務，先問「能往上提一層嗎？」再行動
- [ ] 試用 Reverse Interview Method：讓 Claude 先問你問題

## 相關連結

- [[loop-engineering-6-components]] — Loop Engineering：系統化自動化的具體方法
- [[claude-code-advanced-workflow]] — Boris Cherny 進階工作術（Anti-to-do list 的技術實作）
- [[agentic-engineering-mindset]] — Agentic Engineering 哲學（角色轉型）
- [[synthesis-agentic-engineering]] — 執行者→監督者的完整轉型框架
- [[claude-code-goal-command]] — /goal 指令（「相信結果」的技術實作）
