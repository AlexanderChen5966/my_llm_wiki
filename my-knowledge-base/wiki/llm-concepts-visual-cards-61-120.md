---
title: 圖解 120 個大語言模型（LLM）核心概念（61-120）
date: 2026-09-01
tags: [ai/agent, type/reference]
source: https://juejin.cn/post/7665944296248950810（61-90）／https://juejin.cn/post/7668568406279766054（91-120）／圖庫 https://github.com/linkxzhou/llm120images
type: reference
status: processed
asset_value: 80
---

「圖解 120 個 LLM 核心概念」系列第 61-120 號概念圖卡合輯，接續 [[llm-concepts-visual-cards-1-60]] 完成整套 120 張。圖卡固定格式：概念編號＋英文名稱＋一句話中文定義＋定義區塊＋核心流程／結構圖解＋（多數附）特點優勢或應用場景。全部 60 張已逐一下載並用視覺讀取核實內容。

⚠️ **來源素材本身不含任何文字**（兩篇掘金原文正文只有圖片連結），且 91-120 篇的圖片清單**缺 #97/98/99/101/103/104/105 共 7 張**（原文漏貼）；這 7 張改由作者開源圖庫 `linkxzhou/llm120images` 補齊，因此本條目為 60/60 完整核實。掘金 CDN 網址帶簽名有效期（`x-expires`），日後回溯應以開源圖庫為準。

## T（核心論點）

這 60 張補完了 1-60 未涵蓋的兩塊：**訓練工程的數學底層**（#61-96：優化器、激活函數家族、正則化、後訓練對齊演算法、效率技術、MoE/Mamba 架構）與**2026 年 Agent 工程的完整詞彙表**（#101-120：Harness / Loop / Context Engineering、Agent Scaffold、Eval Harness、Memory Hierarchy、Long-Horizon Agent 等）。

🔑 **後半段（#101-120）與本知識庫的關注方向幾乎完全重疊**——本庫已有 Harness Engineering、Loop Engineering、Context Engineering、Agent Memory、Multi-Agent、Coding Agent 等多篇深入條目，這 20 張的價值不在提供新知，而在**提供一組「一句話標準定義」與各概念間的層級關係**（例如 #102 Harness Engineering 是方法論、#109 Agent Scaffold 是它的軟體實體、#111 Eval Harness 是其中一層），適合對外解釋或校準用詞時引用。

前半段（#61-96）則是本庫相對缺乏的訓練側基礎，可補 [[machine-learning-intro-for-beginners]] 之後、實際看論文之前的名詞落差。

## B（適用場景）

**適用：** 查標準定義措辭（尤其 Agent 工程術語在中文圈用法混亂時）；補訓練側名詞落差（激活函數家族、PPO/GRPO/DPO 的差別）；教學或對外簡報要一組層級清楚的概念圖。
**不適用 / 注意：**
- 每張卡是高度濃縮的定義＋流程圖，**適合已有基礎者複習或校準，不適合當唯一學習教材**。
- 具體公式與數值（GELU 近似式係數、GRPO 的優勢計算式、Sigmoid 對照表）以原圖為準，未回溯原圖前不應引用衍生內容。
- #101 OpenClaw 是**單一產品介紹**，性質與其餘概念卡不同，不宜當通用概念看待。
- ⚠️ #114、#117 兩張卡的頁首標題誤植為「圖解 114／117 個大語言模型核心概念」（原作排版錯誤），內容無誤。

## R（60 個概念完整清單）

### 優化、激活函數與正則化（#61-77）

