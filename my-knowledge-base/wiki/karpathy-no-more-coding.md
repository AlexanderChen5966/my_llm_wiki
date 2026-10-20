---
title: Karpathy 2026：幾乎不再自己寫 code，AI Agent 成為主力
date: 2026-05-22
tags: [ai/agent, ai/workflow]
source: https://www.koc.com.tw/archives/643333
type: article
status: processed
asset_value: 80
---

Karpathy 在《No Priors》65 分鐘專訪中宣示軟體開發的根本轉變：從 2025 年 12 月起幾乎不再親手寫 code（由 20/80 → 現在更極端），Token 是新的 GPU（最大化吞吐量才是工程師的新責任），多 Agent 並行的「巨觀操作」是需要刻意練習的新技能。他以 Dobby 家庭 AI 管家、auto research 自主研究、microGPT 200 行教學法三個具體案例，說明 AI Agent 時代工程師角色的實質轉變。

## T（核心論點）

「Agent 做不到的事，現在就是你的工作了。Agent 做得到的事，它們可能做得比你好，或者很快就會。所以你應該策略性地思考你到底要把時間花在哪裡。」

工程師的角色從「寫 code 的人」轉為「設定目標、審查 diff、把概念濃縮到最純粹本質的人」。Karpathy 的貢獻是「那幾點」——理解本質，其他一切（解釋、調校、教育）交給 Agent。

## B（適用場景）

**適用：** 理解 2026 年 AI 開發範式轉移；重新評估個人工作流程與時間分配；理解 multi-agent 巨觀操作的思維模式

**不適用 / 注意：** Karpathy 是頂尖研究員，「幾乎不寫 code」的前提是他有能力定義「對的問題」；對 AI 能力邊界不清楚的開發者，直接模仿可能產生未被發現的錯誤

## R（核心洞察拆解）

### 工作流徹底翻轉：20/80 比例反轉

Karpathy 描述自己從 2025 年 12 月以來完全不再親手寫 code：
- 舊模式：80% 自己寫 + 20% 工具輔助
- 新模式：20% 監督方向 + 80% Agent 執行（現在更極端）
- 心理狀態：「精神病態」（psychosis）——不斷把 Agent 推向極限

### Token 即新 GPU：算力焦慮的轉移

- 博士生時代：GPU 沒在跑就焦慮（FLOPs 是稀缺資源）
- 現在：訂閱額度有剩就焦慮（Token 吞吐量是稀缺資源）
- 多 Agent 並行時，**你是系統的瓶頸**——這是技能問題，不是能力問題

### 巨觀操作（Macro Action）：多 Agent 的新肌肉記憶

Peter Steinberg 模式（同時跑 20+ Codex Agent）：
- 不說「這裡有一行程式碼」，而說「這裡有一個新功能，交給 Agent 1；這裡另一個不衝突的功能，交給 Agent 2」
- 盡你所能 **審查** 他們的工作——審查能力才是核心
- 這種思維模式是需要刻意培養的「新肌肉記憶」

### Dobby 家庭 AI 管家：一週完成全家自動化

- 用自然語言控制 Sonos、HVAC、燈光、窗簾、游泳池、保全
- 門外攝影機用 Quinn 模型識別訪客，WhatsApp 即時通知
- 結論：「客戶不再是使用者，而是 Agent」——App Store 裡的智慧家居 App 「根本不應該存在」，應該只是 API

### Auto Research：自主研究系統

- 讓 Agent 在客觀指標引導下自主進行 ML 實驗、調整超參數
- Karpathy 自己忘記的 value embeddings weight decay + Adam betas 問題，被 auto research 一晚找出來
- 終極願景：整個研究組織用 `program.md` Markdown 文件描述，Meta Agent 優化這些文件——「對研究本身的研究」

### microGPT：200 行純 Python 提煉本質

- 零依賴（無 PyTorch、無 NumPy、無 GPU）
- 包含：autograd 引擎（~100 行）+ GPT-2 架構（~50 行）+ Adam（~10 行）+ 訓練推理
- 核心洞察：神經網路訓練的複雜性來自「效率」，去掉效率需求，算法只有 200 行
- 類似 nanoGPT → makemore → microGrad → microGPT 的十年提煉路

### 教育典範轉移

- 舊思維：拍影片解釋給「人」聽
- 新思維：解釋給「Agent」聽——Agent 能以對方的語言、無限耐心傳達
- 文件應寫給 Agent 看的 Markdown，而非給人看的 HTML

### AI 鋸齒狀能力與模型物種分化

- 「鋸齒狀」（Jaggedness）：同一個模型可以進行複雜 Agentic 任務，但講笑話還是三年前的老梗
- 原因：可驗證（verifiable）任務有 RL 優化，軟性領域沒有
- 預測：未來出現更多「模型物種」——針對 Lean 數學、特定領域的專門化小型模型

## C（關鍵行動）

- [ ] 重新審視自己 20/80 的分配：哪些任務可以交給 Agent，自己的「那幾點」是什麼？
- [ ] 練習「巨觀操作」：不是給 Agent 一行指令，而是把整個功能邊界定義清楚後交給 Agent 執行
- [ ] 建立 Agent 審查能力：審查 diff 的能力比寫 code 的能力更重要
- [ ] Token 吞吐量意識：多個 Agent 同時運作時，自己是不是系統的瓶頸？

## 相關連結

- [[agentic-engineering-mindset]] — Karpathy 哲學觀：Bounded Context 護城河，品味不可外包
- [[agentic-engineering-sop]] — 六階段實戰 SOP：把 Karpathy 理念轉化為可操作工作流
- [[agentic-engineering-patterns]] — Simon Willison 實用工程模式
- [[llm-cost-optimization-strategies]] — Token 成本優化策略（Token 即新 GPU 的具體應對）
- [[harness-engineering]] — 護城河在環境而非模型
