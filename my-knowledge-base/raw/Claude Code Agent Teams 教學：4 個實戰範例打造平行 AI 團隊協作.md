---
title: "Claude Code Agent Teams 教學：4 個實戰範例打造平行 AI 團隊協作"
source: "https://moksaweb.com/claude-code-agent-teams/"
author:
  - "[[Moksa]]"
published: 2026-04-20
created: 2026-09-02
description: "Claude Code Agent Teams 多代理團隊完整教學，從 Agent 定義檔建立、角色分工設計、worktree 隔離平行工作到任務自動分配與結果整合，附企業級多 Agent 協作的架構範例。"
tags:
  - "clippings"
status: processed
---
## 1\. 什麼是 Claude Code Agent Teams？

Claude Code Agent Teams 讓你同時協調多個 Claude Code 實例作為一個團隊工作。一個 session 擔任團隊領導（lead），負責協調工作、分配任務和彙整結果；其他 session 擔任隊友（teammates），各自在獨立的上下文視窗中執行任務，並且可以互相直接溝通。這和子代理（subagents）最大的差別是——子代理只能向主代理回報結果，不同子代理之間無法對話；而 Claude Code Agent Teams 的隊友可以互相發訊息、質疑彼此的發現、自主認領待處理的任務。

Claude Code Agent Teams 目前是實驗性功能，預設關閉，需要設定環境變數才能啟用。它最適合需要平行探索且隊友之間需要溝通的複雜任務——例如多角度的程式碼審查、競爭假說的除錯調查、以及跨層（前端、後端、測試）的功能開發。

> Moksa 觀點：第一次用 Claude Code Agent Teams 做 PR 審查時，我指派了三個隊友分別看安全性、效能和測試覆蓋率。以前一個 Claude 做審查，容易只注意到某一類問題就停了；三個隊友同時看，每個人專注自己的面向，最後彙整出來的報告比單一審查全面太多了。最讓我印象深刻的是「對抗式假說」模式——讓隊友互相質疑彼此的發現，存活下來的結論可信度高很多。但要注意 token 成本，大約是一般 session 的 7 倍，建議用 Sonnet 模型。

