---
title: "Harness Engineering 完全解析：當 AI Agent 的護城河不再是模型，而是環境"
source: "https://hackmd.io/@BASHCAT/SkQEW0F2bg"
author:
published:
created: 2026-05-13
description: "深入解析 2026 年最重要的 AI 工程新學科 Harness Engineering。從 Prompt 到 Context 到 Harness 的三代範式躍遷，Guides x Sensors 技術框架，OpenAI、Anthropic、Stripe 的實戰案例，以及你今天就能動手的第一個 Harness。"
tags:
  - "clippings"
status: processed
---
## 那個凌晨三點，AI Agent 把我的 Production 炸了

故事要從一個深夜說起。

你精心設計了一個 AI coding agent，給它最好的模型，餵它最完整的上下文，然後滿懷信心地讓它跑一整夜。隔天早上醒來，發現它不只完成了任務——它還「順便」重構了你沒讓它碰的三個模組，引入了一個循環依賴，然後在凌晨三點自信滿滿地把自己的 PR merge 了進去。

CI 亮了一排紅燈。Slack 炸了。你的 tech lead 在群組裡 @all。

這不是虛構的故事。隨著 AI Agent 在 2025 年從實驗工具進化為生產系統，類似的慘劇每天都在上演。但問題到底出在哪裡？是模型不夠聰明嗎？

OpenAI 的 Codex 團隊在一次內部實驗後給出了一個讓整個產業重新思考的答案：

> **"Agents aren't hard; the Harness is hard."**  
> （Agent 不難，難的是 Harness。）

