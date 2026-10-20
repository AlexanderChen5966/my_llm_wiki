---
title: "Claude Code Cost Optimization 教學：7 個省錢技巧讓每月費用降低一半"
source: "https://moksaweb.com/claude-code-cost-optimization/"
author:
  - "[[Moksa]]"
published: 2026-04-13
created: 2026-07-20
description: "Claude Code Cost Optimization 完整教學，涵蓋模型選擇、上下文管理、Extended Thinking 優化、PreToolUse Hook 過濾、團隊費用管理、CI/CD 預算防護，附 6 個可直接複製的實戰範例。"
tags:
  - "clippings"
status: processed
---
## 1\. 什麼是 Claude Code Cost Optimization？

Claude Code Cost Optimization是每個使用 Claude Code 的開發者都需要掌握的實戰知識。根據官方數據，平均每位開發者每天的花費約 6 美元，90% 的使用者每日花費低於 12 美元。以 Sonnet 4.6 為主力模型計算，每月每位開發者大約 100 到 200 美元。這個數字看起來合理，但如果不注意 token 消耗，費用可能會快速飆升——特別是在使用 Extended Thinking、Agent Teams 或大量 MCP 工具的情況下。

Claude Code Cost Optimization的核心概念很簡單：token 成本和上下文大小成正比。Claude 每次處理的上下文越大，你付的錢就越多。所以所有省錢策略都圍繞著一個目標——讓上下文盡可能精簡。Claude Code 本身已經內建了自動優化機制（Prompt Caching 和 Auto-compaction），但主動管理上下文可以進一步大幅降低成本。

> Moksa 觀點：剛開始用 Claude Code 時，我完全不在意 token 消耗。直到有一天用 /cost 一看，才發現一個下午就花了快 20 美元——原因是我開了 Extended Thinking 最高設定，又同時掛了五個 MCP 伺服器。後來學會了這篇 Claude Code Cost Optimization的技巧後，同樣的工作量費用降了超過一半。最有效的三招是：任務切換時用 /clear 清空上下文、日常工作用 Sonnet 而不是 Opus、以及把冗長的測試輸出用 hook 過濾後再餵給 Claude。

