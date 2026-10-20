---
title: 把 agent 軌跡變成訓練訊號：Harness-Zero 蒸餾 harness 行為、Perplexity 修正中途錯誤，強制規則仍留在程式層
date: 2026-09-29
tags: [ai/agent, type/concept]
source: https://www.aiposthub.com/harness-zero-agent-harness-distillation-44-3-percent/（Harness-Zero 解讀，🔴 論文出處待查）; https://www.aiposthub.com/perplexity-learning-from-real-world-mistakes/（Perplexity 解讀；一手出處 https://www.perplexity.ai/hub/blog/learning-from-real-world-mistakes 、https://x.com/perplexity_ai/status/2102493613192298913）
type: article
status: processed
asset_value: 60
---

> 兩個案例放在同一條：它們都是**訓練時期、改權重**，並以 agent 實際跑出來的軌跡當資料。本庫既有的做法多在推論時期（harness 外掛、reflection＋memory 不改權重），放在一起才看得出這條軸線。兩者的差異用對照表呈現，不混成單一結論。

## T（核心論點）

**agent 的一部分行為可以從軌跡蒸餾進權重，讓外層 harness 變薄；但權限、金流、資料刪除、法遵、不可逆動作這類強制規則，仍要留在 deterministic enforcement（不依賴模型判斷的固定執行規則）。**

這兩個研究重劃的是「**可以學成習慣的行為**」與「**必須由程式強制的規則**」之間的邊界，不是在說 harness 可以不要。Harness-Zero 部署時仍保留一個最小 harness，論文也承認沒有把外部框架全部消除。原文作者的結論是：「模型與 harness 應該重新分工，而非二選一。」

## B（適用場景）

**適用：**
- 設計 harness 時判斷某條規則該寫成程式碼強制，還是可以期待模型學會（偏好、工作技巧、驗證習慣屬後者）
- 讀「agent 自我改進」類研究或廠商公告時，拆解它到底量了什麼
- 評估高頻、穩定、成功條件清楚的任務，值不值得先付訓練成本換取較薄的部署框架

**不適用 / 注意：**
- 本庫是應用層，不會自己訓練模型；可遷移的是**分類與讀數紀律**，不是訓練流程本身
- 🔴 **Harness-Zero 論文出處待查**：剪藏只寫「2026 年 9 月的 arXiv 預印本」，沒有 arXiv 編號、作者、機構，也沒有論文或 repo 連結。本條目只能引用 aiposthub 的解讀
- 🔴 **Harness-Zero 的數字一律是論文自報、single-run、未經同儕審查與外部複現**
- 🔴 **Perplexity 的數字一律是廠商自述**（官方研究文＋X 貼文），不是外部審計
- 需求常變、每週更新工具、任務量小的團隊，維護可編輯的 harness 可能比重新蒸餾更實際（改 skill 或 middleware 可以 code review、回滾、hotfix；改權重要新資料、重新訓練、重新驗證）

## R（兩個案例）

### 一、Harness-Zero：把專用 harness 誘發的行為蒸餾進模型

**名詞**：最小 harness `h`（一個 system prompt＋一支 Bash 工具，mini-SWE-agent 風格）；專用 harness `h*`（針對領域演化出的工具、middleware、skills、memory）。

**三階段**：

1. **evolve and adapt**：用訓練任務演化出 `h*`，再改寫成私有參考 `K`（工具→動作配方、middleware→審查警告、skills／memory→診斷準則）
2. **agent-as-harness**：學生在 `h` 裡提出下一步，reviewer 參考 `K` 決定 PASS（原樣執行）或 REPLACE（改寫成 `h` 能執行的回覆）。被拒的版本與審查討論不進學生軌跡，避免把「老師在批改」的視角教給學生
3. **train and deploy**：成功軌跡做 LoRA SFT（2 epochs；三領域過濾後各 487／282／500 條 rollouts），部署時移除 `h*`、`K` 與 reviewer，只留學生＋`h`

難點在 action space 不同：專用工具（例如「驗證分子」）在只有 Bash 的學生那裡無法執行，reviewer 要把「效果」翻譯成學生可用的動作，例如把過早宣布完成的回覆，換成先讀回檔案確認結果的 Bash 操作。

**設定**：學生 Qwen3.5-9B；reviewer GPT-5.6 Sol；harness 演化用 Kimi K3＋Kimi Code。

**結果（🔴 論文自報、single-run、未同儕審查）**：

| 設定 | SpreadsheetBench Verified | AppWorld | USPTO Retrosynthesis | Macro avg. |
|---|---|---|---|---|
| 基礎模型＋最小 harness | 31.0 | 26.8 | 12.0 | 23.3 |
| 基礎模型＋evolved meta-harness | 39.0 | 48.2 | 38.0 | 41.7 |
| Harness-Zero 模型＋最小 harness | 44.0 | 58.9 | 30.0 | 44.3 |

