---
title: 白話 LLM 原理：不需要數學基礎也能理解
date: 2026-06-15
tags: [ai/agent, type/concept]
source: https://www.0xkato.xyz/how-llms-actually-work/ (轉引自 https://codelove.tw/@tony/post/apXr2a)
type: article
status: processed
asset_value: 68
---

LLM 的一次生成，本質是一個迴圈：**「基於目前所有上下文，下一個最合理的 token 是什麼？」**—— 每步都在問這個問題，反覆採樣直到結束。

## T（核心論點）

LLM 不讀「文字」，它只操作整數（token ID）和高維向量（embedding）。理解這個根本事實，就能解釋幾乎所有 LLM 的奇怪行為：數不清字母、不同 tokenizer 導致中文/英文計費差異、同一問題不同用詞得到不同答案。

## B（適用場景）

**適用：**
- 想理解 LLM 行為背後的機制（為什麼某些 prompt 有效，某些無效）
- 解釋 token 計費與 context window 的根本原因
- 向非技術人員說明 AI 原理的背景知識

**不適用 / 注意：**
- 這是概念理解文章，不含訓練/微調/RLHF 等進階主題
- Embedding 空間的數學性質（語義相似性計算）只作比喻，不深入

## R（6個關鍵概念）

### 1. Tokenization：模型不讀文字，只讀整數

- LLM 先把文字轉為 token ID（整數索引）
- **Token ≠ 單字**，使用 subword 分割：`tokenization → ["token", "ization"]`
- subword 是折衷：詞表不會太大（避免泛化差），序列不會太長（避免長度爆炸）
- **為什麼 LLM 數不清 "strawberry" 有幾個 r？** 因為它不是按字母處理，而是按 token ID 處理
- 不同模型 tokenizer 不同（GPT 系列 BPE 變體、LLaMA 系列 SentencePiece）→ 同一句話在不同 tokenizer 下 token 數不同 → 影響計費、速度、中文處理能力

### 2. Embedding：整數 → 高維向量

- token ID 只是索引，模型真正處理的是從 embedding matrix 查出的高維向量
- 7B 模型 hidden size 約 4096 → 每個 token 映射成 4096 維向量
- 直觀理解：每個 token 有一張「4096 項特徵表」（蘋果 = 水果? 甜? 紅色? Apple 公司? 賈伯斯?）

### 3. 位置編碼（Positional Encoding）

- Transformer 本身沒有位置概念，位置編碼讓模型知道 token 在序列中的位置
- 讓「貓吃魚」和「魚吃貓」的意思不同

### 4. Attention：token 之間交換資訊

- Multi-head self-attention 讓每個 token 學習關注哪些其他 token
- 多層 Transformer 堆疊 → 逐層精緻化理解

### 5. FFN（Feed-Forward Network）

- 每個 Transformer 層內的局部轉換（非跨 token 的全局）
- 被認為儲存了模型的「知識」（事實記憶）

### 6. Next-token Prediction：一次生成一個 token

- 最後一個 token 的向量映射成詞表的機率分布
- 依溫度（temperature）採樣選出下一個 token
- 重複直到生成結束符號 → 這就是 LLM 的完整生成過程

## C（關鍵行動）

- [ ] 用 Tokenizer 工具（OpenAI Tokenizer / Claude 的 beta token counter）實際看看自己的 prompt 分解成多少 token，體感 token 計費

## 相關連結

- [[claude-code-glossary]] — 38 個 AI 開發關鍵詞速查（含 Token / Context Window 定義）
- [[context-engineering-fundamentals]] — Context 壓縮機制（基於對 token 的深入理解）
- [[llm-cost-optimization-strategies]] — LLM 成本優化（token 理解是成本計算的前提）
- [[llm-concepts-visual-cards-1-60]] — 圖解 120 個 LLM 核心概念（1-60）：Tokenization/Embedding/Attention 等本篇概念的視覺化速查卡
- [[machine-learning-intro-for-beginners]] — 更廣的傳統 ML 基礎（監督/無監督/強化/自監督、損失函數、梯度下降），本篇聚焦 LLM，兩篇互補；自監督式學習是 LLM 預訓練的接點
