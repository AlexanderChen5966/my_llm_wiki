---
title: 圖解 120 個大語言模型（LLM）核心概念（1-60）
date: 2026-07-16
tags: [ai/agent, type/reference]
source: https://codelove.tw/@tony/post/x8Oyoq（1-30）／https://codelove.tw/@tony/post/qJRkZq（31-60）
type: reference
status: processed
asset_value: 78
---

「圖解 120 個 LLM 核心概念」系列第 1-60 號概念圖卡合輯，每張圖卡固定格式：概念編號＋英文名稱＋一句話中文定義＋核心概念圖解／流程＋（部分附）關鍵價值或應用場景。全部 60 張已逐一下載並核實內容（imgur 遠端圖 56 張 + 原掘金簽名網址易失效已改存本地檔的 4 張：#44/#49/#50/#55）。本條目取代先前分別建立、內容未完整核實的 `llm-concepts-visual-cards-31-60`（wiki）與 `llm-concepts-visual-cards-1-30`（ideas）兩份條目。

## T（核心論點）

這是一份結構完整的 LLM 技術詞彙速查表，依知識層次遞進排列：#1-20 打基礎架構（模型家族、Transformer 結構、Attention 變體），#21-35 講推理與評估機制（解碼策略、採樣、評測指標），#36-50 講可靠性與 Agent 技巧（幻覺、Prompt 技巧、RAG、Agent），#51-60 講協議與訓練數學（MCP/A2A、多模態模型、Loss/梯度下降）。可作為技術面試複習、寫技術文件時的標準定義來源、或教學快速參照。

## B（適用場景）

**適用：** 快速複習/教學展示 LLM 技術概念；寫技術文件需要標準定義措辭時查閱；面試前抓重點名詞。
**不適用 / 注意：** 每張卡片是高度濃縮的定義+流程圖，適合已有基礎者複習，不適合零基礎者當作唯一學習教材；具體公式/數值以卡片原文為準，不應在未核對原圖前逕自引用衍生內容。

## R（60 個概念完整清單）

### 基礎架構（#1-20）

| # | 概念 | 一句話定義 |
|---|------|-----------|
| 1 | LLM | 海量文本訓練出的語言理解與生成系統：Token化→Embedding→Transformer→生成 |
| 2 | Transformer | Encoder-Decoder 架構，靠 Self-Attention 捕捉序列依賴，不受距離影響 |
| 3 | GPT | 僅 Decoder 架構，自回歸預測下一詞，預訓練+微調兩階段 |
| 4 | BERT | 僅 Encoder 架構，Masked Language Modeling 雙向理解上下文 |
| 5 | LLaMA | Meta 開放權重模型家族，涵蓋多種參數規模（8B/70B/400B） |
| 6 | LRM（Large Reasoning Model） | 具備記憶/規劃/結構化思考的進階推理模型，如 o1、DeepSeek-R1 |
| 7 | Multimodal LLMs | 跨文本/圖像/音訊/影片的統一理解與生成 |
| 8 | Context Window | 單次可處理的最大 Token 數，Gemini 2.5/Grok 3 達 1M tokens |
| 9 | Common Crawl | 開放網頁爬取資料倉庫，LLM 訓練資料的主要來源之一 |
| 10 | Embeddings | 將 Token/詞/句映射為稠密向量，語義相近則向量距離相近 |
| 11 | BPE（Byte-Pair Encoding） | 分詞技術，反覆合併高頻字符對構建子詞詞表 |
| 12 | Positional Encoding | 向 Transformer 注入 Token 順序信息（正弦/餘弦或可學習編碼） |
| 13 | RoPE | 對 Q/K 向量做旋轉變換編碼相對位置，長度外推能力佳 |
| 14 | Self-Attention | Q/K/V 線性映射→相似度矩陣→softmax→加權聚合 V |
| 15 | MHA（Multi-Head Attention） | 並行多個 Attention Head 分別關注不同子空間再拼接 |
| 16 | Causal Masking | 屏蔽未來位置，確保自回歸生成的因果一致性 |
| 17 | Flash Attention | 分塊（Tiling）+ GPU 內核優化，精確計算但省顯存、更快 |
| 18 | GQA（Group Query Attention） | Query Head 分組共享 K/V，兼顧效能與顯存（Llama 使用） |
| 19 | MQA（Multi-Query Attention） | 所有 Query Head 共享同一份 K/V，KV Cache 壓至 1/h（PaLM 使用） |
| 20 | MLA（Multi-head Latent Attention） | K/V 壓縮進潛在空間再按需重建，大幅省顯存（DeepSeek 使用） |

### 推理與評估機制（#21-35）

| # | 概念 | 一句話定義 |
|---|------|-----------|
| 21 | LLM Inference | Prompt→Token化→前向計算→選字→循環追加的完整生成過程 |
| 22 | Autoregression | 每個輸出都基於此前已生成的輸出，逐步生成 |
| 23 | MLM（Masked Language Modeling） | 隨機遮蓋 Token，模型依上下文預測被遮蓋內容（BERT 訓練法） |
| 24 | KV Caching | 緩存已計算的 K/V，避免重複計算，加速自回歸生成 |
| 25 | Softmax | 把 Logits 轉換成機率分布，各項和為 1 |
| 26 | Greedy Decoding | 每步直接選機率最高的 Token，確定但多樣性低 |
| 27 | Beam Search | 同時擴展多條候選序列，保留累計得分最高的 K 條 |
| 28 | Top-K Sampling | 只從機率最高的 K 個候選中隨機採樣 |
| 29 | Top-P Sampling（Nucleus） | 從累積機率達閾值 P 的最小候選集合中採樣 |
| 30 | Temperature | 縮放機率分布的超參數，越低越保守、越高越有創意 |
| 31 | Test-Time Scaling | 推理階段投入更多計算（多次嘗試+整合）換取更高準確率，不改模型參數 |
| 32 | Perplexity | 預測詞平均負對數似然的指數值，越低代表模型預測越好 |
| 33 | BLEU | 譯文與參考譯文的 N-gram 重合度評分 |
| 34 | Pass@K Accuracy | 生成 K 個候選中至少一個正確的機率 |
| 35 | Win Rate | 成對比較中被評測者偏好選中的百分比 |

