---
title: Karpathy AutoResearch - 630 行代碼讓 AI 自主做研究
date: 2026-06-05
tags: [ai/agent, ai/workflow]
source: https://hackmd.io/@BASHCAT/BkHvMMXcWe
type: article
status: processed
asset_value: 60
---

## T（核心論點）

AutoResearch 是 Andrej Karpathy 開源的 630 行 Python 腳本，讓 AI Agent 自動執行 ML 實驗迴圈：讀取訓練代碼、形成改進假設、修改代碼、跑 5 分鐘固定時間預算訓練、比較 val_bpb 指標、有效就留。其最精妙的設計是 **Frozen Metric（凍結指標）**——Agent 可改任何東西，唯獨不能碰評估標準，這解決了「AI 同時改考卷和答案」的 alignment 問題。兩天跑 650 個實驗，找到的改進甚至比 Karpathy 本人十多年經驗的手動優化更好，且可從 depth 12 轉移到 depth 24 模型。

## B（適用場景）

**適用：**
- ML 研究中的超參數搜索與架構微調（給定訓練腳本，自動找最優配置）
- 單 GPU 環境下的自動化實驗（硬體門檻低）
- 需要大量平行實驗但人力有限的場景

**不適用 / 注意：**
- 不能提出全新理論框架或發明新架構（只做已知方向的優化）
- 固定 5 分鐘時間預算意味著不同硬體的結果不可直接比較
- 目前僅適用於小型 GPT 模型訓練，不支援 CV、NLP 下游任務或多模態

## R（操作步驟）

1. 準備 `train.py`（完整訓練代碼）和 `program.md`（自然語言研究方向指引）
2. 啟動 AutoResearch 迴圈：Agent 讀取代碼 → 形成假設 → 修改 → git commit → 訓練 5 分鐘 → 檢查 val_bpb
3. 有效改動保留、無效丟棄，每小時約 12 個實驗，一晚約 100 個
4. 早上起來檢查哪些 commit 被保留，review 改動

## C（關鍵行動）

- [ ] 閱讀 AutoResearch 原始碼，理解 Frozen Metric 設計模式
- [ ] 考慮將「評估標準放在 Agent 能力邊界之外」的原則應用到自己的 Agent 工作流

## 原始摘要

合併兩篇來源：一篇是 BASHCAT 的深度技術解析（含 Frozen Metric 哲學分析、社群反應、SETI@home 分散式願景），另一篇是數位時代的報導（含 Shopify CEO 實測 53% 效能提升案例、GitHub 4 萬星數據）。核心訊息一致：AutoResearch 的智慧不在 Agent 有多聰明，而在 Karpathy 建了一個「Agent 無法作弊」的世界。人類角色從「實驗者」轉變為「實驗設計者」——寫好 program.md，然後去睡覺。

## 補充資料

### 官方 README 細節（GitHub repo: karpathy/autoresearch）

專案刻意只有三個核心檔案：
- **`prepare.py`**：固定常數、一次性資料準備（下載訓練資料、訓練 BPE tokenizer）、運行時工具（dataloader、evaluation）。不可修改。
- **`train.py`**：Agent 唯一可編輯的檔案，包含完整 GPT 模型、Muon + AdamW optimizer、訓練迴圈。架構/超參數/optimizer/batch size 全部可改。
- **`program.md`**：人類編輯的 Agent 指令，本質上是一個超輕量 "skill"。

**設計哲學：**
- 單檔案修改（scope 可管理、diff 可審查）
- 固定 5 分鐘時間預算（wall clock）→ 每小時約 12 個實驗 → 睡一覺約 100 個實驗
- 指標 val_bpb（validation bits per byte）→ vocab-size-independent，架構變更可公平比較
- 自包含：無分散式訓練、無複雜設定、單 GPU 單檔案單指標

**小型硬體（非 H100）調整建議：**
1. 用低熵資料集如 TinyStories（GPT-4 生成的短故事）
2. 降低 vocab_size（8192 → 4096/2048/1024/256）
3. 降低 MAX_SEQ_LEN（甚至到 256）
4. 降低 EVAL_TOKENS
5. 降低 DEPTH（默認 8 → 4）
6. WINDOW_PATTERN 改為純 "L"
7. 降低 TOTAL_BATCH_SIZE（到 2^14 ~16K）

**啟動方式：** 在 repo 中開啟 Claude/Codex，禁用所有權限，提示 "Hi have a look at program.md and let's kick off a new experiment!"

## 相關連結

- [[karpathy-no-more-coding]] - Karpathy 2026 宣言：幾乎不再自己寫 code
- [[agentic-engineering-mindset]] - Agentic Engineering 哲學觀
- [[harness-engineering]] - Harness Engineering：護城河在環境而非模型
