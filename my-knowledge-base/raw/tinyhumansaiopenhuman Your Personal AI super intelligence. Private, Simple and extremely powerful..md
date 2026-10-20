---
title: "tinyhumansai/openhuman: Your Personal AI super intelligence. Private, Simple and extremely powerful."
source: "https://github.com/tinyhumansai/openhuman"
author:
  - "[[tinyhumansai]]"
published:
created: 2026-06-08
description: "Your Personal AI super intelligence. Private, Simple and extremely powerful. - tinyhumansai/openhuman"
tags:
  - "clippings"
status: processed
---
## OpenHuman

[![The Tet](https://github.com/tinyhumansai/openhuman/raw/main/gitbooks/.gitbook/assets/demo.png)](https://github.com/tinyhumansai/openhuman/blob/main/gitbooks/.gitbook/assets/demo.png)

 [![tinyhumansai%2Fopenhuman | Trendshift](https://camo.githubusercontent.com/d42b49d0eb4e0a292677087039747ff8964f9dac7a9d83fd1e5dbb26fe3d3dc3/68747470733a2f2f7472656e6473686966742e696f2f6170692f62616467652f7265706f7369746f726965732f3233363830)](https://trendshift.io/repositories/23680)[![OpenHuman - An open source AI harness built with the human in mind | Product Hunt](https://camo.githubusercontent.com/fd0441af97283aaba0f7879562509b095ac973f5c00a0e8f011328cf42f077e7/68747470733a2f2f6170692e70726f6475637468756e742e636f6d2f776964676574732f656d6265642d696d6167652f76312f746f702d706f73742d62616467652e7376673f706f73745f69643d31313336393032267468656d653d6c6967687426706572696f643d6461696c7926743d31373738393136303232383233)](https://www.producthunt.com/products/openhuman?embed=true&utm_source=badge-top-post-badge&utm_medium=badge&utm_campaign=badge-openhuman)**OpenHuman is your Personal AI super intelligence: local memory, managed services where needed, simple and powerful.**

[Discord](https://discord.tinyhumans.ai/) • [Reddit](https://www.reddit.com/r/tinyhumansai/) • [X/Twitter](https://x.com/intent/follow?screen_name=tinyhumansai) • [Docs](https://tinyhumans.gitbook.io/openhuman/) • [Follow @senamakel (Creator)](https://x.com/intent/follow?screen_name=senamakel)

🇺🇸 [English](https://github.com/tinyhumansai/openhuman/blob/main/README.md) | 🇨🇳 [简体中文](https://github.com/tinyhumansai/openhuman/blob/main/README.zh-CN.md) | 🇯🇵 [日本語](https://github.com/tinyhumansai/openhuman/blob/main/README.ja-JP.md) | 🇰🇷 [한국어](https://github.com/tinyhumansai/openhuman/blob/main/README.ko.md) | 🇩🇪 [Deutsch](https://github.com/tinyhumansai/openhuman/blob/main/README.de.md) | 🇵🇰 [اردو](https://github.com/tinyhumansai/openhuman/blob/main/README.ur-pk.md)

> **Early Beta**: Under active development. Expect rough edges.

> **Local + managed services, upfront:** OpenHuman stores its Memory Tree, Obsidian-style Markdown vault, workspace config, and local runtime state on your machine. The default managed experience still uses OpenHuman-hosted services for account sign-in, model routing, web search proxying, and managed integration/OAuth flows through the Composio connector layer. Choose custom/local settings if you want to bring your own model, search, or Composio credentials; some real-time triggers and hosted features still require the managed backend.

## Install

Download installers from [tinyhumans.ai/openhuman](https://tinyhumans.ai/openhuman?utm_source=github&utm_medium=readme) or from the [GitHub Releases](https://github.com/tinyhumansai/openhuman/releases/latest) page. For terminal installs, the native package paths below are preferred — they ride your OS package-manager's signing chain.

These paths verify the artifact through your OS package manager's signing chain (Homebrew bottle hash, signed apt repo, MSI signature).

**macOS (Homebrew tap):**

```
brew tap tinyhumansai/core
brew install openhuman
```

**Linux (Debian/Ubuntu — signed apt repo):**

```
sudo apt-get install -y --no-install-recommends gnupg2 curl ca-certificates
curl -fsSL https://tinyhumansai.github.io/openhuman/apt/KEY.gpg \
  | sudo gpg --dearmor -o /etc/apt/keyrings/openhuman.gpg
echo "deb [signed-by=/etc/apt/keyrings/openhuman.gpg arch=amd64] \
  https://tinyhumansai.github.io/openhuman/apt stable main" \
  | sudo tee /etc/apt/sources.list.d/openhuman.list
sudo apt-get update
sudo apt-get install -y openhuman
```

**Linux (Arch — AUR):** the [`openhuman-bin` AUR recipe](https://github.com/tinyhumansai/openhuman/blob/main/packages/arch/openhuman-bin) is in the repo. Once published, Arch users can install it with `yay -S openhuman-bin`.

**Windows:** download the signed `.msi` from the [latest release](https://github.com/tinyhumansai/openhuman/releases/latest) and run it.

**Manual `.dmg` / `.deb` / `.AppImage` / `.msi`:** grab the installer for your platform directly from the [latest release page](https://github.com/tinyhumansai/openhuman/releases/latest).

> **Linux:** the AppImage can crash on launch under Wayland (and on Arch-based distros with `sharun: Interpreter not found!`) — see [#2463](https://github.com/tinyhumansai/openhuman/issues/2463) for the cause and env-var workarounds. The `.deb` package above avoids those failure modes on Debian/Ubuntu.

## Alternative: script install (no integrity check)

> **Warning — unverified install.** These scripts are served live from `raw.githubusercontent.com` and do **not** ship a separate signature, so `curl … | bash` and `irm … | iex` have no way to detect tampering of the script bytes. Prefer the **native package** paths above whenever possible. If you must use the script, see "Verified script install" below.

```
# macOS or Linux x64
curl -fsSL https://raw.githubusercontent.com/tinyhumansai/openhuman/main/scripts/install.sh | bash

# Windows (PowerShell)
irm https://raw.githubusercontent.com/tinyhumansai/openhuman/main/scripts/install.ps1 | iex
```

## Verified script install status

A separately signed script-install path is not currently available. Issue [#2620](https://github.com/tinyhumansai/openhuman/issues/2620) is closed after the native package paths were promoted, but current release assets do not include `install.sh.asc` / `install.ps1.asc` for pre-execution script verification. Treat the script install path as unverified and prefer the native package options above when possible.

## What is OpenHuman?

OpenHuman is an open-source agentic assistant designed to integrate with you in your daily life. Each bullet links to the deeper writeup in the [docs](https://tinyhumans.gitbook.io/openhuman/).

- **Simple, UI-first & Human** A clean desktop experience and short onboarding paths take you from install to a working agent in a few clicks — no config-first setup, no terminal required. The agent has [a face](https://tinyhumans.gitbook.io/openhuman/features/mascot): a desktop mascot that speaks, reacts to its surroundings, [joins your Google Meets](https://tinyhumans.gitbook.io/openhuman/features/mascot/meeting-agents) as a real participant, remembers you across weeks, and keeps thinking in the background even when you've stopped typing.
- **[118+ third-party integrations](https://tinyhumans.gitbook.io/openhuman/features/integrations) with [auto-fetch](https://tinyhumans.gitbook.io/openhuman/features/obsidian-wiki/auto-fetch)**: plug into Gmail, Notion, GitHub, Slack, Stripe, Calendar, Drive, Linear, Jira and the rest of your stack with **one-click OAuth**. Every connection is exposed to the agent as a typed tool, and every twenty minutes the core walks each active connection and pulls fresh data into the [memory tree](https://tinyhumans.gitbook.io/openhuman/features/integrations/auto-fetch). No prompts, no polling loops you have to write, so the agent already has tomorrow's context this morning.
	Managed integrations use OpenHuman's Composio connector layer. OAuth handshakes and integration tool calls are proxied through the managed backend by default. If you want to run Composio directly instead, configure direct mode with your own Composio API key; real-time trigger webhooks then need to be hosted and wired by you.
- **[Memory Tree](https://tinyhumans.gitbook.io/openhuman/features/memory-tree) + [Obsidian Wiki](https://tinyhumans.gitbook.io/openhuman/features/obsidian-wiki)**: a local-first knowledge base built from your data and your activity. Everything you connect is canonicalized into ≤3k-token Markdown chunks, scored, and folded into hierarchical summary trees stored in **SQLite on your machine**. The same chunks land as `.md` files in an Obsidian-compatible vault you can open, browse and edit, inspired by Karpathy's [obsidian-wiki workflow](https://x.com/karpathy/status/2039805659525644595).
- **Batteries included**: web search, a web-fetch [scraper](https://tinyhumans.gitbook.io/openhuman/features/native-tools), a full coder toolset (filesystem, git, lint, test, grep), and [native voice](https://tinyhumans.gitbook.io/openhuman/features/voice) (STT in, ElevenLabs TTS out, mascot lip-sync, live Google Meet agent) are wired in by default. By default, [model routing](https://tinyhumans.gitbook.io/openhuman/features/model-routing) uses the OpenHuman backend to select and proxy the right LLM for each workload (reasoning, fast, or vision). One subscription includes all models. No "install a plugin to read files" friction. Use [optional local AI via Ollama](https://tinyhumans.gitbook.io/openhuman/features/model-routing/local-ai) for supported on-device workloads.
- **[Smart token compression (TokenJuice)](https://tinyhumans.gitbook.io/openhuman/features/token-compression)**: every tool call, scrape result, email body, and search payload is run through a token compression layer before it touches any LLM Model. HTML is converted to Markdown, long URLs are shortened, and verbose tool output is deduped and summarized via a configurable rule overlay etc... CJK, emoji, and other multi-byte text are preserved grapheme-by-grapheme — never stripped. You get the same information but at a fraction of the tokens. Reducing cost & latency by up to 80%.
- **[Messaging channels](https://tinyhumans.gitbook.io/openhuman/features/integrations#messaging-channels)** and **[privacy & security](https://tinyhumans.gitbook.io/openhuman/features/privacy-and-security)**: inbound/outbound across the channels you already use, with workflow data that stays on device, encrypted locally, treated as yours.

## Contributing from source

New contributor? Start with [`CONTRIBUTING.md`](https://github.com/tinyhumansai/openhuman/blob/main/CONTRIBUTING.md) for the fork/PR workflow and local validation commands, or use the copy-paste AI-agent prompt in [`CONTRIBUTING-BEGINNERS.md`](https://github.com/tinyhumansai/openhuman/blob/main/CONTRIBUTING-BEGINNERS.md#optional-let-an-ai-coding-agent-guide-you). The short path is:

1. Install Git, Node.js 24+, pnpm 10.10.0, Rust 1.93.0 (`rustfmt` + `clippy`), CMake, Ninja, ripgrep, and the platform desktop build prerequisites.
2. Fork and clone the repo, then run `git submodule update --init --recursive` before `pnpm install` so the vendored Tauri/CEF sources are present.
3. Use `pnpm dev` for web-only UI work, `pnpm --filter openhuman-app dev:app` for the desktop shell, and focused checks such as `pnpm typecheck`, `pnpm format:check`, and `cargo check -p openhuman --lib` before opening a PR.

Deeper docs: [Architecture](https://tinyhumans.gitbook.io/openhuman/developing/architecture) · [Getting Set Up](https://tinyhumans.gitbook.io/openhuman/developing/getting-set-up) · [Cloud Deploy](https://github.com/tinyhumansai/openhuman/blob/main/gitbooks/features/cloud-deploy.md).

## Context in minutes, not weeks

OpenHuman is the first agent harness that gets to know you in minutes. Inspired by [Karpathy's LLM Knowledgebase](https://x.com/karpathy/status/2039805659525644595). Most agents start cold. Hermes learns by watching you work; OpenClaw waits for plugins to ferry context in. Either way, you spend days or weeks before the agent knows enough about your stack to be genuinely useful.

[![OpenHuman context-building diagram](https://github.com/tinyhumansai/openhuman/raw/main/gitbooks/.gitbook/assets/image%20(1).png)](https://github.com/tinyhumansai/openhuman/blob/main/gitbooks/.gitbook/assets/image%20\(1\).png)

> OpenHuman summarizes and compresses all your documents, emails & chats; and creates a memory graph that lets your agent remember everything about you.

OpenHuman skips the wait. Connect your accounts, let [auto-fetch](https://tinyhumans.gitbook.io/openhuman/features/integrations/auto-fetch) pull data locally on a 20-minute loop, and then have [Memory Trees](https://tinyhumans.gitbook.io/openhuman/features/memory-tree) compress everything into Markdown files stored intelligently in a [Karpathy-style Obsidian wiki](https://tinyhumans.gitbook.io/openhuman/features/obsidian-wiki).

In just one sync pass, the agent has full (compressed) context of your inbox, your calendar, your repos, your docs, your messages. No training period. No "give it a few weeks.". It becomes you, controlled by you.

Already self-host [agentmemory](https://github.com/rohitg00/agentmemory) across other coding agents? OpenHuman ships an optional `Memory` backend that proxies to it — set `memory.backend = "agentmemory"` in `config.toml` and the same durable store powers OpenHuman alongside Claude Code, Cursor, Codex, and OpenCode. See the [agentmemory backend](https://tinyhumans.gitbook.io/openhuman/features/obsidian-wiki/agentmemory-backend) page for setup.

## OpenHuman vs Other Agent Harnesses

High-level comparison (products evolve, so verify against each vendor). OpenHuman is built to **minimize vendor sprawl**, keep **workflow knowledge on-device**, and give the agent a **persistent memory** of your data, not only chat.

|  | Claude Cowork | OpenClaw | Hermes Agent | OpenHuman |
| --- | --- | --- | --- | --- |
| **Open-source** | 🚫 Proprietary | ✅ MIT | ✅ MIT | ✅ GNU |
| **Simple to start** | ✅ Desktop + CLI | ⚠️  Terminal-first | ⚠️  Terminal-first | ✅ Clean UI, minutes |
| **Cost** | ⚠️  Sub + add-ons | ⚠️  BYO models | ⚠️  BYO models | ✅ One sub + TokenJuice |
| **Memory** | ✅ Chat-scoped | ⚠️  Plugin-reliant | ✅ Self-learning | 🚀 Memory Tree + Obsidian vault, optional [agentmemory](https://github.com/rohitg00/agentmemory) backend |
| **Integrations** | ⚠️  Few connectors | ⚠️  BYO | ⚠️  BYO | 🚀 118+ via OAuth |
| **Auto-fetch** | 🚫 None | 🚫 None | 🚫 None | ✅ 20-min sync into memory |
| **API sprawl** | 🚫 Extra keys | 🚫 BYOK | 🚫 Multi-vendor | ✅ One account |
| **Model routing** | 🚫 Single model | ⚠️  Manual | ⚠️  Manual | ✅ Built-in |
| **Native tools** | ✅ Code-only | ✅ Code-only | ✅ Code-only | ✅ Code + search + scraper + voice |

## Star us on GitHub

*Building toward AGI and artificial consciousness? Star the repo and help others find the path.*[![Star History Chart](https://camo.githubusercontent.com/08cd2c7d6d7859009628b6ecd133042fabab2d7205b42e879ed0d6e91d487027/68747470733a2f2f6170692e737461722d686973746f72792e636f6d2f7376673f7265706f733d74696e7968756d616e7361692f6f70656e68756d616e26747970653d64617465266c6567656e643d746f702d6c656674)](https://www.star-history.com/#tinyhumansai/openhuman&type=date&legend=top-left)## Contributors Hall of Fame

Show some love and end up in the hall of fame. Contributors get free merch and special access to our [Discord](https://discord.tinyhumans.ai/).

[![OpenHuman contributors](https://camo.githubusercontent.com/f4ea2bd379e93d3c44c1ad27d44a1c1799b5fd3f750b7b44d6dff9ed5aca0bae/68747470733a2f2f636f6e747269622e726f636b732f696d6167653f7265706f3d74696e7968756d616e7361692f6f70656e68756d616e)](https://github.com/tinyhumansai/openhuman/graphs/contributors)