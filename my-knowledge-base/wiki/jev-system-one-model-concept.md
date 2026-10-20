---
title: Jev / System One Model — 概念、定位與 TypeSafe AI 背景
date: 2026-09-23
tags: [ai/agent, type/concept]
source: https://codelove.tw/@tony/post/3DRvQq（原文 https://qiita.com/nasuvitz/items/44f20ed515b9734afb26）; https://codelove.tw/@tony/post/anbj0a（原文 https://qiita.com/yushibats/items/472325ce548da370ed5d）; https://tools.wingzero.tw/articles/4073; https://www.bnext.com.tw/article/92319/typesafe-jev-decision-model-explainer; https://rar.design/posts/jev-typesafe-system-one-model-guide; https://www.aiposthub.com/jev-as-a-judge-confidence-cascade-277x-cheaper/; https://vervecode.dev/ai/jev-laya-decision-models/; https://foreverwebs.com/blog/laya-open-source-decision-model-guide; https://klab.tw/2026/09/laya-review-tutorial/
type: article
status: processed
asset_value: 67
updated: 2026-10-08
---

TypeSafe AI 於 2026-09-15 以早期存取形式發表 Jev，是該公司「System One Model」系列的第一個公開模型。定位語是
「unstructured state in, typed probabilistic decisions out」——**吃非結構化狀態，吐帶型別的機率式判斷**，不生成任何文章。
實作面（API 參數、設計模式、限制、取用管道）見 [[jev-implementation-guide]]。

## T（核心論點）

🔑 **Jev 不是「更便宜的 LLM」，是一個不同的原語：一個會回傳型別、並附上「自己有多值得信任」的 function call。**

四篇來源在這點上互相補強，但各自的措辭值得分開記：

- **考卷比喻**（qiita/yushibats）：LLM 是**記述式作答**——一個 token 一個 token 寫出 JSON，接收端還得先確認格式沒壞掉；
  Jev 是**劃卡式作答**——出題的程式先備好選項，模型只回傳每格該塗多滿、以及有多少把握。
  因為不必把答案寫出來、且能一次算完所有欄位，所以更快，也不會冒出沒預先準備的選項。
- **架構比喻**（qiita/nasuvitz）：差別在**處理流程究竟由程式決定、還是由 AI 決定**。
  LLM + AI Agent 是「AI 選下一個工具」，Jev + 非 Agent 應用是「AI 只做判定，分支寫在程式碼裡」。
- ⚠️ **同一位作者的降溫觀點（本庫應保留）**：LLM 其實也能輸出含機率的相同格式，
  就「可生成的資訊」而言兩者**並無決定性差異**；Jev 的主張在於針對分類與機率輸出做了特化訓練與設計，
  因此更快、更便宜，而不是做得到別人做不到的事。

🔴 **「Zero Hallucinations」＝零「型別」幻覺，不是零語意錯誤。**
這是本批 4 篇來源（wingzero、aiposthub、dev.to/valyu、qiita/yushibats）**全部主動澄清**的跨篇共識，
不是任何單一作者的個人解讀：Jev 保證不會回傳 schema 以外的值，但**完全可能回傳「錯的、但合法的」值**。
官方自陳那個 0% 是**設計上的宣稱、不是實測值**（原話大意：「Schema matching 是保證的，所以我們可以自信地把 0% 放進圖表」）。

🔑 **confidence 是「有多篤定」，不是「答對機率」。**
它是把機率分布濃縮成的 0–1 單值——分布集中就高、分散就低。
之所以能拿來當分流門檻，前提是官方宣稱這些機率經過 **calibration（校準）**：
白話定義是「模型回答 80% 的那批判斷收集起來，大約有 8 成真的正確」。
⚠️ 題目太短 confidence 會下降、重跑之間也有微小浮動（數位時代）。
⚠️ 反證（上述轉述研究，待原論文核對）：在把錯誤答案寫得更精緻的 RM-Bench hard pairs 上，q 0.9–0.95 仍有 1/3 答錯；無參考答案的文章評分，confidence 對錯誤的 AUROC 只有 0.518。校準是否成立要看任務，見 [[llm-judge-confidence-cascade]]。

## B（適用場景）