| # | 概念 | 一句話定義 |
|---|------|-----------|
| 61 | Optimizer | 依損失函數反饋計算參數更新方向與步長的演算法；讀參數→算損失→更新參數的迭代閉環 |
| 62 | Activation Function | 作用於神經元輸出的非線性函數；無它則無論疊幾層都等價於線性模型（Sigmoid/Tanh/ReLU/Leaky ReLU 對比） |
| 63 | ReLU（Rectified Linear Unit） | `max(0, x)`，負值截斷為 0；計算高效、稀疏激活，現代深度學習的預設激活函數 |
| 64 | Sigmoid Function | `1/(1+e^-x)` 映射到 (0,1)；x=0 處斜率最大 0.25，極值處梯度趨 0（易梯度消失） |
| 65 | Swish | `x × sigmoid(βx)`，用 Sigmoid 當門控在線性與 ReLU 間平滑過渡；β=0 退化為線性、β=1 即 SiLU |
| 66 | SiLU（Sigmoid Linear Unit） | `x·σ(x)`，Swish 的別名（β=1）；全域平滑可導，避免 ReLU 在 0 處不可導 |
| 67 | GELU（Gaussian Error Linear Unit） | `x × Φ(x)`（Φ 為標準正態 CDF），按輸入落在高斯分布左側的機率加權；常用 Tanh 或 `σ(1.702x)` 近似 |
| 68 | GLU（Gated Linear Unit） | 兩路線性投影：一路生候選特徵 V、一路經 Sigmoid 生門控 G，逐元素相乘 `Y = V ⊙ G` |
| 69 | SwiGLU（Swish-Gated Linear Unit） | GLU 的門控改用 Swish 激活；Llama / PaLM / Gemma 的 FFN 主流選擇，取代 ReLU/GELU |
| 70 | Model Activations | Token 過 Transformer 每層產生的中間向量；淺層抓局部特徵→中層組合模式→深層語義抽象→末層任務對齊 |
| 71 | Underfitting | 模型容量不足或訓練不充分，**訓練集與測試集誤差都高且差距不大**；解法是加容量/強化特徵/延長訓練 |
| 72 | Overfitting | 記住訓練資料細節（含噪聲），訓練集表現很高但測試集很低；解法含增資料/正則化/Dropout/早停/簡化模型 |
| 73 | Regularization | 在損失函數加入與模型複雜度相關的懲罰項 `L_total = L_task + λ·Ω(θ)`；λ 過強會欠擬合、過弱抑制不足 |
| 74 | L1 Regularization（Lasso） | 懲罰權重絕對值之和，**促使許多權重精確為 0**（產生稀疏解、可做特徵選擇） |
| 75 | L2 Regularization（Ridge） | 懲罰權重平方和，**收縮權重但不置零**；λ 越大模型越簡單、偏差可能增大 |
| 76 | Dropout | 訓練時按機率隨機把部分神經元置 0，**推理階段關閉並使用完整網路**；等效於隱式模型集成 |
| 77 | Layer Normalization | 對單一樣本的所有特徵維度標準化＋可學習縮放平移；Post-LN vs Pre-LN 兩種放置，現代 Transformer（GPT/LLaMA）多用 Pre-LN |

### 訓練病症、預訓練與後訓練對齊（#78-91）

