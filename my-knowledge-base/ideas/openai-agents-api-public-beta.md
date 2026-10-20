---
title: OpenAI 發表 Agents API 公開測試版：開放 Codex 同款 Harness 與沙盒基礎設施
date: 2026-09-11
tags: [ai/agent, ai/mcp]
source: https://www.aiposthub.com/openai-agents-api-public-beta/; https://www.aiposthub.com/openai-agents-api-computer-use-guide/（引 https://openai.com/index/devday-2026-recap/）
type: note
status: processed
asset_value: 48
---

OpenAI 於 2026-09-10 推出 Agents API 公開測試版，把支撐 Codex 與 ChatGPT for Work 的同一套 Harness（代理執行框架）與運行基礎設施，透過單一 API 開放給外部開發者串接，開發者不需自建情境管理、工具路由、多代理協調與沙盒環境即可組出生產就緒代理。核心可轉用判準：三項封裝好的工程解法——①情境管理（Session 逼近 Context Limit 時自動壓縮舊內容並保留關鍵資訊）、②工具搜尋＋程式化工具呼叫（動態載入相關工具定義降低 Token 消耗，支援平行呼叫與程式化篩選結果）、③多代理協作（子代理各自維持獨立情境，平行拆解任務）；執行環境可選 OpenAI 代管沙盒或九大合作夥伴（Modal、Cloudflare、Daytona、Blaxel、Runloop、Vercel、Oracle、E2B、DigitalOcean）。定價無額外框架費，僅按 Token 與工具用量計價；仍屬公開測試版，行為會隨模型更新演進。與既有 Codex 工具類條目定位不同——本篇聚焦「開放給外部開發者的平台級 Agents API」，而非終端工具本身的操作功能。若要深入查 API 參數與範例程式碼，回原文。

## 補充：DevDay 2026 回顧轉述與導入檢查（2026-10-08 補）

來源：《OpenAI Agents 開發接口是什麼？從 Computer use 到多代理編排的完整指南》（aiposthub，2026-09-30），引 OpenAI DevDay 2026 官方回顧（openai.com/index/devday-2026-recap）。
⚠️ 與本條原來源**同一作者（Philo／aiposthub）**，不算兩個獨立來源，因此本條仍留在 ideas/，待官方文件或非 aiposthub 來源再考慮升級為 wiki。

- 📅 **日期並列（不擇一）**：舊篇（2026-09-11）說「**9/10** 官方部落格推出公開測試版」；新篇（2026-09-30）說「**DevDay 2026** 公開」。可能是 9/10 先上公開測試、DevDay 回顧再整理，兩說各標日期保留
- 元件（官方回顧轉述）：工作階段、編排、上下文壓縮、失敗恢復、沙箱、工具／MCP、Computer use、多代理編排
- 計價：兩篇一致——公開測試期間不另收平台費，模型 token、工具、沙箱照用量計 ✅
- Responses API vs Agents API（作者整理，**以官方文件為準**）：Responses 是模型回應＋工具呼叫；Agents API 再往上加持續工作階段、編排、壓縮、恢復、沙箱、多代理
- 🔴 **Agents SDK ≠ Agents API**：Agents SDK 是開源框架（本庫提及見 [[ai-agent-dev-concepts-practitioner]]、[[context-isolation-and-compaction]]），Agents API 是 OpenAI 託管平台；兩者不可混寫
- 七步導入（**作者觀點**）：明確任務 → 20–50 個真實案例 → 最少工具 → 設預算上限 → 人工批准點 → 回傳證據 → 影子模式
- 監控（作者觀點）：**首次完成率與重試後完成率差距大，代表靠重試掩蓋問題**
- 與決策模型的關係：Decisions API 選下一步，Agents API 或應用層負責執行 → [[decision-model-ecosystem]]
- （第十批補）**AWS Bedrock Managed Agents**（aiposthub 2026-09-30〈OpenAI Marketplace 與 AWS Bedrock Managed Agents〉，同作者）：OpenAI 與 AWS 合作，把 Agents API 核心（工作階段、工具、沙箱、內容壓縮、復原、多代理）做成 Amazon Bedrock 原生代理，可整合 IAM、S3、監控等 AWS 資源（官方轉述＋作者展開）
- 🔴 可用區域、價格、GA 狀態**原文都沒寫**；同作者來源，本條維持留在 ideas/ 的判斷不變

## 相關連結
- [[openai-codex-record-replay]] — 同屬 OpenAI Codex 生態，但聚焦 Skill 生成與 agentskills.io 標準，屬終端工具功能層而非本篇的平台 API 層
- [[codex-computer-use-modes]] — Codex 操作模式，同為終端工具功能層，可與 Agents API 的沙盒基礎設施互相對照
- [[ai-agent-dev-concepts-practitioner]] — 框架比較表中的 OpenAI Agents SDK（開源框架，≠ 本條的託管 Agents API）
- [[context-isolation-and-compaction]] — Agents SDK 的 Handoff；對照 Agents API 的上下文壓縮
- [[fara-computer-use-agent]] — Computer use 的風險（畫面變動、提示注入）
- [[agent-loop-stop-conditions]] — 預算上限與停止條件
- [[claude-code-agent-teams-when-to-multiply]] — 什麼時候值得用多代理
- [[decision-model-ecosystem]] — Decisions API 選下一步、Agents API 執行的分工
- [[agentcore-rag-agent-tutorial]] — AWS 自家代理框架，可對照 Bedrock 代管的 Agents API