讀數要點：

- 23.3 → 44.3 是 **+21.0 個百分點**，相對提升 90.1%。「提高 90.1%」不能寫成增加 90.1 個百分點
- 三個領域的 metric 不同（SpreadsheetBench 與 USPTO 是 pass@1，AppWorld 是 scenario goal completion），**macro-average 不是單一成功率**
- 🔴 **USPTO 30.0 仍低於專用 harness 的 38.0**：平均勝過，不代表每個領域都勝過
- 28 種 harness-exclusive patterns（先檢查再修改、儲存後重讀、避免覆寫預填欄位、用 RDKit 驗證分子、避免重複失敗呼叫等）平均恢復 **82.3%**，但只在「專用 harness 做得到、最小 harness 從未做到」的任務上評分，起點由篩選規則定為 0%，不是整體成功率；AppWorld 完整分頁擷取只恢復 40%、變更後 readback 50%
- 無參數更新測試（兩個 frontier 模型、六組平均）：agent-as-harness 81.1%／code-as-harness 78.1%／最小 harness 68.6%。逐項不一定勝出，例如 AppWorld 上 GPT-5.6 Sol：agent-as-harness 96.4＝最小 harness 96.4＞meta-harness 94.6

**成本與限制**：

- 「Zero」不是 zero-shot，也不是零資料、零 API、零算力；比較接近「部署時不再需要專用 harness」
- 收集軌跡時每一步多一次 reviewer 呼叫，USPTO 延遲變成 2.4 倍（部署時消失，但訓練前要先付）
- reviewer 要夠強，弱 reviewer 可能有害
- SFT 不一定吸收得了深層領域知識；context management 不易蒸餾成一次回覆
- 權重較難 hotfix 與回滾
- repo 資訊（Python 3.12–3.13、Docker、Harbor sandbox；模型路由可用 OpenAI／Azure AI Foundry／OpenRouter；訓練範例用 Tinker；三個 LoRA checkpoints）⚠️ **剪藏未附連結**

**原文建議的評估方式**：先挑一種高頻、穩定、成功條件清楚的工作；保留未參與演化與 SFT 的 private test set；比較基礎模型、專用 harness、reviewer 包覆、蒸餾後模型四種設定；除了 task success，也量錯誤類型、工具呼叫數、p95 latency、訓練與部署成本、安全違規。分數提高但更常越權或更難回溯，不算上線成功。

### 二、Perplexity：從真實工作階段的中途錯誤學習

**機制**（基礎模型 GLM 5.2）：

- **成功步驟**用拒絕採樣微調（RFT）模仿
- **可避免的中途錯誤**用策略內自我蒸餾（OPSD）：同一個模型當老師時看得到指出錯誤的提示，當學生時看不到，學生學老師看到提示後的下一步預測。例子：工具文件寫明回傳清單，模型卻當字典處理而出現型別錯誤
- 只有「具備有效、可追溯提示」的錯誤步驟才當修正目標，其餘保留作上下文；模仿目標替訓練定錨，避免師生靠忽略上下文而看似一致
- **歸因篩選**：提示必須指向模型當時拿得到的工具文件、系統指示或前文；排除含個資的工作階段、選擇退出訓練的使用者、已知的基礎設施故障與權限遭拒

**測量方式（🔴 廠商自述）**：

- 兩次**分開的**線上 A/B 測試，每個測試條件約 10 萬名使用者（沒有公布每組實際的工具呼叫總數）
- 指標是「有狀態紀錄的工具呼叫」失敗率，**不是任務完成率**

| 測試 | 比較版本 | 工具呼叫失敗率 | 結論 |
|---|---|---|---|
| 第一次 | 原始 GLM 5.2 vs 第一個後訓練版 | 2.94% vs 2.82% | **未達統計顯著** |
| 第二次 | 第一個後訓練版 vs 較新版 | 2.24% → 1.77% | 相對下降 21.2%（絕對 0.47 個百分點），達統計顯著 |

- 第二次的失敗率相對下降 21.2%，絕對差只有 0.47 個百分點，約每 1,000 次工具呼叫少 4.7 次失敗
- 兩次不能合併解讀成「新版比原始 GLM 5.2 少失敗 21.2%」，官方明說沒有把第二版直接和原始 GLM 5.2 做線上比較
- 中度或高度不滿意的**模型估計**機率 2.58% → 2.54%，**不顯著**；這是估計值，不是實際抱怨數
- 不代表模型會即時記住個別使用者的修正，流程是篩資料 → 後訓練 → 部署新版

### 三、兩個案例對照

