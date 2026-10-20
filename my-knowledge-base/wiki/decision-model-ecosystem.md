---
title: 決策模型生態：Jev／Laya／OpenAI Decisions API 與 /v1/systemone 介面
date: 2026-10-08
tags: [ai/agent, ai/workflow, type/comparison]
source: https://www.aiposthub.com/jev-laya-decision-layer-agent-routing-tools-guardrails/; https://www.aiposthub.com/ollama-local-decision-model-deployment-guide/; https://www.aiposthub.com/openai-decisions-api-luna-guide/; https://www.aiposthub.com/llamacpp-systemone-local-decision-models/（官方公告 https://huggingface.co/blog/ggml-org/decision-models-in-llamacpp）; 比較數字引自 https://klab.tw/2026/09/laya-review-tutorial/、https://foreverwebs.com/blog/laya-open-source-decision-model-guide、https://vervecode.dev/ai/jev-laya-decision-models/
type: reference
status: processed
asset_value: 60
---

決策模型（只回答程式預先定義的選擇題／評分題／是非題，不生成文字）的**總綱條目**：只做比較、生態與「決策層放在哪」。
各產品細節不在這裡展開——Jev 的定位與數字口徑見 [[jev-system-one-model-concept]]，Jev 的 API 與設計模式見 [[jev-implementation-guide]]，
Laya 的安裝、數字前提與移植見 [[laya-open-source-decision-model]]。

⚠️ **來源集中度揭露**：本條的生態事實（決策層分工、Ollama、Decisions API、llama.cpp）來自 aiposthub 同一位作者（Philo）的 4 篇文章，
其中 Ollama、Decisions API、llama.cpp 三篇作者自陳**沒有實測**，屬「aiposthub 轉述官方」。
同作者的重複措辭（如「信心值≠正確率」「選擇≠授權」）**不算多來源共識**；本條生態事實目前**沒有獨立的第二來源**。
比較表中的 Laya／Jev 數字則來自 3 位不同作者的實測（klab、vervecode、恆遠數位科技；後者為廠商來源）。

## T（核心論點）

🔑 **決策模型在三週內從單一產品變成一個品類**：`/v1/systemone` 從 TypeSafe 的私有端點，變成至少 4 個執行環境共用的路徑名稱
（TypeSafe → laya-serve → Ollama 0.35 → llama.cpp）。這是**事實上的共用路徑**，不是任何組織訂定的正式標準，各家欄位相容到什麼程度也還沒有人驗證。

🔑 **選型的第一刀是「託管 vs 自架」，不是「哪個模型準」**——而且 🔴 **任何本機工具都跑不了 Jev 本體**（閉源），
「本機跑決策模型」指的是 Laya 與其他開放權重模型。

🔴 **各模型的 confidence 算法不同，門檻不能跨模型沿用**：換掉服務位址（例如 Jev client 改指向 laya-serve）程式能跑，但門檻要重設。

## B（適用場景）

**適用：**
- 要在 Agent／工作流裡加一個「快速判斷層」（分類、路由、下一步選擇），正在比較該用哪個產品或該不該自架
- 評估資料能不能離開本機、延遲預算多少、願不願意自己微調

**不適用 / 注意：**
- 不適用於查某個產品的參數細節——請直接看 jev-*／laya 條目
- ⚠️ 本條是 2026-09-15 至 10-04 之間的快照，生態變動極快（Laya 一週 28 版），Decisions API、Ollama、llama.cpp 三節都只有單篇二手轉述
- 🔴 延遲數字不能跨口徑比較：「Laya 比 Jev 快 7／7.8 倍」比的是**本機推論對跨海 API**，**不是模型速度**；Jev 延遲的三種口徑並列見 [[jev-system-one-model-concept]]

## R（選型步驟）

### 1. 先看三個產品的差異（每格一句，細節見連結）

