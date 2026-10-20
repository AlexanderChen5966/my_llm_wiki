---
title: Claude 模型怎麼選？Fable、Opus、Sonnet、Haiku 一表看懂
date: 2026-07-16
tags: [ai/claude]
source: https://www.bnext.com.tw/article/90667/claude-model-comparison-opus-sonnet-haiku
type: article
status: processed
asset_value: 65
updated: 2026-09-29
---

Claude 模型陣容從三款擴充為四層級：Haiku 4.5、Sonnet 5、Opus 4.8，以及 2026 年 6 月推出、位階凌駕 Opus 之上的 Mythos 級模型 Fable 5，四者輸出成本最貴與最便宜相差達 10 倍。

## T（核心論點）

選錯模型的代價是成本或品質的雙輸——規模化自動處理交給 Haiku、多數日常產出交給 Sonnet、高難度邏輯任務交給 Opus，只有最吃重的長時程代理任務且值得為品質多付兩倍成本時，才升級到 Fable 5。判斷依據可簡化成三個維度：能力（是否具備特定功能）、速度（應用場景需要多快）、成本（開發與生產預算）。

⚠️ 2026-09-23 陣容已更新，見下方〈2026-09-23 陣容更新〉；R 層舊表保留為 2026-07-16 的快照。

## B（適用場景）

**適用：** 日常高頻的「這個任務該用哪個模型」決策參考。
**不適用 / 注意：** Fable 5 強制 30 天資料保留、不提供零資料保留（ZDR）選項，對有合規需求的企業使用者是重要考量，需另外評估。

## R（四款模型比較）

> ⚠️ 本表為 **2026-07-16 快照、已過時**（陣容與價格已被下方〈2026-09-23 陣容更新〉取代），表格內容保留原樣作為歷史紀錄。

| 模型 | 定位 | 適合任務 | 定價（輸入/輸出，每百萬 token） |
|------|------|---------|------|
| **Fable 5** | Mythos 級，目前最強 | 大型程式庫跨檔案重構、長時程自主代理工作流、高複雜度深度研究 | $10 / $50 |
| **Opus 4.8** | 標準線旗艦，最智慧 | 複雜軟體開發除錯、數小時研究分析、企業級自動化代理 | $5 / $25 |
| **Sonnet 5** | 兼具智慧與速度，預設主力 | 程式碼生成、資料分析、內容寫作、多輪代理工具使用 | $3 / $15（8月底前優惠 $2/$10） |
| **Haiku 4.5** | 最快速、高性價比 | 即時互動、大規模批次任務、成本敏感原型開發 | $1 / $5 |

Fable 5 內建安全備援機制：涉及資安/生物等高風險領域問題會自動改由 Opus 4.8 回答。四款模型上下文窗口皆為 1M tokens（Haiku 為 200K），最大輸出皆 128K（Haiku 為 64K）。

## 2026-09-23 陣容更新

> 來源：rar.design〈Claude Opus 5.5 為什麼更強還更便宜？〉（2026-09-23，整理 Anthropic 官方公告與 Claude 平台文件）。跑分、API 破壞性變更與方案細節見 [[claude-opus-5-5-release-notes]]，本段只放選型需要的內容。

| 模型 | 定位 | 預設 effort | 定價（輸入/輸出，每百萬 token） |
|------|------|------|------|
| **Mythos 5.1** | 最高階，只開放給審核通過的組織 | — | 不公開列價 |
| **Fable 5.1** | 最強的公開模型，留給高難度推理與長時程代理任務 | high | $10 / $50 |
| **Opus 5.5** | **新的日常主力**，官方稱大部分工作追平 Fable 5.1 | medium | $4 / $20 |
| **Sonnet 5** | 速度與智慧的平衡 | — | $2 / $10（9/23 官方平台文件列價；是 8 月底優惠延長還是永久降價無法判斷，待核對官方定價頁） |
| **Haiku 4.5** | 最快、最便宜 | — | $1 / $5 |

**新的選型規則：**
1. 預設用 Opus 5.5（官方平台文件：不確定用哪顆，先從 Opus 5.5 開始；卡住先調高 effort）。
2. Opus 5.5 開到高 effort 仍做不好的最難任務，才升 Fable 5.1。
3. Pro 方案：Opus 5.5 吃訂閱額度；用 Fable 5.1 要另開 usage credits、按 API 費率計費。

> 📎 2026-10-08：實測立場不同——Tim Wei 用真實任務評測（樣本小，每題 1–2 次）後的建議是平常用 Sonnet 5.5，條件多、要多想一步再換 Opus 或 Fable。官方預設與個人實測是立場差，不是事實矛盾。見 [[claude-models-real-task-eval-tokyo-trip]]。

Sonnet 5.5／Haiku 5.5 官方說「未來幾週」推出，未給日期，屆時再更新本段。

> ⚠️ 2026-10-08 更新：Sonnet 5.5 已於 2026-09-28 發布（作者轉述官方，見 [[claude-models-real-task-eval-tokyo-trip]]）；Haiku 5.5 截至 10 月初仍未推出。Claude Code 內 Opus 5.5 與 Sonnet 5.5 的預設 effort 都是 medium。

**陣容描述的更正（相對於上方 7/16 舊表）：**
- 7/16 版把 Fable 5 稱為「Mythos 級」；但 9/23 官方家族排序中，Mythos 是 Fable 之上的**獨立一層**（Mythos 5.1 ＞ Fable 5.1 ＞ Opus 5.5 ＞ Sonnet 5 ＞ Haiku 4.5）。
- Opus 4.8 不再是標準線旗艦，現在主要是 Opus 5.5 防護機制中資安題目的轉送目標。
- B 層「Fable 5 不提供 ZDR」的提醒是否延續到 Fable 5.1：本批來源沒有提到，**未確認**。

## C（關鍵行動）

- [ ] 不確定該用哪個模型時，先用 Haiku 4.5 跑一輪日常任務，確認哪些環節「不夠用」再往上升級
- [ ] 高複雜度且長時程的代理任務才考慮 Fable 5，並先確認公司是否有 ZDR 合規要求
- [ ] 定期依任務重量切換模型，而非固定用同一款處理所有任務
- [ ] 2026-09-23 起預設模型改成 Opus 5.5，舊的 Fable 5 升級判準改用 Fable 5.1

## 相關連結

- [[claude-code-model-and-thinking-depth]] — 🔑 補兩件事：`opusplan` 在圖形選單裡看不到要用打的；`think hard` 在 Claude Code 裡無效，唯一有效關鍵字是 `ultrathink`

- [[claude-conversation-style-value-axes]] — Claude 各模型的「回話風格」差異研究，可與本篇的能力/成本維度合併判斷選型
- [[claude-code-billing-usage]] — Claude Code 計費說明，可搭配本篇的模型定價做額度規劃
- [[claude-code-model-boomerang]] — 追蹤 session 被安全分類器/限額踢出主模型並一鍵切回，正對應本篇 Fable 5 遇高風險改跑 Opus 的備援機制
- [[claude-opus-5-5-release-notes]] — 2026-09-23 Opus 5.5 發布：價格、跑分、API 破壞性變更與方案差異（本篇〈2026-09-23 陣容更新〉的詳細來源）
- [[claude-models-real-task-eval-tokyo-trip]] — 2026-10 真實任務評測（5 模型、單一作者小樣本）：簡單任務分不出高下，差距在「承認不確定」與「題目沒講的事」；記載 Sonnet 5.5 於 9/28 發布
