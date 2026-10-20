---
title: "DietrichGebert/ponytail: Makes your AI agent think like the laziest senior dev in the room. The best code is the code you never wrote."
source: "https://github.com/DietrichGebert/ponytail"
author:
  - "[[DietrichGebert]]"
published:
created: 2026-06-17
description: "Makes your AI agent think like the laziest senior dev in the room. The best code is the code you never wrote. - DietrichGebert/ponytail"
tags:
  - "clippings"
status: processed
---
![Ponytail, the lazy senior dev](https://github.com/DietrichGebert/ponytail/raw/main/assets/logo-dark.png)

## Ponytail

*He says nothing. He writes one line. It works.*

**80-94% less code · 3-6× faster · 47-77% cheaper**  
<sub>Per-task code, latency, and cost on the Claude API, not your plan's quota. The ruleset re-injects each turn, so on one short prompt that overhead can outweigh the savings. Median of 10 runs across Haiku, Sonnet, and Opus. <a href="https://github.com/DietrichGebert/ponytail/blob/main/benchmarks">Reproduce it yourself.</a></sub>

---

You know him. Long ponytail. Oval glasses. Has been at the company longer than the version control. You show him fifty lines; he looks at them, says nothing, and replaces them with one.

Ponytail puts him inside your AI agent.

## Before / after

You ask for a date picker. Your agent installs flatpickr, writes a wrapper component, adds a stylesheet, and starts a discussion about timezones.

With ponytail:

```
<!-- ponytail: browser has one -->
<input type="date">
```

More survivors in [examples/](https://github.com/DietrichGebert/ponytail/blob/main/examples).

## Numbers

Five everyday tasks (email validator, debounce, CSV sum, countdown timer, rate limiter), three models, three arms: no skill, the [caveman](https://github.com/JuliusBrussee/caveman) skill, and ponytail. Ten runs per cell, median reported.

[![Median lines of code per arm across Haiku, Sonnet and Opus; ponytail writes 80-94% less code than the no-skill baseline](https://github.com/DietrichGebert/ponytail/raw/main/assets/benchmark-3model.svg)](https://github.com/DietrichGebert/ponytail/blob/main/assets/benchmark-3model.svg)

**80-94% less code, 47-77% less cost, and 3-6× faster than a no-skill agent, on every model.** Every shortcut ponytail takes is marked in the code with a `ponytail:` comment naming its upgrade path. Reproduce it yourself: `npx promptfoo eval -c benchmarks/promptfooconfig.yaml`. Method and raw numbers: [benchmarks/](https://github.com/DietrichGebert/ponytail/blob/main/benchmarks). Production-grade tasks, where an unconstrained agent bloats far more, are written up in [benchmarks/results/](https://github.com/DietrichGebert/ponytail/blob/main/benchmarks/results).

## How it works

Before writing code, the agent stops at the first rung that holds:

```
1. Does this need to exist?   → no: skip it (YAGNI)
2. Stdlib does it?            → use it
3. Native platform feature?   → use it
4. Installed dependency?      → use it
5. One line?                  → one line
6. Only then: the minimum that works
```

Lazy, not negligent: trust-boundary validation, data-loss handling, security, and accessibility are never on the chopping block.

## Install

The most effort ponytail will ever ask of you:

The Claude Code and Codex plugins run two tiny Node.js lifecycle hooks, so `node` needs to be on your PATH (note for Nix/nvm users: it must be on the non-interactive shell's PATH). If it isn't, the skills still work, the always-on activation just stays quiet instead of erroring on every prompt.

### Claude Code

```
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
```

### Codex

```
codex plugin marketplace add DietrichGebert/ponytail
codex
```

Open `/plugins`, select the Ponytail marketplace, and install Ponytail. Then open `/hooks`, review and trust its two lifecycle hooks, and start a new thread.

This same install also covers the Codex desktop app: restart the app after installing and it picks up the plugin.

### GitHub Copilot CLI

```
copilot plugin marketplace add DietrichGebert/ponytail
copilot plugin install ponytail@ponytail
```

In an interactive Copilot CLI session, use the slash equivalents:

```
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
```

Copilot CLI namespaces plugin commands by plugin name. For example:

```
/ponytail:ponytail ultra
/ponytail:ponytail-review
```

### Pi agent harness

```
pi install git:github.com/DietrichGebert/ponytail
```

### OpenCode

Run OpenCode from a checkout of this repo (the plugin reuses its `hooks/` and `skills/`), and add to `opencode.json`:

```
{ "plugin": ["./.opencode/plugins/ponytail.mjs"] }
```

Injects the ruleset every turn at the active level; adds the `/ponytail` commands (see [Commands](#commands)). OpenCode also auto-loads this repo's `AGENTS.md`, so the rules hold even without the plugin. The plugin adds the `lite/full/ultra/off` levels.

The `./` path resolves against your project's `opencode.json`; to share one checkout across projects, point it at the absolute path of the `.mjs` instead (it finds its `hooks/` and `skills/` relative to its own file).

### Gemini CLI

```
gemini extensions install https://github.com/DietrichGebert/ponytail
```

Loads the ruleset as always-on context every session and registers the `/ponytail` commands; the `skills/` ship too, activated when a task needs them.

### Antigravity CLI

Google is renaming Gemini CLI to Antigravity CLI (the `agy` binary); the same extension installs there:

```
agy plugin install https://github.com/DietrichGebert/ponytail
```

It reuses this repo's `gemini-extension.json`. One difference: Antigravity converts the `/ponytail` commands into skills, so you type them into the chat (e.g. `/ponytail-review` as a message) instead of picking them from a slash menu. Until the migration completes (around June 18, 2026), `gemini extensions install` still works too. To run it as an always-on rule instead, drop the ruleset into `.agents/rules/`.

### OpenClaw

```
clawhub install ponytail
```

Installs ponytail as an OpenClaw skill from ClawHub; the review, audit, debt, and help skills install the same way (`clawhub install ponytail-review`, and so on). OpenClaw applies it on coding tasks and also exposes it as a `/ponytail` command. Without ClawHub, copy [`.openclaw/skills/ponytail`](https://github.com/DietrichGebert/ponytail/blob/main/.openclaw/skills) into `~/.openclaw/skills/`.

That was it. He'd be proud. He won't say it.

Active every session, with a handful of commands (see [Commands](#commands)). `/ponytail ultra` exists for when the codebase has wronged you personally. Startup and mode-change text shows the current mode.

Set the level for every new session with the `PONYTAIL_DEFAULT_MODE` env var (`lite` / `full` / `ultra` / `off`), or a `defaultMode` field in `~/.config/ponytail/config.json` (`%APPDATA%\ponytail\config.json` on Windows). The default is `full`.

Cursor, Windsurf, Cline, GitHub Copilot (editor), Aider, Kiro: copy the matching rules file from this repo ([`.cursor/rules/`](https://github.com/DietrichGebert/ponytail/blob/main/.cursor/rules), [`.windsurf/rules/`](https://github.com/DietrichGebert/ponytail/blob/main/.windsurf/rules), [`.clinerules/`](https://github.com/DietrichGebert/ponytail/blob/main/.clinerules), [`.github/copilot-instructions.md`](https://github.com/DietrichGebert/ponytail/blob/main/.github/copilot-instructions.md), [`AGENTS.md`](https://github.com/DietrichGebert/ponytail/blob/main/AGENTS.md), [`.kiro/steering/`](https://github.com/DietrichGebert/ponytail/blob/main/.kiro/steering)).

Kiro: copy `.kiro/steering/ponytail.md` to `~/.kiro/steering/` (global) or `.kiro/steering/` in your project.

GitHub Copilot CLI fallback (instruction-only mode): it reads `AGENTS.md` and `.github/copilot-instructions.md` in a project, or copy the rules into `~/.copilot/copilot-instructions.md` to run ponytail in every project. This path keeps always-on guidance, but does not add plugin mode switches or hooks.

VS Code with the Codex extension reads `AGENTS.md`, which this repo ships, so it works from the repo root with no setup (`~/.codex/AGENTS.md` makes Codex global).

Which files map to which agent: [Agent portability](https://github.com/DietrichGebert/ponytail/blob/main/docs/agent-portability.md).

## Commands

| Command | What it does |
| --- | --- |
| `/ponytail [lite \| full \| ultra \| off]` | Set the intensity, or turn it off. No argument reports the current level. |
| `/ponytail-review` | Review the current diff for over-engineering, hands back a delete-list. |
| `/ponytail-audit` | Audit the whole repo for over-engineering, not just the diff. |
| `/ponytail-debt` | Harvest the `ponytail:` shortcuts you've deferred into a ledger, so "later" doesn't become "never". |
| `/ponytail-help` | Quick reference for the commands above. |

Commands need a skill-capable host (Claude Code, Codex, OpenCode, Gemini, pi). In Codex they're skills, invoke with `@` (`@ponytail-review`). The instruction-only adapters (Cursor, Windsurf, Cline, Copilot, Kiro, Antigravity) load the always-on ruleset without the commands.

## Development

When changing the compact rule text, keep the agent copies aligned:

```
node scripts/check-rule-copies.js
npm test
```

The OpenClaw skill package (`.openclaw/skills/`) is generated from `skills/`; rerun `node scripts/build-openclaw-skills.js` after changing a skill, the test suite fails if it is stale.

The correctness benchmark spawns Python for email and CSV checks; `python3` is tried before `python`. CSV checks need `pandas` installed locally.

## FAQ

**Does it need a config file?** No. An optional `~/.config/ponytail/config.json` or `PONYTAIL_DEFAULT_MODE` env var can set the default level, but nothing is required.

**What if I really need the 120-line cache class?** You don't. Insist anyway and he'll build it. Slowly. Correctly. While looking at you.

**Does it scale?** The code you never wrote scales infinitely. Zero bugs, zero CVEs, 100% uptime since forever.

**Why "ponytail"?** You know exactly why.

## License

[MIT](https://github.com/DietrichGebert/ponytail/blob/main/LICENSE). The shortest license that works.