![Youtube video](https://i.ytimg.com/vi/CBneTpXF1CQ/hqdefault.jpg)

Claude Code Updates: Haiku 4.5, Claude Code on Web, and More

## 2\. Claude Code Cost Optimization 有哪些核心功能？

Claude Code Cost Optimization涵蓋五個面向：費用追蹤、模型選擇、上下文管理、工具優化和團隊管理。每個面向都有對應的指令和設定可以調整。

費用追蹤方面，API 使用者可以用 `/cost` 指令即時查看當前 session 的 token 消耗和費用明細。Pro 和 Max 訂閱者因為費用已包含在月費中，可以改用 `/stats` 查看使用模式。你也可以在狀態列設定中開啟上下文視窗使用量的持續顯示，隨時掌握目前消耗了多少 token。

Claude Code Cost Optimization的模型選擇是最直接有效的省錢方式。Sonnet 能處理大部分程式碼工作，成本比 Opus 低得多。用 `/model` 指令可以在 session 中途切換模型。子代理還可以設定用 Haiku 這種更便宜的模型來執行簡單的搜尋和探索任務。

在上下文管理方面， `/clear` 可以在切換任務時清空上下文，避免不相關的舊對話持續消耗 token。 `/compact` 可以手動觸發上下文壓縮，加上自訂指令可以告訴 Claude 壓縮時要保留哪些重要資訊。Extended Thinking 的 token 預算也可以透過 `/effort` 或環境變數 `MAX_THINKING_TOKENS` 來調整。

Claude Code Cost Optimization在工具優化方面，MCP 工具預設使用延遲載入（Tool Search），只有 Claude 實際需要的工具才會進入上下文。但如果有不常用的 MCP 伺服器，建議用 `/mcp` 停用它們。能用 CLI 工具（如 `gh` 、 `aws` ）解決的任務，優先用 CLI 而不是 MCP，因為 CLI 不會增加工具描述的上下文開銷。

## 3\. Claude Code Cost Optimization 適合哪些使用場景？

以下是最常見需要用到 Claude Code Cost Optimization的場景：

- **個人開發者日常開發** ：Claude Code Cost Optimization建議用 Sonnet 作為預設模型，只在遇到複雜架構決策時切換 Opus。每次切換不同任務前用 `/clear` 清空上下文。把 CLAUDE.md 控制在 200 行以內。
- **團隊導入初期** ：Claude Code Cost Optimization建議先用小規模試點團隊建立使用模式，再推廣到全組織。在 Console 設定工作空間的花費上限，監控每位開發者的日均消耗。
- **CI/CD 自動化管道** ：Claude Code Cost Optimization在自動化場景中特別重要。使用 `--max-budget-usd` 設定每次執行的費用上限，搭配 `--max-turns` 限制執行輪數，防止失控任務產生意外費用。
- **大型程式碼庫分析** ：Claude Code Cost Optimization推薦安裝 Code Intelligence Plugin 讓 Claude 用精確的符號導航取代暴力搜尋，減少不必要的檔案讀取。把詳細的分析任務委派給子代理，只讓摘要回到主對話。
- **Agent Teams 協作** ：Claude Code Cost Optimization建議 Agent Teams 使用 Sonnet 模型、保持團隊規模精簡、任務完成後及時清理。Agent Teams 的 token 消耗大約是一般 session 的 7 倍。
- **Extended Thinking 深度推理** ：預設的 thinking budget 可能每次請求消耗數萬個 token。用 `/effort` 調低級別，或設定 `MAX_THINKING_TOKENS=8000` 限制預算，簡單任務不需要深度推理。
![Youtube video](https://i.ytimg.com/vi/BER3EhUIyz0/hqdefault.jpg)

Claude Plays Catan: Managing Agent Context

## 4\. 如何設定 Claude Code Cost Optimization？

Claude Code Cost Optimization的所有可調整參數和指令整理如下，涵蓋費用追蹤、模型切換、上下文管理和 token 限制：

| 指令/設定 | 說明 | 省錢效果 |
| --- | --- | --- |
| `/cost` | 顯示當前 session 的 token 用量和費用（API 使用者） | 追蹤監控 |
| `/stats` | 顯示使用模式統計（Pro/Max 訂閱者） | 追蹤監控 |
| `/model` | 切換模型（Opus/Sonnet/Haiku） | 高（Sonnet 比 Opus 便宜數倍） |
| `/effort` | 調整 Extended Thinking 努力級別 | 高（降低 thinking token） |
| `/clear` | 清空上下文，切換任務時使用 | 高（減少每次請求的 token） |
| `/compact` | 手動觸發上下文壓縮 | 中（保留重點、壓縮冗餘） |
| `/context` | 查看目前上下文中各項目的 token 消耗 | 診斷分析 |
| `/mcp` | 管理 MCP 伺服器，停用不需要的 | 中（減少工具描述的 token） |
| `--max-budget-usd` | 設定 Headless 模式的費用上限 | 防護（避免失控費用） |
| `--max-turns` | 限制 agent loop 最大輪數 | 防護（避免無限迴圈） |
| `MAX_THINKING_TOKENS` | 環境變數，限制 thinking token 預算 | 高（直接減少 output token） |

### 方案比較

實踐 Claude Code Cost Optimization的第一步是選擇適合的方案。以下是三種主要使用方式的費用比較：

| 方案 | 月費 | 適合對象 | 計費方式 |
| --- | --- | --- | --- |
| Pro | $20/月 | 個人輕度使用 | 固定月費含用量 |
| Max | $100/月 或 $200/月 | 個人重度使用 | 固定月費含更高用量 |
| API（按量計費） | 依用量 | 團隊和自動化 | 按 token 計費 |

對於團隊使用 API 的情況，官方建議的 TPM（每分鐘 token 數）配額依團隊規模遞減：1-5 人建議每人 20-30 萬、5-20 人建議 10-15 萬、20-50 人建議 5-7.5 萬。團隊越大，同時使用的比例越低，所以人均配額可以降低。

## 5\. Claude Code Cost Optimization 實戰範例怎麼用？

以下提供五個完整的 Claude Code Cost Optimization實戰範例，從日常操作習慣到進階的自動化成本控制。

![Youtube video](https://i.ytimg.com/vi/fHWFF_pnqDk/hqdefault.jpg)

Vibe Coding in Prod | Code w/ Claude

### 範例一：日常開發的省錢工作流程

```
#!/bin/bash
# Daily cost-optimized Claude Code workflow
# These commands show the recommended sequence for cost-effective daily use

# 1. Start session and check current model
claude
# Inside session:
# /model              — verify you're on Sonnet (not Opus)
# /cost               — check starting baseline (should be near $0)
# /context            — see what's consuming context tokens

# 2. Work on Task A (e.g., implement a feature)
# Give specific prompts to minimize unnecessary file reads:
# "Add input validation to the login function in src/auth/login.ts"
# NOT: "improve the authentication module"

# 3. Before switching to Task B, clear context
# /rename "feature-auth-validation"   — save session for later reference
# /clear                              — start fresh, remove all stale context

# 4. Work on Task B (e.g., fix a bug)
# "Fix the null pointer error in src/api/orders.ts line 45"

# 5. For complex architectural decisions, temporarily switch to Opus
# /model opus
# "Design the database schema for the new notification system"
# /model sonnet        — switch back after the decision is made

# 6. Check costs at end of day
# /cost                — review total spend
# Target: under $6/day for average workload

# 7. Optional: reduce thinking budget for simple tasks
# /effort low          — less thinking tokens for straightforward edits
# /effort high         — restore for complex reasoning tasks
```

### 範例二：PreToolUse Hook 過濾冗長輸出

這是 Claude Code Cost Optimization中最有效的進階技巧之一。用 hook 在工具執行前過濾輸出，讓 Claude 只看到精簡的結果：

```
#!/bin/bash
# ~/.claude/hooks/filter-test-output.sh
# PreToolUse hook that filters verbose test output to show only failures
# This can reduce context from tens of thousands of tokens to hundreds

input=$(cat)
cmd=$(echo "$input" | jq -r '.tool_input.command')

# Check if the command is running tests
if [[ "$cmd" =~ ^(npm\ test|pytest|go\ test|cargo\ test|jest|vitest) ]]; then
  # Modify command to filter output: show only failures and errors
  # Limit to first 100 lines to prevent context explosion
  filtered_cmd="$cmd 2>&1 | grep -A 5 -E '(FAIL|ERROR|error:|FAILED|panic:)' | head -100"

  # Return modified command with permission to execute
  echo "{\"hookSpecificOutput\":{\"hookEventName\":\"PreToolUse\",\"permissionDecision\":\"allow\",\"updatedInput\":{\"command\":\"$filtered_cmd\"}}}"
else
  # For non-test commands, pass through unchanged
  echo "{}"
fi
```

搭配的 settings.json 設定：

```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/filter-test-output.sh"
          }
        ]
      }
    ]
  }
}
```

### 範例三：團隊費用管理腳本

這個 Claude Code Cost Optimization的腳本幫助團隊管理員監控和控制整體 API 花費：

```
#!/bin/bash
# team-cost-monitor.sh — Monitor Claude Code API costs for your team
# Requires: ANTHROPIC_API_KEY with admin permissions

set -euo pipefail

echo "=== Claude Code Team Cost Monitor ==="
echo "Date: $(date '+%Y-%m-%d %H:%M:%S')"
echo ""

# Configuration
DAILY_BUDGET_PER_DEV=12.00          # Maximum daily spend per developer
MONTHLY_BUDGET_TOTAL=5000.00         # Total monthly budget for the team
ALERT_THRESHOLD=0.80                 # Alert when 80% of budget consumed
TEAM_SIZE=25                         # Number of developers using Claude Code

# Calculate recommended rate limits based on team size
if [ "$TEAM_SIZE" -le 5 ]; then
  TPM_PER_USER=250000
  RPM_PER_USER=6
elif [ "$TEAM_SIZE" -le 20 ]; then
  TPM_PER_USER=125000
  RPM_PER_USER=3
elif [ "$TEAM_SIZE" -le 50 ]; then
  TPM_PER_USER=62500
  RPM_PER_USER=1.5
elif [ "$TEAM_SIZE" -le 100 ]; then
  TPM_PER_USER=30000
  RPM_PER_USER=0.75
else
  TPM_PER_USER=15000
  RPM_PER_USER=0.3
fi

TOTAL_TPM=$((TPM_PER_USER * TEAM_SIZE))

echo "Team Configuration:"
echo "  Team size: $TEAM_SIZE developers"
echo "  Daily budget per developer: \$$DAILY_BUDGET_PER_DEV"
echo "  Monthly budget total: \$$MONTHLY_BUDGET_TOTAL"
echo "  Recommended TPM per user: $(printf '%d' $TPM_PER_USER)"
echo "  Recommended RPM per user: $RPM_PER_USER"
echo "  Total TPM allocation: $(printf '%d' $TOTAL_TPM)"
echo ""

# Cost optimization recommendations based on team size
echo "Cost Optimization Recommendations:"
echo "  1. Default model: Sonnet 4.6 (best cost/performance ratio)"
echo "  2. Set workspace spend limit in Console: \$$MONTHLY_BUDGET_TOTAL"
echo "  3. Configure per-developer daily alerts at: \$$DAILY_BUDGET_PER_DEV"
echo "  4. Use --max-budget-usd in all CI/CD scripts"
echo "  5. Install code intelligence plugins to reduce file reads"
echo "  6. Move detailed CLAUDE.md instructions to on-demand skills"
echo ""

# Estimated monthly cost breakdown
SONNET_COST_PER_DEV=150     # Average monthly cost with Sonnet
OPUS_COST_PER_DEV=450       # Average monthly cost with Opus
MIXED_COST_PER_DEV=200      # Average with 80% Sonnet, 20% Opus

echo "Estimated Monthly Costs:"
echo "  All Sonnet: \$$(( SONNET_COST_PER_DEV * TEAM_SIZE )) (\$$SONNET_COST_PER_DEV/dev)"
echo "  All Opus:   \$$(( OPUS_COST_PER_DEV * TEAM_SIZE )) (\$$OPUS_COST_PER_DEV/dev)"
echo "  Mixed 80/20: \$$(( MIXED_COST_PER_DEV * TEAM_SIZE )) (\$$MIXED_COST_PER_DEV/dev)"
echo ""
echo "Recommendation: Use Sonnet as default, Opus only for complex architecture tasks"
```

以下是一個完整的 Claude Code Cost Optimization settings.json 範本，整合了所有和費用相關的最佳設定：

```
{
  // Cost-optimized settings.json for Claude Code
  // Place in .claude/settings.json for team-wide defaults

  // Use Sonnet as default model (cheaper than Opus)
  // Developers can switch to Opus with /model when needed
  "model": "sonnet",

  // Reduce extended thinking token budget for routine tasks
  // Default can be tens of thousands of tokens per request
  // 8000 tokens is sufficient for most coding tasks
  "thinkingTokenBudget": 8000,

  // Enable auto-compaction to prevent context overflow
  // Custom compaction instructions preserve important context
  "compactInstructions": "Focus on code changes, test results, and error messages. Discard verbose logs and file listings.",

  // Default permission mode: acceptEdits reduces confirmation overhead
  // Less time waiting for confirmations = fewer idle token costs
  "defaultMode": "acceptEdits",

  // Environment variables for cost control
  "env": {
    // Limit thinking tokens globally
    "MAX_THINKING_TOKENS": "8000",
    // Increase MCP output limit only if needed (default: 25000)
    "MAX_MCP_OUTPUT_TOKENS": "25000",
    // Set MCP server connection timeout (default: 5000ms)
    "MCP_TIMEOUT": "8000"
  },

  // Permission rules to prevent accidental expensive operations
  "permissions": {
    "allow": [
      "Bash(npm test *)",
      "Bash(git *)",
      "Bash(* --version)",
      "Read",
      "Edit"
    ],
    "deny": [
      // Prevent running entire test suites without filters
      // (use PreToolUse hook to filter output instead)
      "Bash(npm run test:all)",
      // Prevent expensive operations
      "Bash(git push --force*)",
      "Bash(rm -rf *)"
    ]
  },

  // Hooks for cost optimization
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/filter-test-output.sh"
          }
        ]
      }
    ]
  }
}
```

### 範例四：子代理成本優化設定

Claude Code Cost Optimization中，善用子代理的模型路由可以大幅降低成本。把簡單的搜尋和探索任務路由到 Haiku 模型：

```
# .claude/agents/cheap-search.md — Cost-optimized search agent using Haiku
---
name: cheap-search
description: Fast file search and code exploration. Use when looking for files, functions, or patterns.
tools: Read, Grep, Glob
model: haiku
maxTurns: 10
color: green
---

You are a fast code search assistant. Your job is to find files, functions,
and code patterns as quickly as possible with minimal tool calls.

Rules for cost efficiency:
- Use Glob first to narrow down file candidates before reading
- Use Grep with specific patterns instead of reading entire files
- Return file paths and line numbers, not full file contents
- Limit results to the 5 most relevant matches
- Never read files larger than 500 lines without user confirmation
```
```
# .claude/agents/smart-reviewer.md — Cost-balanced code review agent
---
name: smart-reviewer
description: Review code for quality and security. Use after making code changes.
tools: Read, Grep, Glob
model: sonnet
maxTurns: 15
memory: project
color: blue
---

Review code changes for security vulnerabilities, code quality issues,
and best practices violations. Focus on:
1. Security: injection, XSS, hardcoded secrets, missing validation
2. Quality: naming, function length, complexity, error handling
3. Performance: obvious bottlenecks, unnecessary loops, missing indexes

Before reviewing, check your agent memory for known patterns in this codebase.
After reviewing, update memory with new patterns you discovered.
Keep your response concise: list only actionable findings with file and line references.
```

### 範例五：CLAUDE.md 瘦身與 Skills 遷移

把 CLAUDE.md 瘦身也是 Claude Code Cost Optimization的關鍵技巧。CLAUDE.md 的內容在每次 session 啟動時都會載入上下文，太長的檔案會持續消耗 token。把特定工作流程的詳細指令遷移到 Skills，只在需要時按需載入：

```
# BEFORE: bloated CLAUDE.md (400+ lines, loaded every session)
# This wastes tokens on instructions irrelevant to current task

## Build Commands
- npm install && npm run build && npm test ...

## PR Review Process (50 lines of detailed instructions)
...detailed PR review workflow...

## Database Migration Guide (80 lines)
...step-by-step migration instructions...

## Deployment Checklist (60 lines)
...deployment verification steps...

# ============================================

# AFTER: lean CLAUDE.md (under 200 lines) + on-demand skills
# Only essential instructions loaded at startup

## Build Commands
- pnpm install
- pnpm test
- pnpm build

## Code Standards
- TypeScript strict mode, 2-space indentation
- Named exports, async/await over .then()
- All functions must have explicit return types

## Architecture
- API routes: src/app/api/[resource]/route.ts
- Services: src/services/ (business logic)
- Repositories: src/repositories/ (data access)

## Detailed Workflows (moved to skills, loaded on demand)
# These are now invoked only when needed:
# /pr-review    — detailed PR review process
# /db-migrate   — database migration guide
# /deploy       — deployment checklist
# /incident     — incident response playbook

# Token savings: ~2000 tokens per session saved
# Over 20 sessions per day: ~40,000 tokens saved daily
# Monthly savings at Sonnet rates: approximately $15-20
```

以下 Python 腳本可以幫你分析和優化 CLAUDE.md 的 token 使用效率：

```
#!/usr/bin/env python3
"""
claude_md_optimizer.py — Analyze and optimize CLAUDE.md token efficiency
Reads all CLAUDE.md files in the project tree and reports token estimates,
identifies candidates for migration to skills, and detects duplicates.

Usage: python3 claude_md_optimizer.py [project_root]
"""

import os
import sys
from pathlib import Path
from dataclasses import dataclass, field

@dataclass
class FileAnalysis:
    """Analysis result for a single CLAUDE.md file."""
    path: str
    line_count: int = 0
    estimated_tokens: int = 0
    sections: list = field(default_factory=list)
    skill_candidates: list = field(default_factory=list)
    warnings: list = field(default_factory=list)

def estimate_tokens(text: str) -> int:
    """Rough token estimate: ~4 characters per token for mixed content."""
    return len(text) // 4

def analyze_claude_md(filepath: Path) -> FileAnalysis:
    """Analyze a single CLAUDE.md file for optimization opportunities."""
    result = FileAnalysis(path=str(filepath))

    try:
        content = filepath.read_text(encoding="utf-8")
    except Exception as e:
        result.warnings.append(f"Could not read file: {e}")
        return result

    lines = content.split("\n")
    result.line_count = len(lines)
    result.estimated_tokens = estimate_tokens(content)

    # Check line count against recommended maximum
    MAX_RECOMMENDED_LINES = 200
    if result.line_count > MAX_RECOMMENDED_LINES:
        result.warnings.append(
            f"File has {result.line_count} lines "
            f"(recommended max: {MAX_RECOMMENDED_LINES}). "
            f"Consider splitting into .claude/rules/ files."
        )

    # Identify sections and find skill migration candidates
    current_section = None
    section_start = 0
    section_lines = 0

    for i, line in enumerate(lines):
        if line.startswith("## ") or line.startswith("# "):
            if current_section and section_lines > 30:
                result.skill_candidates.append({
                    "section": current_section,
                    "start_line": section_start + 1,
                    "line_count": section_lines,
                    "estimated_tokens": estimate_tokens(
                        "\n".join(lines[section_start:i])
                    ),
                    "recommendation": "Move to skill for on-demand loading"
                })
            current_section = line.lstrip("#").strip()
            section_start = i
            section_lines = 0
            result.sections.append(current_section)
        section_lines += 1

    # Check last section
    if current_section and section_lines > 30:
        result.skill_candidates.append({
            "section": current_section,
            "start_line": section_start + 1,
            "line_count": section_lines,
            "estimated_tokens": estimate_tokens(
                "\n".join(lines[section_start:])
            ),
            "recommendation": "Move to skill for on-demand loading"
        })

    # Check for import statements
    import_count = sum(1 for line in lines if line.strip().startswith("@"))
    if import_count > 0:
        result.sections.append(f"({import_count} imports)")

    return result

def scan_project(root: Path) -> list:
    """Scan project tree for all CLAUDE.md and related files."""
    targets = [
        "CLAUDE.md", "CLAUDE.local.md",
        ".claude/CLAUDE.md", ".claude/settings.json"
    ]
    results = []

    # Check root level files
    for target in targets:
        filepath = root / target
        if filepath.exists():
            results.append(analyze_claude_md(filepath))

    # Check .claude/rules/ directory
    rules_dir = root / ".claude" / "rules"
    if rules_dir.exists():
        for md_file in sorted(rules_dir.rglob("*.md")):
            results.append(analyze_claude_md(md_file))

    # Check subdirectory CLAUDE.md files (lazy-loaded)
    for md_file in sorted(root.rglob("CLAUDE.md")):
        if md_file != root / "CLAUDE.md" and ".claude" not in str(md_file):
            analysis = analyze_claude_md(md_file)
            analysis.warnings.append("Subdirectory file: loaded on-demand only")
            results.append(analysis)

    return results

def print_report(results: list):
    """Print optimization report to stdout."""
    total_tokens = sum(r.estimated_tokens for r in results)
    total_lines = sum(r.line_count for r in results)

    print("=" * 60)
    print("  CLAUDE.md Token Optimization Report")
    print("=" * 60)
    print(f"  Total files analyzed: {len(results)}")
    print(f"  Total lines: {total_lines}")
    print(f"  Estimated total tokens: {total_tokens:,}")
    print(f"  Estimated cost per session: ${total_tokens * 0.000003:.4f}")
    print(f"  Estimated cost per month (20 sessions/day):")
    print(f"    ${total_tokens * 0.000003 * 20 * 30:.2f}")
    print()

    for result in results:
        print(f"  {result.path}")
        print(f"    Lines: {result.line_count} | "
              f"Tokens: ~{result.estimated_tokens:,}")

        for warning in result.warnings:
            print(f"    WARNING: {warning}")

        if result.skill_candidates:
            print(f"    Skill migration candidates:")
            for candidate in result.skill_candidates:
                print(f"      - '{candidate['section']}' "
                      f"({candidate['line_count']} lines, "
                      f"~{candidate['estimated_tokens']} tokens)")

    # Summary recommendations
    print()
    print("  Recommendations:")
    all_candidates = []
    for r in results:
        all_candidates.extend(r.skill_candidates)

    if all_candidates:
        saveable = sum(c["estimated_tokens"] for c in all_candidates)
        print(f"  - Move {len(all_candidates)} sections to skills")
        print(f"  - Potential token savings: ~{saveable:,} per session")
        print(f"  - Monthly savings estimate: "
              f"${saveable * 0.000003 * 20 * 30:.2f}")
    else:
        print("  - CLAUDE.md files are well optimized")

    if total_lines > 400:
        print(f"  - Total lines ({total_lines}) exceeds recommendation")
        print(f"  - Consider consolidating or removing outdated sections")

if __name__ == "__main__":
    project_root = Path(sys.argv[1]) if len(sys.argv) > 1 else Path(".")
    results = scan_project(project_root)

    if not results:
        print("No CLAUDE.md files found in project.")
        sys.exit(0)

    print_report(results)
```

### 範例六：CI/CD 費用防護完整設定

這個 Claude Code Cost Optimization的範例展示如何在 GitHub Actions 中設定完整的費用防護機制，確保自動化任務不會產生意外帳單：

```
# .github/workflows/claude-code-review.yml
# Cost-optimized Claude Code automation with budget guards
# This workflow demonstrates every cost protection mechanism available

name: Claude Code PR Review (Cost-Optimized)

on:
  pull_request:
    types: [opened, synchronize]
    branches: [main, develop]

env:
  ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
  # Global cost controls via environment variables
  MAX_THINKING_TOKENS: "4000"        # Limit extended thinking per request
  MAX_MCP_OUTPUT_TOKENS: "10000"     # Limit MCP tool output size
  MCP_TIMEOUT: "5000"               # 5 second timeout for MCP connections

permissions:
  contents: read
  pull-requests: write

jobs:
  # Job 1: Quick security scan (budget: $0.25, Sonnet model)
  security-scan:
    name: Security Scan
    runs-on: ubuntu-latest
    timeout-minutes: 5               # Hard timeout prevents runaway jobs
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Get changed files only (reduce scope to save tokens)
        id: changed
        run: |
          FILES=$(git diff --name-only origin/${{ github.base_ref }}..HEAD \
            -- '*.ts' '*.js' '*.py' | head -20)
          echo "files=$FILES" >> $GITHUB_OUTPUT
          echo "count=$(echo "$FILES" | wc -l)" >> $GITHUB_OUTPUT

      - name: Run focused security review
        if: steps.changed.outputs.count > 0
        run: |
          # Use --bare to skip all local config (hooks, MCP, plugins)
          # Use --max-budget-usd to cap API spend for this task
          # Use --max-turns to prevent infinite agent loops
          # Use Sonnet model via --model flag for cost efficiency
          echo "${{ steps.changed.outputs.files }}" | claude --bare \
            -p "Review these changed files for security vulnerabilities only.
                Check for: SQL injection, XSS, hardcoded secrets, missing validation.
                Be concise: list only confirmed issues with file:line references.
                Changed files: $(cat)" \
            --allowedTools "Read,Grep,Glob" \
            --max-budget-usd 0.25 \
            --max-turns 5 \
            --output-format json > /tmp/security-review.json

          # Parse result and post to PR only if issues found
          RESULT=$(jq -r '.result // empty' /tmp/security-review.json)
          COST=$(jq -r '.cost_usd // 0' /tmp/security-review.json)
          echo "Security scan cost: \$$COST"

          if [ -n "$RESULT" ] && ! echo "$RESULT" | grep -qi "no.*issues"; then
            echo "REVIEW=$RESULT" >> $GITHUB_ENV
            echo "HAS_ISSUES=true" >> $GITHUB_ENV
          fi

      - name: Post security findings to PR
        if: env.HAS_ISSUES == 'true'
        uses: actions/github-script@v7
        with:
          script: |
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body: \`## Security Review\n\n${process.env.REVIEW}\`
            });

  # Job 2: Code quality review (budget: $0.50, Sonnet model)
  quality-review:
    name: Code Quality
    runs-on: ubuntu-latest
    timeout-minutes: 10
    needs: security-scan             # Run after security scan passes
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run code quality review with session continuation
        run: |
          # Step 1: Initial analysis (budget: $0.30)
          ANALYSIS=$(git diff origin/${{ github.base_ref }}..HEAD | claude --bare \
            -p "Analyze this diff for code quality issues.
                Focus on: naming, complexity, error handling, test coverage gaps.
                Output as numbered list, max 10 items." \
            --allowedTools "Read,Grep,Glob" \
            --max-budget-usd 0.30 \
            --max-turns 8 \
            --output-format json)

          SESSION_ID=$(echo "$ANALYSIS" | jq -r '.session_id')
          COST1=$(echo "$ANALYSIS" | jq -r '.cost_usd // 0')

          # Step 2: Get improvement suggestions (budget: $0.20, reuse session)
          SUGGESTIONS=$(claude --bare \
            -p "For the top 3 most important issues, suggest specific fixes." \
            --resume "$SESSION_ID" \
            --max-budget-usd 0.20 \
            --max-turns 3 \
            --output-format json)

          COST2=$(echo "$SUGGESTIONS" | jq -r '.cost_usd // 0')
          echo "Quality review total cost: \$$COST1 + \$$COST2"

  # Job 3: Weekly cost report (scheduled, budget: $1.00)
  weekly-cost-report:
    name: Weekly Cost Report
    runs-on: ubuntu-latest
    timeout-minutes: 15
    if: github.event_name == 'schedule'  # Only run on cron trigger
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Generate cost optimization report
        run: |
          claude --bare \
            -p "Analyze the codebase and suggest ways to reduce Claude Code costs:
                1. Check CLAUDE.md size and suggest content to move to skills
                2. List MCP servers in .mcp.json and identify unused ones
                3. Check for overly broad permission rules that cause extra prompting
                4. Suggest hook-based output filtering for common verbose commands
                Save the report to cost-optimization-report.md" \
            --allowedTools "Read,Grep,Glob,Write" \
            --permission-mode acceptEdits \
            --max-budget-usd 1.00 \
            --max-turns 20
```

這個工作流程展示了 Claude Code Cost Optimization在 CI/CD 中的完整實踐：每個 job 有獨立的費用上限、用 `--bare` 避免載入不必要的設定、用 `--max-turns` 防止無限迴圈、只掃描變更的檔案而非整個程式碼庫。

## 6\. 使用 Claude Code Cost Optimization 要注意什麼？

在實踐 Claude Code Cost Optimization時，以下幾點需要特別注意：

- **背景 token 消耗** ：Claude Code Cost Optimization要注意，Claude Code 在閒置時也會消耗少量 token，包括對話摘要和指令處理等背景任務，通常每個 session 低於 0.04 美元。這不是 bug，是正常的系統行為。
- **Agent Teams 的 token 放大效應** ：Agent Teams 中每個成員都有獨立的上下文視窗，token 消耗大約是一般 session 的 7 倍。使用 Sonnet 模型、保持團隊精簡、任務完成後及時清理，是 Claude Code Cost Optimization中控制 Agent Teams 費用的關鍵。
- **Prompt Caching 是自動的** ：Claude Code Cost Optimization不需要手動設定快取，因為 Claude Code 已經內建 Prompt Caching 機制，重複的系統提示和上下文會自動快取以降低費用。你不需要手動設定，但了解它的存在有助於理解為什麼有時候費用比預期低。
- **Auto-compaction 會改變上下文** ：當對話接近上下文限制時，Claude Code 會自動壓縮歷史訊息。壓縮後某些細節可能遺失。如果某個指令在壓縮後消失了，把它寫進 CLAUDE.md（壓縮後會重新載入）。
- **/cost 只對 API 使用者有意義** ：Pro 和 Max 訂閱者的費用已包含在月費中， `/cost` 顯示的金額不代表額外收費。訂閱者應該用 `/stats` 查看使用模式。
- **模型切換需要判斷力** ：Claude Code Cost Optimization不是「永遠用最便宜的模型」。Opus 在複雜推理任務上的表現明顯優於 Sonnet，用錯模型導致反覆修正反而更貴。正確做法是日常用 Sonnet，遇到多步驟架構決策或困難 bug 才切 Opus。
- **vague prompt 是最大的隱形開銷** ：「改善這個程式碼庫」這種模糊請求會觸發大範圍的檔案掃描。「在 auth.ts 的 login 函式加入輸入驗證」這種具體請求讓 Claude 用最少的檔案讀取完成工作，是 Claude Code Cost Optimization中最基本也最重要的習慣。

## 如何降低 Claude Code 費用？

### 選擇合適模型

日常任務用 Sonnet，簡單任務用 Haiku，只有複雜推理才用 Opus。

### 控制 Context

定期用 /compact 壓縮對話，善用 Sub-agents 避免主 context 膨脹。

### 設定費用上限

在 print mode 使用 –max-budget-usd 參數，設定單次任務費用上限。

### 善用快取

利用 Prompt Caching 降低重複 context 的費用，CLAUDE.md 內容會自動快取。

## 7\. 哪裡可以找到更多 Claude Code Cost Optimization 資源？

- [Moksa Claude Code 完整教學系列](https://moksaweb.com/category/claude-code/)
- [Claude Code 費用管理官方文件](https://docs.anthropic.com/en/docs/claude-code/costs)
- [Anthropic 官方網站](https://www.anthropic.com/)

## 8\. Claude Code Cost Optimization 常見問題 FAQ

### Claude Code Cost Optimization中最有效的省錢方法是什麼？

最有效的三個方法是：用 Sonnet 取代 Opus 作為預設模型、任務切換時用 /clear 清空上下文、以及把 CLAUDE.md 從 400 行瘦身到 200 行以內。這三招可以讓日均費用從 12 美元降到 6 美元以下。

### Claude Code Cost Optimization怎麼控制 Extended Thinking 的費用？

Extended Thinking 的 token 是以 output token 費率計費，預設預算可能每次請求消耗數萬個 token。用 /effort 指令降低思考級別，或設定環境變數 MAX\_THINKING\_TOKENS=8000 來限制預算。簡單的程式碼修改不需要深度推理，降低 effort 不會影響品質。

### Pro/Max 訂閱者也需要省錢嗎？

訂閱者不需要擔心金錢費用，但用量是有限的。過度消耗 token 會更快觸及用量上限。同樣的省錢攻略（清空上下文、選對模型、精簡 CLAUDE.md）可以讓你在同樣的配額內完成更多工作。

### Agent Teams 的費用怎麼控制？

Agent Teams 的 token 消耗大約是一般 session 的 7 倍。建議用 Sonnet 模型（不是 Opus）、保持團隊規模最小化、spawn prompt 盡量精簡、任務完成後及時清理。預設情況下 Agent Teams 是停用的，需要設定環境變數才能啟用。

### 如何在 CI/CD 中防止費用失控？

在所有 CI 腳本中加上 –max-budget-usd 設定費用上限和 –max-turns 限制執行輪數。搭配 –bare 旗標避免載入不必要的 hooks 和 MCP 伺服器。建議每個 CI 任務使用獨立的 API key 方便追蹤個別任務的花費。

### MCP 伺服器會增加多少 token 成本？

MCP 工具預設使用 Tool Search 延遲載入機制，只有工具名稱進入上下文。Claude 實際使用某個工具時才載入完整描述。但如果連接了大量伺服器，建議用 /mcp 停用不常用的。能用 CLI 工具解決的任務，優先用 CLI 而不是 MCP。