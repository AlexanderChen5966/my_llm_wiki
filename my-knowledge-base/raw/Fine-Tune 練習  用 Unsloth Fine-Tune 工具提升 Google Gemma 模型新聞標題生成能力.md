---
title: "[ Fine-Tune 練習 ] 用 Unsloth Fine-Tune 工具提升 Google Gemma 模型新聞標題生成能力"
source: "https://medium.com/@simon3458/google-gemma-2-fine-tune-news-generation-unsloth-tool-ab4a094909df"
author:
  - "[[Simon Liu]]"
published: 2024-12-04
created: 2026-06-02
description: "此文章，我將會透過近期在學習如何透過 Fine-Tune 工具 Unsloth 來針對新聞資料集，進行 Fine-Tune Google Gemma 2 9b 模型作業進行簡單的紀錄。"
tags:
  - "clippings"
status: processed
---
![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ELL1s9Dz1sJzczw__yc_7A.png)

\[ Fine-Tune 練習 \] 用 Unsloth Fine-Tune 工具提升 Google Gemma 模型新聞標題生成能力

此文章，我將會透過近期在學習如何透過 Fine-Tune 工具 Unsloth 來針對新聞資料集，進行 Fine-Tune Google Gemma 2 9b 模型作業進行簡單的紀錄。

## Fine-Tune LLM 工具 — Unsloth

Unsloth 是一個專為提升大模型訓練效率而設計的開源專案工具，此專案號稱確保重寫後的模型計算結果與原始版本完全一致，沒有任何近似計算，保證模型訓練的準備度影響可以降至最低。

## Get Simon Liu’s stories in your inbox

Join Medium for free to get updates from this writer.

需要注意的是，目前 Unsloth 僅支援單卡運算，但是，對於我這種硬體資源有限的使用者而言，這一點目前還不會帶來太多困擾，畢竟我也只有 Google Colab 來進行 GPU 的操作。

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*8cb0eUs9A3p7eIgohu9SwA.png)

## 資料集 — AWeirdDev/zh-tw-pts-articles-sm

此資料集是存放在 HuggingFace 的資料，該作者從公視新聞網（news.pts.org.tw）所抓取的新聞資料。資料集共有 1,400 筆資料，每筆資料包含以下欄位：

- **image** ：縮圖
- **title** ：標題
- **conclusion** ：結論（「結論先講」一欄，可能為 None）
- **content** ：文章內容
- **timestamp** ：發布時間
- **category** ：類別（可能為 None）
- **link** ：文章連結

我們可以使用 Hugging Face 的 `datasets` 套件下載、使用或編輯此資料集，方法如下：

```c
from datasets import load_dataset

dataset = load_dataset("AWeirdDev/zh-tw-pts-articles-sm")
```

本次實驗，我將會透過「文章內容」欄位作為 Fine-Tune 資料，來進行新聞「標題」欄位的生成工作。

## Fine-Tune 程式碼 — 官方範例

本次微調，我將以官方所提供的 Google Colab 程式碼進行修改，讓我能夠快速的建置 Fine-Tune 環境進行處理：

## [Google Colab](https://colab.research.google.com/drive/1vIrqH5uYDQwsJ4-OO3DErvuv4pBgVwk4?usp=sharing&source=post_page-----ab4a094909df---------------------------------------)

### Edit description

colab.research.google.com

- 其中，在資料的前處理上，根據任務進行調整：
```c
alpaca_prompt = """以下是描述任務的指令，並與提供進一步上下文的輸入配對。編寫適當完成請求的回應。

### Instruction:
{}

### Input:
{}

### Response:
{}"""

EOS_TOKEN = tokenizer.eos_token  # Must add EOS_TOKEN

def formatting_prompts_func(examples):
    # The instruction is constant across all examples, so it's not in the dataset.
    instructions = "請根據新聞內容，給予合適的新聞標題。"  # Fixed instruction for all examples
    inputs = examples["content"]
    outputs = examples["title"]
    
    # Create the formatted prompts
    texts = []
    for input, output in zip(inputs, outputs):
        text = alpaca_prompt.format(instructions, input, output) + EOS_TOKEN
        texts.append(text)
    
    return {"text": texts}

from datasets import load_dataset
dataset = load_dataset("AWeirdDev/zh-tw-pts-articles-sm", split="train")
dataset = dataset.map(formatting_prompts_func, batched=True)
```

我們就可以利用 Google Colab 所提供的 T4 GPU 進行 Fine-Tune 工作。

## Fine-Tune 結果

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Oy82SIfO5k80N9UGrKbp-Q.png)

- 就 Training Loss 來說，整體是有下降，預期模型的 Fine-Tune 學習到了有用的特徵。
- 雖然整體是下降的，但在某些時間段， Loss 出現了一些波動。這可能表明模型在局部範圍內的學習過程不穩定，需要更細緻的參數調整。

透過未訓練的新聞內文來進行測試，整體來說效果不錯：

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*o6KwdNpp6nGFsqNmhZsr6Q.png)

Fine-Tune 前的 Google Gemma 9b 模型，所生成出來的新聞標題結果

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*eZy7FbV3Ye-O6SQS4UhhzA.png)

Fine-Tune 過後的模型，生成新聞標題結果

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*cDjkX1Pn5-jwHXmc9CxDOQ.png)

測試三個不同的新聞，所得到的結果，表現不錯

## Fine-Tune 模型

我將 Fine-Tune 過後的結果放在 HuggingFace 上，大家可以參考看看：

## [Simon-Liu/gemma2-9b-zhtw-news-title-generation-finetune · Hugging Face](https://huggingface.co/Simon-Liu/gemma2-9b-zhtw-news-title-generation-finetune?source=post_page-----ab4a094909df---------------------------------------)

### We're on a journey to advance and democratize artificial intelligence through open source and open science.

huggingface.co

## 結論

透過此次實驗，我成功應用了 Unsloth 工具在有限資源環境下進行了 Google Gemma 2 9b 模型的 Fine-Tune。藉由使用 Hugging Face 的中文新聞資料集，結合 Google Colab 的 GPU 支援，完成了一個有效的新聞標題生成任務模型。

Unsloth 簡化了 Fine-Tune 的過程，確保了準確性與效率，尤其對於硬體受限的用戶提供了友好的使用體驗。在資料處理和模型 Fine-Tune 上，整體結果顯示 Loss 持續下降，Fine-Tune 後模型在標題生成的質量上有明顯提升。

## I am Simon

大家好，我是 Simon 劉育維，是一位 AI 領域解決方案專家，目前也擔任 Google GenAI 領域開發者專家 (GDE)，期待能夠幫助企業導入人工智慧相關技術解決問題。如果這篇文章對您有幫助，請在 Medium 上按一下鼓勵，並追蹤我的個人帳號，這樣您就可以隨時閱讀我所撰寫的文章。歡迎在我的 Linkedin 上留言提供意見，並與我一起討論有關人工智慧的主題，期待能夠對大家有所幫助！

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/0*_A2JPnEp2MlZxIj0.png)

My Personal Website: https://simonliuyuwei-4ndgcf4.gamma.site/