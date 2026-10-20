---
title: "Chat On Steroids：讓 ChatGPT 直接寫程式、跑指令，還不消耗 Codex 額度 | 網路資源"
source: "https://tools.wingzero.tw/articles/4065"
author:
  - "[[赤紅翼零]]"
published: 2026-09-13
created: 2026-09-15
description: "Chat On Steroids 是開源 Local MCP 工具，讓 ChatGPT 直接讀寫本機專案、執行 Terminal、跑測試並控制桌面，支援 Multi-Agent、Goal Loop 與 Compact Resume。本文介紹安裝、功能與安全注意事項，以及如何建立不消耗 Codex quota 的 AI Coding Agent 工作流程。"
tags:
  - "clippings"
status: processed
---
網站連結 [https://github.com/totec448-spec/chat-on-steroids](https://github.com/totec448-spec/chat-on-steroids)

Chat On Steroids 是一套開源的 Local MCP 工具，主要用途是讓 ChatGPT 不只回答 Coding 問題，而是可以直接進入使用者的本機專案工作，包括讀取與修改檔案、執行 Terminal Command、跑測試、維持長時間工作的 Terminal，甚至控制 Windows／macOS 的畫面、滑鼠與鍵盤。

它最大的特色，是把一般 ChatGPT Conversation 直接變成具有「動手能力」的 Coding Agent。使用者不需要一直把程式碼複製到 ChatGPT，再手動貼回 IDE，而可以形成：

`ChatGPT → MCP → 本機專案 → 修改程式 → 執行測試 → 查看結果 → 繼續修正`

這種完整工作流程。

目前比較值得注意的功能包括：

- **直接讀寫本機 Project** ：ChatGPT 可以查看與修改實際專案檔案。
- **執行 Terminal** ：能跑程式、測試與 Shell Command。
- **Multi-Agent Workers** ：可以把不同工作拆給多個 AI Worker 平行處理。
- **Goal / Loop** ：讓 Agent 持續處理尚未完成的長任務。
- **Compact & Resume** ：Conversation 太長時，可以把工作狀態與 Worker History 帶到新的 Chat 繼續。
- **Desktop Control** ：Windows／macOS 可進一步使用 Screen、Mouse、Keyboard 與 Clipboard。
- **不消耗 Codex quota** ：官方明確表示它使用的是 ChatGPT Conversation，而不是 Codex quota。

最後一點尤其值得注意。

如果平常大量使用 Codex 處理 Coding 任務，Chat On Steroids 提供了另一種工作模式： **把 ChatGPT 本身的模型能力，透過 MCP 接到本機檔案、Terminal 與開發環境。**

因此，一部分原本需要交給 Codex 執行的工作，例如閱讀 Project、修改 Code、跑 Tests、分析執行結果與持續修正，可以改由 ChatGPT + Chat On Steroids 完成。從實際使用策略來看，確實有機會大幅降低對 Codex quota 的依賴，把 Codex 額度保留給更適合它的任務。

不過這並不代表「AI Token 變成免費」。官方特別註明，使用者自己的 ChatGPT Account 仍然有 Model Availability、Usage 與 Context Limits；差別主要在於這套工作流程 **不計入 Codex quota** 。

Multi-Agent 也是它相當有特色的能力。例如可以讓一個 Worker 修改 Frontend、另一個處理 Backend，再讓其他 Worker 執行測試。Worker 會保留自己的 Context，後續工作也可以接續原本狀態。

安裝方面，它也不像許多 GitHub AI 專案需要自行配置大量 Python Environment 與 API Key。基本流程是安裝 CoS、授權 Project Folder、在 ChatGPT Developer Mode 接上 Core MCP，再載入 Chrome Companion Extension，完成後就能直接從 ChatGPT 下達工作。

需要注意的是，目前 Chat On Steroids 仍屬於 Beta，而且權限相當高。Shell Command 會使用目前 OS User 的正常權限執行，Desktop Control 更可以操作畫面、滑鼠、鍵盤與 Clipboard，因此不建議一開始就授權整顆硬碟或重要資料目錄。

整體而言，Chat On Steroids 最吸引人的地方，就是 **把現有 ChatGPT 變成可以直接操作本機開發環境的 Coding Agent** 。如果本來就有 ChatGPT 訂閱，又經常遇到 Codex quota 不夠用的情況，這套工具尤其值得注意：它不是增加另一個需要付費的 AI API，而是嘗試把既有 ChatGPT Conversation、MCP、本機工具與 Multi-Agent Workflow 串在一起，建立另一條完整的 AI Coding 工作路徑。