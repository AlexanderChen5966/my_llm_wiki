---
title: "Claude Code Agent SDK 完整指南：Python 與 TypeScript 4 大核心功能教學"
source: "https://moksaweb.com/claude-code-agent-sdk/"
author:
  - "[[Moksa]]"
published: 2026-04-05
created: 2026-07-14
description: "Claude Code Agent SDK 完整教學，詳解 Python 和 TypeScript 雙語言 SDK 的安裝設定、Agent 類別建立、Tool Use 整合、Streaming 串流處理與多 Agent 協作的實戰範例。"
tags:
  - "clippings"
status: processed
---
## 1\. 什麼是 Claude Code Agent SDK？

Claude Code Agent SDK 是 Anthropic 推出的程式化介面（Programmatic Interface），讓你用 Python 或 TypeScript 把 Claude Code 的完整能力內嵌進自己的應用程式與自動化流程。這個 Claude Code Agent SDK 提供與 Claude Code CLI 相同的工具集、agent loop 機制、以及上下文管理能力，讓開發者可以用幾行程式碼建立一個能自主讀取檔案、執行 Bash 指令、搜尋網路、修改程式碼的 AI agent，無需手動管理複雜的 tool use response loop。

> Moksa 觀點：我原本以為 Agent SDK 只是把 Claude Code 包一層 API 而已，實際用過才發現它的設計重點是「把你從工具迴圈（tool loop）解放出來」。用 Anthropic 原始的 Client SDK，你要自己處理 tool\_use response、自己呼叫工具、自己把結果塞回去；Claude Code Agent SDK 直接幫你跑完這個迴圈，你只需要告訴它要做什麼、允許用哪些工具。對於要在 CI/CD 或自動化腳本裡跑的情境，這個差別非常明顯。