**適用：**
- Classification / Routing / Scoring / Verification / Guardrails / RAG relevance scoring / Model routing
- 「傳統 if/else 寫不出來、但叫 LLM 做又太慢太貴」的那條縫（wingzero 的定位敘述）
- 昂貴 context window 之前的相關性過濾、對 LLM 輸出評分或設保護欄
- 過去因單價不划算而放棄的大量標註工作

**不適用 / 注意：**
- 生成任何內容（文字、程式碼、摘要）——它根本沒被訓練來生成文字
- 算術、計數、日期先後與區間運算
- 需要寫給稽核人員看的「判斷理由」
- 一次性的複雜推理、真正開放式的答案空間
- ⚠️ 「超過 30 天轉催收」這種**固定規則**，官方自己建議寫在程式裡，不要問模型

### 與四種相鄰做法的分界

| 方法 | 答案形式 | 產生方式 | 事前訓練 | 信賴程度 |
|---|---|---|---|---|
| LLM 自由文章 | 文章 | token 依序生成 | 不需要 | 只能靠文中詞彙表達 |
| LLM 的 Structured Output | 指定格式 JSON | token 依序生成 | 不需要 | 要自己把欄位寫出來 |
| 專用分類模型 | 學過的標籤 | 一次計算、對候選評分 | **需要帶標籤資料** | 各標籤機率 |
| **Jev** | 各選項機率 | 一次計算、對候選評分 | **不需要**，問題用自然語言寫 | 機率 + confidence |

（來源：qiita/yushibats。🔑 分界點：「一次計算就對候選評分」的想法與傳統分類模型相同，
差別在**不必準備標註資料、問題可用自然語言臨時寫**。）

### 與 AI Agent 框架的分界（本批唯一以 Agent 為對照組的視角）

| 觀點 | 結合 Jev 的業務應用程式 | 以 LLM 為核心的 AI Agent |
|---|---|---|
| 交由 AI 的內容 | 分類、條件判定、評分 | 情境解讀、下一步行動/工具的選擇 |
| 交由程式定義的內容 | 對應判定結果的處理與其執行條件、順序 | 可用工具、權限、核准條件、執行限制 |
| 下一步如何決定 | 收到判定結果後，由**寫在程式裡的條件分支**決定 | 由 LLM 依指令、對話歷史、工具結果選擇 |
| 客服範例 | 判定為退款要求 → 進入預定義的請款確認與核准流程 | 視情況選擇請款確認／追加提問／轉真人 |

（來源：qiita/nasuvitz，以 Amazon Bedrock AgentCore 為傳統技術的對照例。
🔑 結論：用 Jev 時**不需要 AgentCore 這類 Agent 基礎架構**；代價是應用程式碼裡沒有代理式流程，
彈性換成了可讀的程式碼分支。與 [[agent-tool-vs-workflow-vs-agent-boundary]] 的判準是同一條線的兩種說法。）

## R（操作步驟）

判斷「這件事該不該交給 Jev」的思考順序：

1. **先問這個判斷會不會被程式精確算出來** — 會的話寫程式，不要問模型（官方元規則）
2. **再問需不需要生成文字** — 需要就用 LLM；只需要「選一個 / 打個分 / 是或否」才輪到 Jev
3. **確認它只能看到你塞進 state 的東西** — Jev 無法查外部資料，state 就是它的全世界
4. **設 confidence 門檻，而且每個動作各設各的** — 出錯代價越高、門檻越高
   （範例門檻：>0.90 自動執行、0.60–0.90 轉 LLM、<0.60 轉人工；官方指南另給 ≥0.9 自動指派、≥0.5 顯示候選給承辦人員、其餘送人工）
   （2026-09-29 補：門檻不能跨任務、跨 fallback 模型沿用，見 [[llm-judge-confidence-cascade]]）
5. **級聯而非取代** — Jev 便宜地分類與路由，程式碼做它能做的，前沿模型只處理少數困難案例

## C（關鍵行動）

- [ ] 盤點現有 workflow 裡「叫 LLM 回傳 JSON 只為了做 if/else」的呼叫點——那些正是 Jev 的目標
- [ ] 任何引用「零幻覺」的地方，都補上「零**型別**幻覺」的限定詞
- [ ] 要把 confidence 寫進架構前，先用自己的流量驗證 calibration 是否在你的領域也成立
- [ ] 中文場景先做小規模試跑（英文為主要訓練語言，CJK 準確度較低，詳見 [[jev-implementation-guide]]）

## 數字與口徑（每個數字都標出處）

