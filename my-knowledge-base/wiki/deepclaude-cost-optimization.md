---
title: deepclaude：替 Claude Code 換便宜後端，月費從 NT$7,200 降到 NT$416
date: 2026-06-15
tags: [ai/claude]
source: https://vocus.cc/article/6a1576d4fd897800014a887e
type: article
status: processed
asset_value: 58
---

deepclaude 是一個 shell script，不改 Claude Code 的工作流，只把 API 後端換成便宜的選項（如 DeepSeek V4 Pro）。它解決的不是「AI 能力不夠」，而是「token-based 計費悄悄累積」的問題。

## T（核心論點）

**成本分層，不是便宜替代品**：把 Claude 留給真正需要它的時候，日常大量消耗 token 的任務交給比較便宜但夠用的後端。

原理：在啟動 Claude Code 前，設定幾個環境變數：
- `ANTHROPIC_BASE_URL` → 換成 DeepSeek 或其他供應商的 endpoint
- `ANTHROPIC_AUTH_TOKEN` → 換成對應的 API key
- `DEFAULT_MODEL` / `SUBAGENT_MODEL` → 指定模型

Claude Code 的 agent loop、bash executor、tool dispatch 全部不動，只有背後的模型不一樣。

## B（適用場景）

**適合：**
- 每天用 Claude Code，API 月費已超過 NT$1,600（$50）的人
- Indie hacker、學生、自由工作者需要精算工具費
- 大多數任務是日常 coding，不是每次都需要最高階推理

**不需要：**
- Claude Code 用量很低（月帳單 < NT$640）
- 工作內容高度敏感，公司規定只能用特定供應商
- 金融交易、安全漏洞分析等高風險任務

## R（完整內容）

### 費用差距

| 項目 | Anthropic Opus 級 | DeepSeek V4 Pro | 差距 |
|------|-------------------|-----------------|------|
| Input（每百萬 tokens） | NT$96 | NT$14 | 6.8x |
| Output（每百萬 tokens） | NT$480 | NT$28 | 17.2x |
| 每月估算（每日 0.5M output）| NT$7,200 | NT$416 | 94%↓ |

⚠️ 透過 OpenRouter 等中間平台，實際差距約 3-5x（不是 17x），但仍相當顯著。

### 支援的後端選項

| 後端 | 延遲 | 適合場景 |
|------|------|---------|
| DeepSeek V4 Pro | 150-300ms | 日常 coding、重構 |
| OpenRouter | 中等 | 便宜且彈性的日常使用 |
| Fireworks AI | 較快 | 互動式 debugging |
| Anthropic | 50-100ms | 關鍵決策、深度推理 |

### 使用方式

```bash
# GitHub: aattaran/deepclaude
deepclaude          # 使用預設便宜後端啟動
deepclaude --switch anthropic  # session 中切回 Anthropic
deepclaude --benchmark         # 測試不同後端延遲
deepclaude --cost              # 查看費用估算
```

### 建議工作流

- **一般修改**（改小 bug、整理型別、補測試）→ 便宜後端
- **卡住的 bug** → 換較強模型試一次
- **架構決策** → Anthropic 或你最信任的模型
- **成本快爆** → 先看帳單，不要假裝沒看到

### 注意事項

- DeepSeek 伺服器位置可能造成較高延遲
- Claude Code 的 extended thinking 不一定能完整對應
- 少數 tool calling 邊緣案例可能有相容性問題
- 公司合規要求永遠優先於個人省錢偏好

### AI 成本節省五件套

deepclaude 是五件套的第一件（門檻最低）：
1. **deepclaude**：換後端，最快見效
2. **ds4**：本地跑（需要硬體）
3. **headroom**：壓縮 context，tool-heavy 任務省 47-92% token
4. **agentmemory**：避免每 session 重燒上下文 token
5. **opensquilla**：自動路由選「夠用且便宜」的模型

## C（關鍵行動）

- [ ] 先到 Anthropic Console 看最近三個月用量，確認是否有「換後端」的必要
- [ ] 如果月費有感，試用 `aattaran/deepclaude`（MIT 開源）
- [ ] 記得到 console.anthropic.com 設 monthly spending limit（預設無上限）

## 相關連結

- [[synthesis/synthesis-llm-cost-routing]] — LLM 成本效率知識精粹（本主題 6 篇整合：上下文體積 × 任務路由兩大槓桿）
- [[claude-code-billing-usage]] — Claude Code 計費說明（API Key 陷阱、雙層限速）
- [[llm-cost-optimization-strategies]] — LLM 成本優化四層策略（Prompt Caching / 路由 / Semantic Cache）
- [[agentmemory]] — agentmemory：跨 session 持久記憶（五件套第四件）
- [[dual-tool-strategy]] — 雙工具策略：Gemini CLI 探索 + Claude Code 執行
- [[claude-code-cost-optimization-7-tips]] — Claude Code 官方管道內 7 招省錢技巧（不換後端，用 /clear、PreToolUse hook、--max-budget-usd 精簡上下文）
