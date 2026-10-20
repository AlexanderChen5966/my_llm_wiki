---
title: "MCP 開發，ChatGPT 報錯 Connector is not safe 解法"
source: "https://blog.ray-realms.com/mcp-kai-fa-chatgpt-bao-cuo-connector-is-not-safe-jie-fa/"
author:
  - "[[Ray 貓]]"
published: 2025-11-01
created: 2026-05-19
description: "📅 發布時間: 2025-11-01🏷️ 標籤: MCP開發, ChatGPT, OpenAI錯誤, 連接器安全開發 MCP 伺服器整合 ChatGPT 遇到「Connector is not safe」錯誤？立即了解此常見 OpenAI 錯誤的核心成因（與隱私描述有關）與最新實用解法！快速解決連接器安全問題，讓您的應用程式順利上線。問題：ChatGPT 報錯 Connector is not safe？最近很常遇到開發 MCP 伺服器時，想綁到 ChatGPT 的應用程式與連接器結果報錯 \"Connector is not safe\"明明我的程式碼都符合最佳實踐，版本也在最新到底是哪裡不安全？！我來告訴你怎麼解決 👇「工具描述」只要你的某個工具描述中包含「個人資料」或者說要「"
tags:
  - "clippings"
status: processed
---
> 📅 發布時間: 2025-11-01  
> 🏷️ 標籤: MCP開發, ChatGPT, OpenAI錯誤, 連接器安全

開發 MCP 伺服器整合 ChatGPT 遇到「Connector is not safe」錯誤？立即了解此常見 OpenAI 錯誤的核心成因（與隱私描述有關）與最新實用解法！快速解決連接器安全問題，讓您的應用程式順利上線。

---

## 問題：ChatGPT 報錯 Connector is not safe？

最近很常遇到開發 MCP 伺服器時，想綁到 ChatGPT 的應用程式與連接器  
結果報錯 "Connector is not safe"

![](https://ghost-storage.ray-realms.com/blog-images/1761968838335-l77czq.png)

明明我的程式碼都符合最佳實踐，版本也在最新  
到底是哪裡不安全？！

## 我來告訴你怎麼解決 👇

「工具描述」  
只要你的某個工具描述中包含「個人資料」或者說要「取得用戶資訊」或者其他嘗試觸犯用戶隱私的文字時，就會出現該問題

由於他們是用 LLM 判斷，所以這裡的文字是很模稜兩可，只是 LLM 覺得侵犯隱私那就沒救。

### 提示詞範例

（Ｘ）不要說取得用戶資訊  
（Ｏ） 取得用戶授權的可公開資料

![](https://ghost-storage.ray-realms.com/blog-images/1761969108238-ag52fl.png)

![](https://ghost-storage.ray-realms.com/blog-images/1761969118703-trdvpe.png)

經過剛剛隨意地實驗才發現這件事

我被這個問題卡了很久，他們論壇也很多人在討論這個問題，OpenAI 官方沒回應