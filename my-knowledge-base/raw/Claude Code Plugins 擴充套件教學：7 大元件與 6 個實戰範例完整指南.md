---
title: "Claude Code Plugins 擴充套件教學：7 大元件與 6 個實戰範例完整指南"
source: "https://moksaweb.com/claude-code-plugins/"
author:
  - "[[Moksa]]"
published: 2026-04-14
created: 2026-07-20
description: "Claude Code Plugins 模組化擴充系統完整教學，詳解 Skills、Agents、Hooks、MCP Server、LSP Server 等 7 大元件的建立方式，附 Plugin Manifest 設定、Marketplace 分發與從 Standalone 遷移的 6 個實戰範例。"
tags:
  - "clippings"
status: processed
---
Claude Code Plugins 是 Anthropic 推出的模組化擴充系統，讓你把自訂的 Skills、Agents、Hooks 和 MCP Servers 打包成可分享、可版本控制的 Plugin Package，跨專案和跨團隊重複使用。和直接放在 `.claude/` 目錄的 Standalone Configuration 不同，Claude Code Plugins 有 Namespace 機制防止命名衝突，有 Marketplace 分發機制讓你一行指令安裝，還有 Semantic Versioning 追蹤更新。如果你已經在 `.claude/skills/` 和 `.claude/agents/` 中累積了一堆自訂設定，Claude Code Plugins 就是把它們「產品化」的標準方式，從個人工具升級為團隊資產。

> **Moksa 觀點：** 我從 Claude Code 剛推出 Skills 功能就開始寫自訂指令，但一直覺得 `.claude/` 目錄的分享方式太原始——要嘛 Copy-Paste，要嘛 Git Submodule，版本管理很痛苦。我的團隊有 5 個工程師，每個人都自己維護一份 `.claude/skills/` ，結果同樣的 Code Review Skill 出現了 5 個不同版本。Claude Code Plugins 終於解決了這個問題，用 `plugin.json` Manifest 定義 Metadata，用 Marketplace 統一分發，用 Namespace 避免衝突。我現在把所有團隊共用的 Code Review、Security Scan、Deploy 流程都包裝成 Plugin，新人加入只要 `/plugin install` 就搞定。不過 Plugin 系統目前還比較新，生態系還在建立中，Official Marketplace 的 Plugin 數量還不多，這篇會從零開始教你建立、測試、分發你自己的 Claude Code Plugins。

## 1\. 什麼是 Claude Code Plugins？

Claude Code Plugins 是一個目錄結構加上 Manifest 檔案的標準化封裝格式。一個 Plugin 可以包含任意組合的 Skills（自訂 Slash Commands）、Agents（自訂 Sub-Agents）、Hooks（Lifecycle Event Handlers）、MCP Servers（外部工具整合）和 LSP Servers（程式碼智慧提示）。每個 Plugin 有唯一的 `name` 作為 Namespace Prefix，所有 Skills 都用 `/plugin-name:skill-name` 格式呼叫，防止多個 Plugin 之間的 Name Collision。

Plugin 和 Standalone Configuration（`.claude/` 目錄）的核心差異在於 Portability（可攜性）和 Distribution（分發機制）。Standalone 設定只存在於單一專案中，要分享就得手動複製；Claude Code Plugins 可以透過 Marketplace 用 `/plugin install` 一行指令安裝，支援 Semantic Versioning 追蹤版本，團隊成員都能確保使用相同版本的工具和流程。對企業團隊來說，這個差異是 Game Changer——以前要讓 50 個工程師都用同樣的 Lint Hook 和 Deploy Skill，需要在內部 Wiki 寫長篇教學還要追進度，現在發布一個 Plugin 全部解決。

Claude Code Plugins 的 Manifest 檔案位於 `.claude-plugin/plugin.json` ，定義 Plugin 的 Name、Description、Version 和 Author。Anthropic 提供 Official Marketplace 讓你提交 Plugin 給全世界使用，也可以自建 Team Marketplace 只在內部分發。Team Marketplace 通常是一個 Git Repository，包含 Plugin 的程式碼和一個 marketplace.json 索引檔案，團隊成員透過設定指向這個 Repository 就能自動載入所有 Plugins。這種設計讓內部 IP 不需要公開到 Anthropic Official Marketplace 也能享受 Plugin 系統的便利。

