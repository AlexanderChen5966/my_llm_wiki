---
title: "Claude-Code-Workflow: JSON-driven multi-agent  cadence-team development framework with   intelligent CLI orchestration (Gemini/Qwen/Codex),   context-first architecture, and automated workflow   execution"
source: https://github.com/catlog22/Claude-Code-Workflow
author:
  - "[[catlog22]]"
published:
created: 2026-06-01
description: JSON-driven multi-agent  cadence-team development framework with   intelligent CLI orchestration (Gemini/Qwen/Codex),   context-first architecture, and automated workflow   execution - catlog22/Claude-Code-Workflow
tags:
  - clippings
status: processed
---
---

## ✨ Key Features

| ### 🎯 Skill-based Workflows  From `lite-plan` (lightweight) to `brainstorm` (multi-role analysis)  ### 🔄 Multi-CLI Orchestration  Gemini, Qwen, Codex, Claude - auto-select or manual  ### ⚡ Team Architecture v2  Role-based agents with inner loop execution  ### 🔧 Queue Scheduler  Background queue execution service | ### 📦 Session Lifecycle  start/resume/complete/sync workflow sessions  ### 🖥️ Terminal Dashboard  Multi-terminal grid with execution monitor  ### 🎨 Orchestrator Editor  Template-based workflow visual editing  ### 💬 A2UI  Agent-to-User interactive interface |
| --- | --- |

