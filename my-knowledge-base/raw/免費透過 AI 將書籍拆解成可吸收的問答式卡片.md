---
title: "免費透過 AI 將書籍拆解成可吸收的問答式卡片"
source: "https://muki.tw/free-to-break-down-books-into-qa-cards-via-ai/"
author:
  - "[[MUKI space*]]"
published: 2025-06-04
created: 2026-05-18
description: "前兩天跟大家分享了我是如何使用 Google Noteboo..."
tags:
  - "clippings"
status: processed
---
免費透過 AI 將書籍拆解成可吸收的問答式卡片.md

前兩天跟大家分享了我是如何 [使用 Google Notebook LM 幫助我學習書本上的知識](https://muki.tw/notebook-lm-knowledge/) ，我除了上傳筆記內容當作來源外，也會上傳像 HQA 筆記法這樣的問答卡片，協助我更近一步地收斂內容。除了自己整理問答卡片外，有沒有什麼方法，可以透過 AI 將書籍內容拆解成問答卡片呢？

你別說，還真的有！

今天要介紹的這套工具：Easy Dataset，就可以透過 AI 將書籍變成一張張的問答卡片，整個過程不需要寫程式，只要提供書籍內容就能自動完成。而且不管是 Easy Dataset 工具本身還是使用的 AI 模型，都是完全免費的。

## Easy Dataset 是什麼

Easy Dataset 是由 ConardLi 做的一個 Git 開源專案，支援 Windows 和 Mac 平台，可以直接下載使用，也能用 docker 部署，作者目前就職於抖音，平台只提供簡體中文與英文兩個語系。

![GitHub - ConardLi/easy-dataset: A powerful tool for creating fine-tuning datasets for LLM](https://opengraph.githubassets.com/f988962a2e970d49da389ff922a7db0c6f9738eaa59cc1c6027ba2459f994163/ConardLi/easy-dataset)

GitHub - ConardLi/easy-dataset: A powerful tool for creating fine-tuning datasets for LLM

A powerful tool for creating fine-tuning datasets for LLM - ConardLi/easy-dataset

https://github.com/ConardLi/easy-dataset

關於 Easy Dataset

Easy Dataset 問世有一段時間了，作者更新得還蠻頻繁，而且在 GitHub 上有超過 8,000 的 stars，是一個熱門的開源專案。但如果你下載 dmg 檔案安裝，可能會出現「未知的開發者」這樣的警告訊息，如果對軟體來源有疑慮的話，可以斟酌使用。

AD

## 使用免費的 AI 模型

Easy Dataset 提供了非常多的 AI 模型，如 OpenAI、DeepSeek，如果你想要用雲端的 API，只要編輯設定 API key 即可。

![](https://muki.tw/wordpress/wp-content/uploads/2025/06/CleanShot-2025-06-04-at-10.14.03.png)

如果想要免費，我們可以自行架設本機的 AI 模型，例如 Ollama 或 LM Studio。

Ollama 使用相對簡單，到 [官網下載](https://ollama.com/) 後，安裝並啟動 Ollama 時，他會提供一個本地端的 REST API 服務，預設的 port 是 11434，所以 API 網址為： `http://127.0.0.1:11434/api`

點擊「刷新模型列表後」，預設有兩個模型可選：

- llama3.2:latest
- nomic-embed-text:latest

請選擇「llama3.2:latest」，因為「nomic-embed-text:latest」是專門用來做文字嵌入 (embeddiing) 向量化的模型，他沒有語言生成的能力，沒辦法跟我們聊天。

![](https://muki.tw/wordpress/wp-content/uploads/2025/06/CleanShot-2025-06-04-at-10.19.26.png)

▼ 如果你有設定多個語言模型，可以隨時透過右上角的下拉選單切換，他會根據我們當前選擇的模型做後續處理。所以在使用 AI 做數據蒸餾或問答卡片時，記得先確認現在使用的模型是你想要的。

![](https://muki.tw/wordpress/wp-content/uploads/2025/06/CleanShot-2025-06-04-at-10.24.34.png)

## 上傳文件檔案

Easy DataSet 支援的格式有 PDF、Markdown、TXT、還有難得一見的 Docx。他跟 Google Notebook LM 一樣，也可以在同一個資料夾中上傳多個檔案。

上傳後，選擇「上傳並處理文件」，一段時間過後，就可以看到他解析的內容。

![](https://muki.tw/wordpress/wp-content/uploads/2025/06/CleanShot-2025-06-04-at-11.41.54.png)

如果想要使用不同的分割方式，可以選擇「更多」->「項目設置」->「任務配置」的分塊策略，根據你的需求做選擇。

![](https://muki.tw/wordpress/wp-content/uploads/2025/06/CleanShot-2025-06-04-at-17.45.36.png)

## 數據蒸餾功能

數據蒸餾是我很喜歡的一個功能，目的是從大量資料中收斂出更精確、品質更高的小型資料集，通常可以用這些資料集來訓練或微調模型，達到更好的學習效率與推理品質。以書籍為例，不是越多越好，而是要挑重點，達到去蕪存菁的效果。

在數據蒸餾的選單中，我會選擇「全自動蒸餾數據集」，可以照他預設的設定，也可以自行修改。可以從右側的任務預估訊息知道會產生的標籤和問題數量，太多可以進行增減。這邊可以根據你使用的 AI 模型來調整，例如使用帶有推理的 AI 模型 (如 DeepSeek)，產生的速度會比較慢。

![](https://muki.tw/wordpress/wp-content/uploads/2025/06/CleanShot-2025-06-04-at-11.49.21.png)

▼ 使用蒸餾功能，會同時產生標籤與問題

![](https://muki.tw/wordpress/wp-content/uploads/2025/06/CleanShot-2025-06-04-at-13.02.39@2x.png)

▼ 蒸餾完成後，可以看到標籤與問題

![](https://muki.tw/wordpress/wp-content/uploads/2025/06/CleanShot-2025-06-04-at-17.17.20.png)

## 根據問題產生答案

我們透過 Easy DataSet 產生了有層級的標籤，每個標籤也有對應的問題了，最後一步是根據這些問題產生 **答案** 。Easy DataSet 的設定中將答案叫做「數據集」，所以選擇頂部選單的「問題管理」-> 「自動生成 **數據集** 」就可以產生答案。

Easy DataSet 會根據問題、問題對應的文本區塊以及標籤一起產生答案，確保答案和文獻內容的關聯性。

![](https://muki.tw/wordpress/wp-content/uploads/2025/06/CleanShot-2025-06-04-at-17.21.48.png)

如果你不想一次產生全部的答案，可以勾選你想要的問題後，選擇「批量構造數據集」，這樣就會只針對我們勾選的問題產生答案。

## 觀看答案

最後一步，要在哪裡看答案呢？

▼ 從頂部選單選擇「數據集管理」，再點選問題就可以看答案

![](https://muki.tw/wordpress/wp-content/uploads/2025/06/CleanShot-2025-06-04-at-17.26.32.png)

▼ 如果使用的是推理模型，還可以看到推理過程的思維鏈

![](https://muki.tw/wordpress/wp-content/uploads/2025/06/CleanShot-2025-06-04-at-17.29.39.png)

最後也能匯出所有的答案(數據集)，可以匯出至本地端，也能一鍵上傳到如 LLaMa Factory 等平台。Easy DataSet 的官方文件有詳細的圖文說明，可以參考：

![数据集导出 | Easy Dataset](https://docs.easy-dataset.com/~gitbook/ogimage/ltfcWX8NqsKq2Sdbmqgs)

数据集导出 | Easy Dataset

Copyright © 2025 Easy Dataset

## 法律與版權注意事項

我使用的書籍是自己合法購買的電子書，轉換與處理都只用於個人學習用途，並未將原文或 AI 產出大規模公開分享或商業販售。若你也想試試，請確保你的用途為非商業性質，並盡量避免大段原文輸出。此外，我的模型部署方式為本地 Ollama，資料完全不離開電腦，也避免了 AI 服務存取隱私資料的風險。

## 小結

Easy DataSet 的官方文件有詳細的使用圖文教學與流程，有興趣想更進一步瞭解的朋友，可以點擊閱讀： [https://docs.easy-dataset.com/](https://docs.easy-dataset.com/) 。

這次使用 Easy DataSet 讓我更加相信，現在對我而言，AI 的價值不只是幫我閱讀，還能幫我整理資料與產生我所要的資訊。現在的我同時使用 Easy DataSet 和 Google Notebook LM 這兩套工具幫助我學習，未來的挑戰是該如何再內化，還有如何搭配 Obsidian 建構更完整的資訊流。

如果大家有任何學習的想法或工具，也歡迎一起討論唷！

muki@journal ~/AI 應用 $