---
title: Fara-7B：Microsoft 開源的螢幕視覺操作 Computer Use Agent
date: 2026-06-16
tags: [ai/agent]
source: https://tools.wingzero.tw/article/sn/3946
type: article
status: processed
asset_value: 70
---

多模態 Agent（如 [[multimodal-multi-agent-extension]]）能「看懂」一張圖片並給建議，但無法「動手操作」電腦。Fara-7B 進一步把這個能力落地：直接讀螢幕截圖、理解 UI、輸出具體座標動作（點擊、輸入、滾動），真正讓 Agent 自己操作瀏覽器完成任務。

## T（核心論點）

Fara-7B 是 Microsoft Research 開源的「Agentic Small Language Model」（7B 參數），屬於 Computer Use Agent（CUA）類別。其核心設計是用視覺語言模型（VLM）直接讀取螢幕截圖（而非解析 DOM 結構）來理解介面，並輸出座標化的具體操作指令；訓練資料來自 FaraGen——一套多 Agent 協作生成合成訓練資料的系統（出題、解題、驗證三角色循環），而非依賴大量真實使用者操作紀錄。

## B（適用場景）

**適用：**
- 需要操作沒有 API 的網頁/應用程式（瀏覽器自動化、表單填寫、跨網站資訊蒐集）
- 受控、低風險的測試或內部自動化環境
- 想用小模型（7B）在本地（vLLM / LM Studio / Ollama）部署 GUI 自動化能力，不依賴雲端大模型

**不適用：**
- 涉及敏感帳號、金融交易等高風險操作——官方建議必須有人工監督（Human-in-the-Loop）
- 已有穩定 API 可用的場景（直接呼叫 API 比視覺操作更可靠快速）

## R（核心設計）

### 視覺優先而非 DOM 解析

傳統 Web Agent 多透過解析 HTML/DOM 結構定位元素，但 DOM 結構在不同網站差異大、易受前端框架影響而失效。Fara-7B 改採人類視角：直接讀螢幕截圖，靠視覺理解 UI 佈局，輸出包含座標的具體動作：

- click（點擊指定座標）
- type（輸入文字）
- scroll（滾動頁面）
- open link（開啟連結）
- search（搜尋）
- submit（提交表單）

### FaraGen：多 Agent 合成訓練資料生成

不依賴海量真實使用者操作日誌，而是用多 Agent 協作循環自動生成訓練資料：

1. 出題 Agent：生成具體操作任務
2. 解題 Agent：嘗試完成任務並記錄操作序列
3. 驗證 Agent：檢查任務是否真正完成、操作是否合理

這個生成-驗證循環讓 Microsoft 能在缺乏大規模真實人類操作資料的情況下，仍訓練出可用的 GUI 操作模型。

### 技術規格與部署

- MIT 授權，開源可商用
- 128K context length
- 支援 vLLM、LM Studio、Ollama 等本地推理框架部署
- 已整合進 Microsoft 的 **Magentic-UI** 專案

## C（關鍵行動）

- [ ] 評估瀏覽器自動化需求時，若目標網站無 API 或 DOM 結構不穩定，考慮 Fara-7B 等視覺優先的 CUA 方案
- [ ] 涉及帳號登入、付款、敏感資料的自動化流程，務必保留人工確認節點，不可全自動授權執行
- [ ] FaraGen 的「多 Agent 生成+驗證」資料生成模式可參考應用於其他缺乏標註資料的訓練場景

## 補充資料：同類小型 CUA 模型——Apple MintAct（2026-09-29 併入）

來源：aiposthub〈Apple MintAct 是什麼？8B 視覺 Agent 統一操作手機、電腦、網頁與 500 種工具〉（Philo，2026-09-29），解析 Apple 研究團隊的 arXiv 預印本。與本條目 T 同類：**純截圖 VLM 小模型＋合成資料／合成環境訓練的 CUA**。

### MintAct vs Fara-7B

