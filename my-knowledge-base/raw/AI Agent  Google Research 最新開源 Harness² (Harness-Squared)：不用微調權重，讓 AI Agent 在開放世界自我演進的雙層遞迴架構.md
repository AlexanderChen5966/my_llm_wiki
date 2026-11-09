---
title: "[ AI Agent ] Google Research 最新開源 Harness² (Harness-Squared)：不用微調權重，讓 AI Agent 在開放世界自我演進的雙層遞迴架構"
source: "https://medium.com/@simon3458/ai-agent-harness-squared-intro-bdb182241a04"
author:
  - "[[Simon Liu]]"
published: 2026-11-04
created: 2026-11-05
description: "最近 Google Research 與美國西北大學（Northwestern University）聯合開源了 Harness²（Harness-Squared），在社群引起不少討論。"
tags:
  - "clippings"
status: processed
---
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Gg4Ex4bDoApsezhjFgAvtg.png)

\[ AI Agent \] Google Research 最新開源 Harness² (Harness-Squared)：不用微調權重，讓 AI Agent 在開放世界自我演進的雙層遞迴架構

最近 **Google Research** 與美國西北大學（Northwestern University）聯合開源了 **Harness²** （Harness-Squared），在社群引起不少討論。

這項研究的核心主張是：在完全不微調模型權重的前提下，透過「雙層遞迴」的自我改進，讓 AI Agent 在沒有標準答案的開放世界中持續演進。

GitHub 開源專案：

## [GitHub - google-research/harness2](https://github.com/google-research/harness2?source=post_page-----bdb182241a04-----------------------------------------)

### Contribute to google-research/harness2 development by creating an account on GitHub.

github.com

## I. 前言：為什麼要讓「裝備」自我演進

這幾年我在第一線協助企業打造 AI Agent，開發團隊最常問我的一個問題是：

> *「我們的 Agent 常常在最後一步漏看檔案、或是格式沒對齊，是不是該編一筆預算去微調模型？」*

老實說，每次聽到團隊想為了流程偏差，去微調模型權重，我都會先請大家緩一緩，微調的運算成本高、資料標註週期長，更麻煩的是可能引起「災難性遺忘」，把原本不錯的通用推理能力一起弄壞。

然而，團隊在觀察 Agent 時常常忽略一件事：Agent 在實戰中的表現，其實由兩個部分共同決定，一個是底層的核心模型 M（Model），另一個是包在模型外面的執行裝備 H（Execution Harness）。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*42isve-JV3_AJIpmSkHZgQ.png)

## 1\. 開放世界的兩個難題

過去要做 Agent 的遞迴自我改進，只要離開封閉的合成題庫、進入真實的開放世界（Open World），幾乎都會卡在兩個地方：

- **沒有標準答案驗證器（No Ground-Truth Verifiers）** ：使用者交付的任務在設計階段都是未知的，線上執行時，不會有一個話外之音，告訴 Agent「你剛才改的那條規則是加分還是減分」
- **過度貼合單一任務，無法泛化（Fails to Generalize）** ：很多自適應提示詞機制，只是在眼前這一題的 Prompt 上打補丁，修好任務 A、卻在任務 B 翻車，改進經驗沒辦法跨任務累積

## 2\. Harness² 的解法

為了解決這兩個問題，Google 研究團隊發表了 Harness²（論文： *Harness²: Recursive Agent Harnessing for an Open World* ）。

他們的核心格言是：

> *“Harness the agent. Then harness its self-improvement.”  
> 先為 Agent 打造裝備，再為「它的自我改進過程」打造裝備。*

整體思路是這樣的：模型權重完全凍結，不需要任何 GPU 重新訓練。

系統一方面在任務執行期間即時演進執行裝備 H，另一方面把跨任務的改進經驗沉澱到經驗庫 R（Improvement Harness）。

這意味著，「沒有驗證器」與「無法泛化」這兩個問題，分別由兩個層級去處理。

## II. 什麼是 Editable Harness：八大可編輯元件

過去大家調校 Agent 時，常常只盯著 System Prompt，遇到問題就把規則一條一條塞進去，最後 Context 爆炸、模型注意力被稀釋。

