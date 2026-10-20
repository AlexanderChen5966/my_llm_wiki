---
title: MCP Server 的評估判準與 Function Calling 層級切分
date: 2026-08-21
tags: [ai/mcp, type/concept]
source: https://ithelp.ithome.com.tw/articles/10404043
type: note
status: processed
asset_value: 36
---

## 核心論點

MCP 統一的是「連接規格」而不是「工具品質」——接得上不等於好用。評估一個 MCP Server 要看它的工具描述是否清楚、回傳是否結構化、會不會一次拉回大量無關內容；底層工具本來就無法按條件篩選的話，包一層 MCP 只會得到「規格正確但依然難用」的工具。

## 重點整理

- 層級切分：**Function Calling ＝ 模型這一次怎麼呼叫某個功能；MCP ＝ AI 應用怎麼用共同規格接上並認識外部能力**。兩者常同時出現而非互相取代——MCP 讓能力被找到與接上，模型最後仍可能以 Function Calling 方式選擇並呼叫
- MCP Server 提供三種能力：`Tools`（可以請它做什麼）、`Resources`（可以讀到什麼）、`Prompts`（可重複使用的提問範本）；初學常只看到 Tools，這正常
- MCP 不是把外部資料一次塞進 Context：AI 應用先取得能力說明，再依任務決定呼叫哪一項，直接連到 Context Engineering
- 評估判準四問：工具描述是否清楚？回傳資料是否好讀／結構化？一次會不會拿回太多無關內容？出錯時有沒有說明哪裡出問題？
- 反例：旅行資料庫只能一次列出全台景點、無法依城市／營業時間／距離篩選，包 MCP 後 Agent 仍得拉一大堆資料自己過濾；回傳版面複雜的 PDF 常導致 Agent 讀錯時間或漏備註
- 設計原則：能由程式確定完成的篩選、排序與格式整理就交給程式，不要每件雜務都丟給模型猜
- Local vs Remote MCP：本機靠近自己的檔案，遠端便於共用與集中維護；真正該問的是資料去哪、誰能用、需要什麼憑證、能改多大範圍
- 五步入門順序：①挑一件小事 ②在 Agent 裡找 MCP 設定（Claude Code 可用 `claude mcp`）③先看工具清單與權限 ④只從唯讀工具開始 ⑤需要時再動手
- 安裝前的白話確認 prompt：「請先用白話說明它能做什麼、需要哪些權限、會碰到哪些資料；先不要替我安裝或執行任何工具。」
- 權限面：MCP 讓能力變多也放大 prompt injection 的傷害面。**提醒像工作守則（CLAUDE.md），權限則像門禁；兩者可以合作，不能互相假裝成對方**——敏感操作應遵守最小權限並保留確認／拒絕步驟
- 版本提醒：MCP 於 `2026-07-28` 發布重大規格更新，連接流程與多項協議細節明顯改變；讀教學前先看日期與採用版本

## 可用之處

挑選或自建 MCP Server 時可直接當檢查表，尤其「回傳結構化 vs 複雜 PDF」與「一次拉太多」兩點會直接影響 Claude Code 的 context 消耗。門禁 vs 工作守則的區分也校正了一個常見誤解：CLAUDE.md 寫「寄信前先問」不等於權限控制。

## 相關連結

- [[agent-skills-vs-mcp]]
- [[mcp-fastmcp-server]]
- [[agent-tool-authorization-gatekeeper]]
- [[prompt-injection-vs-jailbreak]]
- [[skill-as-installable-work-method]]
- [[borrowed-rules-customization-loop]]
- [[rethink-workflow-not-just-accelerate]]