| 項目 | **Jev**（TypeSafe AI） | **Laya**（Convai Innovations） | **OpenAI Decisions API** |
|---|---|---|---|
| 身分 | 2026-09-15 early access 的閉源託管 API → [[jev-system-one-model-concept]] | 2026-09-18 開源（程式碼＋權重 Apache 2.0），自稱「System 1 決策引擎」；開發者在印度，與 convai.com、TypeSafe 皆無關 → [[laya-open-source-decision-model]] | OpenAI 官方產品，DevDay 2026 發表，limited preview（見下方第 4 節） |
| 「Jev 替代品」說法 | — | 🔴 「Jev 的開源替代」是**第三方定位**（klab 標題、恆遠被動句），**Laya 不是 TypeSafe 產品**（klab FAQ 明說） | 原文未拿來和 Jev 比較 |
| 取用 | 官方 API 與多個託管管道 → [[jev-implementation-guide]] 取用管道時間軸 | `pip install laya`、`laya-serve`、llama.cpp（10/2 起） | OpenAI API，需 limited preview 資格 |
| 價格 | 只計輸入、輸出免費 → [[jev-implementation-guide]] | 免費，自付硬體與維運 | **未公布** |
| 開箱準確度 | TypeSafe 自評、無 ground truth → [[jev-implementation-guide]] 成績單 | 🔴 **基礎版零樣本 0.362，低於永遠猜最大類的 0.461**；官方 0.766 是用該 benchmark 訓練集**微調過**的版本 → [[laya-open-source-decision-model]] | **未公布** |
| 選項上限 | 255 → [[jev-implementation-guide]] | 實際約 20（選項共用 token 預算）→ [[laya-open-source-decision-model]] | 未公布 |
| 機率／confidence | Choice／Score 有 confidence，Noul 沒有 → [[jev-implementation-guide]] | 有，但算法與 Jev 不同，門檻不可沿用 → [[laya-open-source-decision-model]] | **未確認是否回傳** |
| 微調 | 不提供 | 可以，而且幾乎必須 | 未公布 |
| 一句話定位 | 託管、開箱即用的判斷 API | 可帶回家、但要先微調的判斷「底座」 | OpenAI 平台內的受控選擇器 |

### 2. `/v1/systemone` 時間線：同一個路徑名稱被多個環境採用

| 日期 | 誰提供 `/v1/systemone` | 能跑的模型 | 來源 |
|---|---|---|---|
| 2026-09-15 | TypeSafe 官方 API | Jev（閉源） | 官方 |
| ≤ 2026-09-25（laya 0.3.20） | Laya `laya-serve` | Laya 三個 checkpoint | klab 實測；🔴 首次出現的版本待核對 |
| 2026-10-01 | Ollama 0.35 | Nimble、Tev1（實驗性） | aiposthub 轉述，只有截圖 |
| 2026-10-02 | llama.cpp | Julia-1、Laya、Kev-4B、lev、OpenJev | aiposthub 轉述 ggml-org 官方公告 |

- OpenAI Decisions API 就目前資訊**沒有**採用這個路徑
- 🔴 **相容程度未驗證**：Ollama 一文沒描述機率怎麼回傳；Laya 自己就列了一份與 Jev 的移植差異清單（選項上限、Score 描述、confidence 算法）。
  能用同一個 client 呼叫 ≠ 回傳語意相同

### 3. 要自架時：本機執行矩陣

| 執行環境 | 支援的決策模型 | 授權 | 備註 |
|---|---|---|---|
| Ollama 0.35（10/1） | Nimble、Tev1（Tev1 標示實驗性） | 原文未提 | 名單**沒有 Jev、沒有 Laya** |
| llama.cpp（10/2 起） | Julia-1、Laya、Kev-4B、lev、OpenJev | OpenJev 為 **CC BY-NC 4.0（限制商業使用）**，其餘 Apache 2.0 | OpenJev 是名單中**唯一支援圖片**的模型 |
| Laya 自家套件／laya-serve | Laya 三個 checkpoint | Apache 2.0 | 見 [[laya-open-source-decision-model]] |

- 🔴 **Ollama、llama.cpp 都跑不了 Jev 本體**（閉源，不公開權重）
- Laya 不走 Ollama：9/25 klab 記「官方沒有 Ollama 版本」，10/1 Ollama 名單亦無；截至 10/4 Laya 走 llama.cpp 或自家套件
- 官方提醒（llama.cpp 公告）：**同一封模糊信在 Julia-1 與 Kev-4B 上信心值差很多**——換模型就要重選門檻（同類現象見 [[llm-judge-confidence-cascade]]）
- Ollama 版本更新附註見 [[ollama-2026-complete-guide]]

### 4. OpenAI Decisions API（資訊薄，不單獨建條）

- **OpenAI 官方產品**，DevDay 2026 發表（官方回顧 openai.com/index/devday-2026-recap 有示意圖，aiposthub 2026-09-30 轉述）
- 用途：內容分類、請求路由、決定 Agent 的下一個動作；開發者給一組有限、預先定義的答案，由「Luna」選出一個
- 狀態：**limited preview**，官方「預期數日內擴大」，但沒有正式日期
- 🔴 **原文只寫「Luna」**：未說明是 GPT-6 Luna 還是專用變體、**未公布價格**、**未確認是否回傳機率或信心值**。
  因此 **不可套用** [[gpt-6-astra-prompting-guide]] 記錄的 GPT-6 Luna $0.10／$0.50
- 與 Agents API 的分工（原文圖說）：Decisions API 選下一步，真正執行工具仍由 Agents API 或應用層控制（🔴 Agents API 是託管平台，不是開源的 Agents SDK，見 [[openai-agents-api-public-beta]]）
- 作者的設計建議（**作者觀點**）：選項要互斥、完整、可執行；一定保留「不確定／人工審查」；不要拿商業動作當類別名稱（「拒絕客戶」改成「高風險需審查」）；評測看各類別精確率、召回率、混淆矩陣，並用影子模式上線