![Youtube video](https://i.ytimg.com/vi/XSZP9GhhuAc/hqdefault.jpg)

Prompting for Agents | Code w/ Claude

## 2\. Claude Code Agent Teams 有哪些核心功能？

Claude Code Agent Teams 由四個核心元件組成：

| 元件 | 說明 |
| --- | --- |
| Team Lead | 主 session，負責建立團隊、產生隊友、分配任務和彙整結果 |
| Teammates | 獨立的 Claude Code 實例，各自在自己的上下文視窗中工作 |
| Task List | 共享的任務清單，隊友可以認領和完成任務，支援任務依賴 |
| Mailbox | 訊息系統，讓代理之間可以互相溝通 |

Claude Code Agent Teams 支援兩種顯示模式。In-process 模式是預設值，所有隊友在同一個終端機裡執行，用 Shift+Down 切換隊友。Split-pane 模式讓每個隊友有自己的獨立面板，需要 tmux 或 iTerm2。預設的 auto 模式會自動偵測——如果你已經在 tmux session 中就用 split-pane，否則用 in-process。

和子代理的主要差異在於：Claude Code Agent Teams 的隊友可以互相發送訊息和廣播、共享任務清單上的進度、自主認領待處理的工作、以及透過 hook 機制強制品質把關。子代理只能向主代理回報摘要，彼此之間完全隔離。

## 3\. Claude Code Agent Teams 適合哪些使用場景？

Claude Code Agent Teams 最適合以下需要平行探索的場景：

- **多角度程式碼審查** ：Claude Code Agent Teams 可以指派三到五個隊友，分別專注安全性、效能、測試覆蓋率、程式碼風格等不同面向，最後由 lead 彙整所有發現。比單一審查員更全面。
- **競爭假說除錯** ：當 bug 的根本原因不明確時，Claude Code Agent Teams 讓多個隊友各自調查不同假說，互相質疑彼此的理論。存活下來的結論可信度更高，避免單一調查者的錨定偏差。
- **跨層功能開發** ：前端、後端、測試各由一個隊友負責，平行開發需要同步修改的功能。每個隊友擁有不同的檔案，避免互相覆蓋。
- **新模組平行建置** ：Claude Code Agent Teams 適合需要同時建立多個獨立模組的場景。每個隊友負責一個模組，完成後由 lead 整合。
- **技術研究與評估** ：Claude Code Agent Teams 的多個隊友同時研究不同的技術方案或函式庫，各自產出評估報告，最後由 lead 做出決策。比依序研究快得多。
- **大規模重構** ：Claude Code Agent Teams 可以把重構任務拆分成互不依賴的檔案組，每個隊友負責一組。Claude Code Agent Teams 的任務依賴系統確保有順序要求的任務不會亂序執行。
![Youtube video](https://i.ytimg.com/vi/uhJJgc-0iTQ/hqdefault.jpg)

Building More Effective AI Agents

## 4\. 如何設定 Claude Code Agent Teams？

### 啟用與基本設定

```
# Enable Agent Teams in settings.json
# Agent Teams are experimental and disabled by default
# Add this to your .claude/settings.json or ~/.claude/settings.json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}

# Or set as environment variable before starting Claude Code
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
claude

# Configure display mode in ~/.claude.json (global config)
# Options: "auto" (default), "in-process", "tmux"
{
  "teammateMode": "in-process"
}

# Override display mode for a single session via CLI flag
claude --teammate-mode in-process
claude --teammate-mode tmux

# Display mode details:
# "auto"       — uses split panes if inside tmux, otherwise in-process
# "in-process" — all teammates in one terminal, Shift+Down to cycle
# "tmux"       — each teammate in its own pane, requires tmux or iTerm2

# Requirements:
# - Claude Code v2.1.32 or later (check with: claude --version)
# - tmux or iTerm2 with it2 CLI (only for split-pane mode)
# - Recommended: Sonnet model for teammates (cost-effective)
```

### 團隊管理指令

Claude Code Agent Teams 的所有操作都透過自然語言向 lead 下達指令：

```
# Creating and managing agent teams — all commands are natural language
# Type these directly in the Claude Code lead session

# === Create a Team ===

# Basic team creation with task description
Create an agent team to review PR #142. Spawn three reviewers:
one focused on security, one on performance, one on test coverage.

# Specify model for teammates (cost optimization)
Create a team with 4 teammates using Sonnet to refactor these modules in parallel.

# Require plan approval before implementation
Spawn an architect teammate to refactor the auth module.
Require plan approval before they make any changes.

# === Communicate with Teammates ===

# In-process mode navigation:
# Shift+Down  — cycle through teammates
# Enter       — view a teammate's session
# Escape      — interrupt teammate's current turn
# Ctrl+T      — toggle task list visibility

# Send message to specific teammate
Tell the security reviewer to also check for CSRF vulnerabilities

# Direct interaction: use Shift+Down to select a teammate, then type

# === Task Management ===

# Check task status
What's the status of all tasks?

# Reassign work
Move the API endpoint task from the backend teammate to the frontend teammate

# Add new tasks
Add a task to write integration tests for the new auth endpoints

# === Shutdown and Cleanup ===

# Graceful teammate shutdown
Ask the researcher teammate to shut down

# Wait for all work to finish
Wait for your teammates to complete their tasks before proceeding

# Clean up team resources (always do this via the lead)
Clean up the team

# Note: always shut down all teammates before running cleanup
# Teammates must finish their current tool call before shutting down
```

### 品質把關 Hooks

Claude Code Agent Teams 支援三種 hook 事件來強制品質把關：

```
// settings.json — Quality gate hooks for Agent Teams
// These hooks enforce rules when teammates complete work
{
  "hooks": {
    // TeammateIdle: runs when a teammate is about to go idle
    // Exit code 2 sends feedback and keeps the teammate working
    "TeammateIdle": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/check-teammate-idle.sh"
          }
        ]
      }
    ],

    // TaskCompleted: runs when a task is being marked complete
    // Exit code 2 prevents completion and sends feedback
    "TaskCompleted": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/validate-task-completion.sh"
          }
        ]
      }
    ],

    // TaskCreated: runs when a task is being created
    // Exit code 2 prevents creation and sends feedback
    "TaskCreated": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/validate-task-creation.sh"
          }
        ]
      }
    ]
  }
}
```

## 5\. Claude Code Agent Teams 實戰範例怎麼用？

以下提供四個完整的 Claude Code Agent Teams 實戰範例。

![Youtube video](https://i.ytimg.com/vi/iF9iV4xponk/hqdefault.jpg)

The Future of Agentic Coding with Claude Code

### 範例一：多角度 PR 審查

```
# Multi-perspective PR review with Agent Teams
# Type this in the Claude Code lead session after enabling agent teams

# Step 1: Create the review team with specialized roles
Create an agent team to review PR #142. Spawn three reviewers:
- "security-reviewer": focused on security vulnerabilities, authentication
  bypass, injection attacks, and hardcoded secrets. Use Sonnet model.
- "performance-reviewer": focused on performance impact, N+1 queries,
  unnecessary loops, missing caching, and memory leaks. Use Sonnet model.
- "test-reviewer": focused on test coverage gaps, missing edge cases,
  flaky test patterns, and assertion quality. Use Sonnet model.

Have each reviewer examine the PR diff independently, then share their
findings with the team. After all three finish, synthesize a consolidated
review report organized by severity (Critical, High, Medium, Low).

# Step 2: Monitor progress
# Use Shift+Down to cycle through teammates and check their work
# Or ask the lead:
What's the status of each reviewer?

# Step 3: Add follow-up instructions if needed
Tell the security reviewer to also check the new API endpoints
for rate limiting and CORS configuration.

# Step 4: Get consolidated results
Synthesize all findings into a single review comment for the PR.
Group by severity and include file:line references.

# Step 5: Clean up
Ask all teammates to shut down, then clean up the team.

# Expected output: a comprehensive PR review covering security,
# performance, and testing from three independent perspectives.
# Token cost: approximately 3x a single review session.
```

### 範例二：競爭假說除錯

這是 Claude Code Agent Teams 最獨特的使用模式。讓隊友互相質疑彼此的發現，像科學辯論一樣找出最可靠的結論：

```
# Adversarial hypothesis debugging with Agent Teams
# This pattern leverages inter-teammate communication for higher accuracy

# Step 1: Describe the bug and spawn investigators
Users report the app exits after one message instead of staying connected.
Spawn 4 agent teammates to investigate different hypotheses:

- "websocket-investigator": hypothesis is a WebSocket connection handling bug
- "auth-investigator": hypothesis is an authentication token expiry issue
- "state-investigator": hypothesis is a state management race condition
- "infra-investigator": hypothesis is a server-side timeout or load balancer issue

Have them talk to each other to try to disprove each other's theories,
like a scientific debate. Each investigator should:
1. Gather evidence supporting their hypothesis
2. Share evidence with the team
3. Actively try to disprove other investigators' theories
4. Update their confidence level based on counter-evidence

After 15 minutes of investigation, synthesize findings and identify
the most likely root cause based on surviving evidence.

# Why this works:
# - Sequential investigation suffers from anchoring bias
# - Once one theory is explored, subsequent investigation is biased toward it
# - Multiple independent investigators with active disproval reduces false positives
# - The theory that survives adversarial scrutiny is more likely correct

# Step 2: Let the debate play out, then ask for consensus
What consensus has emerged? Which hypothesis survived the debate?

# Step 3: Implement the fix based on the winning hypothesis
Based on the investigation, implement the fix and add a regression test.
```

### 範例三：跨層功能開發

```
# Cross-layer feature development with Agent Teams
# Each teammate owns a different layer to avoid file conflicts

# Step 1: Spawn specialized teammates for each layer
Create an agent team to implement the new notification system:

- "backend-dev": implements the notification API endpoints and database models
  in src/api/ and src/models/. Use Sonnet model.
  Key tasks: REST endpoints, WebSocket event handlers, database schema migration.

- "frontend-dev": implements the notification UI components and state management
  in src/components/ and src/stores/. Use Sonnet model.
  Key tasks: notification bell component, dropdown panel, real-time updates.

- "test-dev": writes comprehensive tests for both backend and frontend
  in tests/. Use Sonnet model. Wait for backend and frontend to finish
  their initial implementation before writing integration tests.
  Key tasks: API endpoint tests, component tests, E2E notification flow test.

Require plan approval for backend-dev and frontend-dev before they
start implementing. I want to review their approach first.

# Step 2: Review and approve plans
# The lead will present each teammate's plan for your approval
# Approve or reject with feedback

# Step 3: Monitor parallel implementation
# backend-dev and frontend-dev work simultaneously on different files
# test-dev waits for dependencies (task dependency system handles this)

# Step 4: Integration
Once all teammates finish, run the full test suite to verify
the notification system works end-to-end.

# File ownership prevents conflicts:
# backend-dev  → src/api/notifications/, src/models/notification.ts
# frontend-dev → src/components/notifications/, src/stores/notification.ts
# test-dev     → tests/notifications/
```

### 範例四：使用子代理定義的團隊

使用 Claude Code Agent Teams 可以引用已定義的子代理類型作為隊友角色，讓你重複使用標準化的角色設定：

```
# Using subagent definitions as teammate roles
# First, create reusable agent definitions in .claude/agents/

# File: .claude/agents/security-reviewer.md
---
name: security-reviewer
description: Reviews code for security vulnerabilities. Use after code changes.
tools: Read, Grep, Glob
model: sonnet
color: red
---

Analyze code for security vulnerabilities including injection attacks,
authentication bypass, hardcoded secrets, and insecure dependencies.
Rate each finding by severity (Critical, High, Medium, Low).
Always check OWASP Top 10 categories.

# File: .claude/agents/perf-analyzer.md
---
name: perf-analyzer
description: Analyzes code for performance bottlenecks. Use for optimization.
tools: Read, Grep, Glob, Bash
model: sonnet
color: yellow
---

Identify performance issues including N+1 queries, unnecessary loops,
missing database indexes, memory leaks, and missing caching opportunities.
Run benchmarks when possible to quantify impact.

# Then reference these definitions when spawning teammates:
Spawn a teammate using the security-reviewer agent type to audit the auth module.
Spawn a teammate using the perf-analyzer agent type to profile the API endpoints.

# Benefits of using subagent definitions:
# - Reusable: same role definition for both subagents and team teammates
# - Consistent: tools and model settings applied automatically
# - Team-aware: coordination tools (SendMessage, task management) always available
# Note: skills and mcpServers from the definition are NOT applied to teammates
# Teammates load skills and MCP servers from project/user settings instead
```

以下腳本展示如何在 CI 中自動建立 Claude Code Agent Teams 進行平行審查，以及搭配的 TeammateIdle hook 確保品質：

```
#!/bin/bash
# agent-team-pr-review.sh
# Automated multi-perspective PR review using Claude Code Agent Teams
# Usage: ./agent-team-pr-review.sh 

set -euo pipefail

PR_NUMBER=${1:?"Usage: $0 "}
REVIEW_TIMEOUT=600  # 10 minutes maximum review time

echo "=== Agent Team PR Review ==="
echo "PR: #$PR_NUMBER"
echo "Timeout: ${REVIEW_TIMEOUT}s"
echo ""

# Ensure agent teams are enabled for this session
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1

# Build the review prompt with structured team roles
REVIEW_PROMPT=$(cat << 'EOF'
Create an agent team to review PR #PRNUM with three specialized reviewers:

Team structure:
- "security": Review for security vulnerabilities including injection attacks,
  authentication bypass, authorization flaws, hardcoded secrets, and CSRF.
  Check OWASP Top 10 categories. Use Sonnet model.

- "quality": Review for code quality including naming conventions, function
  complexity, error handling completeness, logging adequacy, and adherence
  to project coding standards from CLAUDE.md. Use Sonnet model.

- "testing": Review for test coverage gaps, missing edge cases, flaky test
  patterns, and assertion quality. Verify that all new public functions
  have corresponding test cases. Use Sonnet model.

Instructions for each reviewer:
1. Read the PR diff and understand the change scope
2. Apply your specialized review criteria systematically
3. Share findings with other teammates for cross-validation
4. Rate each finding: Critical, High, Medium, or Low severity
5. Include file path and line number for each finding

After all reviews complete, synthesize a consolidated report with:
- Executive summary (3 bullet points maximum)
- Findings grouped by severity
- Specific code suggestions for Critical and High items
- Overall recommendation: approve, request changes, or needs discussion

Output the final report as structured markdown.
EOF
)

# Replace PR number placeholder
REVIEW_PROMPT="${REVIEW_PROMPT//PRNUM/$PR_NUMBER}"

# Run the review with timeout and budget controls
timeout "$REVIEW_TIMEOUT" claude --bare \
  -p "$REVIEW_PROMPT" \
  --allowedTools "Read,Grep,Glob,Agent" \
  --max-budget-usd 3.00 \
  --max-turns 30 \
  --output-format json > /tmp/team-review-result.json 2>/dev/null

# Extract results
REVIEW=$(jq -r '.result // "Review incomplete"' /tmp/team-review-result.json)
COST=$(jq -r '.cost_usd // "unknown"' /tmp/team-review-result.json)

echo "Review completed. Cost: \$$COST"
echo ""
echo "$REVIEW"

# Post review to PR as comment if gh CLI is available
if command -v gh &> /dev/null; then
  gh pr comment "$PR_NUMBER" --body "## Agent Team Code Review

$REVIEW

---
*Reviewed by Claude Code Agent Teams (3 reviewers: security, quality, testing)*
*Review cost: \$$COST*"
  echo "Review posted to PR #$PR_NUMBER"
fi

# Cost breakdown for different team sizes:
# 3 reviewers (recommended): approximately $1.50-3.00 per review
# 5 reviewers (thorough): approximately $2.50-5.00 per review
# Single reviewer baseline: approximately $0.50-1.00 per review
#
# Token usage scales linearly with number of teammates
# Each teammate maintains its own independent context window
# Use Sonnet model for all teammates to minimize cost
# Set --max-budget-usd to prevent runaway costs
#
# Recommended team configurations by PR size:
# Small PR (under 100 lines changed): single reviewer, no team needed
# Medium PR (100-500 lines): 3 reviewers (security, quality, testing)
# Large PR (500+ lines): 4-5 reviewers with domain-specific roles
# Critical PR (auth, payment, data): 3 reviewers + plan approval required
#
# Integration with GitHub Actions:
# Add this script to .github/workflows/pr-review.yml
# Trigger on: pull_request (opened, synchronize)
# Set ANTHROPIC_API_KEY from repository secrets
# Set CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1 in env
# Use timeout-minutes: 15 for the review job
```
```
#!/bin/bash
# validate-teammate-idle.sh — TeammateIdle hook script
# Checks if a teammate has unfinished tasks before allowing idle state
# Exit code 2 sends feedback and keeps the teammate working

INPUT=$(cat)
TEAMMATE_NAME=$(echo "$INPUT" | jq -r '.teammate_name // empty')
TASK_STATUS=$(echo "$INPUT" | jq -r '.task_status // empty')

# Check if there are still pending tasks assigned to this teammate
PENDING_COUNT=$(echo "$INPUT" | jq -r '.pending_tasks_count // 0')

if [ "$PENDING_COUNT" -gt 0 ]; then
  echo "Teammate $TEAMMATE_NAME still has $PENDING_COUNT pending tasks. Continue working." >&2
  exit 2  # Keep the teammate working
fi

# Check if the teammate produced any deliverables
DELIVERABLES=$(echo "$INPUT" | jq -r '.deliverables_count // 0')
if [ "$DELIVERABLES" -eq 0 ]; then
  echo "Teammate $TEAMMATE_NAME completed tasks but produced no deliverables. Please summarize findings." >&2
  exit 2  # Request summary before going idle
fi

# Allow idle if all tasks done and deliverables produced
exit 0
```
```
#!/bin/bash
# validate-task-completion.sh — TaskCompleted hook script
# Ensures tasks meet quality standards before they can be marked complete
# Exit code 0: allow completion
# Exit code 2: reject completion with feedback message

INPUT=$(cat)
TASK_ID=$(echo "$INPUT" | jq -r '.task_id // empty')
TASK_TITLE=$(echo "$INPUT" | jq -r '.task_title // empty')
TEAMMATE_NAME=$(echo "$INPUT" | jq -r '.teammate_name // empty')

echo "Validating task completion: $TASK_TITLE (by $TEAMMATE_NAME)" >&2

# Check 1: Verify the teammate actually made file changes
CHANGED_FILES=$(git diff --name-only HEAD 2>/dev/null | wc -l)
if [ "$CHANGED_FILES" -eq 0 ]; then
  # Allow completion for research tasks that produce no file changes
  if echo "$TASK_TITLE" | grep -qiE '(research|investigate|review|analyze)'; then
    exit 0
  fi
  echo "No file changes detected. Implementation tasks must produce code changes." >&2
  exit 2
fi

# Check 2: Verify no syntax errors in changed files
for file in $(git diff --name-only HEAD 2>/dev/null); do
  if [[ "$file" == *.ts ]] || [[ "$file" == *.tsx ]]; then
    if ! npx tsc --noEmit "$file" 2>/dev/null; then
      echo "TypeScript errors found in $file. Fix before marking complete." >&2
      exit 2
    fi
  fi
  if [[ "$file" == *.py ]]; then
    if ! python3 -m py_compile "$file" 2>/dev/null; then
      echo "Python syntax error in $file. Fix before marking complete." >&2
      exit 2
    fi
  fi
done

# Check 3: Run quick lint check on changed files
if command -v npx &>/dev/null && [ -f ".eslintrc.json" ] || [ -f "eslint.config.js" ]; then
  LINT_ERRORS=$(git diff --name-only HEAD 2>/dev/null | grep -E '\.(ts|tsx|js|jsx)$' | \
    xargs npx eslint --quiet 2>/dev/null | grep -c "error" || true)
  if [ "$LINT_ERRORS" -gt 0 ]; then
    echo "Found $LINT_ERRORS lint errors in changed files. Fix before marking complete." >&2
    exit 2
  fi
fi

echo "Task validation passed: $TASK_TITLE" >&2
exit 0
```

## 6\. 使用 Claude Code Agent Teams 要注意什麼？

Claude Code Agent Teams 是實驗性功能，預設關閉。在正式使用前需要了解以下限制和已知問題：

- **Token 成本大幅增加** ：使用 Claude Code Agent Teams 時每個隊友都有獨立的上下文視窗，token 消耗與隊友數量成正比。大約是一般 session 的 7 倍（使用 plan mode 時）。建議用 Sonnet 模型而非 Opus，保持團隊精簡（3-5 人），任務完成後及時清理。
- **不支援 session 恢復** ：Claude Code Agent Teams 重啟後 in-process 模式的隊友無法恢復。如果 lead 嘗試向已不存在的隊友發訊息，告訴它產生新隊友。
- **任務狀態可能有延遲** ：隊友有時沒有正確標記任務完成，導致依賴任務被卡住。手動檢查工作是否完成，必要時更新狀態或讓 lead 提醒隊友。
- **關閉過程可能較慢** ：Claude Code Agent Teams 的隊友會等當前的工具呼叫或請求完成才關閉。
- **每個 session 只能一個團隊** ：限制每個 lead 只管理一個團隊。清理當前團隊後才能建立新的。
- **隊友不能巢狀建立團隊** ：只有 lead 可以管理團隊，隊友不能自己再建子團隊。
- **權限在產生時設定** ：所有隊友繼承 lead 的權限模式。產生後可以個別調整，但無法在產生時預設不同權限。
- **避免檔案衝突** ：Claude Code Agent Teams 中兩個隊友同時編輯同一個檔案會互相覆蓋。務必讓每個隊友負責不同的檔案組。

## 如何建立 Claude Code Agent Teams？

### 定義 Agent 角色

在.claude/agents/ 資料夾建立多個 agent 定義檔，各自負責不同領域。

### 設定協作模式

使用 /batch 或 worktree isolation 讓多個 agent 平行工作互不干擾。

### 分派任務

給出複雜任務描述，讓主 agent 自動分配子任務給 team 成員。

### 整合結果

各 agent 完成後，主 agent 彙整結果並合併修改到主分支。

## 7\. 哪裡可以找到更多 Claude Code Agent Teams 資源？

- [Moksa Claude Code 完整教學系列](https://moksaweb.com/category/claude-code/)
- [Claude Code Agent Teams 官方文件](https://docs.anthropic.com/en/docs/claude-code/agent-teams)
- [Anthropic 官方網站](https://www.anthropic.com/)

## 8\. Claude Code Agent Teams 常見問題 FAQ

### Claude Code Agent Teams 和子代理有什麼差別？

子代理在單一 session 內運作，完成任務後只能向主代理回報結果摘要，不同子代理之間無法直接溝通。Agent Teams 中每個隊友是獨立的 Claude Code 實例，擁有共享的任務清單，可以互相發送訊息、質疑彼此的發現、並自主認領待處理的任務。子代理適合專注且自足的任務，Agent Teams 適合需要協作和討論的複雜平行工作。

### Claude Code Agent Teams 的 token 成本大約是多少？

大約是一般 session 的 7 倍（使用 plan mode 時），因為每個隊友都有獨立的上下文視窗。建議用 Sonnet 模型而非 Opus 來降低成本，保持團隊規模在 3-5 人，每個隊友 5-6 個任務，任務完成後及時清理。

### Claude Code Agent Teams 怎麼啟用？

在 settings.json 中加入 env 欄位設定 CLAUDE\_CODE\_EXPERIMENTAL\_AGENT\_TEAMS 為 1，或在啟動前設定環境變數 export CLAUDE\_CODE\_EXPERIMENTAL\_AGENT\_TEAMS=1。需要 Claude Code v2.1.32 或更新版本。

### 隊友之間怎麼避免檔案衝突？

把工作分配成不重疊的檔案組，每個隊友只負責自己的目錄。例如前端隊友只改 src/components/，後端隊友只改 src/api/，測試隊友只改 tests/。兩個隊友同時編輯同一個檔案會互相覆蓋，這是目前的已知限制。

### 可以直接和隊友對話嗎？

可以。在 in-process 模式中用 Shift+Down 切換到目標隊友，然後直接打字發送訊息。在 split-pane 模式中直接點擊隊友的面板。每個隊友都是完整的 Claude Code session，你可以給它額外指令、問問題或調整方向。

### Claude Code Agent Teams 支援哪些顯示模式？

支援三種：auto（預設，自動偵測環境）、in-process（所有隊友在同一終端，用 Shift+Down 切換）、tmux（每個隊友獨立面板，需要 tmux 或 iTerm2）。在 settings 中設定 teammateMode 或用 –teammate-mode 旗標指定。VS Code 整合終端不支援 split-pane 模式。