這句話出自 OpenAI 工程師 [Ryan Lopopolo 的文章](https://openai.com/index/harness-engineering/) 。他帶領團隊用 **零行人工編寫的程式碼** ，完全靠 Codex Agent 建構了一個完整的軟體產品。但讓這件事成功的關鍵，不是更強大的模型——而是他們為模型建構的那個「環境」。

他們把這門新學科叫做 **Harness Engineering** 。

2026 年的此刻，這個概念正在以驚人的速度席捲整個軟體工程界。Martin Fowler 的網站專文介紹它，Anthropic 圍繞它重新設計了 Claude Code 的架構，Stripe 靠它每週生成數千個 AI PR，Datadog 用它建構了可觀測性閉環。甚至連 Manus 都用同一個模型重寫了五次 Harness，因為他們發現——真正的技術護城河不在模型，而在環境。

這篇文章會帶你從頭到尾搞懂 Harness Engineering。不是那種泛泛而談的概念介紹，而是真的能讓你在讀完之後，回去就開始動手改善你團隊 AI 工作流的那種深度。

---

## 從 Prompt 到 Harness：AI 工程的三次範式躍遷

要理解 Harness Engineering 為什麼重要，得先回頭看這四年來 AI 工程實踐的演變。每一代範式的誕生，都是因為前一代撞上了天花板。

### 第一代：Prompt Engineering（2022-2024）

> 關注點：「 **我該怎麼說** 」

那是個人人都在研究 prompt 技巧的年代。Few-shot learning、Chain-of-thought、Role-playing，每個人的筆記本裡都存著幾百條精心調校的 prompt。我們相信，只要找到那個「完美的指令」，AI 就能給出完美的回答。

這個信念在簡單任務上成立。但當任務變複雜——需要多步驟推理、需要記住前面的對話、需要存取外部工具——單一 prompt 就力不從心了。你沒辦法在一條指令裡塞下整個專案的架構文檔、所有的 API 規格、再加上「記得跑測試」。

### 第二代：Context Engineering（2025）

> 關注點：「 **模型能看到什麼** 」

2025 年中，Andrej Karpathy 拋出了那個改變遊戲規則的類比：

*LLM 是 CPU，context window 是 RAM，而你是負責載入正確資訊的作業系統。*

這就是 Context Engineering 的核心。不再執著於怎麼「問」，而是專注於怎麼「餵」。RAG、Memory System、Tool Definition、Structured Context Injection——所有技術都圍繞著一個問題： **如何在有限的上下文窗口裡，放入對當前任務最有用的資訊。**

Context Engineering 是一個巨大的進步。但它有個致命的盲點： **它仍然只管理單一 Agent 的視角。**

當你需要多個 Agent 協作、需要在 Agent 完成工作後驗證結果、需要在 Agent 犯錯時自動回滾——Context Engineering 幫不了你。管理上下文窗口是必要的，但它只是拼圖的一塊。

### 第三代：Harness Engineering（2026）

> 關注點：「 **我該建什麼系統** 」

Harness Engineering 不是取代前兩代，而是把它們 **吸收為子模組** 。 [Bits Bytes NN 的分析文章](https://bits-bytes-nn.github.io/insights/agentic-ai/2026/04/05/evolution-of-ai-agentic-patterns-en.html) 用了一個精闢的比喻：

> 「Harness Engineering 包含 Context Engineering，Context Engineering 包含 Prompt Engineering。Prompt Engineering 沒有死——它被升職了，成為更大系統的一個子模組。」

Chad Fowler 把這個現象叫做「 **嚴謹性的遷移** 」（Relocating Rigor）：工程紀律從來沒有消失，只是不斷地搬家——從 prompt 搬到 context，從 context 搬到 harness。

讓我用一張表來說清楚三者的差異：

| 世代 | 核心問題 | 比喻 | 局限 |
| --- | --- | --- | --- |
| Prompt Engineering | 怎麼「說」 | 寫一封完美的信 | 無法處理多步驟、缺乏記憶 |
| Context Engineering | 模型「看」什麼 | 附上所有相關附件 | 只管單一 Agent 的視角 |
| **Harness Engineering** | **建什麼「系統」** | **設計整個郵務系統** | **仍在發展中（見後文）** |

---

## 拆解 Harness：Guides x Sensors 技術框架

理解了「為什麼」之後，來看「是什麼」和「怎麼做」。

2026 年 4 月，Thoughtworks 的傑出工程師 [Birgitta Böckeler 在 Martin Fowler 的網站](https://martinfowler.com/articles/harness-engineering.html) 上發表了一篇文章，提供了目前最結構化的 Harness Engineering 技術框架。我認為這是目前最值得每個工程師讀的一篇文章——它不只是概念，而是一套可操作的心智模型。

### 兩大控制機制：前饋與回饋

Harness 的核心是兩種控制機制的組合：

**Guides（引導 / 前饋控制）** 在 Agent 行動 **之前** 介入。它們告訴 Agent 「好的程式碼長什麼樣」、「這個專案的架構規則是什麼」、「做完之後要怎麼測試」。

具體來說，這些是 Guides：

- `AGENTS.md` / `CLAUDE.md` 檔案
- 架構文檔和設計規範
- Skills（例如 `/how-to-test` ）
- MCP Server 提供的知識存取

**Sensors（感測器 / 回饋控制）** 在 Agent 行動 **之後** 介入。它們觀察 Agent 做了什麼，然後產生修正訊號。

具體來說，這些是 Sensors：

- ESLint、semgrep 等 linter
- TypeScript type checker
- 測試套件
- AI Code Review

這兩者缺一不可。Böckeler 說得很直白：

> 只有前饋？Agent 編碼了規則但永遠不知道規則有沒有被遵守。  
> 只有回饋？Agent 不斷重複犯同樣的錯誤。

### 兩種執行類型：計算型與推理型

在前饋和回饋之外，還有另一個維度的分類：

**Computational（計算型）** ——確定性的、快速的、由 CPU 執行。結果是二元的：過或不過。例子包括 type checker、linter、結構分析工具。

**Inferential（推理型）** ——概率性的、較慢的、由 GPU/NPU 執行。結果是判斷性的：「這段程式碼可能有安全漏洞」。例子包括 AI code review、LLM-as-judge。

### 2x2 矩陣：完整的 Harness 地圖

把這兩個維度交叉，就得到了 Harness Engineering 的完整地圖：

|  | Computational（確定性） | Inferential（推理性） |
| --- | --- | --- |
| **Guide（前饋）** | LSP、TypeScript 型別系統、架構文檔 | [AGENTS.md](http://agents.md/) 、AI 生成規劃、Skills |
| **Sensor（回饋）** | ESLint、semgrep、coverage 檢查 | AI Code Review、Architecture Review |

一個成熟的 Harness 需要 **四個象限都有覆蓋** 。大部分團隊的問題是只有左下角（一個 linter 加幾個測試），卻缺少右上角（系統性的前饋引導）和右下角（推理型的語義審查）。

### Steering Loop：讓 Agent 自我修正的迴路

這些元素組合起來，形成了一個「轉向迴路」（Steering Loop）：

```
#mermaid-1778664048426-9tx1pa3an{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;fill:#333;}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#mermaid-1778664048426-9tx1pa3an .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#mermaid-1778664048426-9tx1pa3an .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#mermaid-1778664048426-9tx1pa3an .error-icon{fill:#552222;}#mermaid-1778664048426-9tx1pa3an .error-text{fill:#552222;stroke:#552222;}#mermaid-1778664048426-9tx1pa3an .edge-thickness-normal{stroke-width:1px;}#mermaid-1778664048426-9tx1pa3an .edge-thickness-thick{stroke-width:3.5px;}#mermaid-1778664048426-9tx1pa3an .edge-pattern-solid{stroke-dasharray:0;}#mermaid-1778664048426-9tx1pa3an .edge-thickness-invisible{stroke-width:0;fill:none;}#mermaid-1778664048426-9tx1pa3an .edge-pattern-dashed{stroke-dasharray:3;}#mermaid-1778664048426-9tx1pa3an .edge-pattern-dotted{stroke-dasharray:2;}#mermaid-1778664048426-9tx1pa3an .marker{fill:#333333;stroke:#333333;}#mermaid-1778664048426-9tx1pa3an .marker.cross{stroke:#333333;}#mermaid-1778664048426-9tx1pa3an svg{font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:16px;}#mermaid-1778664048426-9tx1pa3an p{margin:0;}#mermaid-1778664048426-9tx1pa3an .label{font-family:"trebuchet ms",verdana,arial,sans-serif;color:#333;}#mermaid-1778664048426-9tx1pa3an .cluster-label text{fill:#333;}#mermaid-1778664048426-9tx1pa3an .cluster-label span{color:#333;}#mermaid-1778664048426-9tx1pa3an .cluster-label span p{background-color:transparent;}#mermaid-1778664048426-9tx1pa3an .label text,#mermaid-1778664048426-9tx1pa3an span{fill:#333;color:#333;}#mermaid-1778664048426-9tx1pa3an .node rect,#mermaid-1778664048426-9tx1pa3an .node circle,#mermaid-1778664048426-9tx1pa3an .node ellipse,#mermaid-1778664048426-9tx1pa3an .node polygon,#mermaid-1778664048426-9tx1pa3an .node path{fill:#ECECFF;stroke:#9370DB;stroke-width:1px;}#mermaid-1778664048426-9tx1pa3an .rough-node .label text,#mermaid-1778664048426-9tx1pa3an .node .label text,#mermaid-1778664048426-9tx1pa3an .image-shape .label,#mermaid-1778664048426-9tx1pa3an .icon-shape .label{text-anchor:middle;}#mermaid-1778664048426-9tx1pa3an .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#mermaid-1778664048426-9tx1pa3an .rough-node .label,#mermaid-1778664048426-9tx1pa3an .node .label,#mermaid-1778664048426-9tx1pa3an .image-shape .label,#mermaid-1778664048426-9tx1pa3an .icon-shape .label{text-align:center;}#mermaid-1778664048426-9tx1pa3an .node.clickable{cursor:pointer;}#mermaid-1778664048426-9tx1pa3an .root .anchor path{fill:#333333!important;stroke-width:0;stroke:#333333;}#mermaid-1778664048426-9tx1pa3an .arrowheadPath{fill:#333333;}#mermaid-1778664048426-9tx1pa3an .edgePath .path{stroke:#333333;stroke-width:2.0px;}#mermaid-1778664048426-9tx1pa3an .flowchart-link{stroke:#333333;fill:none;}#mermaid-1778664048426-9tx1pa3an .edgeLabel{background-color:rgba(232,232,232, 0.8);text-align:center;}#mermaid-1778664048426-9tx1pa3an .edgeLabel p{background-color:rgba(232,232,232, 0.8);}#mermaid-1778664048426-9tx1pa3an .edgeLabel rect{opacity:0.5;background-color:rgba(232,232,232, 0.8);fill:rgba(232,232,232, 0.8);}#mermaid-1778664048426-9tx1pa3an .labelBkg{background-color:rgba(232, 232, 232, 0.5);}#mermaid-1778664048426-9tx1pa3an .cluster rect{fill:#ffffde;stroke:#aaaa33;stroke-width:1px;}#mermaid-1778664048426-9tx1pa3an .cluster text{fill:#333;}#mermaid-1778664048426-9tx1pa3an .cluster span{color:#333;}#mermaid-1778664048426-9tx1pa3an div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:"trebuchet ms",verdana,arial,sans-serif;font-size:12px;background:hsl(80, 100%, 96.2745098039%);border:1px solid #aaaa33;border-radius:2px;pointer-events:none;z-index:100;}#mermaid-1778664048426-9tx1pa3an .flowchartTitleText{text-anchor:middle;font-size:18px;fill:#333;}#mermaid-1778664048426-9tx1pa3an rect.text{fill:none;stroke-width:0;}#mermaid-1778664048426-9tx1pa3an .icon-shape,#mermaid-1778664048426-9tx1pa3an .image-shape{background-color:rgba(232,232,232, 0.8);text-align:center;}#mermaid-1778664048426-9tx1pa3an .icon-shape p,#mermaid-1778664048426-9tx1pa3an .image-shape p{background-color:rgba(232,232,232, 0.8);padding:2px;}#mermaid-1778664048426-9tx1pa3an .icon-shape rect,#mermaid-1778664048426-9tx1pa3an .image-shape rect{opacity:0.5;background-color:rgba(232,232,232, 0.8);fill:rgba(232,232,232, 0.8);}#mermaid-1778664048426-9tx1pa3an :root{--mermaid-font-family:"trebuchet ms",verdana,arial,sans-serif;}否是否是Guides 前饋控制Agent 生成程式碼Sensors 回饋控制通過?Agent 自我修正人工審查整合合併Pipeline 再次驗證通過?部署
```

OpenAI Codex 團隊有一個特別巧妙的做法： **自定義 linter 的錯誤訊息裡直接包含修復指示** 。這是一種「正向的 prompt injection」——當 Agent 的程式碼觸發了 linter 錯誤，錯誤訊息本身就告訴 Agent 該怎麼修。這讓回饋迴路的效率倍增。

### 三大 Harness 模板

Böckeler 還定義了三種可複用的 Harness 模板，你可以根據需求選擇性地建構：

**1\. Maintainability Harness（可維護性）**

- 目標：命名規範、檔案結構、日誌格式的一致性
- 工具組合：自定義 linter（計算型感測器）+ AI style review（推理型感測器）
- 適用：所有專案

**2\. Architecture Fitness Harness（架構適配性）**

- 目標：防止架構漂移、依賴違規、API 不一致
- 工具組合：dep-cruiser + structural tests + AI architecture review
- 適用：中大型專案、多 Agent 協作場景

**3\. Behaviour Harness（行為正確性）**

- 目標：確保功能符合規格書
- 工具組合：測試套件（回饋）+ 規格文件（前饋）+ 人工審查
- 適用：有明確需求規格的功能開發

---

## 模型驅動的雙重革命

讀到這裡你可能會有個疑問：這跟「模型驅動」有什麼關係？

答案是：Harness Engineering 正在開啟一種 **全新形態的模型驅動開發** 。但要理解這個「新」，得先知道「舊」。

### 舊世界：Model-Driven Engineering (MDE)

如果你有軟體工程的學術背景，你可能聽過 MDE——Model-Driven Engineering。這是一個起源於 2000 年代的軟體開發方法論，核心想法是： **用抽象模型（UML、SysML、DSL）來驅動程式碼生成** 。

你畫一張 UML 類別圖，轉換引擎幫你自動生成對應的 Java 程式碼。理論上很美好：人類專注於高層設計，機器負責低層實現。

但 MDE 在實務上一直受限於 **轉換規則的僵硬性** 。真實世界的需求太複雜、太多邊角案例，導致自動生成的程式碼經常需要大量手動修改。學術界從未停止研究（光是 arXiv 上就有一篇 [分析 98 篇論文的系統性回顧](https://arxiv.org/html/2307.04599v1) ），但產業採用率始終有限。

### 新世界：AI 模型驅動的 Harness Engineering

Harness Engineering 可以被視為 **MDE 在 AI 時代的精神繼承者** ——但換了一個完全不同的「模型」。

| 維度 | 傳統 MDE | Harness Engineering |
| --- | --- | --- |
| 「模型」是什麼 | UML/SysML 抽象圖 | LLM / Foundation Model |
| 驅動方式 | 模型 → 轉換引擎 → 程式碼 | 模型 → Agent 迴路 → 程式碼 |
| 人類角色 | 繪製模型圖 | **設計執行環境和約束** |
| 自動化程度 | 模板化生成 | 端到端自主開發 |
| 驗證方式 | 模型一致性檢查 | Guides + Sensors 全方位驗證 |
| 知識表示 | 元模型、DSL | [AGENTS.md](http://agents.md/) 、Skills、MCP Server |
| 適應性 | 靜態轉換規則 | 動態學習 + 自我修正迴路 |

兩者的核心共通點驚人地一致： **都追求抽象化、都靠約束確保品質、都把領域知識編碼為機器可處理的形式。**

差別在於，傳統 MDE 的「模型」是人類手繪的抽象圖，而 Harness Engineering 的「模型」是能理解自然語言、能自主推理的 LLM。這意味著：

- 不再需要嚴格的 DSL 語法——用 Markdown 寫的 `AGENTS.md` 就夠了
- 不再受限於預定義的轉換規則——LLM 能處理前所未見的需求
- 不再是「生成一次就完事」——Agent 能在持續的回饋迴路中自我修正

這是一個根本性的轉變： **從「模型驅動程式碼生成」進化為「AI 模型驅動軟體開發」。**

學術界已經注意到這個趨勢。MDE4AI（用 MDE 方法開發 AI 系統）和 AI4MDE（用 AI 增強 MDE）兩個研究方向正在快速發展。2026 年的 [MDEML 研討會](https://dsd-seaa.com/mdeml/) 和 [MDE4SA 國際研討會](https://mde4sa.github.io/) 都把 AI 與模型驅動的交叉列為核心議題。

但在產業實踐層面，Harness Engineering 已經跑在學術研究的前面了。

---

## 實戰解剖：五大企業如何建構 Harness

理論講完了，來看真實世界。五家公司的實踐，五種不同的切入角度，但都指向同一個結論。

### OpenAI Codex 團隊：零行人工程式碼的實驗

這是最具標誌性的案例。Ryan Lopopolo 帶領團隊從一個 **空的 git repository** 開始，完全靠 Codex Agent 構建了一個完整軟體產品——大約 100 萬行程式碼，零行是人類手寫的。

他們怎麼做到的？

**Repository Knowledge as System of Record** ：所有知識都編碼在代碼庫內部。一個結構化的 `docs/` 目錄包含架構圖、execution plans、設計規範。Agent 不需要存取任何外部知識庫——一切都在 repo 裡。

**自定義 Linter 即 Sensor** ：他們為專案量身打造了一系列 linter，不只檢查格式問題，更關鍵的是—— **linter 的錯誤訊息本身就包含修復指示** 。當 Agent 違反了命名規範，錯誤訊息會直接說「請將 fooBar 改為 foo\_bar，因為本專案使用 snake\_case」。這等於在回饋迴路裡埋入了前饋指引。

**Garbage Collection Agent** ：排程執行的 Agent 定期掃描整個 codebase，找出文檔不一致、架構違規、技術債。發現問題就自動提交修復 PR，大部分在一分鐘內自動 merge。這是持續性的、小額的品質投資，取代了傳統的週期性大重構。

**Agent 自主 PR 流程** ：Agent 撰寫 PR → 先請其他 Agent review → 回應 review 意見 → 持續迭代 → 所有 Agent reviewer 滿意後 → squash & merge。人類只在高層設計決策時介入。

### Anthropic Claude Code：三代理 Harness 架構

[Anthropic 的方法](https://www.anthropic.com/engineering/harness-design-long-running-apps) 來自一個核心洞察： **模型無法可靠地評估自己的工作** 。他們的解決方案帶有 GAN（生成對抗網路）的影子——把生成和評估拆成不同的 Agent：

| Agent | 角色 | 職責 |
| --- | --- | --- |
| **Planner** | 規劃者 | 把產品規格分解為可執行的任務列表 |
| **Generator** | 生成者 | 一次實作一個 feature，保持增量開發 |
| **Evaluator** | 評估者 | 驗證生成結果，回饋修正指令 |

另一個關鍵創新是 **Initializer Agent** 。在第一個 context window 裡，一個專門的初始化 Agent 會設定整個工作環境：建立 `init.sh` 腳本、創建 `claude-progress.txt` 進度追蹤檔案、做第一個 git commit。這樣後續的 coding agent 每次啟動時，都能從檔案系統中恢復完整的上下文。

這套架構讓 Claude Code 能處理多小時的長時間自主開發任務，而不會在中途迷失方向。

### Stripe Minions：每週數千個 AI PR

Stripe 的 [Minions 系統](https://www.mindstudio.ai/blog/what-is-ai-agent-harness-stripe-minions/) 展示了 Harness Engineering 在大規模落地時的樣貌：

- 每週生成 **數千個 AI Pull Request**
- 每個 PR 都在隔離的沙箱中執行測試
- Agent 讀取測試失敗訊息 → 診斷問題 → 修復程式碼 → 重新跑測試
- Harness 控制最大迭代次數（通常 3-5 次）
- 超過迭代上限未通過？自動升級給人類工程師

他們的經驗揭示了一個關鍵原則： **Harness 需要有明確的退出條件** 。讓 Agent 無限制地重試只會浪費 token 和時間。設定一個上限，超過就果斷升級。

### Datadog：可觀測性閉環

[Datadog 的 Engineering Blog](https://www.datadoghq.com/blog/ai/harness-first-agents/) 提出了 **Harness-first Engineering** 概念，他們的獨特貢獻在於把 **生產環境的可觀測性** 納入 Harness 的閉環：

```
Agent 生成 → Harness 驗證 → 部署 → Production Telemetry 驗證
     ↑                                              │
     └──────── 回饋更新 Harness ←──────────────────┘
```

他們用形式化方法（Formal Methods）來表達系統不變量（Invariants），然後讓 Agent 自動生成對應的 property tests。而 production 的 metrics、logs、traces 是最終的真實來源——當模型行為和生產數據出現偏差時，回饋不只修正 Agent，更修正 Harness 本身。

Datadog 的觀點很犀利：

> 「沒有可觀測性，迴路就沒有閉合。」

### Manus：五次重寫的啟示

Manus 的案例最簡單也最有說服力： **6 個月內用相同的模型重寫了 5 次 Harness** 。每次重寫帶來的效能提升都比換模型大得多。

這直接證明了 Aakash Gupta [在 Medium 上](https://aakashgupta.medium.com/2025-was-agents-2026-is-agent-harnesses-heres-why-that-changes-everything-073e9877655e) 的觀點：

> 「更好的模型讓 Harness 更重要，而不是更不重要。」

2026 年 3 月 30 日，OpenAI 甚至開源了 `codex-plugin-cc` ——一個讓你在 Claude Code 裡直接呼叫 Codex 的官方插件。一家 AI 公司把自己的 Agent 做成了競爭對手工具的插件？因為他們想通了：\*\*護城河在 Harness，不在模型。\*\*與其讓使用者不用 Codex，不如讓 Codex 在任何 Harness 裡都能跑。

---

## 動手做：你的第一個 Harness

理論和案例都講完了，該你了。這一節我會給你一個可以立刻開始的漸進式路徑。

### Level 1：建立你的第一個 Guide

在專案根目錄建立一個 `AGENTS.md` （如果你用 Claude Code 就叫 `CLAUDE.md` ）。這是最基本的前饋控制：

```markdown
# AGENTS.md

## 專案概覽
這是一個 Next.js 14 + TypeScript + Prisma 的 SaaS 應用。

## 架構規則
- 所有 API routes 放在 \`src/app/api/\` 下
- 業務邏輯放在 \`src/services/\`，不允許在 route handler 中直接寫
- 資料庫查詢必須透過 Prisma service layer
- 禁止在 client component 中直接呼叫資料庫

## 命名規範
- 檔案名：kebab-case（例：user-service.ts）
- 函式名：camelCase
- 型別名：PascalCase
- 環境變數：SCREAMING_SNAKE_CASE

## 測試要求
- 每個 service 函式必須有對應的單元測試
- 測試檔案放在同層目錄的 \`__tests__/\` 資料夾
- 使用 vitest 執行測試：\`npm run test\`

## 安全規則
- 所有 API endpoint 必須有 authentication middleware
- 使用者輸入必須用 zod 驗證
- 禁止在 client-side 暴露 API keys
```

這份文件不需要很長。重點是把你團隊裡「大家都知道但沒有寫下來」的規則明確化。因為 Agent 不是你的同事，它不會在茶水間聽到這些潛規則。

### Level 2：添加你的第一個 Computational Sensor

有了 Guide 之後，你需要至少一個 Sensor 來驗證 Agent 是否遵守了規則。最簡單的起點是 ESLint 自定義規則：

```javascript
// .eslintrc.js - 自定義規則範例
module.exports = {
  rules: {
    // 禁止在 route handler 中直接引入 prisma
    'no-restricted-imports': ['error', {
      patterns: [{
        group: ['@prisma/client'],
        // 關鍵：錯誤訊息就是修復指示
        message: '不要在 route handler 中直接引入 Prisma。' +
                 '請改用 src/services/ 中的 service layer。' +
                 '範例：import { getUserById } from "@/services/user-service"'
      }]
    }],
  },
  overrides: [
    {
      files: ['src/app/api/**/*.ts'],
      rules: {
        'no-restricted-imports': ['error', {
          patterns: [{
            group: ['@prisma/client'],
            message: '在 API route 中禁止直接使用 Prisma。請透過 service layer 操作資料庫。'
          }]
        }]
      }
    }
  ]
};
```

注意看那個 `message` 欄位——這就是 OpenAI 團隊說的「正向 prompt injection」。當 Agent 的程式碼觸發這條規則，它不只知道「哪裡錯了」，還知道「該怎麼改」。

### Level 3：加入 CI Pipeline

把 Sensor 整合進 CI，讓每個 Agent PR 都自動驗證：

```yaml
# .github/workflows/agent-harness.yml
name: Agent Harness Check
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  harness-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install dependencies
        run: npm ci
      
      # Computational Sensors
      - name: Type Check
        run: npx tsc --noEmit
      
      - name: Lint Check
        run: npx eslint . --max-warnings 0
      
      - name: Unit Tests
        run: npm run test -- --coverage
      
      - name: Dependency Check
        run: npx depcheck
      
      # Architecture Fitness
      - name: Import Structure Check
        run: npx dependency-cruiser src --config .dependency-cruiser.cjs
```

### Level 4：引入 Inferential Sensor

當 Computational Sensor 覆蓋了基本面之後，可以加入 AI 驅動的語義審查。如果你用 Claude Code，可以建立一個 code review skill：

```markdown
# .claude/skills/code-review/SKILL.md

## 任務
審查最近的程式碼變更，重點關注：
1. 業務邏輯是否符合 PR 描述的意圖
2. 是否有潛在的安全漏洞（SQL injection、XSS、未驗證的輸入）
3. 是否有效能問題（N+1 查詢、不必要的重新渲染）
4. 是否遵守 AGENTS.md 中定義的架構規則

## 輸出格式
- PASS：沒有發現問題
- WARN：發現非阻塞性問題，建議改善
- FAIL：發現必須修復的問題，附帶修復建議
```

### Level 5：建立可觀測性

最後一層是追蹤 Agent 的行為指標。你不需要 Datadog 那麼複雜的基礎設施——一個簡單的 metrics dashboard 就夠起步了：

追蹤這些指標：

- Agent PR 的首次通過率（越高代表 Guides 越有效）
- 平均修正迭代次數（越低代表 Sensors 越精準）
- Agent 生成程式碼的測試覆蓋率
- 人工審查駁回率（越低代表 Harness 越成熟）

當這些指標開始下降，就是你需要更新 Harness 的信號。

---

## 踩坑紀錄：Harness Engineering 的五個陷阱

不是建了 Harness 就萬事大吉。根據各家的實踐經驗和我的觀察，這五個坑最常讓人跌進去。

### 陷阱一：過度約束扼殺 Agent 創造力

OpenAI 的文章裡有一句很關鍵但容易被忽略的話：

> 「在人類優先的工作流中，這些規則可能顯得過於死板。對 Agent 來說，它們是乘數效應。」

但「乘數效應」有個前提——約束要在 **正確的層級** 。如果你把 `AGENTS.md` 寫成了 200 行的微操手冊，Agent 的每一步都被限死，那你得到的只是一個很昂貴的 code template。

**解法** ：約束架構決策（「API endpoint 必須走 service layer」），但不要約束實作細節（「變數名必須以 data 開頭」）。給 Agent 自由度去解決問題，但用護欄確保它在正確的路上。

### 陷阱二：Harness 本身的 bug

> "Who watches the watchmen?"

你的 linter 規則可能有漏洞。你的 AI reviewer 可能有偏見。你的測試套件可能覆蓋率不足。Harness 不是完美的——它本身也需要維護和改善。

**解法** ：Datadog 的做法值得參考——用 production telemetry 來驗證 Harness 的有效性。如果一段通過了所有 Sensor 的程式碼在生產環境出了問題，不只要修 bug，更要回頭問「為什麼 Harness 沒抓到？」然後更新 Harness。

### 陷阱三：只有回饋沒有前饋

我見過太多團隊的 Harness 是這樣的：讓 Agent 寫程式碼 → 跑測試 → 失敗 → Agent 改 → 跑測試 → 失敗 → Agent 改 → ……循環 5 次之後超時。

問題不在回饋不夠，而在前饋不足。如果 Agent 從一開始就不知道正確的架構長什麼樣，它就是在盲人摸象。

**解法** ：投資前饋。寫好 `AGENTS.md` ，提供架構文檔，建立 Skills。讓 Agent 在動手之前就知道「好的結果」長什麼樣。前饋的投資回報率遠高於回饋。

### 陷阱四：忽略推理型感測器

很多團隊覺得有 linter 和測試就夠了。但 Computational Sensor 只能抓到結構性問題——命名錯誤、型別不匹配、依賴違規。

**語義層面的問題呢？**

Agent 可能寫了一段「技術上正確但精神上錯誤」的程式碼——通過了所有測試，符合所有 lint 規則，但完全沒實現使用者真正想要的功能。這種問題只有 Inferential Sensor 才能捕捉。

**解法** ：在你的 Harness 裡加入至少一個 AI code review 步驟。它不需要完美——即使只能抓到 60% 的語義問題，也比 0% 好。

### 陷阱五：把 Harness 當一次性工作

建好 `AGENTS.md` 和幾條 lint rule，然後就再也不更新了？

OpenAI 團隊有一個核心原則：

> 「當 Agent 犯錯時，把它當作信號：找出缺少什麼——工具、護欄、文檔——然後補回去。」

Harness 是活的。它需要隨著專案演進、隨著模型更新、隨著每一次 Agent 犯錯而持續改善。最好的做法是把 Harness 維護本身也自動化——就像 OpenAI 的 Garbage Collection Agent 那樣。

---

## 2026 下半場：Harness Engineering 何去何從

最後，讓我對下半年的趨勢做幾個判斷。

### 趨勢一：Harness 成為真正的技術護城河

OpenAI 在 Claude Code 裡發布 Codex 插件這件事，已經說明了一切。當模型提供商自己都承認「護城河不在模型」的時候，遊戲規則已經改變了。

2026 下半年，我預期會看到更多企業把 Harness 視為核心資產——就像十年前對待 CI/CD pipeline 一樣。差別在於，Harness 的設計品質直接決定了 AI Agent 的產出品質。

### 趨勢二：AGENTS.md 走向標準化

目前 Claude Code 用 `CLAUDE.md` ，Cursor 用 `.cursor/rules/` ，Codex 用 `docs/` 目錄。這種碎片化不會持續太久。 `AGENTS.md` 正在成為事實上的跨 Harness 標準——一份文件，多個 Agent 都能讀懂。

[Escape.tech 的報導](https://escape.tech/blog/everything-i-learned-about-harness-engineering-and-ai-factories-in-san-francisco-april-2026/) 提到一個有趣的做法：用 symlink 讓 `CLAUDE.md` 指向 `AGENTS.md` ，這樣你只需要維護一份文件。

### 趨勢三：Harness 自己也會被 AI 優化

GitHub 上的 [AutoAgent](https://github.com/kevinrgu/autoagent) 專案已經在做這件事：給它一個任務和一個 benchmark，它會在一夜之間自動迭代 system prompt、tool 配置、agent 編排策略——保留得分提升的變更，捨棄降低的。

Harness Engineering 的 Harness Engineering——meta 到了極致，但完全合理。

### 趨勢四：QA 角色的根本重塑

[Test Collab 的分析](https://testcollab.com/blog/harness-engineering) 說得好：

> 「QA 一直走在抽象化的弧線上。手動測試讓位給自動化測試，自動化測試讓位給 AI 輔助測試。Harness Engineering 是這條弧線的下一步。」

QA 工程師的工作不再是寫測試，而是 **設計讓 Agent 能自主測試的環境** 。這意味著：設計 agent-legible 的測試環境、審查 agent 生成測試的覆蓋缺口、擁有回饋迴路的持續改善。

### 對不同角色的建議

| 如果你是… | 你今天該做的第一件事 |
| --- | --- |
| CTO / Tech Lead | 選定一個主要 Harness（Claude Code 或 Codex），建立 `AGENTS.md` ，要求所有 Agent PR 通過 CI + 自動化 review。設定 per-session 成本告警 |
| 軟體工程師 | 把你腦中「大家都知道」的規則寫進 `AGENTS.md` 。為你最常見的 Agent 錯誤建立一條自定義 lint rule |
| QA 工程師 | 評估你的測試環境對 Agent 的「可讀性」。Agent 能自己啟動你的 app 嗎？能讀懂你的 log 嗎？能截圖並推理 UI 狀態嗎？ |
| 硬韌體工程師 | 關注 MDE4AI 方向：用 DSL + 模型驅動方法定義嵌入式 ML 任務的自動化管線 |

---

## 結語：建軟體的方式沒有變，變的是你在哪一層工作

[Louis Bouchard 的總結](https://www.louisbouchard.ai/harness-engineering/) 我覺得說得最到位：

> 「Prompting 是最簡單的部分。可靠性才是真正的工作。」

Harness Engineering 告訴我們的不是「工程師要被取代了」，而是 **工程紀律的表現形式正在改變** 。

我們不再一行一行地打字寫程式碼。但我們設計 Agent 能理解的約束、我們建構驗證 Agent 產出的感測器、我們打造讓 Agent 能在其中自由但安全地工作的環境。

寫程式碼的活兒確實在被 Agent 接管。但設計那個讓 Agent 能寫出好程式碼的世界？那依然是我們的工作。而且比以前更有趣。

如果你今天只做一件事，就去你最重要的專案根目錄建一個 `AGENTS.md` 。寫下你團隊的架構規則、命名規範、測試要求。不需要完美，不需要很長。

因為當你的 Agent 下次犯錯的時候，你不會再只是嘆氣然後手動修復。你會問自己：「Harness 缺了什麼？」然後補上它。

這才是 2026 年工程師該有的條件反射。

---

## 延伸閱讀

- [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/) — OpenAI Engineering Blog，Harness Engineering 概念的原始出處
- [Harness engineering for coding agent users](https://martinfowler.com/articles/harness-engineering.html) — Birgitta Böckeler / Martin Fowler，最結構化的技術框架
- [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) — Anthropic Engineering，三代理架構的設計思路
- [Harness design for long-running application development](https://www.anthropic.com/engineering/harness-design-long-running-apps) — Anthropic，長時間開發任務的 Harness 設計
- [Closing the verification loop: Observability-driven harnesses](https://www.datadoghq.com/blog/ai/harness-first-agents/) — Datadog Engineering Blog，可觀測性閉環方法
- [Why Harness Engineering Replaced Prompting in 2026](https://www.epsilla.com/blogs/harness-engineering-evolution-prompt-context-autonomous-agents) — Epsilla Blog，三代演進的完整分析
- [What Is Harness Engineering for AI Agents?](https://milvus.io/blog/harness-engineering-ai-agents.md) — Milvus Blog，概念入門好文
- [From Prompts to Harnesses — Four Years of AI Agentic Patterns](https://bits-bytes-nn.github.io/insights/agentic-ai/2026/04/05/evolution-of-ai-agentic-patterns-en.html) — 四年演進的深度編年史
- [awesome-harness-engineering](https://github.com/ai-boost/awesome-harness-engineering) — GitHub，最完整的 Harness Engineering 資源彙集
- [Bridging MDE and AI: Systematic Review](https://arxiv.org/html/2307.04599v1) — arXiv，MDE 與 AI 交叉的學術系統性回顧