### 5. 決策層放在 Harness 的哪裡

來源：aiposthub（2026-10-02），**作者明說文中情境與數值都是設計示範，不是實測**。

- Harness 負責狀態、工具介面、權限、重試與紀錄；決策層放在 Harness 裡回答「走哪一條路」，**實際執行與權限檢查仍由程式負責**
- 三種分工：**模型路由**（交給哪個模型）／**工具選擇**（用哪個工具）／**風險控管**（能不能做）
- 三個出口：**ASK**（信心不足就補資料）／**REVIEW**（風險不明就人工覆核）／**EXECUTE**（符合條件才執行）；建立可信門檻後再逐步擴大自動處理範圍
- 路由選項可以包含「補資料／等待人工／暫停」，不必只在兩個模型中選一個
- 🔑 **信心值不能替代操作權限**：帳戶權限、可用工具與確認要求由程式執行，模型再有信心也不會因此取得權限 → [[agent-tool-authorization-gatekeeper]]、[[agent-control-layer-permission-hooks-planning]]
- Choice／Score 回傳 confidence、Noul 沒有——細節見 [[jev-implementation-guide]]
- 其他路由實作路線：[[opensquilla-intelligent-model-router]]；路由器與協調器的分界：[[agent-router-vs-coordinator-state-tracking]]

## C（關鍵行動）

- [ ] 選型時先回答「資料能不能離開本機、願不願意自己微調」，再看準確度
- [ ] 任何從 Jev 換到其他 `/v1/systemone` 實作的遷移，都把門檻當成需要重新校準的設定，不沿用
- [ ] 引用 Laya 準確度時永遠和 0.362／0.461 一起寫，不只寫 0.766
- [ ] 引用 Decisions API 時不要套任何 Luna 價格，等正式文件
- [ ] 決策層的輸出只決定「走哪條路」，權限檢查寫在程式裡
- [ ] 下列待核對項目有官方資料後回來更新本條

## 待核對

1. Ollama 0.35 官方發布原文（第 7 篇只有截圖、沒有連結）：版本號、Nimble／Tev1 規格、機率回傳格式
2. llama.cpp 名單中的「lev」是否為拼寫錯誤；OpenJev 是否就是 TheoLeeCJ/openjev（見 [[jev-implementation-guide]] 社群案例）
3. Laya 以 GGUF 在 llama.cpp 執行的實際形態（Laya 是 BERT 系編碼器）
4. OpenAI Decisions API 正式文件：Luna 代數、價格、是否回傳機率、GA 日期
5. Laya 第一次出現 `laya-serve` 的版本（可化解 9/23「沒有 API」的時間點，見 [[laya-open-source-decision-model]]）

## 原始摘要

本條由 raw-pipeline 第九批（2026-10-08）新建，主幹來源：《Jev、Laya 決策層教學：Agent 模型路由與風險控管》（aiposthub，2026-10-02）→ 決策層一節；
《Ollama 支援本機決策模型》（aiposthub，2026-10-01，未實測）→ 本機矩陣；《OpenAI Decisions API 完整解析》（aiposthub，2026-09-30，未實測）→ Decisions API 一節；
《llama.cpp 支援決策模型》（aiposthub，2026-10-04，轉述 ggml-org 10/2 公告，未實測）→ 本機矩陣、時間線、授權。
比較表數字引自 klab（2026-09-25）、恆遠數位科技（2026-09-23，廠商）、vervecode（2026-09-19）三篇實測，細節都在 [[laya-open-source-decision-model]]。

## 相關連結

- [[jev-system-one-model-concept]] — Jev 定位、confidence 定義、數字口徑（含延遲三種口徑）
- [[jev-implementation-guide]] — Jev API、設計模式、取用管道、社群案例
- [[laya-open-source-decision-model]] — Laya 自架評估、數字前提與移植差異
- [[ollama-2026-complete-guide]] — Ollama 0.35 決策介面附註
- [[gpt-6-astra-prompting-guide]] — GPT-6 Luna 價格；🔴 不可套用到 Decisions API
- [[agent-tool-authorization-gatekeeper]] — 信心值不能替代操作權限（LLM 提案、政策裁決）
- [[agent-control-layer-permission-hooks-planning]] — Capability ≠ Permission
- [[llm-judge-confidence-cascade]] — 高信心仍會錯、門檻不能跨模型沿用的同類現象
- [[opensquilla-intelligent-model-router]] — 另一條模型路由實作路線
- [[agent-router-vs-coordinator-state-tracking]] — 路由器與協調器的分界