從技術架構來看，Claude Code Plugins 系統的核心是一個 Plugin Resolver——當 Claude Code 啟動時，它會掃描所有已安裝的 Plugins，讀取每個 Plugin 的 plugin.json，然後把 Plugin 的 Skills、Agents、Hooks 和 MCP Servers 註冊到對應的 Registry。Skills 和 Agents 在註冊時會加上 Namespace Prefix，Hooks 會被合併到全域的 Hook Configuration 中，MCP Servers 會被加入 Active MCP Server List。整個過程在 Session 啟動時完成，Plugin 修改後執行 `/reload-plugins` 即可重新載入。

## 2\. Claude Code Plugins 有哪些核心元件？

一個 Claude Code Plugins Package 可以包含以下 8 種元件，全部都是 Optional，你只需要用到什麼就放什麼：

| 目錄 / 檔案 | 位置 | 用途 |
| --- | --- | --- |
| `.claude-plugin/` | Plugin Root | 存放 `plugin.json` Manifest |
| `skills/` | Plugin Root | 自訂 Slash Commands（ `<name>/SKILL.md` ） |
| `agents/` | Plugin Root | 自訂 Sub-Agent Definitions |
| `hooks/` | Plugin Root | Lifecycle Event Handlers（ `hooks.json` ） |
| `.mcp.json` | Plugin Root | MCP Server Configurations |
| `.lsp.json` | Plugin Root | LSP Server Configurations（程式碼智慧提示） |
| `bin/` | Plugin Root | Executables 加入 Bash Tool 的 PATH |
| `settings.json` | Plugin Root | Plugin 啟用時套用的 Default Settings |

**重要注意事項：** `skills/` 、 `agents/` 、 `hooks/` 這些目錄要放在 Plugin Root 層級，不是放在 `.claude-plugin/` 裡面。`.claude-plugin/` 裡面只有 `plugin.json` 一個檔案。這是新手最常犯的結構錯誤，我自己也踩過——把所有檔案都塞進 `.claude-plugin/` 結果 Plugin 完全不認得 Skills 和 Hooks。Anthropic 官方文件特別用 Warning 強調這點，可見有多少人犯過。

以下是 Claude Code Plugins 和 Standalone Configuration 的完整比較：

| 比較項目 | Standalone（.claude/ 目錄） | Claude Code Plugins |
| --- | --- | --- |
| Skill 名稱 | `/hello` （短名稱） | `/plugin-name:hello` （Namespace） |
| 適合場景 | 個人 Workflow、單一專案自訂 | 團隊分享、跨專案重用、社群分發 |
| 分享方式 | 手動 Copy / Git Submodule | Marketplace `/plugin install` |
| 版本控制 | 無 | Semantic Versioning |
| 命名衝突 | 可能衝突 | Namespace 自動隔離 |
| Hooks 設定 | `settings.json` | `hooks/hooks.json` |
| 更新機制 | 手動 Pull Repo | `/plugin update` |
| 適合對象 | 個人開發者、單一專案維護者 | 團隊、開源社群、企業 IT |

## 3\. Claude Code Plugins 適合哪些使用場景？

以下是 Claude Code Plugins 最常見的 6 個應用場景，每個都附上實際的使用案例：