> 📖 **New?** See [Workflow Guide](https://github.com/catlog22/Claude-Code-Workflow/blob/main/WORKFLOW_GUIDE.md) for complete workflow documentation.

---

## 🚀 Quick Start

### Install CCW

```
npm install -g claude-code-workflow
ccw install -m Global
```

### Codex Configuration (Required for.codex/skills/)

If you use **Codex CLI** with the `.codex/skills/` workflow skills, add these required features to your `~/.codex/config.toml`:

```
[features]
default_mode_request_user_input = true   # Enable request_user_input tool for interactive confirmations
multi_agent = true                       # Enable multi-agent coordination (spawn_agent, wait, etc.)
multi_agent_v2 = true                    # Enable v4 agent API (fork_context, task_name, send_message, assign_task, list_agents)
enable_fanout = true                     # Enable spawn_agents_on_csv for parallel wave execution
```

> These features are required for workflow skills to function properly. Without them, interactive confirmation gates (`request_user_input`), subagent orchestration, and CSV-driven parallel execution will not work.

### Workflow Skills vs Commands

CCW uses two types of invocations:

| Type | Format | Examples |
| --- | --- | --- |
| **Skills** | Trigger phrase (no slash) | `workflow-lite-plan`, `brainstorm`, `workflow-plan` |
| **Commands** | Slash command | `/ccw`, `/workflow/session:start`, `/issue/new` |

### Choose Your Workflow Skill

| Skill Trigger | Use Case |
| --- | --- |
| `workflow-lite-plan` | Lightweight planning, single-module features (hands off to lite-execute) |
| `workflow-multi-cli-plan` | Multi-CLI collaborative analysis |
| `workflow-plan` | Full planning with session persistence |
| `workflow-tdd-plan` | Test-driven development |
| `workflow-test-fix` | Test generation and fix cycles |
| `brainstorm` | Multi-role brainstorming analysis |

### Workflow Examples

```
# Skill triggers (no slash - just describe what you want)
workflow-lite-plan "Add JWT authentication"
workflow-plan "Implement payment gateway integration"
workflow-execute

# Brainstorming
brainstorm "Design real-time collaboration system"

# Slash commands for session management
/workflow/session:start
/workflow/session:resume
/workflow/session:complete
```

---

## 🛠️ CLI Tool Installation

| CLI | Description | Official Docs |
| --- | --- | --- |
| **Gemini** | Google AI analysis | [google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli) |
| **Codex** | OpenAI autonomous coding | [openai/codex](https://github.com/openai/codex) |
| **OpenCode** | Open-source multi-model | [opencode-ai/opencode](https://github.com/opencode-ai/opencode) |
| **Qwen** | Alibaba Qwen-Code | [QwenLM/Qwen](https://github.com/QwenLM) |

---

## 🎭 Semantic CLI Invocation

Users can **semantically specify CLI tools** in prompts - the system automatically invokes the corresponding CLI.

### Basic Invocation

| User Prompt | System Action |
| --- | --- |
| "Use Gemini to analyze the auth module" | Auto-invoke `gemini` CLI for analysis |
| "Let Codex review this code" | Auto-invoke `codex` CLI for review |
| "Ask Qwen about performance optimization" | Auto-invoke `qwen` CLI for consultation |

### Multi-CLI Orchestration

| Pattern | User Prompt Example |
| --- | --- |
| **Collaborative** | "Use Gemini and Codex to collaboratively analyze security vulnerabilities" |
| **Parallel** | "Have Gemini, Codex, and Qwen analyze the architecture in parallel" |
| **Iterative** | "Use Gemini to diagnose, then Codex to fix, iterate until resolved" |
| **Pipeline** | "Gemini designs the solution, Codex implements, Claude reviews" |

---

## 🔍 ACE Tool Configuration

ACE (Augment Context Engine) provides powerful semantic code search.

| Method | Link |
| --- | --- |
| **Official** | [Augment MCP Documentation](https://docs.augmentcode.com/context-services/mcp/overview) |
| **Proxy** | [ace-tool (GitHub)](https://github.com/eastxiaodong/ace-tool) |

---

## 📚 CodexLens Local Search

> ⚠️
> 
> **In Development**: CodexLens is under iterative optimization. Some features may be unstable.

| Search Mode | Description |
| --- | --- |
| **FTS** | Full-text search, based on SQLite FTS5 |
| **Semantic** | Semantic search, using local embedding models |
| **Hybrid** | Hybrid search, combining FTS + Semantic + Reranking |

---

## 💻 CCW CLI Commands

| Command | Description | When to Use |
| --- | --- | --- |
| **/ccw** | Auto workflow orchestrator - analyzes intent, selects workflow, executes | ✅ General tasks, auto workflow selection |
| **/ccw-coordinator** | Smart orchestrator - recommends command chains, allows manual adjustment | 🔧 Complex multi-step workflows |

**Quick Examples**:

```
# /ccw - Auto workflow selection
/ccw "Add user authentication"
/ccw "Fix memory leak in WebSocket"
/ccw "Implement with TDD"

# /ccw-coordinator - Manual chain orchestration
/ccw-coordinator "Implement OAuth2 system"
```

### Session Management Commands

```
/workflow:session:start     # Start new workflow session
/workflow:session:resume    # Resume paused session
/workflow:session:list      # List all sessions
/workflow:session:sync      # Sync session work
/workflow:session:complete  # Complete session
```

### Issue Workflow Commands

```
/issue/new       # Create new issue
/issue/plan      # Plan issue resolution
/issue/queue     # Form execution queue
/issue/execute   # Execute issue queue
```

### Other CLI Commands

```
ccw install           # Install workflow files
ccw view              # Open dashboard
ccw cli -p "..."      # Execute CLI tools (Gemini/Qwen/Codex)
ccw upgrade -a        # Upgrade all installations
```

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     Workflow Skills                              │
│  📝 workflow-lite-plan / workflow-multi-cli-plan (lightweight)  │
│  📊 workflow-plan / workflow-tdd-plan (session-based)           │
│  🧪 workflow-test-fix / workflow-test-fix         │
│  🧠 brainstorm (multi-role analysis)                            │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                   Team Architecture v2                          │
│  🤖 team-worker agents with role-spec based execution           │
│  🔄 Inner loop framework for sequential task processing         │
│  📢 Message bus protocol with team coordination                 │
│  🧠 Wisdom accumulation (learnings/decisions/conventions)       │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                   Queue Scheduler Service                       │
│  ⚙️ Background execution service with API endpoints             │
│  📊 Queue management and unified CLI execution settings         │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                   Terminal Dashboard & Orchestrator             │
│  🖥️ Multi-terminal grid with execution monitor                  │
│  🎨 Template-based workflow editor with slash commands          │
│  📡 Real-time agent communication via A2UI                      │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📦 Project Structure

```
Claude-Code-Workflow/
├── .claude/
│   ├── agents/          # 22 specialized agents (team-worker, cli-discuss, etc.)
│   ├── commands/        # Slash commands (5 categories)
│   │   ├── ccw.md       # Main orchestrator
│   │   ├── ccw-coordinator.md
│   │   ├── cli/         # CLI commands (cli-init, codex-review)
│   │   ├── issue/       # Issue management (plan, execute, queue)
│   │   ├── memory/      # Memory commands (prepare, style-skill-memory)
│   │   └── workflow/    # Workflow commands (session, ui-design, etc.)
│   └── skills/          # 37 modular skills
│       ├── workflow-lite-plan/
│       ├── workflow-plan/
│       ├── workflow-tdd-plan/
│       ├── workflow-test-fix/
│       ├── brainstorm/
│       ├── team-*/      # Team coordination skills
│       └── ...
├── ccw/
│   ├── src/             # TypeScript source code
│   │   ├── commands/    # CLI command implementations
│   │   ├── core/        # Core services (a2ui, auth, hooks, routes)
│   │   ├── mcp-server/  # MCP server implementation
│   │   └── tools/       # Tool implementations
│   └── frontend/        # React frontend (Terminal Dashboard, Orchestrator)
├── codex-lens/          # Local semantic code search engine
└── docs/                # Documentation
```

---

## 🎼 Team Cadence Control (Beat Model)

The v2 team architecture introduces an **event-driven beat model** for efficient orchestration:

```
Beat Cycle (single beat)
======================================================================
  Event                   Coordinator              Workers
----------------------------------------------------------------------
  callback/resume --> +- handleCallback -+
                      |  mark completed   |
                      |  check pipeline   |
                      +- handleSpawnNext -+
                      |  find ready tasks |
                      |  spawn workers ---+--> [team-worker A] Phase 1-5
                      |  (parallel OK)  --+--> [team-worker B] Phase 1-5
                      +- STOP (idle) -----+         |
                                                     |
  callback <-----------------------------------------+
  (next beat)              SendMessage + TaskUpdate(completed)
======================================================================
```

**Key Benefits:**

- 🎯 **Event-driven**: Coordinator only wakes when needed (callback/resume)
- ⚡ **Fast-advance**: Simple successors spawn directly without coordinator roundtrip
- 🔄 **Dynamic pipelines**: Generated per-task from dependency graph
- 📊 **Parallel execution**: Independent tasks run concurrently

---

## 🖥️ Frontend Highlights

### Terminal Dashboard

Multi-terminal grid layout with real-time execution monitoring.

**Features:**

- 🖥️ Multi-terminal grid with resizable panes
- 📊 Execution monitor with agent list
- 📁 File sidebar for project navigation
- 🎯 Session grouping by project tags
- 🌙 Fullscreen/immersive mode

### Orchestrator Editor

Visual workflow template editor with drag-drop.

**Features:**

- 🎨 React Flow-based visual editing
- 📦 Template library with pre-built workflows
- 🔧 Property panel for node configuration
- ⚡ Slash command integration

---

## 🙏 Acknowledgments

- **[Impeccable](https://github.com/pbakaus/impeccable)** — Design audit methodology, OKLCH color system, anti-AI-slop detection patterns, editorial typography standards, motion/animation token architecture, and vanilla JS interaction patterns. The UI team skills (`team-ui-polish`, `team-interactive-craft`, `team-motion-design`, `team-visual-a11y`, `team-uidesign`, `team-ux-improve`) draw heavily from Impeccable's design knowledge.
- **[gstack](https://github.com/garrytan/gstack)** — Systematic debugging methodology, security audit frameworks, and release pipeline patterns. The skills `investigate` (Iron Law debugging), `security-audit` (OWASP Top 10 + STRIDE), and `ship` (gated release pipeline) are inspired by gstack's workflow designs.

---

## 🤝 Contributing

---

## 📄 License

MIT License - see [LICENSE](https://github.com/catlog22/Claude-Code-Workflow/blob/main/LICENSE)

---

## 💬 Community

Join the CCW WeChat group to discuss tips and share experiences with other developers!

[![CCW WeChat Group](https://github.com/catlog22/Claude-Code-Workflow/raw/main/assets/wechat-group-qr.jpg)](https://github.com/catlog22/Claude-Code-Workflow/blob/main/assets/wechat-group-qr.jpg)

<sub>Scan to join the WeChat group (if the QR code has expired, please open an Issue for the latest one)</sub>

---

## ☕ Buy Me a Coffee

Your support is the greatest motivation for updates!

[![WeChat Appreciation QR Code](https://github.com/catlog22/Claude-Code-Workflow/raw/main/assets/7e9099a5e71b02b0db581563d51a0439.png)](https://github.com/catlog22/Claude-Code-Workflow/blob/main/assets/7e9099a5e71b02b0db581563d51a0439.png)

<sub>"您的支持是更新的最大动力"</sub>

---

[![](https://camo.githubusercontent.com/2436d81233f5792fd0f264fd57041b0e01e7d27b18bdbb0f61303e4af59e6c9e/68747470733a2f2f63617073756c652d72656e6465722e76657263656c2e6170702f6170693f747970653d776176696e6726636f6c6f723d6772616469656e7426637573746f6d436f6c6f724c6973743d362c31312c3230266865696768743d3130302673656374696f6e3d666f6f746572)](https://camo.githubusercontent.com/2436d81233f5792fd0f264fd57041b0e01e7d27b18bdbb0f61303e4af59e6c9e/68747470733a2f2f63617073756c652d72656e6465722e76657263656c2e6170702f6170693f747970653d776176696e6726636f6c6f723d6772616469656e7426637573746f6d436f6c6f724c6973743d362c31312c3230266865696768743d3130302673656374696f6e3d666f6f746572)