### 可靠性、Prompt 技巧與 Agent（#36-50）

| # | 概念 | 一句話定義 |
|---|------|-----------|
| 36 | LLM Hallucination | 模型生成看似合理實則錯誤/無法驗證的內容；解法含 RAG、要求附來源 |
| 37 | NIAH Testing | 把已知事實埋入長文本測試模型能否準確檢索（長上下文能力測試） |
| 38 | Prompt Engineering | 明確目標＋提供上下文＋結構化指令＋給範例＋設定約束 |
| 39 | Zero-Shot Prompting | 不提供任何範例，直接依指令完成任務 |
| 40 | Few-Shot Prompting | 提供少量輸入-輸出範例引導模型學習模式 |
| 41 | CoT Prompting（Chain-of-Thought） | 引導模型生成中間推理步驟再得出結論 |
| 42 | Zero-Shot CoT | 只需加「讓我們一步一步思考」，無需範例即可引導逐步推理 |
| 43 | CoD Prompting（Chain-of-Draft） | 保持推理鏈但每步只留壓縮要點，降低輸出冗餘 |
| 44 | Tree of Thoughts | 多條推理路徑組織成樹狀結構，生成-評估-剪枝-收斂找最優解 |
| 45 | ReAct（Reasoning and Acting） | 推理與行動（呼叫工具）交替進行，觀察結果後繼續推理 |
| 46 | Self-Consistency | 多次獨立採樣生成多條思維鏈，取多數一致的答案 |
| 47 | Self-Verification | 用候選答案反向重建被遮蓋的問題部分，依重建準確度評分排序 |
| 48 | Vector Database | 專門存儲/索引/檢索高維向量 Embedding 的資料庫 |
| 49 | RAG（Retrieval Augmented Generation） | 生成前先檢索外部文件增強上下文，提升準確性與可追溯性 |
| 50 | AI Agents | LLM 驅動的自主系統：推理＋規劃＋工具調用＋記憶＋反饋閉環 |

### 協議、多模態與訓練數學（#51-60）

| # | 概念 | 一句話定義 |
|---|------|-----------|
| 51 | MCP（Model Context Protocol） | Anthropic 提出的開放標準，AI 助手透過統一接口安全連接外部工具/資料 |
| 52 | A2A（Agent-to-Agent Protocol） | Google 提出的跨框架/跨廠商 AI Agent 通信協作標準 |
| 53 | LangChain | 建構 LLM 應用的框架：Chains/Agents/Memory/Tools 四大模塊 + LangGraph 圖控制流 |
| 54 | CLIP | 對比學習訓練的圖文匹配模型，圖像與文本嵌入共享語義空間 |
| 55 | ViT（Vision Transformer） | 圖片切分為 Patch 當 Token 處理，無需卷積層的圖像分類架構 |
| 56 | DiT（Diffusion Transformer） | Diffusion 過程 + Transformer 骨幹，逐步去噪生成圖像/文本 |
| 57 | ARC-AGI | 視覺推理通用智能基準，人類平均 90% vs 頂尖 LLM 僅個位數到 20% 多 |
| 58 | Loss Function | 衡量預測與真實值誤差的函數，訓練目標是最小化它 |
| 59 | Cross Entropy | 衡量預測機率分布與真實分布差異的損失函數，MLE 的等價形式 |
| 60 | Gradient Descent | 沿損失函數梯度反方向迭代更新參數，逐步逼近最優解 |

## C（關鍵行動）

- [ ] 查閱特定概念時，優先核對本表格的一句話定義是否符合當下語境，必要時回溯原圖卡（見下方連結）確認完整流程細節
- [ ] 面試/教學複習可依四大分區（基礎架構／推理評估／可靠性與 Agent／協議與多模態）分段複習
- [ ] 若需引用某概念的具體公式或數值（如 GQA 的分組比例、Perplexity 公式），務必核對原始圖卡而非僅憑本表摘要

## 相關連結

- [[llm-concepts-visual-cards-61-120]] — 同系列 61-120 號（訓練數學底層 ＋ 2026 Agent 工程詞彙表），兩者合為完整 120 張
- [[rag-retrieval-augmented-generation]] — RAG 完整流程與 Demo，可與 #49 對照
- [[chain-of-thought-reasoning]] — CoT/ToT/ReAct 三方比較，可與 #41/#44/#45 互相補充
- [[ai-agent-architecture-10-modules]] — AI Agent 10 核心模組完整拆解，對照 #50
- [[llm-how-it-works-no-math]] — 白話 LLM 原理（Tokenization/Embedding/Attention），對照 #10-14
- [[a2a-agent-protocol-implementation]] — A2A 協定實作細節，對照 #52
