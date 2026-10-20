---
title: "open-gsd/gsd-core: Git. Ship. Done - Core"
source: "https://github.com/open-gsd/gsd-core"
author:
  - "[[open-gsd]]"
published:
created: 2026-06-22
description: "Git. Ship. Done - Core. Contribute to open-gsd/gsd-core development by creating an account on GitHub."
tags:
  - "clippings"
status: processed
---
## GSD Core

**Git. Ship. Done.**

**English** · [Português](https://github.com/open-gsd/gsd-core/blob/next/README.pt-BR.md) · [简体中文](https://github.com/open-gsd/gsd-core/blob/next/README.zh-CN.md) · [日本語](https://github.com/open-gsd/gsd-core/blob/next/README.ja-JP.md) · [한국어](https://github.com/open-gsd/gsd-core/blob/next/README.ko-KR.md)

**A light-weight meta-prompting, context engineering, and spec-driven development system for Claude Code, OpenCode, Gemini CLI, Kimi CLI, Kilo, Codex, Copilot, Cursor, Windsurf, and more.**

---

## What is GSD Core

GSD Core is a context-engineering and spec-driven development framework that drives AI coding agents (Claude Code, Codex, Gemini CLI, Kimi CLI, Copilot, Cursor, and more) through a disciplined phase loop. It solves [context rot](https://github.com/open-gsd/gsd-core/blob/next/docs/explanation/context-engineering.md) — the quality degradation that accumulates as an AI fills its context window — by running all heavy research, planning, and execution work in fresh-context subagents while keeping your main session lean.

---

## How it works

Each milestone repeats the same five-step loop, one phase at a time:

1. **Discuss** — capture implementation decisions before anything is planned
2. **Plan** — research, decompose, and verify the plan fits a fresh context window
3. **Execute** — run plans in parallel waves; each executor starts with a clean 200k-token context
4. **Verify** — walk through what was built; diagnose and fix before declaring done
5. **Ship** — create the PR, archive the phase, repeat for the next one

---

## Quickstart

```
npx @opengsd/gsd-core@latest
```

The installer prompts for your runtime (Claude Code, OpenCode, Gemini CLI, Kimi CLI, Kilo, Codex, Copilot, Cursor, Windsurf, and more) and whether to install globally or locally. The installer is required for cross-runtime compatibility — do not copy files from `agents/` or `commands/` directly.

On another runtime or without Node.js? See [Install on your runtime](https://github.com/open-gsd/gsd-core/blob/next/docs/how-to/install-on-your-runtime.md).

Once installed, start your first project:

```
/gsd-new-project
```

New here? Follow [Your first project](https://github.com/open-gsd/gsd-core/blob/next/docs/tutorials/your-first-project.md) for a guided walkthrough from install to first shipped phase.

---

## Documentation

**Tutorials** — learning by doing:

- [Your first project](https://github.com/open-gsd/gsd-core/blob/next/docs/tutorials/your-first-project.md)
- [Onboarding an existing codebase](https://github.com/open-gsd/gsd-core/blob/next/docs/tutorials/onboarding-an-existing-codebase.md)

**How-to guides** — task-focused recipes:

- [Install on your runtime](https://github.com/open-gsd/gsd-core/blob/next/docs/how-to/install-on-your-runtime.md)
- [Plan a phase](https://github.com/open-gsd/gsd-core/blob/next/docs/how-to/plan-a-phase.md)
- [Verify and ship](https://github.com/open-gsd/gsd-core/blob/next/docs/how-to/verify-and-ship.md)
- … [see all how-to guides](https://github.com/open-gsd/gsd-core/blob/next/docs/README.md#how-to-guides)

**Reference** — authoritative facts:

- [Commands](https://github.com/open-gsd/gsd-core/blob/next/docs/COMMANDS.md)
- [Configuration](https://github.com/open-gsd/gsd-core/blob/next/docs/CONFIGURATION.md)
- [CLI tools](https://github.com/open-gsd/gsd-core/blob/next/docs/CLI-TOOLS.md)

**Explanation** — concepts and design decisions:

- [Context engineering](https://github.com/open-gsd/gsd-core/blob/next/docs/explanation/context-engineering.md)
- [The phase loop](https://github.com/open-gsd/gsd-core/blob/next/docs/explanation/the-phase-loop.md)
- [Architecture](https://github.com/open-gsd/gsd-core/blob/next/docs/ARCHITECTURE.md)

Full index: [docs/README.md](https://github.com/open-gsd/gsd-core/blob/next/docs/README.md). Other languages: [日本語](https://github.com/open-gsd/gsd-core/blob/next/README.ja-JP.md) · [한국어](https://github.com/open-gsd/gsd-core/blob/next/README.ko-KR.md) · [Português](https://github.com/open-gsd/gsd-core/blob/next/README.pt-BR.md) · [简体中文](https://github.com/open-gsd/gsd-core/blob/next/README.zh-CN.md).

---

## Why it works

Most AI-coding setups fail at scale because context bloat silently degrades output quality, there is no shared memory between sessions, and nothing verifies that code actually works. GSD Core solves all three: heavy work runs in fresh subagents, structured artifacts like `STATE.md` and `CONTEXT.md` survive session boundaries, and the verify step walks through what was built and generates fix plans before a phase is declared done. See [docs/explanation/context-engineering.md](https://github.com/open-gsd/gsd-core/blob/next/docs/explanation/context-engineering.md) for the full reasoning.

Troubleshooting? See [docs/how-to/recover-and-troubleshoot.md](https://github.com/open-gsd/gsd-core/blob/next/docs/how-to/recover-and-troubleshoot.md).

---

## Community

| Project | Platform |
| --- | --- |
| [gsd-opencode](https://github.com/rokicool/gsd-opencode) | Original OpenCode port |
| [Discord](https://discord.gg/mYgfVNfA2r) | Community support |

---

## Star History

[

![Star History Chart](https://camo.githubusercontent.com/6b3891c2e2b63947947e52541ad216813b6d63be4e28b1cd6f49913618c7eb51/68747470733a2f2f6170692e737461722d686973746f72792e636f6d2f7376673f7265706f733d6f70656e2d6773642f6773642d636f726526747970653d44617465)

](https://star-history.com/#open-gsd/gsd-core&Date)

---

## License

MIT License. See [LICENSE](https://github.com/open-gsd/gsd-core/blob/next/LICENSE) for details.

---

**Claude Code is powerful. GSD Core makes it reliable.**