| # | 概念 | 一句話定義 |
|---|------|-----------|
| 78 | Catastrophic Forgetting | 在新任務／新領域微調時遺忘先前已學會的能力（卡片案例：醫療微調後拒答翻譯任務）；解法含資料重放、正則約束、參數高效微調 |
| 79 | Concept Drift | 真實世界資料的統計特性隨時間變化，使歷史資料訓練的模型逐漸失配、預測性能退化；需持續監控與定期重訓 |
| 80 | SSL（Self-Supervised Learning） | 用輸入資料的一部分預測另一部分，從無標籤資料自動生成「標籤」（類 GPT 的預測下一個 Token） |
| 81 | Pretraining | 在海量無標籤文本上以自監督學習通用語言模式與世界知識；只學通用規律，不針對具體任務 |
| 82 | SFT（Supervised Fine-Tuning） | 用人工標註的輸入-輸出資料對繼續有監督學習，使模型專精特定任務／領域／風格；損失函數常用交叉熵 |
| 83 | LoRA（Low-Rank Adaptation） | **凍結原始權重**，只訓練額外低秩矩陣 `ΔW = BA`（秩 r ≪ min(d_in,d_out)），最終 `W = W₀ + ΔW`；參數少、顯存省、可合併部署 |
| 84 | LLM Alignment | 微調與引導使輸出符合人類價值觀、目標與倫理標準；以 RLHF 為例的五步流程（SFT→收集比較資料→訓 Reward Model→RL 優化→對齊後模型） |
| 85 | RLHF（RL from Human Feedback） | 人工對回覆做偏好排序→訓練 Reward Model→用 RL（如 PPO）優化策略；「人類反饋→獎勵建模→強化學習」的閉環 |
| 86 | RLAIF（RL from AI Feedback） | 改用**另一個高能力 AI 模型**生成偏好判斷取代人工標註，降低成本、易於規模化 |
| 87 | DPO（Direct Preference Optimization） | 直接用偏好對計算對比損失優化模型，**跳過獎勵模型與強化學習**；訓練更直接穩定、標註成本較低 |
| 88 | Knowledge Distillation | 小「學生」模型學習大「教師」模型的**軟知識**（機率分布、隱藏表示、注意力模式），以更少參數保留性能（案例：DeepSeek-R1 → R1-Distill-Qwen-7B） |
| 89 | PPO（Proximal Policy Optimization） | 對新策略與參考策略的機率比值做**裁剪**（限制在 `[1-ε, 1+ε]`），避免策略更新幅度過大；後訓練的經典基石 |
| 90 | GRPO（Group Relative Policy Optimization） | 對一組（大小 G）輸出做**組內歸一化相對優勢** `A_i = (r_i-μ)/(σ+ε)`，**取代價值函數與裁剪機制**；結構更簡單、方差更低 |
| 91 | RLVR（RL with Verifiable Rewards） | 獎勵信號來自**可客觀驗證的規則或外部工具**（檢索核對、程式執行/單測、數學求解、格式校驗）而非主觀人類偏好；可複現、可審計 |

### 推理效率與替代架構（#92-96）

| # | 概念 | 一句話定義 |
|---|------|-----------|
| 92 | Weight Pruning | 依重要性度量移除低貢獻權重得到稀疏模型；策略含幅值剪枝／結構化剪枝／漸進式剪枝＋再訓練恢復 |
| 93 | LLM Quantization | 權重與激活值從高精度浮點映射到低精度離散集合（32-bit → 8-bit 甚至 4-bit）；不同層可用不同精度取得更好平衡 |
| 94 | Gradient Checkpointing | 「存一點、算一點」：只選擇性保存部分激活值，其餘在反向傳播時重算；**顯存顯著降低、計算開銷略增、訓練結果不變** |
| 95 | MoE（Mixture of Experts） | 路由器為每個 Token 只激活 Top-K 個相關專家，其餘保持靜默；在相同計算預算下擴大模型容量 |
| 96 | Mamba | 以狀態空間模型（SSM）為核心的序列建模架構，**隨序列長度線性擴展**；選擇性 SSM 機制，長序列任務可媲美甚至超越 Transformer |

### 安全與治理（#97-100）

| # | 概念 | 一句話定義 |
|---|------|-----------|
| 97 | Constitutional AI | 模型依一套預先設定的原則（「憲法」）**自我批評與修訂**回答，再用修訂後結果做監督微調，無需依賴人工反饋 |
| 98 | Red Teaming（對抗性測試） | 以攻擊者視角**在授權範圍內**主動發現並修復 AI 系統漏洞；五步流程（定範圍→威脅建模→對抗探索→驗證評估→修復追蹤）。卡片明確區分「建設性 Red Teaming」與「惡意攻擊」 |
| 99 | Prompt Injection | 攻擊者在輸入中插入隱藏指令，誘使模型忽視原有規則；防護要點含嚴格輸入校驗、最小權限、**指令與資料隔離**、輸出審查、持續監控 |
| 100 | Prompt Moderation | 在模型回應**之前**對使用者輸入做安全與合規檢查，有害則攔截、安全則放行；審核維度含暴力仇恨／色情／自傷／違法／隱私／濫用 |