Harness² 提出的概念是 **Editable Harness** ：把包住模型的整個工程環境，拆成 8 個職責分明、可以各自修改的元件（C1–C8）。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*GCNK0TnJvNgihZVcc_hDOQ.png)

## 1\. 情境建構（Context Construction）

- **C1 System prompt** （ `systemprompt.md` ）：定義角色設定與頂層執行流程，讓 Agent 知道整體目標與基本規範
- **C2 Guardrails** （ `guardrails.md` ）：硬性的品質紅線，例如「未經核對不得交付」、「不能覆寫原始資料」
- **C3 Memory** （ `LongTermMEMORY.md` ）：跨步驟的長期記憶，保存約定、環境路徑與慣例
- **C6 Skills** （ `skills/<name>/SKILL.md` ）：按需載入的領域技能包，平時不佔用 Context，需要時才讀進來

## 2\. 工具介面（Tool Interface）

- **C4 Tool guidance** （ `tool_descriptions/tool_guidance.md` ）：LLM 看得到函式簽名，卻不知道呼叫順序與參數陷阱，這個檔案專門寫工具的使用策略與相依關係
- **C7 Scripts** （ `scripts/<name>.py` ）：確定性的 Python 輔助與驗證腳本

C7 是我自己覺得最實用的一環。

LLM 本來就不擅長大量數字計算、精確的正規表達式比對，或是複雜檔案格式的校驗，把這些工作交給腳本，Agent 只負責呼叫與讀結果，可靠度會明顯提升。

## 3\. 編排與控制（Orchestration & Control）

- **C5 Subagents** （ `sub_agents/<name>/AGENT.md` ）：把子任務委派給擁有獨立 Context 的子代理，避免主對話串被雜訊污染
- **C8 Plugins** （ `plugin/hooks.json` ）：生命週期攔截器，可以在 `PreToolUse` 、 `PostToolUse` 與 `Stop` 掛 hook，例如在 Agent 交出結論前強制再檢查一次，不合格就打回重做

## III. 核心機制：雙層遞迴架構（Two-Level Recursion）

了解 8 個元件之後，真正的挑戰在於：沒有標準答案的情況下，系統怎麼知道該改哪個元件，改完又怎麼判斷有沒有變好。

Harness² 的答案是把改進拆成兩層。

## 1\. Level 1：任務層遞迴（Propose → Probe → Compose）

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*dHhDzCqdV0kuLccaskXCyQ.png)

一個任務進來時，系統會走三個步驟：

**Step 1：Reference Execution。** 執行器（Executor）從乾淨的基礎裝備 H0 出發，完整跑一次任務，記錄所有推理鏈與工具軌跡，當作基準線。

**Step 2：正交拆解，平行探測（Propose & Probe）。** 改進器（Improver）不依賴外部答案，而是從第一輪的軌跡診斷問題，再把改進方向拆成兩個獨立的維度：

- **Invariant（INV，任務無關的通用流程）** ：自檢流程、工具呼叫防護這類通用改進，例如加入前置格式檢查腳本，產出候選裝備 Hi 並實際跑一次
- **Domain-Specific（DOM，領域專屬知識）** ：針對業務規則、資料型別與品質標準的改進，例如注入特定行業的會計處理規範，產出候選裝備 Hd 並實際跑一次

**Step 3：對比合成（Compose）。** Composer 同時看 H0、Hi、Hd 三組真實執行軌跡與產出差異（Behavioral contrasts）。

因為它看的是「改動前」與「改動後」的具體行為，所以能判斷哪些修改真的有效，再把這些有執行證據支持的修改合成為最終裝備 H′。

我會認為這是整個設計最關鍵的地方：在沒有標準答案的情況下，「對照實驗」本身就成了驗證器。

## 2\. 用預算 k 調整遞迴策略

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*-amcyGlkR9OfGdqCeDW-jA.png)

實際部署時，可以依照運算預算 k 選擇兩種模式：

- **Parallel Recursion** （ `--mode parallel` ）：探得更廣（Explores wider）。一次產生 k 個 INV 與 k 個 DOM 候選，探測後合成 k 個候選裝備，再盲測遴選
- **Sequential Recursion** （ `--mode sequential` ）：磨得更深（Refines deeper）。把上一輪合成的裝備與累積證據，帶進下一輪 Propose–Probe–Compose 繼續精煉

