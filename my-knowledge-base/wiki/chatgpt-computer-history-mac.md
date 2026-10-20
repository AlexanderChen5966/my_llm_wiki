---
title: ChatGPT Computer History：從截圖改抓互動事件，以及它放大的風險
date: 2026-08-14
tags: [ai/workflow, ai/agent]
source: https://www.kocpc.com.tw/archives/665255（電腦王阿達，2026-08-14）
type: article
status: processed
asset_value: 55
---

## T（核心論點）

> **Computer History 不是 Chronicle 改名，而是砍掉重練：完全放棄螢幕截圖，改抓 interaction events（點擊、打字、快捷鍵、切換 App，以及 macOS 輔助使用系統曝露的上下文）。**

代價是 OpenAI 官方自己承認的兩個風險：**生成的檔案不加密**，且 **「Computer History 會提高來自 App 和網站內容的提示詞注入風險」**。

## B（適用場景）

**適用：**
- 想讓 AI 接續「做到一半被打斷的任務」而不必重新解釋脈絡
- 評估同類「本機活動記錄 → AI 記憶」功能的隱私邊界

**不適用 / 注意：**
- 🔴 **Mac 桌面版 App 專屬**（需搭配系統內建輔助使用）
- 🔴 **個人方案只有 ChatGPT Pro 能用，Plus 還沒有**；Business／Enterprise 需管理員開放權限
- ⚠️ **預設關閉**，且必須先開啟記憶才能運作
- ⚠️ 整理活動與生成記憶都會**消耗 token**，長時間開啟會佔用額度

## R（資料流與保留方式）

```
Mac 本機累積事件（暫存事件檔，最多留 48 小時）
  → 送到 OpenAI 伺服器處理成記憶
  → 原始事件檔在伺服器端不保留、不用於訓練
  → 記憶檔回存到本機 ~/.codex/memories/extensions/skysight/
```

兩種檔案的分工：

- **暫存事件檔** — 最原始的操作過程（幾點按了哪些鍵、點了哪裡、切到哪個 App），逐筆累積，**紀錄量很大**
- **記憶檔** — 濃縮後的摘要，大約是「下午在 Figma 修改簡報第 3 頁，接著在瀏覽器查了某個規格」這種一兩句話的結論。⚠️ **記憶檔你不刪會一直保存**

**明確排除的項目**：螢幕截圖、螢幕錄影、麥克風輸入、系統音訊，以及**無痕模式的瀏覽活動**。使用者可指定哪些 App／網站要記錄或排除，也能逐項檢視時間軸後再決定是否刪除。

生成的記憶與時間軸 **ChatGPT 與 Codex 兩邊都能引用**，可以問「我剛剛看的那份文件在哪」「幫我總結今天做了哪些事」，或接續中斷的任務。

🔑 **它還會主動觀察操作模式：發現某個流程重複多次，就建議把它轉成可重複使用的 skill 或自動化腳本。**（這與 [[claude-cowork-record-a-skill]] 的錄製路線殊途同歸——一個靠錄製、一個靠被動觀察）

## C（關鍵行動）

- [ ] 開啟前先把含健康、財務或個人資料的 App 加入排除清單
- [ ] **與他人溝通時先關掉**（OpenAI 自己的建議，除非對方事先明確同意）
- [ ] 意識到 `~/.codex/memories/extensions/skysight/` 底下的檔案**不加密**

## 補充資料

### 🔴 官方承認的兩個風險

1. **檔案不加密** — 產生的檔案可能含敏感資訊，且不加密，**其他以你的 macOS 使用者身分執行的程式有可能讀取到它們**
2. **提示詞注入風險被放大** — 官方文件原文即載明會提高來自 App 和網站內容的注入風險。這是 [[prompt-injection-vs-jailbreak]] 那組判準的直接應用：**Source 端從「你主動貼進去的內容」擴大成「你螢幕上出現過的一切」，而 Sink 端是能接續執行任務的 Codex**——兩端同時放大

OpenAI 的緩解建議偏向使用者自律（排除敏感 App、溝通時關閉），而非技術性隔離。

### 與 Chronicle 的差異不只是改名

| | Chronicle（研究預覽） | Computer History |
|---|---|---|
| 擷取方式 | **本機擷取螢幕畫面** | **interaction events**（點擊／打字／快捷鍵／切換 App／輔助使用上下文）|
| 摘要方式 | 定期叫 Codex 把最近活動摘要成記憶 | 事件送伺服器處理成記憶，原始事件不留存 |

放棄截圖這個決定同時解釋了為什麼它需要輔助使用權限、也是為什麼「螢幕截圖／錄影」反而出現在排除清單裡。

## 相關連結

- [[agent-memory-lifecycle]] — Memory 四層生命週期：什麼值得記、何時 Recall；本功能是「Selection 交給系統被動觀察」的一種極端
- [[claude-code-hooks-memory]] — Claude Code 的記憶機制與強制力光譜
- [[prompt-injection-vs-jailbreak]] — 不可信 Source × 高權限 Sink 的判準
- [[claude-cowork-record-a-skill]] — 把重複流程轉成 skill 的錄製路線（三家對照）
- [[chatgpt-skills-usage-guide]] — ChatGPT 的 Skill 使用
- [[codex-learning-roadmap]] — Codex 生態（提及 Chronicle）
- [[chatgpt-chat-work-codex-comparison]] — ChatGPT／Work／Codex 三者定位