### Agent 工程詞彙表（#101-120）

| # | 概念 | 一句話定義 |
|---|------|-----------|
| 101 | OpenClaw | ⚠️ **產品非概念**：開源、本地優先的 AI Agent 助手框架，連接本地工具與聊天渠道（微信/飛書/Telegram/Discord），強調隱私優先與可擴展技能系統 |
| 102 | Harness Engineering | 讓 Agent 從「聰明」變「可靠」的工程方法：**不只寫 Prompt，而是設計圍繞模型的 Harness**（任務拆解、約束、反饋、測試、檢查點、文檔、工具、評估器）；Planner→Generator→Evaluator→Tests/CI→Checkpoint 流水線 |
| 103 | Agent Loop | 自主 Agent 反覆執行的基本閉環：Observe → Plan → Act/Tool Use → Evaluate → Reflect，直到目標完成 |
| 104 | Loop Engineering | 2026 年興起的範式：**人的角色從「寫 Prompt 的操作者」轉為「設計 Loop 的系統設計者」**；與 Prompt Engineering 的線性人工試錯明確對比 |
| 105 | Context Engineering | 精確設計模型「看到什麼」：系統指令／使用者意圖／記憶／RAG 文件／工具結果／輸出 Schema 六類輸入的選擇、壓縮、排序 |
| 106 | Agentic Memory | Agent 的長期記憶系統四層：Session Memory（會話內）／Working Notes（任務中要點）／Vector Memory（向量化跨會話復用）／專案規則檔（AGENTS.md 等持久約束） |
| 107 | Computer Use Agent（CUA） | 像人一樣操作圖形介面的 Agent：螢幕觀察→元素識別→執行操作→狀態變化→結果驗證的閉環，**沒有 API 也能完成任務** |
| 108 | Native Tool Use | 把外部工具／API／資料庫／程式解譯器／瀏覽器／檔案系統／MCP Server 當作**一等動作直接調用**，而不只是生成文字 |
| 109 | Agent Scaffold | 包裹在模型周圍的軟體支架，自下而上分層：Base Model → Planner → Memory → Tool Router → State Manager → Evaluator → Safety Guardrails，橫切能力含日誌／監控告警／重試／配置管理 |
| 110 | Multi-Agent Collaboration | 多個專門化 Agent 透過協調、分工、委派、互審解決單一 Agent 難完成的任務（Manager／Researcher／Coder／Tester／Reviewer／Writer 網絡）；關鍵要點含**合理的調度與終止策略，防止無限循環** |
| 111 | Eval Harness | 可複現的評估外殼：把資料集、Prompt、評分規則、測試運行器、指標、日誌與對比看板包在任務周圍；固定隨機種子確保結果一致 |
| 112 | RACG（Retrieval-Augmented Code Generation） | 生成程式補丁前先從大型程式庫檢索語法／語義相關片段，協助 Coding Agent 處理大型倉庫與長上下文限制 |
| 113 | Coding Agent | LLM 驅動的軟體工程 Agent，能理解 Issue、閱讀程式庫、修改檔案、跑測試、除錯並循環迭代，直到交付可合併的變更（Issue→Read→Patch→Test→Debug→PR） |
| 114 | Long-Horizon Agent | 面向長時程任務：多步驟、多輪工具調用、長時間執行、記憶檢查點與失敗恢復；卡片比喻為「持續工作的專案經理」 |
| 115 | Workflow Orchestration | 把自然語言目標轉為結構化流程（DAG）：有序步驟、依賴關係、條件分支、並行節點、**人工確認點**與工具執行 |
| 116 | Agent Observability | 對 Agent 行為做日誌記錄、鏈路追蹤、可視化與除錯，涵蓋 Prompt／上下文／工具調用／決策／重試／失敗／成本／延遲／結果；回答「發生了什麼、為什麼、如何改進」 |
| 117 | Memory Hierarchy | LLM 系統的分層記憶設計（快而臨時 → 慢而持久）：Immediate Context → Session → Working → Episodic → Semantic → Project Knowledge Base；未命中逐層下探、命中後緩存至上層 |
| 118 | Dynamic Context Compression | Context Engineering 技術：對長歷史／長文檔做過濾→排序→摘要，讓模型在有限 Context Window 中只看到最相關資訊 |
| 119 | World Models for Agents | Agent 內心建構的可交互環境副本，用於模擬行動後果、預測未來狀態並提前規劃，再作用於真實世界；「先思考、後行動」 |
| 120 | Native Multimodal Agents | 原生融合文字／影像／音訊／影片／螢幕 UI／工具／記憶的 Agent，在真實數位環境執行任務並產出動作、檔案、報告、UI 操作 |

