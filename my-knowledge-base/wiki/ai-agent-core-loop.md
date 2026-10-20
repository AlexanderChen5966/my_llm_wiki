---
title: AI Agent 的骯髒祕密：核心其實只是約 80 行的迴圈
date: 2026-07-24
tags: [ai/agent, ai/workflow, type/concept]
source: https://codelove.tw/@tony/post/qzorQx
type: article
status: processed
asset_value: 64
---

AI agent 沒有神祕光環——用 Node.js 寫一個能自我運作的 agent，**核心迴圈只需約 80 行程式碼**。LangChain / CrewAI / Mastra 等框架不是在施魔法，它們只是把對話記憶、工具執行、重試、備援等便利功能包裝起來。理解底層機制後，才能判斷何時真正需要框架。

## T（核心論點）

Agent 的本質是一個 `for`/`while` 迴圈（作者用 `for` 避免無限迴圈燒 token）：**送出提示詞 + 工具清單 → 等模型回應 → 本機執行工具 → 把整段對話歷史送回 → 重複直到模型輸出純文字**。LLM 只負責「決定用哪個工具」與「什麼時候結束」；執行工具、重試、限制迭代、協調迴圈全是應用程式的責任。框架提供的是便利性與正式環境強化，不是魔法。

## B（適用場景）

**適用：** 想搞懂 agent 底層而非只會呼叫框架；評估專案「值不值得引入框架」；學習工具呼叫（function calling）機制。
**不適用 / 注意：** 正式環境仍需框架的指數退避、抖動、自動切換模型等強化；本示範的重試機制刻意做得很基本。

## R（agent 四步驟迴圈）

以作者的程式碼審查 agent「Steve」（senior engineer 人設，審查本機 git diff）為例：

1. **送出提示詞和可用工具**：給模型「使用者訊息 + 工具清單」（`getDiff` / `getFile` / `listFiles`），工具以 `functionDeclarations`（name/description/parameters）宣告。
2. **等待模型回應**：模型可回三種——純文字（完成，即最終結果）、要求呼叫工具（`functionCall`）、或兩者兼有。
3. **在本機執行工具**：應用程式本機跑 `git diff` 或讀檔，把結果作為對話中的另一則訊息（`functionResponse`）送回模型。
4. **重複直到完成**：回到步驟 2，並設最大迭代次數（示範設 10 次）防止無限迴圈。**關鍵細節：每次都把整段對話歷史一起送回**（模型無狀態，靠歷史累積上下文）。

**模型怎麼知道要呼叫函式？** Gemini / GPT 等原生支援工具呼叫，SDK 負責格式化請求，模型本身理解工具宣告並在需要時產生函式呼叫。較舊的本機模型（如舊 Llama）也可用提示詞說明「如何解讀 JSON、以特定 JSON 格式回應」，再 `JSON.parse()`——前提是它真的回傳有效 JSON 而非夾雜 Markdown。新的開源模型也越來越多原生支援工具呼叫。

## C（關鍵行動）

- [ ] 引入 agent 框架前，先問「我需要的只是這個 80 行迴圈，還是真的需要框架的記憶/重試/備援？」
- [ ] 設計自動化 agent 時，把「執行工具、限制迭代次數、停止條件」當成自己的責任而非模型的
- [ ] 記住 stateless 本質：每輪都要回送完整對話歷史，這也是 context 成本的來源
- [ ] 呼應本知識庫的 [[agent-loop-explained]]：本篇是「動手實作」視角，該篇是「Loop Engineering 概念」視角

## 原始摘要

譯自 dev.to Sylwia Lask〈The Dirty Secret Behind AI Agents〉。用 Gemini API + Node.js 做 code review agent「Steve」，核心迴圈約 80 行。四步驟：送提示+工具 → 等回應 → 本機執行 → 回送歷史重複（上限 10 次）。專案倉庫 github.com/sylwia-lask/code-review-agent。作者預告後續會接 MCP 與本機 LLM。

## 相關連結

- [[agent-loop-explained]] — Agent Loop 概念總覽（ReAct 範式、Loop Engineering 四層階梯），本篇為其實作對照
- [[synthesis/synthesis-agent-engineering-architecture]] — Agent 可靠性來自工程結構（工具契約/狀態機/護欄），本篇的「應用程式負責協調」正是此觀點的最小實例
- [[openworker]] — Andrew Ng 的本機 agent（引擎建於 aisuite），可對照框架化的完整實作
- [[ai-spec-collaboration-three-parties]] — AI 時代協作三方觀點（同來源站台議題）
- [[agent-framework-stability-comparison]] — 何時該用框架：四種穩定性哲學
