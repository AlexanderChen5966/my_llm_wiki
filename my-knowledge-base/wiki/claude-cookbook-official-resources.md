---
title: Claude Cookbook：Anthropic 官方 Agent 工程範例庫索引
date: 2026-08-03
tags: [ai/claude, ai/agent, type/reference]
source: https://www.aiposthub.com/claude-cookbook-agent-engineering-guide/
type: reference
status: processed
asset_value: 55
---

**索引型條目**——記錄「Claude Cookbook 裡有哪些資源、卡在什麼問題時該去查哪一篇」，不做完整方法論拆解。

入口：[platform.claude.com/cookbook](https://platform.claude.com/cookbook/) ／ GitHub [anthropics/claude-cookbooks](https://github.com/anthropics/claude-cookbooks)（MIT License，範例以 Python 為主）

## 這是什麼

Anthropic 官方的**實作範例庫**，不是 Prompt 合集。每篇先設定一個具體工程問題，再給架構說明、可執行程式碼與執行方式。定位差異：

| | 官方文件 | Cookbook |
|---|---|---|
| 回答 | API 能做什麼、參數怎麼用 | 多個功能組合成具體案例 |
| 性質 | 規格來源 | 可拆解的實作範本 |

分類軸：Agent Patterns / Tools / Evals / Skills / Integrations / Multimodal / Responses。

## 查詢對照表：卡在什麼問題 → 看哪一篇

| 你的問題 | 該查的 Cookbook 篇章 | 該篇的核心機制 |
|---|---|---|
| 多個 Agent 怎麼分工、傳訊息、結束生命週期 | **Async multi-agent orchestration** | 固定 Agent 團隊 vs 任務中動態建立 subagents；Coordinator 拆分 → Search workers 平行查 → 整合回收 |
| 長任務超出上下文，哪些資料該留 | **Context engineering: memory, compaction, and tool clearing** | 三種機制的取捨：compaction（延長單次任務，但摘要有損）／tool-result clearing（減噪，但需要時得重查）／memory（跨 session 保存，但要決定寫什麼、何時更新） |
| 工具幾百個，模型選錯又燒 token | **Programmatic tool calling (PTC)** ＋ tool search 系列 | 讓 Claude 在 code execution 環境內用程式呼叫工具，減少每次工具執行都回到模型判斷的往返；適合大量查詢/篩選/彙整，**不適合每步都要重新推理的流程** |
| Agent 說「完成了」但實際沒完成 | **Outcomes: agents that verify their own work** | 獨立 grader 依 rubric 檢查產出，不通過就把具體缺口交回原 Agent 修訂 |
| 原型能跑，要變成服務 | **Hosting your agent** / **SRE incident response agent** / **Slack data analyst bot** | 容器、憑證、webhook、檔案掛載、人工審核 |
| 產出「AI 味」很重、介面很制式 | **Prompting for frontend aesthetics** | 把抽象評語（「要有質感」）拆成可控制面向：字體、配色、動效、背景、版型、資訊層級、主色比例 |

**沒有明確問題時的起點**：先看 Context engineering，理解 memory / compaction / tool-result clearing 的差別。

## 使用方式（避免收藏後就沒再打開）

1. 先寫出問題（「長任務會忘記前面的決策」「產出看似完整但引用不可靠」）
2. 找**最接近目前障礙**的模式，不要從最熱門的範例開始
3. 先跑官方最小範例確認環境／套件／API 權限，再換成自己的資料與工具
4. 把範例轉成**驗收條件**：成功條件、失敗處理、成本上限、人工確認點、資料保存規則
5. 回官方最新文件核對——Cookbook 是學習範例，不是 API 規格本身

## ⚠️ 使用限制（官方範例 ≠ 可直接上線）

- 部分內容使用 **Claude Managed Agents beta** 或特定 SDK 版本，介面與行為可能在正式推出前調整
- Cookbook 為教學而簡化情境：**不涵蓋**高流量、惡意輸入、工具誤用、跨系統資料一致性
- 網站與 GitHub 免費，但執行 API 範例會產生用量費用，部分功能需 beta 存取權
- 多數範例需要 API key、Python 環境與基本程式能力，**不是 no-code 模板庫**

## 三個可帶走的判斷原則

原文自己下的結論，比篇目清單更耐用：

1. **多 Agent 不是拆越多越好**——值得看的不是「同時開很多模型」，而是 Agent 間怎麼傳訊息、回報狀態、結束生命週期；一個 Agent 配清楚工具能做完就不必拆（與 [[awesome-agent-architecture-from-scratch]] 的「最少必要機制」判準一致）
2. **工具效率提升之前，先分開權限**——能被程式重複呼叫的查詢工具，和會寄信、付款、刪除資料的工具，不該用完全相同的開放方式
3. **獨立 grader 不是萬靈丹**——rubric 寫得模糊，模型只會更有系統地通過一個模糊標準。優先用在能明確定義完成條件的任務（欄位完整性、引用可追溯性、測試是否通過）；純美感或策略判斷仍須人類決策

> 「使用者說希望 AI 記得所有事情，不代表系統真的該無限制保存」——記憶功能仍要定義資料範圍、有效期限、可刪除性與隱私規則。

## 相關連結

- [[awesome-agent-architecture-from-scratch]] — 22 章 harness 教材；**與本篇互補**：那篇是「地圖」（機制全貌與分層），Cookbook 是「食譜」（依問題查可執行範例）
- [[anthropic-academy-courses]] — Anthropic 官方課程資源，同屬官方學習素材入口
- [[claude-agent-sdk-human-in-the-loop]] — Human-in-the-loop 實作，對應 Cookbook 部署篇的人工審核環節
- [[production-agent-architecture]] — 生產級模組清單，補上 Cookbook 明說「不涵蓋」的上線考量
- [[taste-skill-anti-slop-frontend]] — 前端「AI 味」移除，與 Cookbook 的 frontend aesthetics 同命題不同來源
- [[claude-code-context-management]] — Context 管控實務，對應 Cookbook 的 context engineering 三機制

## 原始摘要

aiposthub（Philo，2026-07-27）介紹 Claude Cookbook。定位為「問題導向的 Agent 工程索引」而非線上課程；整理六類最值得先看的內容（多 Agent 協作／記憶與上下文／工具呼叫／驗證迴圈／部署維運／前端美學），並列出使用限制與五步使用流程。適合對象：串接 Claude API 的開發者、AI 產品經理、做技術原型的創作者。