- **團隊標準化 Workflow** ：把 Code Review Checklist、Deploy Pipeline、Security Scan 流程打包成 Plugin，新成員 `/plugin install` 就能使用團隊標準流程，確保 Consistent Quality。實際案例：我所在的團隊把 OWASP Top 10 的 Security Review 流程做成 Plugin，每個 PR 提交前自動跑一次 `/team:security-review` ，過去 3 個月攔截了 12 個潛在的 Security Vulnerabilities。
- **跨專案重用 Skills** ：寫一次 `/generate-api-docs` 或 `/run-e2e-tests` Skill，在所有專案中安裝使用，不需要每個 Repo 都複製一份。對 Polyglot 的開發者特別有用，可以維護一套通用的 Skill，例如 `/explain-codebase` 、 `/find-tech-debt` 、 `/suggest-refactor` ，無論是 TypeScript、Python 還是 Go 專案都能用。
- **MCP Server 整合套件** ：把常用的 MCP Server Configuration（例如 Jira + Slack + Google Drive 的 Enterprise 套件）打包成 Plugin，統一管理認證和設定。新成員不需要一個一個 `claude mcp add` ，安裝 Plugin 後所有 MCP Server 自動就緒，只需要設定環境變數即可。
- **LSP Code Intelligence** ：為特定程式語言提供 Language Server Protocol 整合，讓 Claude 有即時的 Code Intelligence（Auto-Complete、Go to Definition、Error Diagnostics）。雖然 Claude Code 內建支援主流語言，但對 Niche Languages（Rust 的特定 Crate、自家 DSL）特別有用。
- **Custom Agent 部署** ：把專門化的 Sub-Agent（例如 Security Reviewer、Performance Optimizer、Documentation Writer）打包成 Plugin。Plugin 可以在 settings.json 中指定 agent 欄位，讓 Plugin 啟用時自動切換 Main Thread 為該 Agent。
- **社群分享和 Marketplace 分發** ：把你的 Best Practices 打包成 Plugin 提交到 Anthropic Official Marketplace，讓全世界的 Claude Code 使用者受益。Open Source 社群的早期 Plugins 已經涵蓋了 Database Migration、API Documentation、Performance Profiling、Accessibility Audit 等實用場景。

## 4\. Claude Code Plugins 怎麼建立和設定？

### 建立 Plugin 目錄和 Manifest

執行 mkdir -p my-plugin/.claude-plugin 建立目錄結構，在.claude-plugin/plugin.json 中定義 name、description、version 和 author。

### 新增 Skills / Agents / Hooks

在 Plugin Root 建立 skills/、agents/、hooks/ 目錄，放入 SKILL.md、Agent Definitions 或 hooks.json。

### 本機測試 Plugin

執行 claude –plugin-dir./my-plugin 載入 Plugin 測試，用 /plugin-name:skill-name 呼叫 Skills，用 /reload-plugins 載入修改。

### 分發到 Marketplace

準備好 README.md 後，透過 claude.ai/settings/plugins/submit 或 platform.claude.com/plugins/submit 提交到 Official Marketplace。

## 5\. Claude Code Plugins 實戰範例怎麼用？

**範例 1：建立完整的 Plugin 目錄結構**

```
# Complete Claude Code Plugins directory structure
# Plugin name: team-workflow

team-workflow/
├── .claude-plugin/
│   └── plugin.json          # Plugin Manifest (only file in this directory)
├── skills/
│   ├── code-review/
│   │   └── SKILL.md         # /team-workflow:code-review
│   ├── deploy-staging/
│   │   └── SKILL.md         # /team-workflow:deploy-staging
│   ├── generate-docs/
│   │   └── SKILL.md         # /team-workflow:generate-docs
│   ├── run-security-scan/
│   │   └── SKILL.md         # /team-workflow:run-security-scan
│   └── update-changelog/
│       └── SKILL.md         # /team-workflow:update-changelog
├── agents/
│   ├── security-reviewer.md # Specialized Security Review Agent
│   ├── perf-optimizer.md    # Performance Optimization Agent
│   └── doc-writer.md        # Documentation Writer Agent
├── hooks/
│   └── hooks.json           # PostToolUse auto-lint, PreToolUse safety
├── .mcp.json                # MCP Server config (Jira, Slack integration)
├── .lsp.json                # LSP Server config (custom language support)
├── bin/
│   ├── custom-linter        # Custom executable, auto-added to PATH
│   └── deploy-helper.sh     # Helper script for deployment workflows
├── settings.json            # Default settings when plugin is enabled
├── README.md                # Installation and usage instructions
├── CHANGELOG.md             # Version history
└── LICENSE                  # License file (Apache 2.0, MIT, etc.)
```

**範例 2：Plugin Manifest（plugin.json）完整設定**

