---
title: Claude Opus 4.8 發布：誠實度、動態工作流、effort 控制
date: 2026-06-01
tags: [ai/claude, ai/agent]
source: https://www.bnext.com.tw/article/91083/anthropic-claude-opus-4-8-honesty-dynamic-workflows-effort-control
type: note
status: processed
asset_value: 48
---

Anthropic 於 2026 年 5 月 28 日發表 Claude Opus 4.8，距 4.7 版僅 41 天，定價不變但 fast mode 降至前代三分之一。核心賣點是「誠實度」——讓程式碼瑕疵漏過率降至 Opus 4.7 的四分之一；同步推出動態工作流、effort 控制與 Messages API 中段 system message 三項新功能。

## T（核心論點）

Opus 4.8 的差異化定位是「知道自己不知道」的誠實模型，而非單純的基準分數提升。Anthropic 罕見地自稱「漸進但具體的改進（modest but tangible improvement）」——這種誠實本身也是本次更新的示範。

**三大新功能：**

1. **動態工作流（Dynamic Workflows）**：在 Claude Code 中協調數十至數百個並行 subagents，用於跨大型代碼庫的搬遷、稽核或重寫。目前以 research preview 形式開放（Max/Team/Enterprise 方案）
2. **effort 控制**：在 claude.ai 與 Cowork 介面，可調整 Claude 的「努力程度」。拉高→思考更久品質更好；拉低→回應更快消耗配額更慢。所有方案皆可使用
3. **Messages API 中段 system message**：對話進行中可動態更新指令，不必重發完整 system prompt。對長時間運作的 Agent 特別有用（可動態調整權限、token 預算）

**其他改進：**
- prompt cache 最低門檻從 4,096 token 降至 1,024 token（Simon Willison 指出）
- Mythos 級模型預計「數週內」向所有客戶開放（目前僅限 Glasswing 計畫企業夥伴）
  - ⚠️ 2026-10-19：截至 2026-10，資安能力存取走分層管道（CIDP 夥伴計畫、擴展後的 Cyber Verification Program），官方並承認 Glasswing 找到許多問題但整體風險尚未足夠降低；[[claude-opus-5-5-release-notes]] 亦記 Mythos 5.1 只開放給審核通過的組織。原「數週內向所有客戶開放」是否仍成立，**並列待查、不斷言已取消**，見 [[ideas/anthropic-cyber-mission-cidp-oss-scanner]]（經 aiposthub 轉述）

## B（適用場景）

**適用：**
- 需要長時間運作 Agent、動態調整指令的開發者（Messages API 中段 system message）
- 大型代碼庫搬遷、稽核需要並行 subagent 的場景（動態工作流）
- 想節省配額且不需要最高品質輸出的場景（effort 控制調低）

**不適用 / 注意：**
- 動態工作流目前為 research preview，Enterprise 需管理員手動啟用
- 快節奏更新背後是 OpenAI Codex、Google Gemini Flash 的市場壓力——需持續追蹤能力邊界

## R（操作步驟）

1. **升級 Opus**：claude.ai 介面選擇 Opus 4.8 模型
2. **啟用 effort 控制**：在模型選單旁找到 effort 滑桿
3. **動態工作流（Enterprise）**：管理員到後台手動啟用，再在 Claude Code 中觸發
4. **Messages API 中段 system message**：在 API 的 messages 陣列中插入 system 類型訊息
5. **Prompt caching**：調整快取門檻至 1,024 token 以節省成本

## C（關鍵行動）

- [ ] 測試 effort 控制在日常任務中的省量效果（輕量任務拉低）
- [ ] 追蹤 Mythos 級模型開放時間，準備評估方案
- [ ] 若使用 API，更新 prompt caching 設定以利用 1,024 token 新門檻

## 原始摘要

報導 Anthropic 於 2026 年 5 月 28 日發表 Claude Opus 4.8，核心改進為「誠實度」——讓程式碼瑕疵漏過率降至 Opus 4.7 的四分之一。同步推出動態工作流（Claude Code）、effort 控制與 Messages API 中段 system message 三項新功能，定價不變但 fast mode 降至前代三分之一。

## 相關連結

- [[claude-code-advanced-workflow]] — Claude Code 進階工作流技巧
- [[claude-code-context-management]] — Context 管控與 token 優化
- [[llm-cost-optimization-strategies]] — LLM 成本優化策略（含 Prompt Caching）
- [[claude-ai-guide-cowork-skills-token]] — Claude 完整教學地圖
- [[claude-opus-5-5-release-notes]] — 後續版本 Opus 5.5（2026-09-23）；Opus 4.8 現為其資安題的轉送目標
- 📎 2026-10-19：[[ideas/anthropic-cyber-mission-cidp-oss-scanner]] — Cyber Mission／CIDP／OSS Scanner：Glasswing 之後的資安分層存取
