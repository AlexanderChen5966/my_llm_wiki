---
title: "Unsloth推Dynamic 2.0 GGUF量化方法，兼顧模型一致性與檔案效率"
source: "https://www.ithome.com.tw/news/174118"
author:
  - "[[李建興]]"
published:
created: 2026-06-08
description: "開源大型語言模型微調工具Unsloth推出Dynamic 2.0量化方法，針對每個可量化層動態選擇量化型別，並導入約150萬詞元（Token）的新版校準資料集，適用範圍從MoE架構擴展至一般模型"
tags:
  - "clippings"
status: processed
---
[![.](https://itadstatic.ithome.com.tw/B5/1778845196_6a07060c4f58a.gif)](https://itadapi.ithome.com.tw/ads/click?q=B5%7Eithome_index%7E2605B5004)

![](https://www.ithome.com.tw/sites/default/files/styles/picture_size_large/public/field/image/capture_593_-_unsloth_dynamic_2.0_ggufs_-_unsloth_documentation_-_unsloth.ai_.jpg?itok=ZMpxVg6u "Unsloth Dynamic v2.0在各種量化設定上，相較基準 imatrix GGUF，一致降低Gemma-3 27B模型的KL散度（KL Divergence），顯示量化後更能保留原始模型的行為與準確度。")

Unsloth Dynamic v2.0在各種量化設定上，相較基準 imatrix GGUF，一致降低Gemma-3 27B模型的KL散度（KL Divergence），顯示量化後更能保留原始模型的行為與準確度。

[![.](https://itadstatic.ithome.com.tw/B3/1779780082_6a1549f23944e.gif)](https://itadapi.ithome.com.tw/ads/click?q=B3%7Eithome_index%7E2605B3013)

開源大型語言模型微調工具Unsloth團隊 [公開Dynamic v2.0模型量化方法](https://unsloth.ai/docs/basics/unsloth-dynamic-2.0-ggufs) ，並表示後續將以Dynamic 2.0產出與更新GGUF模型檔，供llama.cpp與LM Studio等常見本地推論引擎使用。官方表示該方法著重在控制GGUF檔案大小與量化誤差之間的取捨，讓量化後的輸出行為更接近原始模型，盡量維持準確度。

Unsloth是一套大型語言模型微調與強化學習的開源工具，團隊近年也延伸到量化模型檔的發行，以及主流推論框架的相容性維護。GGUF不是新的模型架構或訓練標準，而是llama.cpp生態系常用的模型檔案格式設計，用單一檔案封裝模型推論所需資訊，並透過可擴充的鍵值型中繼資料結構，讓執行端更一致地載入模型。

而Dynamic 2.0則是量化方法的更新，主要變化在於針對每個可量化層動態決定量化型別，不再只處理少數層，且不同模型會採用不同的量化配置。同時，Unsloth表示，後續釋出的GGUF量化模型將導入新版校準資料集。官方描述其規模約150萬個詞元（Token），用於提升對話型模型在量化後的對話品質與穩定性。

Unsloth指出，先前Dynamic量化主要適用混合專家（MoE）架構模型，而Dynamic 2.0已可套用到包含MoE與非MoE在內的各類模型，降低方法受限於模型架構的門檻。Unsloth也把Apple Silicon與ARM裝置列為效能與相容性優先目標，並在文件中明確列出新增Q4\_NL、Q5.1、Q5.0、Q4.1與Q4.0等量化格式，讓同一批GGUF模型檔在不同裝置與推論引擎之間，有更完整的格式選項可用。

Unsloth以Gemma 3 27B模型為例說明Dynamic 2.0的表現，分別做了兩種對照。在與標準imatrix量化相比的測試中，Dynamic 2.0在多個量化型別的KL散度（KL Divergence）都有下降，但GGUF檔案大小會小幅增加，呈現以少量檔案大小換取更接近原模型輸出分布。

另外，在5-shot MMLU的基準測試，Unsloth的Dynamic 2.0 4-bit量化（Q4\_K\_XL）分數為71.47，檔案大小15.64 GB。要是以Google的QAT版為基礎再套用Dynamic 2.0，分數為71.07，檔案大小同為15.64 GB。相較之下，Google原生QAT版分數70.64，檔案大小17.2 GB，Unsloth版本在範例中分數略高且檔案更小。