```
// .claude-plugin/plugin.json
// Complete manifest with all supported fields
{
  "name": "team-workflow",
  "description": "Standardized development workflow for the engineering team. Includes code review, security scanning, deployment, and documentation generation.",
  "version": "2.1.0",
  "author": {
    "name": "Moksa Engineering",
    "email": "engineering@moksaweb.com",
    "url": "https://moksaweb.com"
  },
  "homepage": "https://github.com/your-org/team-workflow-plugin",
  "repository": {
    "type": "git",
    "url": "https://github.com/your-org/team-workflow-plugin.git",
    "directory": "claude-code-plugins/team-workflow"
  },
  "license": "MIT",
  "keywords": [
    "code-review",
    "deployment",
    "security",
    "workflow",
    "documentation",
    "team-collaboration"
  ],
  "engines": {
    "claude-code": ">=2.0.73"
  }
}

// Field explanations:
// name: Unique identifier and skill namespace
//   All skills will be called as /team-workflow:xxx
//   Cannot contain spaces, must be URL-safe
//
// description: Shown in plugin manager when browsing/installing
//   Keep concise, mention key features
//
// version: Semantic Versioning (major.minor.patch)
//   1.0.0 -> 2.0.0: Breaking changes (renamed skills, removed features)
//   2.0.0 -> 2.1.0: New features (added skills, new MCP servers)
//   2.1.0 -> 2.1.1: Bug fixes only
//
// author: Optional, helpful for attribution
//   Can be a string or object with name/email/url
//
// homepage: Plugin documentation URL
// repository: Source code location for transparency
// license: SPDX license identifier (MIT, Apache-2.0, etc.)
// keywords: Help discoverability in marketplace search
// engines: Minimum Claude Code version required
```

**範例 3：建立 Code Review Skill with SKILL.md**

```
# skills/code-review/SKILL.md
# A production-grade Code Review Skill that team members can use
---
name: code-review
description: >
  Comprehensive code review following team standards.
  Triggered when user asks to review code, check PR quality,
  or analyze code for issues. Optional argument: PR number to
  review a specific GitHub Pull Request.
allowed-tools:
  - Bash(git diff *)
  - Bash(git log *)
  - Bash(gh pr view *)
  - Bash(gh pr diff *)
  - Bash(rg *)
  - Read
disable-model-invocation: false
---

# Code Review Skill

Review the code changes following these team standards:

## Step 1: Gather context

If $ARGUMENTS contains a PR number:
  Run: gh pr diff $ARGUMENTS --color=never
Otherwise:
  Run: git diff --cached --color=never
  If nothing staged, run: git diff HEAD~1 --color=never

## Step 2: Security Checks

Check the code for these critical security issues:

1. SQL Injection vulnerabilities in raw queries
2. XSS vulnerabilities in template rendering
3. Command Injection in shell command construction
4. Path Traversal in file system operations
5. Hardcoded secrets, API keys, or credentials
6. Authentication bypass logic
7. Authorization checks missing for sensitive operations
8. Weak cryptography (MD5, SHA1, ECB mode encryption)
9. Insecure deserialization (pickle, eval, exec)
10. CORS misconfigurations or overly permissive policies
11. CSRF protection missing on state-changing endpoints
12. Sensitive data leaked in logs or error messages

## Step 3: Code Quality Checks

Check the code for these quality issues:

1. Functions longer than 50 lines (cognitive complexity)
2. Cyclomatic complexity above 10
3. Duplicated code blocks (extract to shared utility)
4. Missing error handling for I/O operations
5. Unhandled promise rejections (JavaScript/TypeScript)
6. Memory leaks in event listeners (missing cleanup)
7. N+1 query patterns (database calls in loops)
8. Missing input validation at API boundaries
9. Inconsistent naming conventions
10. Missing or inadequate tests for new code paths
11. Test coverage below 80% for new files
12. Missing JSDoc/docstrings for public APIs

## Step 4: Performance Checks

1. Synchronous operations that should be async
2. Missing pagination for large data sets
3. Unbounded loops or recursion
4. Inefficient algorithms (O(n^2) when O(n) is possible)
5. Missing caching for expensive computations
6. Bundle size impact (for frontend changes)

## Step 5: Generate Report

Format findings in this structure:

| Severity | File:Line | Issue | Suggested Fix |
|----------|-----------|-------|---------------|
| Critical | auth.py:42 | SQL injection via f-string | Use parameterized query with ? placeholders |
| High | api.ts:78 | Missing rate limiting on /login | Add @RateLimit(60, "1m") decorator |
| Medium | utils.js:15 | Function too long (87 lines) | Extract validation logic to validateInput() |
| Low | styles.css:200 | Unused CSS rule | Remove .legacy-button selector |

## Step 6: Summary

Provide overall assessment:
- Total issues found by severity
- Estimated time to fix all critical and high issues
- Recommended approval status: approve / request-changes / comment
- Top 3 highest-priority fixes to address first
- Suggested follow-up actions if any patterns emerge
```