⚠️ **以下皆為 TypeSafe 官方或其自建評測所公布之數字，截至 2026-09-23 尚未見第三方獨立重現**
（依 `rules/research-protocol.md` 對廠商自評的處理）。
（2026-09-29 補：aiposthub 轉述指出，CMU 團隊（Yubo Li 等）2026-09 做了 JEV-as-a-Judge 評測，屬於 judge 任務的學術第三方評測，但剪藏中沒有論文連結，**原論文待查**；該研究評的是評審準確率，並未重現本表數字。見 [[llm-judge-confidence-cascade]]。）

| 數字 | 內容 | 出處與口徑 |
|---|---|---|
| 70–500 ms | 端到端延遲 | 官方標稱值，wingzero / aiposthub / dev.to / qiita/yushibats 四篇一致 |
| 3–329 秒 | 對照組「頂尖 LLM」的延遲區間 | 數位時代引官方對照表 |
| $0.20–$10 / 百萬 input token | 官方比較表中對照組 LLM 的輸入價區間（2026-09-29 補） | rar.design（2026-09-21）引官方比較表 |
| $0.042 / 百萬 input token | 定價，**輸出免費** | 官方，四篇一致（數位時代換算約新台幣 1.3 元）|
| **193.6x 速度 / 444.6x 成本效率** | 最高改善幅度 | **TypeSafe 官網 Workflow Evaluations**，官方自陳屬偏高端幅度，對照基準為 GPT-6 Astra 與 Fable 5.1 |
| **約 40–200 倍更快** | 同一件事的較寬描述 | **TypeSafe 官方部落格**（數位時代只採這組）|
| $4,000 萬 | DCVC 領投的種子輪 | 數位時代、dev.to 一致 |

🔴 **193.6x/444.6x 與 40–200x 不是互相矛盾，是兩個不同來源層級的口徑**——
前者是官網工作流程評估的**最高值**，後者是官方部落格對整體的**區間描述**。
本庫兩者並列，引用時務必連口徑一起引，不可只寫一個數字。

### 延遲的三種口徑（2026-10-08 補；🔴 口徑不同，不是矛盾）

| 數字 | 內容 | 出處與口徑 |
|---|---|---|
| 70–500 ms | 端到端延遲 | 官方標稱值（即上表第一列）|
| 570–852 ms，中位約 629 ms | 台灣呼叫官方 API 9 次，**含跨海網路** | vervecode 作者實測（2026-09-19）|
| 611 ms | 從台北呼叫 | 恆遠數位科技作者實測（**廠商來源**，2026-09-23）|
| p50 236–276 ms | 第三方量測 | Laya README 引用的第三方數字 |

🔴 「Laya 比 Jev 快 7／7.8 倍」比的是**本機推論對跨海 API**，**不是模型速度**（恆遠、klab 自己都這樣提醒）。

### Laya 官方引用的 Jev 比較數字（2026-10-08 補）

> ⚠️ 引自 Laya 官方 README／模型卡；**Jev 數字來自第三方，Laya 團隊自陳沒有 TypeSafe API 權限、未自測 Jev**。

| 數字 | 內容 | 備註 |
|---|---|---|
| ECE 0.246 | Jev 公布的校準誤差 | Laya 對照值 0.081 是溫度重擬合後（未調整 0.466），見 [[laya-open-source-decision-model]] |
| 0.727 | typed-decisions 資料集 | Laya 對照值 0.766 是微調版 |
| AG News 0.910／DAIR Emotion 0.480／Banking77 0.870 | 三個公開資料集 | Banking77 上 Jev 測 72 類、Laya 測 77 類 |

## TypeSafe AI 的背景（本批唯一的產業脈絡來源：數位時代）

- **創辦人 Diogo Almeida**：OpenAI 前研究員、2022 年 InstructGPT 論文共同作者、參與 RLHF；2024 年離開 OpenAI，於舊金山創立 TypeSafe
  - （2026-09-29 補）TypeSafe 官網自稱他是「RLHF 共同發明人」，這是公司自己的說法，本庫保留數位時代較保守的描述（rar.design 引）