![Youtube video](https://i.ytimg.com/vi/8gTpgWru0Wg/hqdefault.jpg)

Building AI Agents with Claude in Amazon Bedrock

## 2\. Claude Code Agent SDK 有哪些核心功能？

Claude Code Agent SDK 的核心是一個非同步的 `query()` 函式，它接受一個 prompt 和選項物件，以串流方式回傳 agent 執行過程中產生的各種訊息（Messages）。它內建了完整的工具集（Built-in Tool Suite）——包括 Read、Write、Edit、Bash、Glob、Grep、WebSearch、WebFetch、AskUserQuestion——你的 agent 不需要額外實作工具的執行邏輯，只需宣告 `allowed_tools` 清單就好。

本 SDK 支援 Python（套件名 `claude-agent-sdk` ）和 TypeScript（套件名 `@anthropic-ai/claude-agent-sdk` ）兩種語言。除了 Anthropic 原生 API，還支援三個主要雲端平台作為 Backend Provider：Amazon Bedrock（設定 `CLAUDE_CODE_USE_BEDROCK=1` ）、Google Vertex AI（設定 `CLAUDE_CODE_USE_VERTEX=1` ）、以及 Microsoft Azure AI Foundry（設定 `CLAUDE_CODE_USE_FOUNDRY=1` ）。

值得注意的是，這個 SDK 原本叫做「Claude Code SDK」，在近期更名。如果你在舊文件或舊 repo 中看到 `ClaudeCodeOptions` 或 `claude-code` 套件名稱，這就是現在的 `ClaudeAgentOptions` 和 `claude-agent-sdk` 。它與 Anthropic 的一般 Client SDK（ `anthropic` 套件）不同——後者是原始的 API wrapper，前者是完整的 agentic execution framework。

| SDK 語言 | 安裝指令 | 支援功能 | 適合場景 |
| --- | --- | --- | --- |
| Python | pip install claude-agent-sdk | 完整功能 | 後端自動化、資料處理 |
| TypeScript | npm install @anthropic-ai/claude-agent-sdk | 完整功能 | Web 服務、Node.js 應用 |

## 3\. Claude Code Agent SDK 適合哪些使用場景？

Claude Code Agent SDK 最適合用在需要程式化控制（Programmatic Control）、批次處理（Batch Processing）或生產環境部署（Production Deployment）的場景：

- **CI/CD 自動化** ：在 GitHub Actions 或 Jenkins Pipeline 中，讓 Claude 自動審查 PR、執行 lint checks、修復常見的 code style 錯誤。每次有新 PR 就觸發 Claude Code Agent SDK 的 query() 任務，結果回傳後自動留言或更新 pull request status checks。
- **程式碼品質稽核（Automated Code Quality Audit）** ：定期掃描整個 repository，找出 TODO 標記、過時的 deprecated dependencies、未處理的 exception、以及 security vulnerabilities，生成 structured report 或直接透過 GitHub API 提 issues。只需要給 Read、Glob、Grep 三個工具就能完成。
- **多 Agent 協作系統（Multi-agent Orchestration）** ：一個主 orchestrator agent 負責規劃任務，透過 subagent 機制派遣多個 specialized agents 平行處理子任務（例如「code-reviewer」、「doc-generator」、「test-writer」），最後整合所有 agent 的結果。Claude Code Agent SDK 的 `agents` 欄位讓你定義整個 agent team。
- **有狀態的長期任務（Stateful Long-running Tasks）** ：利用 session resume 機制，讓多輪 query() 呼叫共享上下文。第一輪讓 Claude 分析整個 authentication module，第二輪接著問「找出所有呼叫這個模組的地方」，Claude 不需要重新讀檔就有完整的前後文。
- **帶有稽核日誌的生產環境（Production with Audit Trail）** ：透過 PreToolUse 和 PostToolUse hooks 攔截每個工具呼叫，在允許執行前做 validation 檢查，執行後寫入 structured audit logs。這讓 Claude Code Agent SDK 在 compliance-sensitive 的企業環境中也能安全部署。
![Youtube video](https://i.ytimg.com/vi/TUysIAtxyrQ/hqdefault.jpg)

Building AI Agents with Claude in Vertex AI

## 4\. 如何設定 Claude Code Agent SDK？

Claude Code Agent SDK 的主要設定介面是 `ClaudeAgentOptions` （Python）/ options object（TypeScript）。以下依功能類別詳細說明所有支援的設定參數：

### 工具控制（Tool Access Control）

- **`allowed_tools` （Python）/ `allowedTools` （TypeScript）** ：Tool allowlist，列出 Claude Code Agent SDK 中 agent 可以使用的工具清單。可用的 built-in tools： `Read` 、 `Write` 、 `Edit` 、 `Bash` 、 `Glob` 、 `Grep` 、 `WebSearch` 、 `WebFetch` 、 `AskUserQuestion` 、 `Agent` （啟用 subagent 功能，必須在此列出才能派生子 agent）。MCP server 工具格式為 `mcp__server_name__tool_name` 。
- **`permission_mode` （Python）/ `permissionMode` （TypeScript）** ：控制工具操作的確認行為。 `"acceptEdits"` 自動接受所有檔案編輯（Edit/Write）； `"bypassPermissions"` 跳過所有確認（僅限隔離環境）； `"default"` 為標準互動模式； `"plan"` 為唯讀分析模式。

### Session Management（會話管理）

- **`resume`** ：傳入先前 session 的 ID 字串，可在新的 `query()` 呼叫中恢復之前的上下文（Context Resumption）。Session ID 從 `SystemMessage` （ `subtype == "init"` ）中的 `data["session_id"]` 取得。這個功能讓 Claude Code Agent SDK 支援跨 query() 呼叫的 stateful multi-turn conversations。

### Hooks（生命週期回呼）

Claude Code Agent SDK 的 hooks 機制讓你在 agent 執行的關鍵節點插入自訂邏輯（Custom Logic）。可用的 hook events：

- `PreToolUse` ：每次工具呼叫前觸發。回傳 `{"decision": "block"}` 可以阻止執行，回傳 `{}` 允許繼續。
- `PostToolUse` ：每次工具呼叫後觸發，可以讀取工具的 input 和 output。
- `Stop` ：agent 完成所有 turns 後觸發。
- `SessionStart` ：新 session 開始時觸發。
- `SessionEnd` ：session 結束時觸發。
- `UserPromptSubmit` ：使用者提交 prompt 時觸發，可以攔截和修改 prompt content。

### Subagents（子 Agent）

- **`agents`** ：定義可呼叫的 subagents，key 是 agent 名稱，value 是 `AgentDefinition` 物件（含 `description` 、 `prompt` 、 `tools` 、 `model` ）。使用 subagent 功能時， `allowed_tools` 必須包含 `"Agent"` ，否則主 agent 無法派生子 agent。Claude Code Agent SDK 的 subagent 概念與 Claude Code CLI 的.claude/agents/ 定義完全一致。

### MCP Servers 與其他設定

- **`mcp_servers` （Python）/ `mcpServers` （TypeScript）** ：連接外部 MCP（Model Context Protocol）伺服器，key 是伺服器名稱，value 包含 `command` 、 `args` 、 `env` 。讓 Claude Code Agent SDK 中的 agent 使用資料庫、瀏覽器自動化、檔案系統、Slack、GitHub 等外部工具。
- **`setting_sources` （Python）/ `settingSources` （TypeScript）** ：設定 `["project"]` 啟用從專案設定檔讀取，包括 `CLAUDE.md` 、`.claude/commands/` （自訂指令）、`.claude/skills/` （skill 定義）。這讓 Claude Code Agent SDK 能共用 Claude Code CLI 環境的所有設定。
- **`model`** ：指定使用的 Claude model。可用 alias（ `sonnet` 、 `opus` 、 `haiku` ）或完整 model ID（ `claude-opus-4-6` 、 `claude-sonnet-4-6` ）。
- **`max_turns` （Python）/ `maxTurns` （TypeScript）** ：限制 agent 最多可執行幾輪 tool use，防止 runaway loops。

## 如何設定 Claude Code Agent SDK？

### 安裝 SDK

執行 pip install claude-agent-sdk（Python）或 npm install @anthropic-ai/claude-agent-sdk（TypeScript）。

### 設定 API Key

設定 ANTHROPIC\_API\_KEY 環境變數，或在程式碼中傳入 api\_key 參數。

### 建立 Agent

用 SDK 的 Agent 類別定義 system prompt、tools 和 model，建立自訂 agent 實例。

### 執行任務

呼叫 agent.run() 傳入任務描述，SDK 自動處理工具呼叫迴圈和結果回傳。

## 5\. Claude Code Agent SDK 實戰範例怎麼用？

以下提供五個完整的 Claude Code Agent SDK 實作範例，從基本安裝到進階的 multi-agent 系統。

![Youtube video](https://i.ytimg.com/vi/TqC1qOfiVcQ/hqdefault.jpg)

Claude Agent SDK Full Workshop

### 安裝 SDK（Installation）

```
# Python installation
pip install claude-agent-sdk

# TypeScript/Node.js installation
npm install @anthropic-ai/claude-agent-sdk

# Set up authentication
export ANTHROPIC_API_KEY=your-api-key

# For Amazon Bedrock backend
export CLAUDE_CODE_USE_BEDROCK=1
export AWS_ACCESS_KEY_ID=your-key
export AWS_SECRET_ACCESS_KEY=your-secret
export AWS_DEFAULT_REGION=us-east-1

# For Google Vertex AI backend
export CLAUDE_CODE_USE_VERTEX=1
export CLOUD_ML_REGION=us-central1
export ANTHROPIC_VERTEX_PROJECT_ID=your-project-id
```

### 範例一：基本的 Bug 修復 Agent（Python）

這是最簡單的 Claude Code Agent SDK 使用方式，展示如何用少量程式碼建立一個能自主修復 bugs 的 agent：

```
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions

async def fix_bugs(file_path: str) -> str:
    """Auto-fix bugs in the specified file using the agent SDK."""
    result_text = ""

    async for message in query(
        prompt=f"Find and fix all bugs in {file_path}. Explain each fix before making it.",
        options=ClaudeAgentOptions(
            allowed_tools=["Read", "Edit", "Bash"],
            permission_mode="acceptEdits",
            model="sonnet",
            max_turns=20,
        ),
    ):
        # Handle different message types from the query() stream
        if hasattr(message, "result"):
            result_text = message.result
        elif hasattr(message, "content"):
            # Print assistant messages as they stream
            for block in message.content:
                if hasattr(block, "text"):
                    print(block.text, end="", flush=True)
        elif hasattr(message, "subtype") and message.subtype == "init":
            print(f"Session started: {message.data.get('session_id', 'unknown')}")

    return result_text

if __name__ == "__main__":
    result = asyncio.run(fix_bugs("src/auth.py"))
    print(f"\n\nResult: {result}")
```

### 範例二：帶稽核日誌的生產環境 Agent（Python）

在生產環境部署 Claude Code Agent SDK 時，通常需要完整的 audit logging 和 permission control。這個範例展示如何使用 hooks 攔截每個工具呼叫並記錄稽核日誌：

```
import asyncio
import json
from datetime import datetime
from pathlib import Path
from claude_agent_sdk import query, ClaudeAgentOptions, HookMatcher

AUDIT_LOG = Path("./audit.log")
BLOCKED_COMMANDS = {"rm -rf", "sudo", "chmod 777", "git push --force"}

async def pre_bash_hook(input_data, tool_use_id, context):
    """Validate and log bash commands before execution."""
    command = input_data.get("tool_input", {}).get("command", "")

    # Check against blocked commands list
    for blocked in BLOCKED_COMMANDS:
        if blocked in command:
            log_entry = {
                "timestamp": datetime.now().isoformat(),
                "action": "BLOCKED",
                "tool": "Bash",
                "command": command,
                "reason": f"Command contains blocked pattern: {blocked}"
            }
            AUDIT_LOG.open("a").write(json.dumps(log_entry) + "\n")
            return {"decision": "block", "reason": f"Blocked: {blocked}"}

    # Log approved commands
    log_entry = {
        "timestamp": datetime.now().isoformat(),
        "action": "APPROVED",
        "tool": "Bash",
        "command": command
    }
    AUDIT_LOG.open("a").write(json.dumps(log_entry) + "\n")
    return {}  # Return empty dict to allow execution

async def post_edit_hook(input_data, tool_use_id, context):
    """Log all file modifications after they occur."""
    file_path = input_data.get("tool_input", {}).get("file_path", "unknown")
    log_entry = {
        "timestamp": datetime.now().isoformat(),
        "action": "FILE_MODIFIED",
        "tool": "Edit",
        "file_path": file_path
    }
    AUDIT_LOG.open("a").write(json.dumps(log_entry) + "\n")
    return {}

async def main():
    async for message in query(
        prompt="Refactor the authentication module for better security",
        options=ClaudeAgentOptions(
            allowed_tools=["Read", "Edit", "Bash", "Glob"],
            permission_mode="acceptEdits",
            hooks={
                "PreToolUse": [
                    HookMatcher(matcher="Bash", hooks=[pre_bash_hook])
                ],
                "PostToolUse": [
                    HookMatcher(matcher="Edit|Write", hooks=[post_edit_hook])
                ]
            }
        ),
    ):
        if hasattr(message, "result"):
            print(f"Completed: {message.result}")

asyncio.run(main())
```

### 範例三：Session Resume 跨輪對話（TypeScript）

```
import { query, ClaudeAgentOptions } from "@anthropic-ai/claude-agent-sdk";

async function analyzeCodebaseInMultipleTurns() {
  let sessionId: string | undefined;

  console.log("Turn 1: Analyzing authentication module...");

  // First turn: analyze the auth module
  for await (const message of query({
    prompt: "Read and analyze the authentication module. Understand its structure and dependencies.",
    options: {
      allowedTools: ["Read", "Glob", "Grep"],
      model: "sonnet"
    } as ClaudeAgentOptions
  })) {
    if (message.type === "system" && message.subtype === "init") {
      sessionId = (message as any).data?.session_id;
      console.log(\`Session ID: ${sessionId}\`);
    }
    if ("result" in message) {
      console.log("Analysis complete:", message.result);
    }
  }

  if (!sessionId) {
    throw new Error("Failed to capture session ID");
  }

  console.log("\nTurn 2: Finding all callers (using previous context)...");

  // Second turn: continue with full context from first turn
  for await (const message of query({
    prompt: "Find all places that call the authentication module. List file names and line numbers.",
    options: {
      resume: sessionId,  // Resume the previous session
      allowedTools: ["Grep", "Glob"]
    } as ClaudeAgentOptions
  })) {
    if ("result" in message) {
      console.log("Callers found:", message.result);
    }
  }

  console.log("\nTurn 3: Generate refactoring plan...");

  // Third turn: build on accumulated context
  for await (const message of query({
    prompt: "Based on your analysis, create a detailed refactoring plan to improve security.",
    options: {
      resume: sessionId,
      allowedTools: ["Read"]
    } as ClaudeAgentOptions
  })) {
    if ("result" in message) {
      console.log("Refactoring plan:", message.result);
    }
  }
}

analyzeCodebaseInMultipleTurns().catch(console.error);
```

### 範例四：Multi-agent 協作系統（Python）

這個範例展示 Claude Code Agent SDK 的 subagent 機制，建立一個 orchestrator agent 同時派遣多個 specialized subagents：

```
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions, AgentDefinition

async def run_comprehensive_code_review(target_file: str):
    """Run multi-agent comprehensive code review using specialized subagents."""

    async for message in query(
        prompt=f"""Review {target_file} comprehensively by using the available specialized agents:
        1. Use the security-reviewer agent to check for vulnerabilities
        2. Use the performance-analyzer agent to identify bottlenecks
        3. Use the test-generator agent to create unit tests
        4. Synthesize all findings into a final comprehensive report""",

        options=ClaudeAgentOptions(
            allowed_tools=["Read", "Grep", "Glob", "Agent"],  # Must include "Agent"
            permission_mode="acceptEdits",
            model="opus",
            agents={
                "security-reviewer": AgentDefinition(
                    description="Analyzes code for security vulnerabilities. Use proactively for security review.",
                    prompt="""You are a security expert. Analyze code for:
                    - SQL injection and XSS vulnerabilities
                    - Authentication and authorization flaws
                    - Hardcoded secrets and API keys
                    - Insecure cryptography usage
                    - Input validation gaps""",
                    tools=["Read", "Grep", "Glob"],
                    model="sonnet"
                ),
                "performance-analyzer": AgentDefinition(
                    description="Analyzes code for performance issues and bottlenecks.",
                    prompt="""You are a performance optimization expert. Identify:
                    - N+1 query problems
                    - Memory leaks and excessive allocations
                    - Unnecessary loops and inefficient algorithms
                    - Missing database indexes
                    - Synchronous blocking operations""",
                    tools=["Read", "Grep", "Glob"],
                    model="sonnet"
                ),
                "test-generator": AgentDefinition(
                    description="Generates comprehensive unit tests for code.",
                    prompt="""You are a testing expert. Create unit tests covering:
                    - Happy path scenarios
                    - Boundary conditions and edge cases
                    - Error handling and exception paths
                    - Mock external dependencies
                    - Parametrized test cases""",
                    tools=["Read", "Write", "Glob"],
                    model="sonnet"
                )
            }
        ),
    ):
        if hasattr(message, "result"):
            print(f"Review completed:\n{message.result}")
        elif hasattr(message, "content"):
            for block in message.content:
                if hasattr(block, "text") and block.text:
                    print(block.text, end="", flush=True)

asyncio.run(run_comprehensive_code_review("src/api/payment_processor.py"))
```

### 範例五：完整生產環境 Agent（Python + GitHub Actions）

以下是一個完整的 Claude Code Agent SDK 生產部署範例，包含 Python script 和配套的 GitHub Actions workflow。這個組合可以讓 Claude 自動審查每個 PR 並留下結構化評論：

```
# scripts/claude_review_agent.py
# Production-grade agent review script for CI/CD pipelines

import asyncio
import argparse
import json
import logging
import os
import sys
from datetime import datetime
from pathlib import Path
from typing import Optional, List, Dict, Any

from claude_agent_sdk import (
    query,
    ClaudeAgentOptions,
    AgentDefinition,
    HookMatcher,
)

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(name)s: %(message)s",
    handlers=[logging.StreamHandler(sys.stdout)],
)
logger = logging.getLogger("claude-review-agent")

REVIEW_PROMPT = """
You are an expert code reviewer analyzing a pull request.

Tasks to complete:
1. Read the changed files listed in CHANGED_FILES
2. Review each file for bugs, security issues, performance problems
3. Check for code style consistency and maintainability
4. Identify missing test coverage for new features
5. Generate a structured Markdown report with sections:
   - Summary (2-3 sentences)
   - Critical Issues (must fix before merge)
   - Warnings (should fix but not blocking)
   - Suggestions (optional improvements)
   - Test Coverage Assessment
   - Overall Score (1-10 with justification)

CHANGED_FILES:
{changed_files}

Output the complete Markdown report to a file named review_report.md
"""

SECURITY_PROMPT = """
Perform a focused security analysis on the changed files.

Check for OWASP Top 10 vulnerabilities:
- A01 Broken Access Control
- A02 Cryptographic Failures
- A03 Injection (SQL, XSS, Command)
- A05 Security Misconfiguration
- A06 Vulnerable and Outdated Components
- A07 Identification and Authentication Failures
- A09 Security Logging and Monitoring Failures

Also check for:
- Hardcoded credentials, API keys, passwords, tokens
- Unsafe deserialization patterns
- Insecure direct object references
- Missing input validation at API boundaries
- Unsafe file operations (path traversal, arbitrary writes)
- Race conditions in concurrent code
- Insecure random number generation

Output JSON to security_report.json with structure:
{
  "vulnerabilities": [
    {"severity": "critical|high|medium|low", "description": "...", "file": "...", "line": 0, "recommendation": "..."}
  ],
  "summary": "...",
  "risk_level": "critical|high|medium|low|minimal"
}

CHANGED_FILES:
{changed_files}
"""

async def run_code_review(
    changed_files: List[str],
    max_turns: int = 30,
    session_id: Optional[str] = None,
) -> Dict[str, Any]:
    """Execute automated code review via the query() agent loop."""

    audit_entries: List[Dict] = []
    captured_session_id: Optional[str] = None
    start_time = datetime.now()

    async def pre_tool_hook(input_data: Dict, tool_use_id: str, context: Any) -> Dict:
        tool_name = input_data.get("tool_name", "unknown")
        audit_entries.append({
            "ts": datetime.now().isoformat(),
            "event": "pre",
            "tool": tool_name,
            "id": tool_use_id,
        })
        # Block destructive operations in review context
        if tool_name == "Bash":
            cmd = input_data.get("tool_input", {}).get("command", "")
            blocked = ["rm ", "git push", "curl -X DELETE", "drop table", "truncate"]
            for pattern in blocked:
                if pattern.lower() in cmd.lower():
                    logger.warning(f"Blocked command: {cmd[:80]}")
                    return {"decision": "block", "reason": f"Pattern blocked: {pattern}"}
        return {}

    async def post_tool_hook(input_data: Dict, tool_use_id: str, context: Any) -> Dict:
        audit_entries.append({
            "ts": datetime.now().isoformat(),
            "event": "post",
            "tool": input_data.get("tool_name", "unknown"),
            "id": tool_use_id,
        })
        return {}

    options = ClaudeAgentOptions(
        allowed_tools=["Read", "Glob", "Grep", "Write"],
        permission_mode="acceptEdits",
        model="claude-sonnet-4-6",
        max_turns=max_turns,
        resume=session_id,
        hooks={
            "PreToolUse": [HookMatcher(matcher=".*", hooks=[pre_tool_hook])],
            "PostToolUse": [HookMatcher(matcher=".*", hooks=[post_tool_hook])],
        },
    )

    prompt = REVIEW_PROMPT.format(changed_files="\n".join(changed_files))
    final_result = ""

    async for message in query(prompt=prompt, options=options):
        if hasattr(message, "subtype") and message.subtype == "init":
            captured_session_id = message.data.get("session_id")
            logger.info(f"Session: {captured_session_id}")
        elif hasattr(message, "result"):
            final_result = message.result
        elif hasattr(message, "content"):
            for block in message.content:
                if hasattr(block, "text") and block.text:
                    print(block.text, end="", flush=True)

    elapsed = (datetime.now() - start_time).total_seconds()
    return {
        "result": final_result,
        "session_id": captured_session_id,
        "elapsed_seconds": elapsed,
        "tool_calls": len([e for e in audit_entries if e["event"] == "pre"]),
        "audit_log": audit_entries,
    }

async def run_security_scan(changed_files: List[str]) -> Dict[str, Any]:
    """Run security-focused vulnerability analysis via the agent loop."""

    options = ClaudeAgentOptions(
        allowed_tools=["Read", "Glob", "Grep", "Write"],
        permission_mode="acceptEdits",
        model="claude-opus-4-6",
        max_turns=20,
    )

    prompt = SECURITY_PROMPT.format(changed_files="\n".join(changed_files))

    async for message in query(prompt=prompt, options=options):
        if hasattr(message, "result"):
            return {"status": "completed", "result": message.result}

    return {"status": "completed", "result": ""}

def get_changed_files() -> List[str]:
    """Get list of changed files from git diff."""
    import subprocess
    result = subprocess.run(
        ["git", "diff", "--name-only", "HEAD~1", "HEAD"],
        capture_output=True, text=True
    )
    files = [f.strip() for f in result.stdout.strip().split("\n") if f.strip()]
    # Filter to source code files only
    extensions = {".py", ".ts", ".js", ".go", ".rs", ".java", ".rb", ".php", ".cs"}
    return [f for f in files if Path(f).suffix in extensions]

def main():
    parser = argparse.ArgumentParser(description="Automated Agent-Based Code Review Runner")
    parser.add_argument("--pr-number", type=int, help="Pull request number")
    parser.add_argument("--repo", help="GitHub repository (owner/repo)")
    parser.add_argument("--max-turns", type=int, default=30)
    parser.add_argument("--output-format", choices=["markdown", "json"], default="markdown")
    args = parser.parse_args()

    changed_files = get_changed_files()
    if not changed_files:
        logger.info("No source files changed, skipping review")
        sys.exit(0)

    logger.info(f"Reviewing {len(changed_files)} changed files: {changed_files}")

    result = asyncio.run(run_code_review(
        changed_files=changed_files,
        max_turns=args.max_turns,
    ))

    logger.info(f"Review completed in {result['elapsed_seconds']:.1f}s")
    logger.info(f"Tool calls executed: {result['tool_calls']}")

    with open("audit_log.json", "w") as f:
        json.dump(result["audit_log"], f, indent=2, ensure_ascii=False)

    sys.exit(0)

if __name__ == "__main__":
    main()
```
```
# .github/workflows/claude-agent-review.yml
# GitHub Actions workflow for automated agent-based PR review

name: Automated Agent PR Review

on:
  pull_request:
    types: [opened, synchronize, reopened]
    branches: [main, develop, release/*]
  workflow_dispatch:
    inputs:
      target_ref:
        description: "Git ref to analyze"
        required: false
        default: "HEAD"

permissions:
  contents: read
  pull-requests: write
  issues: write
  checks: write

jobs:
  code-review:
    name: Automated Code Review
    runs-on: ubuntu-latest
    timeout-minutes: 25

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 10

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"
          cache: pip

      - name: Install agent SDK and dependencies
        run: |
          pip install claude-agent-sdk anthropic requests

      - name: Run automated code review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          PR_NUMBER: ${{ github.event.pull_request.number }}
          REPO: ${{ github.repository }}
        run: |
          python scripts/claude_review_agent.py \
            --pr-number "$PR_NUMBER" \
            --repo "$REPO" \
            --max-turns 30 \
            --output-format markdown

      - name: Upload review artifacts
        uses: actions/upload-artifact@v4
        if: always()
        with:
          name: claude-review-artifacts
          path: |
            review_report.md
            audit_log.json
          retention-days: 14

      - name: Post review comment to PR
        if: github.event_name == 'pull_request' && always()
        uses: actions/github-script@v7
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const fs = require('fs');
            if (!fs.existsSync('review_report.md')) {
              console.log('No review report generated, skipping comment');
              return;
            }
            const report = fs.readFileSync('review_report.md', 'utf8');
            const body = [
              '## Automated Code Review Report',
              '',
              report,
              '',
              '---',
              '*Generated by [Claude Code Agent SDK](https://docs.anthropic.com/en/docs/claude-code/sdk)*'
            ].join('\n');

            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body: body,
            });

  security-scan:
    name: Security Analysis
    runs-on: ubuntu-latest
    needs: code-review
    if: github.event_name == 'pull_request'
    timeout-minutes: 20

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 5

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"

      - name: Install dependencies
        run: pip install claude-agent-sdk

      - name: Run Claude security scan
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          python scripts/claude_review_agent.py \
            --output-format json

      - name: Fail on critical vulnerabilities
        run: |
          python3 - <<'PYEOF'
          import json, sys
          try:
              with open("security_report.json") as f:
                  report = json.load(f)
              critical = [v for v in report.get("vulnerabilities", [])
                          if v.get("severity") == "critical"]
              if critical:
                  print(f"FAIL: {len(critical)} critical vulnerabilities found")
                  for v in critical:
                      print(f"  [{v['severity'].upper()}] {v['file']}: {v['description']}")
                  sys.exit(1)
              else:
                  risk = report.get("risk_level", "unknown")
                  print(f"PASS: No critical issues. Overall risk: {risk}")
          except FileNotFoundError:
              print("WARNING: security_report.json not found, skipping check")
          PYEOF
```

## 6\. 使用 Claude Code Agent SDK 要注意什麼？

使用 Claude Code Agent SDK 在生產環境部署時，以下幾點需要特別注意：

- **最小權限原則（Principle of Least Privilege）** ：在 Claude Code Agent SDK 的 `allowed_tools` 中只給真正需要的工具。唯讀分析任務只需要 Read、Glob、Grep，不要順手加上 Bash 和 Edit，避免 agent 在不預期的情況下修改檔案或執行系統指令。每個多餘的工具都增加了意外副作用的風險。
- **PreToolUse Hook 的回傳值決定是否繼續** ：Claude Code Agent SDK 的 PreToolUse hook 回傳 `{"decision": "block"}` 才會阻止工具執行，回傳空 dict `{}` 是允許繼續。如果 hook function 拋出 exception，行為取決於 error handling 設定，可能導致整個 agent 停止或靜默通過。在生產環境中，hook 函式必須有完整的 try/except 處理。
- **SDK 更名遷移注意事項** ：Claude Code Agent SDK 原名為 Claude Code SDK（套件名 `claude-code` ），選項類別也從 `ClaudeCodeOptions` 改為 `ClaudeAgentOptions` 。如果你是從舊版本遷移，需要更新 pip install 指令、npm install 指令、以及所有 import 語句。
- **Subagent 訊息的 parent\_tool\_use\_id 追蹤** ：在 Claude Code Agent SDK 中，subagent 產生的訊息帶有 `parent_tool_use_id` 欄位，可以用來辨識哪些訊息來自哪個子 agent 的執行。對於需要精細 structured logging 的生產環境很重要，建議在處理訊息時始終記錄這個欄位。
- **非 Anthropic 後端的認證方式不同** ：使用 Amazon Bedrock、Google Vertex AI 或 Microsoft Azure 作為 Claude Code Agent SDK 的後端時，不設定 `ANTHROPIC_API_KEY` ，而是透過各平台的 native authentication mechanism（AWS IAM credentials、gcloud Application Default Credentials、Azure Service Principal）認證，再加上對應的 environment variable flags 切換後端。
- **Message Type 完整處理** ：Claude Code Agent SDK 的 query() 會 yield 多種 message types，包括 AssistantMessage、ToolResultMessage、SystemMessage、ResultMessage。不同類型有不同的欄位結構，建議在生產環境中針對每種類型做完整的 type checking，而不是只用 hasattr 做模糊匹配。

## 7\. 哪裡可以找到更多 Claude Code Agent SDK 資源？

- [Moksa Claude Code 完整教學系列](https://moksaweb.com/category/claude-code/)
- [Claude Code Agent SDK 官方文件](https://docs.anthropic.com/en/docs/claude-code/sdk)
- [Claude Agent SDK GitHub Repository](https://github.com/anthropics/claude-agent-sdk)

## 8\. Claude Code Agent SDK 常見問題 FAQ

### Claude Code Agent SDK 和直接使用 Anthropic Client SDK 有什麼不同？

最大差異在於工具迴圈的處理方式。Anthropic Client SDK 是直接的 API wrapper，當 Claude 回應 tool\_use 時，你必須自己執行工具、把結果傳回去，手動維護整個 agentic loop。Claude Code Agent SDK 直接幫你處理這個迴圈，你只需要設定 allowed\_tools 和 prompt，SDK 會自動跑完整個 agent execution。簡單說：Client SDK 給你原料，Agent SDK 給你整個廚房。

### 可以在生產環境部署 Claude Code Agent SDK 嗎？計費方式是什麼？

可以，SDK 的設計目標包含生產環境部署。計費方式和 Anthropic API 相同，依實際使用的 token 數計費，SDK 本身不額外收費。使用 Amazon Bedrock、Google Vertex AI 或 Azure 作為後端時，依各平台計費方式處理。注意：不允許第三方開發者利用 claude.ai 的使用限制提供服務，生產部署必須使用 API key 認證。

### 如何在 SDK 中讓 agent 詢問使用者問題，而不是自己做決定？

在 allowed\_tools 中加入 AskUserQuestion。Claude 在需要釐清需求時，會透過這個工具送出問題，你可以在訊息串流中接收並回應。搭配 hooks 的 UserPromptSubmit 事件，可以在程式中攔截問題並自動填入答案，適合測試或全自動化場景。

### Python 版本和 TypeScript 版本的 Claude Code Agent SDK 功能有差異嗎？

兩個版本的功能和 API surface 基本相同，主要差異是語言慣例：Python 用 snake\_case（allowed\_tools、permission\_mode），TypeScript 用 camelCase（allowedTools、permissionMode）。Python 版使用 asyncio 的 async for，TypeScript 版使用 for await…of。兩者支援相同的 tools、hooks、subagents、MCP servers 功能。

### 如何在 Claude Code Agent SDK 中使用 MCP server 的工具？

在 mcp\_servers 欄位定義 MCP server（command 和 args），然後在 allowed\_tools 中加入 mcp\_\_server\_name\_\_tool\_name 格式的工具名稱。例如使用 Playwright MCP 的 navigate 工具，設定 mcp\_servers:{“playwright”:{“command”:”npx”,”args”:\[“@playwright/mcp@latest”\]}} 並在 allowed\_tools 加入 mcp\_\_playwright\_\_browser\_navigate。

### Claude Code Agent SDK 支援 streaming 輸出嗎？

支援。query() 函式本身就是 async generator，以串流方式 yield 訊息。你可以在 async for 迴圈中即時處理每個訊息，包括 assistant 的 text blocks、tool use events、tool results 等。不需要等到 agent 完成才能看到輸出，適合需要即時反饋的使用情境。