**範例 4：設定 Hooks（hooks.json）自動化 Linting**

```
// hooks/hooks.json
// Format is identical to settings.json hooks object
// Plugin hooks merge with user/project hooks at runtime
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "input=$(cat); cmd=$(echo \"$input\" | jq -r '.tool_input.command // empty'); blocked='rm -rf /|DROP TABLE|TRUNCATE|DELETE FROM users|git push --force origin main|chmod -R 777'; if echo \"$cmd\" | grep -qiE \"$blocked\"; then echo 'BLOCKED: Critical destructive operation detected: '\"$cmd\"; exit 1; fi; exit 0"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "input=$(cat); file=$(echo \"$input\" | jq -r '.tool_input.file_path // empty'); if [ -z \"$file\" ]; then exit 0; fi; ext=$(echo \"$file\" | grep -oE '\\.[^.]+$'); case \"$ext\" in .ts|.tsx|.js|.jsx) npx eslint --fix \"$file\" 2>/dev/null;; .py) ruff check --fix \"$file\" 2>/dev/null;; .go) gofmt -w \"$file\" 2>/dev/null;; .rs) rustfmt \"$file\" 2>/dev/null;; .json) prettier --write \"$file\" 2>/dev/null;; esac; exit 0",
            "async": true
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Loading team workflow context...'; if [ -f .claude-context.md ]; then cat .claude-context.md; fi; echo 'Active branch: '$(git branch --show-current 2>/dev/null || echo 'not in git repo'); echo 'Recent commits:'; git log --oneline -5 2>/dev/null || true"
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "if command -v terminal-notifier &>/dev/null; then terminal-notifier -title 'Claude Code' -message 'Task complete' -sound default 2>/dev/null; elif command -v notify-send &>/dev/null; then notify-send 'Claude Code' 'Task complete'; fi; exit 0"
          }
        ]
      }
    ]
  }
}

// Hook explanation:
// PreToolUse: Runs before tool execution, can block dangerous operations
//   Returns exit 1 to block, exit 0 to allow
//   Has access to TOOL_INPUT via stdin
//
// PostToolUse: Runs after Write/Edit, auto-formats based on file extension
//   async: true means it runs in background, doesn't block main flow
//   Supports TypeScript (eslint), Python (ruff), Go (gofmt), Rust (rustfmt)
//
// SessionStart: Runs at session beginning, loads project context
//   Useful for showing recent commits, current branch, todo list
//
// Stop: Runs when task completes, sends desktop notification
//   Cross-platform: terminal-notifier for macOS, notify-send for Linux
```

**範例 5：設定 MCP Server 整合（.mcp.json）**

```
// .mcp.json - placed at Plugin Root
// MCP Servers auto-load when plugin is enabled
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}",
        "GITHUB_OWNER": "${GITHUB_OWNER}",
        "GITHUB_REPO": "${GITHUB_REPO}"
      }
    },
    "linear": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-linear"],
      "env": {
        "LINEAR_API_KEY": "${LINEAR_API_KEY}",
        "LINEAR_TEAM_ID": "${LINEAR_TEAM_ID}"
      }
    },
    "datadog": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-datadog"],
      "env": {
        "DD_API_KEY": "${DD_API_KEY}",
        "DD_APP_KEY": "${DD_APP_KEY}",
        "DD_SITE": "datadoghq.com"
      }
    },
    "postgresql": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-postgresql"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    },
    "redis": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-redis"],
      "env": {
        "REDIS_URL": "${REDIS_URL}"
      }
    }
  }
}

// User experience after plugin install:
// 1. Run: /plugin install team-workflow
// 2. Set environment variables in shell profile or .env.local:
//    export GITHUB_TOKEN=ghp_xxxxx
//    export LINEAR_API_KEY=lin_api_xxxxx
//    export DD_API_KEY=dd_xxxxx
//    export DATABASE_URL=postgresql://user:pass@host:5432/db
//    export REDIS_URL=redis://localhost:6379
// 3. Restart Claude Code or run /reload-plugins
// 4. All MCP servers auto-connect, no manual claude mcp add needed
// 5. Verify with /mcp to see all connected servers
```

