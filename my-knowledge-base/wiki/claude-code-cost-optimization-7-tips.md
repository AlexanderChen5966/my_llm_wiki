---
title: Claude Code 7 招省錢技巧：精簡上下文為核心的成本優化
date: 2026-07-20
tags: [ai/claude, ai/workflow]
source: https://moksaweb.com/claude-code-cost-optimization/
type: article
status: processed
asset_value: 62
---

Claude Code 的 token 成本與上下文大小成正比——Claude 每次處理的上下文越大，付的錢越多。因此所有省錢策略都圍繞同一個目標：**讓上下文盡可能精簡**。平均每位開發者每天約花 6 美元，但若不控管 token（尤其開 Extended Thinking、Agent Teams、掛多個 MCP），費用會快速飆升。

## T（核心論點）

省錢的核心不是「永遠用最便宜的模型」，而是**主動管理上下文**。Claude Code 已內建自動優化（Prompt Caching、Auto-compaction），但主動管理能再降超過一半費用。最有效的三招：以 Sonnet 取代 Opus 當預設、任務切換用 `/clear` 清空上下文、CLAUDE.md 瘦身至 200 行內。

## B（適用場景）

**適用：**
- 個人開發者日常開發（預設 Sonnet，複雜架構才切 Opus）
- CI/CD 自動化管道（用 `--max-budget-usd` 設費用上限最關鍵）
- Agent Teams 協作（token 消耗約為一般 session 的 7 倍，務必控管）

**不適用 / 注意：**
- Opus 在複雜推理明顯優於 Sonnet，用錯模型反覆修正反而更貴——模型切換需要判斷力
- Pro/Max 訂閱者費用已含月費，`/cost` 不代表額外收費，應改用 `/stats` 看使用模式（但省 token 仍能在同配額內做更多事）
- Prompt Caching 是自動的，不需手動設定

## R（7 招操作技巧）

### 1. 模型選擇（最直接有效）
日常用 Sonnet、簡單搜尋探索用 Haiku、只有複雜架構決策才用 Opus。`/model` 可 session 中途切換；子代理可設 `model: haiku` 跑便宜任務。

### 2. 上下文管理三指令
- `/clear`：切換任務時清空上下文（避免舊對話持續耗 token）
- `/compact`：手動觸發壓縮，可加自訂指令指定保留哪些重點
- `/context`：診斷目前上下文各項目的 token 消耗

### 3. Extended Thinking 預算控制
用 `/effort` 調降思考級別，或設環境變數 `MAX_THINKING_TOKENS=8000`。預設 thinking budget 可能每次請求耗數萬 token（以 output token 費率計）。

### 4. PreToolUse Hook 過濾冗長輸出（最有效進階技巧）
在工具執行前過濾輸出，讓 Claude 只看到精簡結果，可把數萬 token 降到數百：

```bash
# ~/.claude/hooks/filter-test-output.sh
input=$(cat)
cmd=$(echo "$input" | jq -r '.tool_input.command')
if [[ "$cmd" =~ ^(npm\ test|pytest|go\ test|cargo\ test|jest|vitest) ]]; then
  filtered_cmd="$cmd 2>&1 | grep -A 5 -E '(FAIL|ERROR|error:|FAILED|panic:)' | head -100"
  echo "{\"hookSpecificOutput\":{\"hookEventName\":\"PreToolUse\",\"permissionDecision\":\"allow\",\"updatedInput\":{\"command\":\"$filtered_cmd\"}}}"
else
  echo "{}"
fi
```

### 5. CI/CD 費用防護
每個 job 設 `--max-budget-usd` 費用上限 + `--max-turns` 限制輪數 + `--bare` 跳過本地設定（hooks/MCP/plugins）+ 只掃變更檔案而非整個 codebase。

### 6. CLAUDE.md 瘦身與 Skills 遷移
CLAUDE.md 每次 session 啟動都載入上下文，太長持續耗 token。把特定工作流的詳細指令遷移到 Skills 按需載入；400 行瘦身到 200 行內，每 session 約省 2000 token。

### 7. 工具優化：MCP vs CLI
MCP 預設 Tool Search 延遲載入，只有工具名進上下文；不常用的 MCP 用 `/mcp` 停用。能用 CLI（`gh`、`aws`）解決的優先用 CLI，因為 CLI 不增加工具描述的上下文開銷。

### 費用追蹤指令
- `/cost`：API 使用者查當前 session token 用量與費用
- `/stats`：Pro/Max 訂閱者查使用模式

## C（關鍵行動）

- [ ] 把 `settings.json` 的 `model` 設為 `sonnet`、`thinkingTokenBudget` 設 8000 當團隊預設
- [ ] 建立 PreToolUse hook 過濾測試輸出（省 token 最有感的一招）
- [ ] 所有 CI 腳本加上 `--max-budget-usd` 與 `--max-turns`
- [ ] 養成具體 prompt 習慣：「在 auth.ts 的 login 函式加輸入驗證」而非「改善這個程式碼庫」（vague prompt 是最大隱形開銷）

## 原始摘要

Moksa 觀點：曾一個下午花近 20 美元（Extended Thinking 最高 + 五個 MCP），學會這些技巧後同樣工作量費用降超過一半。要注意背景 token 消耗（閒置時對話摘要等，通常每 session < 0.04 美元屬正常）、Agent Teams 的 7 倍放大效應、Auto-compaction 壓縮後細節可能遺失（關鍵指令寫進 CLAUDE.md 會重新載入）。

## 相關連結

- [[synthesis/synthesis-llm-cost-routing]] — LLM 成本效率知識精粹（本主題 6 篇整合：上下文體積 × 任務路由兩大槓桿）
- [[deepclaude-cost-optimization]] — 換便宜後端（DeepSeek）的另一種省錢路線；本篇則是 Claude Code 官方管道內的原生省錢技巧
- [[llm-cost-optimization-strategies]] — 通用 LLM 成本優化四層策略（本篇為 Claude Code 專屬落地版）
- [[claude-code-context-management]] — Context 管控全攻略（`/clear` 不是清空、四大消耗源）
- [[claude-code-auto-memory]] — Auto Memory 機制；🔑 `MEMORY.md` 索引常駐 context，超過 200 行會靜默截斷
- [[claude-md-optimization-techniques]] — CLAUDE.md 瘦身技巧（第 6 招的方法論來源）
- [[github-copilot-token-reduction]] — 另一 CLI 工具的 token 削減四法對照
