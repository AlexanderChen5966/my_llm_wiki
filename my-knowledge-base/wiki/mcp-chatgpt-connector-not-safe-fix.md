---
title: MCP 開發：ChatGPT「Connector is not safe」錯誤解法
date: 2026-05-20
tags: [ai/mcp]
source: https://blog.ray-realms.com/mcp-kai-fa-chatgpt-bao-cuo-connector-is-not-safe-jie-fa/
type: article
status: processed
asset_value: 9
---

開發 MCP Server 整合 ChatGPT 時出現「Connector is not safe」報錯，根源在於工具描述（tool description）觸發了 OpenAI 的 LLM 隱私安全偵測，與程式碼品質無關。

## T（核心論點）

ChatGPT 用 LLM 判斷 MCP 工具描述是否涉及隱私侵犯。只要工具描述中包含「個人資料」、「取得用戶資訊」等字眼，就會被標記為不安全，無論程式碼本身是否符合最佳實踐。判斷標準是模稜兩可的，因為是 LLM 做決策。

## B（適用場景）

**適用：** 開發 MCP Server 並試圖整合 ChatGPT Connector 的開發者；遇到「Connector is not safe」錯誤卡住的場景
**不適用 / 注意：** 此解法針對 ChatGPT 特有行為，Claude / Cursor 等其他 MCP 客戶端的安全判斷機制不同；OpenAI 官方目前無正式回應或文件說明

## R（操作步驟）

修改工具描述，避免隱私敏感詞彙：

| 錯誤寫法（觸發報錯）| 正確寫法（通過審核）|
|---|---|
| 「取得用戶資訊」| 「取得用戶授權的可公開資料」|
| 「個人資料」| 改用非隱私導向的描述語言 |

核心原則：**描述工具行為時，強調「授權」與「公開性」，而非「個人」與「用戶」**。

## C（關鍵行動）

- [ ] 開發 MCP Server 時，工具描述使用強調「授權」的語言
- [ ] 遇到報錯時，逐一排查工具描述，找出觸發隱私偵測的詞彙

## 原始摘要

作者 Ray 貓在開發時被此問題卡住很久，OpenAI 論壇有很多人討論但官方沒有回應。核心 insight：判斷標準是「LLM 覺得侵犯隱私」，不是確定性規則，所以即使改一個詞也可能解決問題。

## 相關連結

- [[agent-skills-vs-mcp]] — MCP 與 Skills 的選擇框架