**範例 6：從 Standalone 設定遷移到 Plugin**

```
# Migration script: convert .claude/ standalone config to a Plugin
# Run this from your project root

# Step 1: Create plugin directory structure
mkdir -p my-team-plugin/.claude-plugin
mkdir -p my-team-plugin/skills
mkdir -p my-team-plugin/agents
mkdir -p my-team-plugin/hooks

# Step 2: Create plugin.json manifest
cat > my-team-plugin/.claude-plugin/plugin.json << 'EOF'
{
  "name": "my-team-plugin",
  "description": "Migrated from standalone .claude/ configuration",
  "version": "1.0.0",
  "author": {
    "name": "Migration Tool"
  }
}
EOF

# Step 3: Copy existing configurations
# Skills (newer format)
if [ -d .claude/skills ]; then
  cp -r .claude/skills/* my-team-plugin/skills/
  echo "Copied skills"
fi

# Commands (older format, still works)
if [ -d .claude/commands ]; then
  cp -r .claude/commands my-team-plugin/
  echo "Copied legacy commands"
fi

# Agents
if [ -d .claude/agents ]; then
  cp -r .claude/agents/* my-team-plugin/agents/
  echo "Copied agents"
fi

# Step 4: Migrate Hooks from settings.json
# Extract hooks object and save as hooks/hooks.json
if [ -f .claude/settings.json ]; then
  jq '{hooks: .hooks}' .claude/settings.json > my-team-plugin/hooks/hooks.json
  if [ "$(cat my-team-plugin/hooks/hooks.json)" = '{"hooks":null}' ]; then
    rm my-team-plugin/hooks/hooks.json
    rmdir my-team-plugin/hooks
    echo "No hooks to migrate"
  else
    echo "Migrated hooks to hooks/hooks.json"
  fi
fi

# Step 5: Copy MCP server configs
if [ -f .claude/.mcp.json ]; then
  cp .claude/.mcp.json my-team-plugin/.mcp.json
  echo "Copied MCP server config"
fi

# Step 6: Test the migrated plugin
echo "Testing migrated plugin..."
claude --plugin-dir ./my-team-plugin --version

# Step 7: Manual testing checklist
echo "Manual testing required:"
echo "1. Run: claude --plugin-dir ./my-team-plugin"
echo "2. Test each skill: /my-team-plugin:skill-name"
echo "3. Verify agents appear: /agents"
echo "4. Confirm hooks trigger correctly"
echo "5. Check MCP servers connect: /mcp"
echo ""
echo "After verifying everything works:"
echo "- Add README.md with installation instructions"
echo "- Add LICENSE file (recommend Apache-2.0 or MIT)"
echo "- Initialize git: cd my-team-plugin && git init"
echo "- Push to GitHub for marketplace distribution"
```

**範例 7：建立 Team Marketplace 統一管理**