| 維度 | Harness-Zero | Perplexity |
|---|---|---|
| 學什麼 | 專用 harness 誘發的行為（檢查、重讀、驗證習慣） | 當時可以避免的中途錯誤 |
| 老師是誰 | 外部強 reviewer（GPT-5.6 Sol）替換提案 | 同一個模型，看得到提示 |
| 資料 | benchmark 訓練任務的成功軌跡 | 真實使用者工作階段 |
| 評估 | 離線 benchmark、single-run | 線上 A/B、每組約 10 萬使用者 |
| 數字性質 | 論文自報，出處待查 | 廠商自述 |
| 沒證明的事 | 其他模型、多次實驗是否成立 | 任務完成率、答案正確性、使用者滿意度 |

### 四、可遷移到應用層的兩條紀律

1. **錯誤要能歸因到「模型當時拿得到的資訊」**：基礎設施故障與權限遭拒不算在模型頭上。對應 [[agent-error-recovery-taxonomy]] 的錯誤分類，以及 [[xcode-mcp-test-report-fabrication]] 的「工具層假訊號 vs 模型層造假」
2. **相對降幅要並列絕對差**：21.2% 對 0.47 個百分點、90.1% 對 21.0 個百分點。與 [[claude-opus-5-5-release-notes]]「成本降 40% 不是單價降 40%」同一種讀數紀律

## C（關鍵行動）

- [ ] 把 harness 裡的規則分兩類寫清楚：可以期待模型學會的（偏好、技巧、驗證習慣）與必須程式強制的（權限、金流、刪除、法遵、不可逆動作），後者永遠不交給權重
- [ ] 讀到「平均提升 X%」時，先查各子項是否每項都勝出（例如 USPTO 30.0 < 38.0），再查 metric 是否一致
- [ ] 讀到相對降幅時，自己算出絕對差與基期，寫在同一句
- [ ] 統計 agent 錯誤時，先剔除基礎設施與權限類錯誤，再談模型改進
- [ ] 遇到只有轉述、沒有論文編號的研究，在 source 標「出處待查」，數字標「論文自報」

## 原始摘要

Harness-Zero（aiposthub 解讀，論文出處待查）：用強 reviewer agent 把專用 harness 誘發的行為，改寫成最小 harness（一個 system prompt＋一支 Bash 工具）能執行的軌跡，再用 LoRA SFT 蒸餾進 Qwen3.5-9B。部署時只留最小 harness，三領域 macro-average 從 23.3 升到 44.3，高於掛專用 harness 的 41.7，但 USPTO 30.0 仍低於 38.0；結果為 single-run 預印本。Perplexity（官方研究文，aiposthub 解讀）：成功步驟用 RFT 模仿、可避免的中途錯誤用 OPSD 修正，並排除基礎設施與權限錯誤。第二次線上 A/B 的工具呼叫失敗率從 2.24% 降到 1.77%（相對 21.2%、絕對 0.47 個百分點），第一次比較不顯著，使用者不滿意度也不顯著。兩者都指向同一個分工：可學的習慣進權重，強制規則留在程式。

## 相關連結

- [[harness-engineering]] — 🔑 **張力＋互補**：該篇主張「護城河在 harness」「隨模型變強 harness 應越做越薄」；本條目提供「變薄」的一種具體機制，同時確認強制規則要留在程式層，與 Guides × Sensors 的分工不衝突
- [[reflection-memory-learning-loop]] — 🔑 **推論時 vs 訓練時**：該篇是 reflection＋memory、不改權重；本條目兩個案例都改權重
- [[unhobbling-harness-capability-overhang]] — 🔑 **方向相反**：unhobbling 是改環境，讓模型把已有的能力跑出來；Harness-Zero 是把環境誘發的行為內化進模型
- [[claude-code-harness-internals]] — harness 實際管哪些狀態（context、快取、compaction），對照「context management 不易蒸餾」
- [[deepseek-harness-plugin-architecture]] — harness 以外掛形式存在的另一種做法
- [[llm-concepts-visual-cards-61-120]] — Knowledge Distillation 的基本定義
- [[agent-error-recovery-taxonomy]] — 錯誤分類；Perplexity 只把「模型當時可避免」的錯誤當訓練目標
- [[agent-reliability-timeout-retry-fallback-verification]] — 「儲存後重讀」這類驗證習慣在可靠性機制層的對應
- [[xcode-mcp-test-report-fabrication]] — 工具層訊號與模型層行為要分開歸因
- [[claude-opus-5-5-release-notes]] — 相對 vs 絕對的讀數紀律
- [[ideas/harness-layer-research-agentbug-smith-mosib]] — 📎 2026-10-19 新增：MOSiB 以 Meta-Harness 為基準、不改權重；⚠️ 評測集不同且本篇改權重，兩邊數字不可直接比較
