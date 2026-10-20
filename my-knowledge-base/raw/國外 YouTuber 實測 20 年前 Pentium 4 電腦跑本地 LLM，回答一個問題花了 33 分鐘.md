---
title: "國外 YouTuber 實測 20 年前 Pentium 4 電腦跑本地 LLM，回答一個問題花了 33 分鐘"
source: "https://www.koc.com.tw/archives/643941"
author:
  - "[[Rocky]]"
published: 2026-05-26
created: 2026-06-10
description: "說到要在電腦本地跑 AI，多數人都會想到需要有大容量記憶體，至少配一張 NVIDIA 顯卡，要不然就是買 Mac Studio 才行，不過最近國外 YouTube 頻道 Fully Buffered 就做了一個很有趣的實測，他拿 20 年前的 Intel Pentium 4 平台，挑戰在本機運行 LLM，還把這台老平台取名為「NetBurstGPT」。結果雖然真的成功跑起來，但完全稱不上實用，AI 回答一個問題需要花費 33 分鐘。"
tags:
  - "clippings"
status: processed
---
說到要在電腦本地跑 AI，多數人都會想到需要有大容量記憶體，至少配一張 NVIDIA 顯卡，要不然就是買 Mac Studio 才行，不過最近國外 YouTube 頻道 Fully Buffered 就做了一個很有趣的實測，他拿 20 年前的 Intel Pentium 4 平台，挑戰在本機運行 LLM，還把這台老平台取名為「NetBurstGPT」。結果雖然真的成功跑起來，但完全稱不上實用，AI 回答一個問題需要花費 33 分鐘。

## 20 年前 Pentium 4 真的能跑本機 LLM！實測 Llama 3.2 成功，但一個問題等了 33 分鐘

Fully Buffered 這次實測的是 Intel Pentium 4 641 處理器，屬於 Pentium 4 後期的 Cedar Mill 核心，2006 年推出，採 65nm 製程，具備 3.2GHz 時脈、2MB L2 快取，並支援 Hyper-Threading，這顆還是 D0 revision，TDP 為 65W。

以現在來看，這些規格當然非常古老，但當時在 Pentium 4 家族裡，算是比較後期、效能較好的版本。Fully Buffered 也特別提到，這顆 CPU 有支援 EM64T，這點非常重要，這樣後面才能正常運行 Windows 10 Pro 64 位元和現代 AI 工具：  

記憶體方面，裝了 4 條 2GB A-Data PC2-6400 CL5，總共 8GB DDR2-800。此外，這台電腦雖然有裝顯卡，但 Fully Buffered 表示，他不想靠 GPU 加速，想測試看看 NetBurst 架構本身能不能跑起 LLM。也就是說，會完全靠 Pentium 4 CPU 來運行 AI：  

Fully Buffered 一開始測試 LM Studio，但官方系統需求明確寫到，Windows x64 版本需要 CPU 支援 AVX2 指令集，而 Pentium 4 沒有 AVX2，所以 LM Studio 雖然能開到某些介面，也能做一些操作，但執行模型時就會出錯：  

隨後就改嘗試 Ollama，Ollama 在 v0.1.21 版本支援沒有 AVX 指令集環境，這也讓舊 CPU 或某些虛擬化環境能正常執行。模型部分，他選擇 Meta 的 Llama 3.2 3B，這是一款較輕量的文字模型：  

安裝完成後，他用 Ollama 執行模型，並輸入一個簡單問題：「What’s a Pentium 4？」，接著就開始等待。任務管理員也顯示 100％ 跑在 CPU 上，沒有使用到 GPU：  

最後結果真的成功了，只不過這個簡單問題花了接近 33 分鐘才回答完，prompt eval rate 約 0.27 tokens/s，eval rate 約 0.21 tokens/s：  

Fully Buffered 接著也測試 Linux Mint，想看看同樣的 Ollama 加 Llama 3.2 3B，在 Linux 環境會不會有更好的 CPU 使用率或推論速度。

結果沒有，Linux Mint 上的速度反而更慢，prompt eval rate 約 0.15 tokens/s，eval rate 約 0.13 tokens/s：  

最後他甚至來嘗試超頻 Pentium 4 ，時脈從原本 3.2GHz 拉到 4.3GHz，記憶體速度約 810MT/s。超頻後確實有變更快，約快 20%，prompt eval rate 提升到約 0.36 tokens/s，eval rate 則來到約 0.33 tokens/s：  

為了讓數字更有概念，Fully Buffered 也用自己的現代電腦做對比，同樣跑 Ollama 和同一個模型，自己的 Intel Core i5-12600K 大約快 200 倍，如果換成 NVIDIA Titan V GPU，則大約快 600 倍。

這完全不意外，畢竟 Pentium 4 的 NetBurst 架構是 1990 年代末期設計出來的東西。

完整影片：

![](https://www.youtube.com/watch?v=ILV-eu90te8)