## 3\. Level 2：跨任務經驗遞迴（Reflect → Update）

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*mFcq7RCEFVBhCDcp65ge6w.png)

單一任務完成後，真正的累積才開始。

**Experience Reflector** 會回顧整段改進歷程，把這次採用的 Harness 檔案樹，以及提煉出的情境線索（Cue）、可遷移教訓（Lesson）與執行證據（Evidence），寫進 `EXPERIENCE.md` ，更新到經驗庫 R。

## Get Simon Liu’s stories in your inbox

Join Medium for free to get updates from this writer.

下一個任務進來時，有一個細節很重要：

- Executor 會 **重置回最乾淨的 H0** ，避免被上一題的專屬設定污染
- Improver 則 **帶著經驗庫 R** ，可以檢索過去成功的調校案例，用更少的嘗試改出合適的 Harness

整體而言，Level 2 做的事情就是「執行器歸零、改進器記得」。

## IV. 真實案例與 Benchmark：模型不動，改裝備能進步多少

## 1\. JobBench 案例：訂閱抵免額核對

在真實企業流程評測 **JobBench** 裡，有一道很貼近後台營運的任務：Agent 要調取發票紀錄（Invoices）、系統事故（Incidents）與已入帳的抵免額（Posted credits），算出正確的抵扣數字，最後匯出一份核對過的活頁簿（Workbook）。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*qna1ErUYj8Fb6jwsioe0VA.png)

原始 Base Agent 交出一份排版工整、說明很有說服力的答案。

但回頭比對原始資料才發現，模型有些數字根本沒有逐一回查來源 CSV，存檔之後也沒有重新開啟驗證，直接造成業務上的偏差。

經過 Level 1 的 Propose–Probe–Compose 之後，Harness² 為這個 Agent 補上了三個元件：

- 在 `guardrails.md` 加入雙向對帳的驗證護欄（Verification guardrails）
- 在 `skills/` 生成對帳專屬技能（Reconciliation skill）
- 在 `tool_guidance.md` 規定「寫入試算表後必須重新讀取、比對資料列數」

結果 Agent 抓出了過去忽略的入帳細節，交出正確對齊的試算表。

## 2\. 三大基準測試的量化成果

研究團隊在 **WorkBuddy-Bench** 、 **JobBench** 與 **LAB** （Legal Agent Benchmark）三個基準上做了評測，橫跨 **OpenCode** 與 **Codex** 兩種執行器，以及 **Gemini 3.5 Flash** 與 **GLM-5** 兩款模型。

在模型權重完全凍結、只用單步改進（k = 1）的情況下：

- WorkBuddy-Bench 最高提升 **+11.5 分**
- JobBench 提升 **+4.5 分**
- LAB 法律專業基準提升 **+11.8 個百分點**

## 3\. WorkBuddy-Bench 詳細分數（Gemini 3.5 Flash + OpenCode）

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ipqiLLa4LQUHVpC8mH8JFA.png)

從這組數字可以看到兩件事。

第一，現有的自我改進方法，不論是累積推理經驗的 ReasoningBank，還是 Harness Scaling、Meta-Harness，平均分落在 73.4～76.4 之間；Harness² 在 k = 1 時平均分就來到 77.5，比 Base 高 5.1 分。

第二，預算放大到 k = 5 之後，平均分來到 80.8（+8.4）。其中最容易出現格式漂移的 Office 領域，從 74.0 提升到 83.4（+9.4），是三個領域中進步最多的。

這意味著 INV + DOM 的正交拆解，搭配更多樣的探測，對開放世界裡的長尾問題確實有幫助。

## V. 動手實作：從本地 Demo 到基準評測

官方釋出了一套 CLI 工具，也提供不需要 API Key 的確定性本地 Demo。

我自己測試時是用 **uv** 建立乾淨的 Python 環境，速度快也不會弄亂系統環境。

## Step 1：建立環境並執行本地 Demo

官方的 Demo 不需要任何付費金鑰，就能在本地觀察裝備的生成與對比過程：

