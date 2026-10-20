---
title: "Google推出Gemini Omni 1.1 Flash，靠「回看10秒」無縫延伸場景"
source: "https://www.bnext.com.tw/article/92032/gemini-omni-1-1-flash-video-extension-interpolation"
author:
  - "[[李先泰]]"
published: 2026-08-28
created: 2026-08-28
description: "Google發布Gemini Omni 1.1 Flash正式版，新增場景延伸、首尾影格插補；提供360p預覽及4K升頻，讓影片生成工作流更靈活。"
tags:
  - "clippings"
status: "processed"
---
Google 於美國時間 8 月 27 日（台灣時間 28 日凌晨）正式發表影片生成模型 Gemini Omni 1.1 Flash，並已將其列入 Gemini API 付費層的正式推出版本（免費層暫不提供）。

本次更新的核心並非單純強調畫質規格，而是致力於將影片生成轉化為一套可反覆修改的實用工作流。新版模型引進了多項關鍵功能： **場景延伸、首尾影格指定、360p 快速預覽輸出、1080p 與 4K 畫質升頻，同時支援在輸入端加入最多 3 支（每支最長 3 秒）的參考影片。**

Google 強調，這些升級將使影片生成過程更具可控性、加速迭代效率，並更符合實際商用部署的需求。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

## 從預覽版邁向正式版：首度加入延伸與首尾影格插補

根據 Gemini API 的官方更新紀錄，Google 先前於 2026 年 6 月底推出了主打高速生成與對話式編輯的公開預覽版（gemini-omni-flash-preview）。而此次發布的正式版（gemini-omni-1.1-flash）則進一步加入了影片延伸、首尾影格插補以及更彈性的解析度控制。舊有的預覽版端點則預計將於 2026 年 9 月 30 日終止服務。

在眾多新功能中，「場景延伸」允許開發者匯入既有影片，並從結尾處無縫接續生成新內容。Google 說明，與過往模型僅能參考最後 1 秒的畫面不同，Omni 1.1 可將輸入影片結尾前多達 10 秒的片段納入上下文運算。這項改進大幅提升了視覺一致性與敘事的連貫度。影片延伸會以 10 秒為單位進行，累計生成總長最高可達 40 秒。

「指定首尾影格」功能則賦予模型在兩個關鍵影格之間，自動補算並生成連續影像的能力。官方特別指出，這項技術非常適合應用於複雜的環繞運鏡、變焦轉場，或是製作無縫循環播放的視覺素材。

簡單來說，過去在銜接影片時，模型因為「只看得到最後一秒」，生成的內容往往容易走鐘失真；現在模型具備了「回頭看 10 秒」的記憶能力，能更精準掌握先前的畫面脈絡。

## 支援 360p 快速預覽，正式輸出可升級 1080p 或 4K

為了優化開發流程，Google 本次新增了 360p 較低解析度的輸出選項。相較於標準的 720p，360p 模式的生成速度提升最高可達 60%（官方註明此數據是基於兩者的系統吞吐量比較得出），且成本僅需約三分之一。

這項功能明確瞄準了快速原型製作、分鏡迭代，以及開發平台上的即時算圖需求，讓創作者能在低成本快速確認畫面效果後，再選擇進行 1080p 或 4K 的高畫質正式生成。

> 延伸閱讀：  
> [比爾蓋茲警告：AI恐掀人類史最動盪時期！他提「AI稅、人類保留區」3解方](https://www.bnext.com.tw/article/92025/bill-gates-turbulent-ai-era-human-reserved)

資料來源： [Google 官方公告](https://blog.google/innovation-and-ai/technology/developers-tools/build-with-gemini-omni-1-1-flash/) 、 [Gemini API 更新紀錄](https://ai.google.dev/gemini-api/docs/changelog) 、 [Gemini Omni 1.1 Flash 開發者文件](https://ai.google.dev/gemini-api/docs/omni) 、 [Gemini API 定價](https://ai.google.dev/gemini-api/docs/pricing)

本文初稿為AI編撰，整理．編輯/ 李先泰