| 項目 | Fara-7B（本條目） | Apple MintAct |
|---|---|---|
| 規模 | 7B | 2B／4B／8B |
| 輸入 | 螢幕截圖（不解析 DOM） | 原始截圖，不依賴 DOM、accessibility tree 或平台 API |
| 範圍 | 瀏覽器操作 | 同一組權重統一 UI grounding＋手機／桌面／網頁多步導航＋視覺工具呼叫 |
| 合成資料 | FaraGen 多 Agent 出題／解題／驗證 | 從真實軌跡整理約 3,000 種能力，由 coding agent 生成互動環境與任務 |
| 動作空間 | click／type／scroll 等 | 座標統一正規化到 999×999；各平台動作集由 domain-specific system prompt 限定（手機 long press／swipe、桌面 right click／drag、網頁 hover／back） |
| 可取得性 | MIT 開源、可本地部署 | 【第三方作者判斷】截至 2026-09-28 **無公開權重或官方 repo**；研究，非產品 |

**四階段訓練**：高解析單步 SFT（grounding）→ 低解析多步 SFT（navigation／tool use）→ 各領域 RL 專家＋RFT 蒸餾回單一模型 → mobile＋desktop 跨域非同步 joint RL（桌面 200+ 並行環境、每個 10 CPU／40 GB；AndroidWorld 100+ emulator——「小模型」背後仍需大型訓練基礎設施）。

**數據（【Apple 研究團隊 arXiv 預印本自報，未第三方驗證】）**：MintAct-8B 相對起點 Qwen3-VL-8B——OSWorld-Verified 33.9→48.9、AndroidWorld 47.6→67.0、Weblica 55.5→74.7、Online-Mind2Web 26.5→39.1、MM-ToolSandBox 3.1→24.5。各 benchmark 量尺不同，不能平均成「總成功率」。

> 🔴 MintAct 的 48.9 是 **OSWorld-Verified**；[[claude-opus-5-5-release-notes]] 與 GPT-6（[[gpt-6-astra-prompting-guide]]）引用的是 **OSWorld 2.0**，版本不同，**不可並列比較**。

### 三個可遷移觀點

1. **像素 vs 結構化 UI**：MintAct／Fara 走純截圖路線，泛用但面對相似按鈕、彈窗、縮放與動畫更容易誤點；[[agent-device-cross-platform-testing]] 走相反路線（優先讀 accessibility tree，讀不到才退回截圖座標）
2. **合成環境可補真實資料，但不是替代**：只做合成 RL，AndroidWorld 47.6→60.3、OSWorld-Verified 33.9→38.6、無真實 iOS 資料的 iOSWorld 2.3→18.8；但有真實 in-domain RL 時仍較高（原文對照：AndroidWorld 66.7 vs 合成 59.8、OSWorld 48.3 vs 39.4）
3. **動態工具載入的快取代價**：MM-ToolSandBox 16 領域 500+ 工具，用 `search_tool` 先搜再把定義加進 context，避免 prompt 爆滿，但**改變 prompt prefix、降低 prefix caching 效果**（與 [[claude-code-context-management]] 的 context／快取取捨同一問題）

**取捨與限制**：多域共訓有代價（joint RL 讓 OSWorld-Verified 43.1→48.9，但 AndroidWorld 由 RFT 68.1 小降至 67.0）；navigation 與 tool use 仍大致分開、尚不能自主切換；【第三方作者判斷】論文無 iPhone／Mac 的 latency、RAM、量化、Core ML 實測，8B 16-bit 權重約 16 GB，不能寫成「可在 iPhone 本機執行」；不可逆動作仍需外部權限與人工確認。企業評估建議分開量測 grounding 準確率、端到端成功率、錯誤動作率、步數、p95 latency、人工接管率。

🔎 小型 CUA 模型已有兩個（Fara-7B、MintAct）。再出現第三篇時，可考慮用 knowledge-synthesizer 做精粹。

## 相關連結

- [[multimodal-multi-agent-extension]] — 多模態「讀圖理解」能力，Fara 在此基礎上加入「動手操作」
- [[supervisor-multi-agent-langchain]] — 多 Agent 協作架構的另一種應用（任務分派而非資料生成）
- [[agent-device-cross-platform-testing]] — 相反路線：優先讀 accessibility tree 的行動裝置自動化（2026-09-29 新增）
- [[claude-code-context-management]] — 動態工具載入與 prefix caching 的取捨（2026-09-29 新增）
