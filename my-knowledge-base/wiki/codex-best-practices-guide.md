---
title: Codex 最佳實踐（超長篇）：先搞懂 AI，再用好 AI
date: 2026-06-30
tags: [ai/agent, ai/workflow, type/how-to]
source: https://codelove.tw/@tony/post/aGQ5Mx
type: article
status: processed
asset_value: 78
---

一份從「Agent 與 AI 對話的本質差異」講到模型選擇、上下文管理、Codex 實戰的完整教學長文。核心立場：**理解 AI 的能力邊界與運行原理，比學會某個軟體操作更重要**——先搞懂 AI，才能用好 AI。

## T（核心論點）

多數人用不好 AI，不是工具問題，而是**沒搞懂 Agent 與普通 AI 對話產品的本質差異**。AI 對話產品（豆包、ChatGPT 網頁、Gemini）的聯網只是「文字擷取」，無法操作本地電腦；Agent 產品（Codex、Claude Code、Cursor）擁有檔案讀寫 + 命令執行 + 自主循環（ReAct），能真正接管電腦完成任務。理解這點後，任何能被拆解成「檔案操作 + 命令執行」的事，都可交給 Agent，不該侷限在寫程式。

## B（背景與適用場景）

- 剛接觸 Agent、被各種產品/模型名詞搞混的人
- 想建立對 AI 能力邊界的「高維度理解」，過濾焦慮行銷
- 評估 Codex vs Claude Code vs OpenClaw 該選哪個
- 想優化模型開銷（包月 vs API 計費、主輔模型分工）

## R（主要內容）

**Agent 的兩個核心能力**：檔案讀寫 + 終端機命令執行。電腦上幾乎所有操作都可拆解成這兩件事，因此 Coding Agent 正逐步演化為通用 Agent。

**模型選擇（截至 2026/5）**：
- **Claude 系列（Opus 4.7）**：需求理解最強、程式邏輯嚴密，但最貴；大檔案編輯穩定。
- **GPT 系列（GPT-5.5）**：執行力強、指令遵循度高（長任務/多次壓縮後仍聽話）、包月性價比高；交流感改善。
- **Gemini（3.5 Flash）**：Agent 場景工具呼叫成功率偏低，但審美好、TPS 快、便宜，適合設計與文案。

**選 Agent 模型要看**：彙報交流感、上下文多次壓縮後的聽話程度、修改大檔案的穩定性——這些和「網頁聊天聰不聰明」是不同標準。

**harness 的重要性**：相同底層模型，不同 Agent 框架（工具調度、上下文引導、運行時糾錯）能把模型智慧榨取到不同程度。

**開銷決策**：重度開發優先「包月訂閱」勝過「API 計費」（ReAct 循環每次重讀全上下文，token 指數飆升）；必須用 API 時，用「頂級模型寫方案 + 低價輔模型當子 Agent 執行」降本；別為省錢用低配模型跑高難度重構（反覆失敗反而更貴）。

**多模態實戰**：生成設計稿（GPT Image 2）→ 視覺模型還原網頁（截圖轉 HTML/CSS）；字幕校對用「視覺糾正聽覺」（抽幀 OCR 校對同音字）。

**Codex 定位**：Rust 重構、冷啟動快；與 Claude Code 日常體感幾乎無差異（互動流程、/goal、上下文壓縮、MCP、Skill 全相通），差異僅在模型生態、產品形態（Codex 有桌面/手機端）、帳號穩定性。

## C（對自身的啟發）

- [ ] 把 Codex 當通用 Agent 用，不只寫程式——凡能拆成檔案操作 + 命令執行的任務都可交付
- [ ] 重度開發改用包月訂閱，停止為每次提問算 API 帳單
- [ ] 高難度任務用頂級模型，機械化單點任務用子 Agent 派發給低價模型
- [ ] 與本知識庫 kit 對照：Codex 作為編排中心的定位與 v2.x kit 一致

## 相關連結

- [[codex-advanced-tips-11]] — Codex 進階技巧
- [[codex-learning-roadmap]] — Codex 學習路線
- [[ai-coworker-vs-ai-agent]] — AI 協作者 vs AI Agent 的差異
- [[agent-framework-stability-comparison]] — Agent 框架穩定性比較
- [[llm-cost-optimization-strategies]] — LLM 成本優化策略
- [[claude-code-context-management]] — 上下文管理