- **技術長 Erik (Spock) Gafni**（曾創辦 DNA 定序 AI 公司 Ravel）、**營運長 Sasha Sheng**（前 Meta FAIR 研究工程師）；團隊來自 Stripe / Airbnb / Docker
- **訓練資料全部自行合成**，Almeida 稱這是比「發明 RLHF」更重要的一次下注
- **訓練方法 RLCD**（Reinforcement Learning for Calibrated Decisions）：依**結果**而非人類偏好最佳化機率。
  ⚠️ 截至 2026-09-18 **尚未公開論文**（qiita/yushibats 明確註記）
  （2026-09-29 補：創辦人 7 月底在 AI Engineer 的演講〈Jev Creator: Why RLCD beats RLHF〉（YouTube `cJ0EOzey--o`，rar.design 引），屬演講不是論文）
- **命名**：System One 出自 Kahneman《Thinking, Fast and Slow》的系統 1；Jev 出自經濟學家 William Stanley Jevons（Jevons Paradox，效率提升反使資源使用擴大）

### 第三方採用回報（數位時代整理，皆為個案自述、非受控實驗）

- **Vercel** 工程師 Pranit Sharma：以 Jev 取代 GPT-5.6 Luna 做指令安全審查，快 5–18 倍且更精準
- **Bryo AI** CTO Nikhil Mudholkar：對比 Gemini，Gemini 準確度微幅領先，但**成本貴 10–20 倍**
- Armin Ronacher：提出以 Jev 做 model routing 的前置分流（與 [[opensquilla-intelligent-model-router]] 的分層路由是同一類設計）
- Sam Witteveen 實測：連續 20 個分類任務總成本 0.05 美分；同一句話加重語氣（「現在就退！」）後，緊急度由 7% 升至六、七成

## 相關連結

- [[jev-implementation-guide]] — 同主題的實作面：API、五個設計模式、參數上限、jaggedness 弱項、取用管道
- [[agent-tool-vs-workflow-vs-agent-boundary]] — 「流程由程式決定還是由 AI 決定」的通用判準
- [[opensquilla-intelligent-model-router]] — 以小模型做前置路由、同品質降成本的另一個實作路線
- [[llm-cost-optimization-strategies]] — 成本優化的上層策略脈絡
- [[ai-sycophancy-risk-and-calibration]] — 模型「信心」與可靠度的一般性討論
- [[rag-refusal-evidence-gate-threshold-calibration]] — 門檻值屬實驗值而非真理的前車之鑑，套用 confidence 門檻前先讀
- [[llm-judge-confidence-cascade]] — 🔑 本條目 confidence 前提的第一份外部檢驗（CMU judge 任務評測，原論文待查）：信心夠就採用、不確定才升級，以及高信心仍會錯的反證
- [[decision-model-ecosystem]] — 決策模型生態總綱：Jev／Laya／Decisions API 比較、`/v1/systemone` 時間線（2026-10-08 補）
- [[laya-open-source-decision-model]] — Laya：RLCD 同名方法、Jev 比較數字的前提（2026-10-08 補）
- [[agent-ux-status-transparency-maturity]] — rar.design 的設計師觀點：confidence 是新的設計材料，AI 功能的設計稿要畫出「不確定」時的畫面（對應門檻 → 畫面狀態）

## 原始摘要

四篇來源的分工：
《理解它與傳統型 LLM 與 AI 代理人的差異》（qiita/nasuvitz，2026-09-18）提供四維度對比表與降溫觀點；
《話題中的 Jev 是什麼？》（qiita/yushibats，2026-09-19）提供記述式／劃卡式比喻、四方對照表、calibration 白話定義；
《不跟你聊天、只負責做決定的新型 AI》（wingzero，2026-09-20）提供最易讀的入門敘述與 Jev/LLM/Code 三層分工；
《TypeSafe AI 新模型功能、價格、用途》（數位時代，2026-09-21）提供公司、融資、命名與第三方採用數據。

2026-09-29 補充來源：《只會答選擇題的 AI 有什麼用？》（rar.design，2026-09-21）補官方比較表輸入價區間、創辦人自我宣傳用語與 RLCD 演講；《JEV-as-a-Judge 是什麼？》（aiposthub，2026-09-29）提供 judge 任務的外部評測與 confidence 反證（原論文待查）。

2026-10-08 補充來源：《Jev 與 Laya 決策模型怎麼用？》（vervecode，2026-09-19）、《Laya 模型是什麼？》（恆遠數位科技，2026-09-23，廠商）、《Laya 評測與使用教學》（klab，2026-09-25）→ 數字口徑表的延遲三種口徑與 Laya 官方引用的比較數字。