```cs
# 使用 uv 建立 Python 3.12 虛擬環境並安裝 harness2
uv venv --python 3.12 .venv
source .venv/bin/activate
uv pip install -e '.[render]'
```
```cs
# 執行本地免金鑰 Demo（輸出至 runs/demo，k=2）
harness2-demo --output runs/demo --k 2
```

執行完成後，可以在 `runs/demo` 資料夾裡看到 H0、Hi、Hd 到最終 H′ 的元件演變過程，對理解整個架構很有幫助。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*aTrcZ6jl5mBB0gzr_jKZpw.png)

打開 `runs/demo` 可以看到完整的輸出結構，對照前面的架構圖會很清楚：

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*K8KdaHbvXUcg4tGoFj3tVg.png)

以這次 k = 2 的 Demo 為例，跑完 2 個任務總共執行了 28 次、呼叫改進器 32 次。

我自己覺得最值得看的是 `library/` 這個資料夾：第一個任務開始前經驗庫是空的，到了第二個任務，Improver 已經可以參考第一個任務沉澱下來的經驗，這就是前面提到的 Level 2 跨任務遞迴。

## Step 2：執行 LAB 基準測試

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*eFCQAwdp1OSSOut3H275_g.png)

正式跑基準測試（LAB、JobBench 或 WorkBuddy-Bench）時，官方提供了標準化的評測流程。以 LAB 為例：

```cs
# 下載與初始化 LAB 基準測試集
harness2-setup lab

# 檢查測試集與環境相依性
harness2-check --bench lab

# 執行遞迴改進（支援 --mode parallel 或 --mode sequential）
harness2 --bench lab --k 1 --mode parallel \
  --only-domains antitrust-competition --max-tasks 1

# 自動化裁判評分
harness2-judge --bench lab --k 1 --mode parallel

# 產生量化評測報表
harness2-report --bench lab --k 1 --mode parallel
```

從資料集設置、檢查、執行、評分到報表，整套 CLI 的切分很清楚，基本上就是一條 DevOps in AI Agent 的標準流水線。

## VI. 結論

回顧 Harness² 這項研究，我最大的收穫其實不是分數，而是它點出了一件事情：我們可能太執著於模型權重，而忽略了模型身上穿的「裝備」。

在傳統軟體工程裡，程式邏輯出錯時，我們不會去改編譯器或 CPU ，而是去重構設定檔、補單元測試、設計 Middleware。

Harness² 把同樣的工程思維帶進了 AI Agent：

- **模組化分工** ：把雜亂的 Prompt 拆成 8 個元件，讓自我改進有具體可以修改的對象
- **正交探測與對比合成** ：沒有標準答案時，用 INV 與 DOM 的行為差異找出真正有效的修改
- **經驗沉澱** ：把每一次踩坑轉成可遷移的 `EXPERIENCE.md` ，讓改進可以跨任務累積

對正在評估企業 AI Agent 架構的團隊來說，這是一個成本相對低、又不會傷到模型通用能力的方向，目前看起來很值得投入實驗。

推薦大家 Clone 專案、跑跑看官方 Demo，親自感受一下「為自我改進打造裝備」是什麼樣的設計。

## 參考資源

- 官方互動展示網站： [https://harness2.github.io](https://harness2.github.io/)
- GitHub 開源專案： [https://github.com/google-research/harness2](https://github.com/google-research/harness2)
- arXiv 論文： [https://arxiv.org/abs/2610.14224](https://arxiv.org/abs/2610.14224)
- arXiv PDF： [https://arxiv.org/pdf/2610.14224](https://arxiv.org/pdf/2610.14224)

## I am Simon

大家好，我是 Simon 劉育維，是一位 AI 領域解決方案專家，目前也擔任 Google Cloud AI 領域開發者專家 (GDE)，期待能夠幫助企業導入人工智慧相關技術解決問題。如果這篇文章對您有幫助，請在 Medium 上按一下鼓勵，並追蹤我的個人帳號，這樣您就可以隨時閱讀我所撰寫的文章。歡迎在我的 Linkedin 上留言提供意見，並與我一起討論有關人工智慧的主題，期待能夠對大家有所幫助！

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*dyJnGHLh49k6wy2S.png)

我是 Simon，敬請按讚、分享、訂閱、開啟通知，才能夠第一手知道相關資訊