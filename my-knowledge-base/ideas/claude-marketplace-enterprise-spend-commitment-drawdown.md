---
title: Claude Marketplace 擴版：企業用「承諾額度抵扣」採購 CrowdStrike/Cursor/Factory/Gamma/Vercel
date: 2026-09-10
tags: [ai/claude, ai/workflow]
source: https://www.aiposthub.com/claude-marketplace-expands-crowdstrike-cursor-factory-gamma-vercel-enterprise-spend-commitment-deep-dive/
type: note
status: processed
asset_value: 40
---

Anthropic 於 2026/9/9 宣布 Claude Marketplace 新增五大企業夥伴（CrowdStrike、Cursor、Factory、Gamma、Vercel），核心機制是「承諾額度抵扣（Spend Commitment Drawdown）」：企業既有的 Anthropic 年度預付額度可直接抵扣採購 Marketplace 產品，透過單一主合約與統一發票繞過傳統 4–9 個月的供應商審查流程。可轉用判準是採購策略設計本身——用「零佣金＋額度抵扣」把 B2B 生態夥伴綁進自己的結算樞紐，對比 OpenAI 走「中心化超級應用」路線。若要深入查原文，可看文中對比表（Anthropic vs OpenAI vs AWS/Azure Marketplace 三種分銷哲學）與 Marketplace 三波擴張歷程（2026/3、5、9月）。

> ⚠️ **對比框架附註（2026-10-08）**：首段「OpenAI 走中心化超級應用路線」的對比，在 DevDay 2026 後**至少在採購機制上已不成立**——OpenAI 也推出 commitment 抵用的 Marketplace，兩家已收斂到同一模式。原句保留為 2026-09-10 快照，詳見下方 2026-10-08 補充。

## 2026-09-23 改版為三類整合入口（2026-09-29 補充）

來源：aiposthub〈Claude Marketplace 是什麼？企業如何用 Anthropic 合約額度採購 AI 工具〉（2026-09-24）。來源原文措辭為「9/23 推出」，但本筆記已記錄 Marketplace 在 2026/3、5、9 月分三波擴張（9/9 新增五家夥伴），所以 9/23 **不是首次上線**，本庫寫成「9/23 改版為三類整合入口」。

- **三類內容**：連接器與外掛（Slack、Notion、Google、Microsoft）／AI 產品與代理（Cursor、CrowdStrike、Harvey、Snowflake）／服務夥伴（Accenture、Deloitte、BCG）
- **規模**：連接器與外掛超過 2,000 個；付費產品目錄 15 項（作者 9/24 查閱）。兩者是不同類別，不能相加
- **購買流程**：「Request to buy」要透過帳戶團隊，頁面上不能直接結帳；合作夥伴支出計入既有的 Anthropic 承諾、由 Anthropic 開發票（與上方機制一致）
- **名詞釐清**：商務 Claude Marketplace ≠ Claude Code 的 `/plugin marketplace`（外掛分發機制）≠ 組織自建的內部外掛 marketplace，見 [[claude-code-plugins-guide]]
- 作者推論（非官方）：Anthropic 藉此把企業 AI 預算留在自家生態系

## OpenAI Marketplace：同型 commitment 抵用（2026-10-08 補充）

- 來源：aiposthub〈OpenAI Marketplace 與 AWS Bedrock Managed Agents：企業 AI 生態系完整解析〉（2026-09-30，引 DevDay 2026 官方回顧）。⚠️ 本條原始來源也是 aiposthub，**同一作者不算兩個獨立來源**，「兩家收斂到同一模式」的觀察同樣待非 aiposthub 來源佐證
- 對象：符合資格的 Enterprise 客戶，可把既有 OpenAI commitments 用於核准的第三方軟體；首波 32 家夥伴（官方轉述）
- 🔴 夥伴名單、抵用比例與期限**原文都沒寫**
- 「核准上架 ≠ OpenAI 背書」屬作者觀點

## 相關連結

- [[claude-code-plugins-guide]] — Claude Code 外掛與 `/plugin marketplace`；含外掛提交入口與三種 marketplace 名詞釐清
- [[agent-plugins-open-standard]] — 外掛開放標準（plugin.json＋MCP servers＋Agent Skills）
- [[ideas/openai-devday-2026-chatgpt-platform-updates]]
- [[ideas/openai-agents-api-public-beta]] — Bedrock Managed Agents 部分