## C（關鍵行動）

- [ ] 查閱特定概念時，優先核對本表的一句話定義是否符合當下語境；需要公式或流程細節時回溯開源圖庫 `linkxzhou/llm120images` 的對應 `images/llmNN.png`（勿用掘金 CDN 網址，簽名會過期）
- [x] 用 #102/#103/#104/#105/#109/#111 這組卡片校準本庫既有 Harness／Loop／Context Engineering 條目的**用詞層級**（方法論 vs 軟體實體 vs 評估層）→ **已完成，成果見 [[concepts/agent-engineering-vocabulary-layers]]**：20 張卡歸位四層抽象並逐格標覆蓋深度，🔴 並發現三張範式卡缺少 Prompt→Context→Harness→Loop 的順序、需自行補上
- [ ] 訓練側名詞（#61-96）若要進一步深入，優先補 PPO/GRPO/DPO 三者的實務取捨——本庫目前只有 [[langchain-agent-interview-guide]] 提及 RLHF vs DPO，缺 GRPO
- [ ] 引用 #101 OpenClaw 時註明它是產品而非通用概念，避免與其餘 119 張概念卡混為一談

## 相關連結

- [[concepts/agent-engineering-vocabulary-layers]] — **#101-120 的四層抽象對齊地圖**：把這 20 張卡歸位範式／機制／實體／應用四層，逐格標出本庫覆蓋深度與三個真缺口
- [[llm-concepts-visual-cards-1-60]] — 同系列 1-60 號，兩者合為完整 120 張
- [[harness-engineering]] — Harness Engineering 深入條目，對照 #102（本庫深度遠超該卡）
- [[loop-engineering-6-components]] — Loop Engineering 六塊積木，對照 #104
- [[context-engineering-fundamentals]] — Context Engineering 基礎，對照 #105/#118
- [[agent-loop-explained]] — Agent Loop 概念總覽與四層階梯，對照 #103
- [[agent-memory-lifecycle]] — Agent 記憶生命週期，對照 #106/#117
- [[memharness-memory-reconstruction]] — 🔴 靜態回放記憶反而降低表現，是 #106/#117 分層記憶樂觀敘事的重要反面證據
- [[agent-tool-runtime-seven-layers]] — Tool Call 只是工作單、其餘七層屬 Harness，補足 #108 Native Tool Use 未談的執行邊界
- [[fara-computer-use-agent]] — Computer Use Agent 實作案例，對照 #107
- [[multi-agent-papers-three-elements]] — 多 Agent 方法歸納為迴圈/提示/集約三要素，對照 #110
- [[prompt-injection-vs-jailbreak]] — Prompt Injection 與 Jailbreak 的來源差異，補足 #99
- [[rag-retrieval-augmented-generation]] — RAG 完整流程，對照 #112 RACG 的檢索前置
- [[machine-learning-intro-for-beginners]] — 損失函數／過擬合／梯度下降的入門鋪墊，銜接 #61-77