```
# Team Marketplace Repository Structure
# This is your internal "App Store" for Claude Code Plugins

team-claude-marketplace/
├── marketplace.json              # Marketplace index file
├── plugins/
│   ├── code-review-suite/       # Plugin 1
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── skills/
│   │   └── README.md
│   ├── deployment-tools/        # Plugin 2
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   └── ...
│   └── data-analysis-kit/       # Plugin 3
│       ├── .claude-plugin/
│       │   └── plugin.json
│       └── ...
└── README.md

# marketplace.json content
{
  "name": "team-claude-marketplace",
  "description": "Internal marketplace for Engineering team plugins",
  "version": "1.0.0",
  "owner": {
    "name": "Engineering Platform Team",
    "email": "platform@example.com"
  },
  "plugins": [
    {
      "name": "code-review-suite",
      "source": {
        "source": "github",
        "repo": "your-org/team-claude-marketplace",
        "path": "plugins/code-review-suite",
        "ref": "main"
      },
      "category": "code-quality",
      "tags": ["security", "review", "owasp"]
    },
    {
      "name": "deployment-tools",
      "source": {
        "source": "github",
        "repo": "your-org/team-claude-marketplace",
        "path": "plugins/deployment-tools",
        "ref": "main"
      },
      "category": "devops",
      "tags": ["deploy", "kubernetes", "ci-cd"]
    }
  ]
}

# Team members add this marketplace once:
# In ~/.claude/settings.json
{
  "marketplaces": [
    {
      "source": "github",
      "repo": "your-org/team-claude-marketplace"
    }
  ]
}

# Then they can install any plugin from the marketplace:
> /plugin install code-review-suite
> /plugin install deployment-tools

# Updates are pushed by simply pushing new commits to the repo
# Team members run /plugin update to get latest versions
# Semantic versioning ensures backward compatibility
```

## 6\. Claude Code Plugins 的缺點和限制有哪些？

Claude Code Plugins 雖然解決了分享和版本控制的問題，但目前還有以下限制：

**生態系還在建立中（Early Ecosystem Stage）**

- Official Marketplace 的 Plugin 數量還不多，很多常見需求還沒有現成 Plugin。早期使用者需要自己造輪子。
- 社群生態還在早期階段，Plugin 品質和維護狀態參差不齊。安裝前最好確認最近一次 Commit 時間和 Issue 處理速度。
- Plugin Discovery 機制還不夠成熟，找到適合的 Plugin 需要花時間搜尋。Anthropic 的 Marketplace 還沒有 Reviews 或 Star Rating 機制。

**Namespace 強制導致指令變長（Longer Command Names）**

- Plugin Skills 必須用 `/plugin-name:skill-name` 格式，比 Standalone 的 `/skill-name` 長很多。
- 如果 Plugin Name 很長，每次呼叫都很冗長，例如 `/enterprise-security-suite:run-owasp-scan` 22 個字元。
- 目前沒有 Alias 機制可以縮短 Plugin Skill 名稱，必須打全名或用 Tab 自動完成。

**Settings 支援有限（Limited Settings Scope）**

- Plugin 的 `settings.json` 目前只支援 `agent` Key，其他 Settings 無法透過 Plugin 自動配置。
- 環境變數需要使用者自行設定，Plugin 無法自動 Provision Secrets。安裝 Plugin 後還是要看 README 設定 5-10 個環境變數。
- Plugin 之間的相依性（Dependencies）目前不支援，無法宣告「需要先安裝 Plugin A 才能用 Plugin B」。

**安全考量（Security Considerations）**

- 第三方 Plugin 的 Hooks 可以執行 Arbitrary Shell Commands，安裝前務必 Review Source Code，特別是 hooks/hooks.json 和 bin/ 目錄。
- Managed Settings 可以 Force-Enable 特定 Marketplace Plugins，無法被 `--plugin-dir` Override。企業環境可能會強制安裝某些 Plugin。
- `bin/` 目錄的 Executables 會自動加入 PATH，惡意 Plugin 可能 Override 系統指令（例如放一個叫 `git` 或 `ls` 的 Executable）。

## 7\. 哪裡可以找到更多 Claude Code Plugins 資源？

以下 3 支官方影片展示 Claude Code Plugins 的建立和分發流程：

