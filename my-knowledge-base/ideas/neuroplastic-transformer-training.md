---
title: 神經可塑性 Transformer：從小模型動態成長的 LLM 訓練路線
date: 2026-06-02
tags: [ai/agent]
source: 社群貼文
type: idea
status: processed
asset_value: 36
---

## T（核心論點）

在算力有限的前提下，不走大公司「先練大模型再蒸餾」路線，而是從 0.5B/0.6B 小模型開始，讓 Transformer 在訓練過程中像神經系統一樣動態「長大」或「修剪」。核心思路：哪裡 capacity 不夠就補哪裡，哪裡長期沒貢獻就砍掉。

## B（適用場景）

**適用：**
- 算力有限的個人研究者（單卡 RTX 5080 16GB 等級）
- 想從零預訓練小型 LLM 的實驗性探索
- 對 Transformer 內部結構有興趣的研究者

**不適用 / 注意：**
- 作者自述「不是成熟工程方案」，距離穩定可用有距離
- 工程複雜度高：需處理 optimizer state、checkpoint、torch.compile 相容性

## R（五種神經可塑性機制）

| 機制 | 對應操作 | 觸發條件 |
|------|---------|---------|
| 神經新生 | 新增 MLP neuron / attention head | layer 出現 capacity pressure（loss plateau 且非 overfitting）|
| 神經修剪 | 移除低貢獻 neuron / head | 長期低激活、低梯度、ablation 後 loss 不變 |
| 神經分裂 | duplicate 高壓力/高重要性節點 | 節點負載過重 |
| 恆定調節 | 限制 active parameters 總量 | 防止模型無限膨脹 |
| 凋亡機制 | 先 mask → 觀察 → 最後物理刪除 | 漸進式移除，避免誤刪 |

### 工程設計重點

- 新增參數用 **zero-output growth** 或 **duplicate/split**，不破壞原本模型輸出
- 用 **active mask / gate** 控制節點存活，不在每步改 tensor shape
- 到 **checkpoint boundary** 才做 physical compaction
- 優先從細粒度開始：MLP neuron → attention head → head dimension → 最後才考慮 layer-level

### 相關論文脈絡

| 論文/方法 | 核心概念 |
|----------|---------|
| Net2Net | function-preserving expansion，小網路變大不破壞函數 |
| DEN (Dynamically Expandable Networks) | 根據任務動態增加必要 units |
| NeST / Grow-and-Prune | 網路拓撲自我演化 |
| RigL / Dynamic Sparse Training | 根據 gradient/magnitude 動態更新稀疏連線 |
| Attention Head Pruning | Transformer 中可移除的 attention head 研究 |

## 開放問題

1. grow trigger 怎麼定義？只看 loss plateau 夠不夠？
2. neuron/head importance 用 activation、gradient、Fisher、還是 ablation delta？
3. 節點級 growth/pruning 如何乾淨處理 optimizer state？
4. active mask + checkpoint compaction 是否比直接重建模型更實際？
5. 此方法對小模型 pretraining 是幫助還是增加不穩定因素？

## 補充觀察

作者提到用 GPT 討論想法 → 看論文 → GPT 寫 spec → Codex 實做的工作流。反映了 AI 輔助研究的低門檻趨勢：「技術的落地變得越來越簡單了」。

## 相關連結

- [[karpathy-no-more-coding]] — Karpathy 2026 宣言：Token 即新 GPU，個人也能做研究