![Youtube video](https://i.ytimg.com/vi/kS1MJFZWMq4/hqdefault.jpg)

Creating Custom Skills with Claude — Skills 和 Plugin 建立教學

![Youtube video](https://i.ytimg.com/vi/fOxC44g8vig/hqdefault.jpg)

Claude Agent Skills Explained — Skills 系統深度解析

![Youtube video](https://i.ytimg.com/vi/gv0WHhKelSE/hqdefault.jpg)

Claude Code Best Practices — Plugin 管理最佳實務

- [Moksa Claude Code 完整教學系列](https://moksaweb.com/category/claude-code/)
- [Claude Code Plugins 官方文件](https://code.claude.com/docs/en/plugins)
- [Anthropic 官方網站](https://www.anthropic.com/)

## 8\. Claude Code Plugins 常見問題 FAQ

### Claude Code Plugins 和.claude/ 目錄的 Standalone 設定有什麼差別？

最大的差別是 Distribution 和 Namespace。Standalone 設定只存在於單一專案，Skill 名稱是短格式如 /hello；Plugin 可以透過 Marketplace 分發，Skill 名稱有 Namespace 前綴如 /my-plugin:hello 防止衝突。建議先在.claude/ 中開發和測試，確定要分享時再轉換成 Plugin。

### 怎麼安裝別人的 Claude Code Plugins？

用 /plugin install 指令安裝 Marketplace 上的 Plugin，或用 claude –plugin-dir./path-to-plugin 載入本機 Plugin。安裝後用 /reload-plugins 載入，用 /help 查看可用的 Skills。Team Marketplace 可以在 Repository 層級設定，讓團隊成員自動載入。

### Plugin 的 Hooks 和 settings.json 的 Hooks 格式一樣嗎？

完全一樣。Plugin 的 Hooks 放在 hooks/hooks.json 中，格式和.claude/settings.json 中的 hooks 物件完全相同，支援 PreToolUse、PostToolUse、SessionStart、Stop 等所有 Lifecycle Events。唯一的差異是位置不同——Plugin 放在 hooks/ 目錄，Standalone 放在 settings.json。

### Plugin 可以包含 MCP Server 設定嗎？

可以。在 Plugin Root 放一個.mcp.json 檔案，格式和.claude/.mcp.json 完全相同。Plugin 安裝後 MCP Server 會自動載入。環境變數可以用 ${VAR\_NAME} 語法引用，使用者只需要設定對應的環境變數，不需要手動 claude mcp add。

### settings.json 在 Plugin 中可以設定什麼？

目前只支援 agent Key。設定 agent 會在 Plugin 啟用時自動啟用指定的 Custom Agent 作為 Main Thread，套用該 Agent 的 System Prompt、Tool Restrictions 和 Model。其他 Settings Key 目前不支援，Unknown Keys 會被靜默忽略。

### 怎麼把現有的.claude/ 設定轉換成 Plugin？

四個步驟：1) mkdir -p my-plugin/.claude-plugin 建立結構；2) 建立 plugin.json Manifest；3) 把.claude/ 中的 commands/、skills/、agents/ 複製到 Plugin Root；4) 把 settings.json 中的 hooks 移到 hooks/hooks.json。用 claude –plugin-dir./my-plugin 測試後就完成了。

### 怎麼提交 Plugin 到 Anthropic Official Marketplace？

兩個提交入口：claude.ai/settings/plugins/submit 或 platform.claude.com/plugins/submit。提交前確保 Plugin 有完整的 plugin.json Manifest、README.md 使用說明、正確的 Semantic Versioning。Anthropic 會審核後上架。

### 多個 Plugin 的 Skill 名稱衝突怎麼辦？

不會衝突。每個 Plugin 的 Skills 都有 Namespace 前綴，例如 Plugin A 的 /pluginA:deploy 和 Plugin B 的 /pluginB:deploy 是完全獨立的。這是 Claude Code Plugins 使用 Namespace 機制的主要原因。如果 –plugin-dir 載入的 Plugin 和已安裝的 Marketplace Plugin 同名，本機版本優先。

### Plugin 的 bin/ 目錄有什麼安全風險？

bin/ 目錄中的 Executables 會自動加入 Bash Tool 的 PATH。這意味著惡意 Plugin 可能放一個和系統指令同名的 Executable（例如 ls 或 git）來 Override 正常行為。安裝第三方 Plugin 前務必 Review Source Code，特別檢查 bin/ 目錄和 hooks/hooks.json 的內容。

### 開發 Plugin 時怎麼快速測試修改？

用 claude –plugin-dir./my-plugin 載入後，每次修改 Plugin 檔案不需要重啟 Claude Code，只要在互動模式中輸入 /reload-plugins 就能重新載入所有變更。可以同時載入多個 Plugin：claude –plugin-dir./plugin-one –plugin-dir./plugin-two。