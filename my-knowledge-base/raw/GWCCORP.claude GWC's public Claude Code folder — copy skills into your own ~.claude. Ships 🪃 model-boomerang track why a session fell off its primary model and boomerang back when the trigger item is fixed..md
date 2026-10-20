---
title: "GWCCORP/.claude: GWC's public Claude Code folder — copy skills into your own ~/.claude. Ships 🪃 model-boomerang: track why a session fell off its primary model and boomerang back when the trigger item is fixed."
source: "https://github.com/GWCCORP/.claude"
author:
published:
created: 2026-07-22
description: "GWC's public Claude Code folder — copy skills into your own ~/.claude. Ships 🪃 model-boomerang: track why a session fell off its primary model and boomerang back when the trigger item is fixed. - GWCCORP/.claude"
tags:
  - "clippings"
status: processed
---
## .claude

**GWC's public Claude Code folder — clone it, copy what you like.**

This repo is the shareable part of the `~/.claude` setup we run at GWC (Grow With Cloud). Everything in it is meant to be copied: drop a skill into your own `~/.claude/skills/` and it just works.

## What's inside

| Path | What it is |
| --- | --- |
| [`skills/model-boomerang/`](https://github.com/GWCCORP/.claude/blob/main/skills/model-boomerang) | 🪃 **Model Boomerang** — tracks why your session fell back from its primary model (e.g. Fable 5's safety flag re-running a request on Opus and the session *staying* there), pins the exact trigger item, and drives the switch-back the moment that item is fixed |

More is coming — next up is our MAOE multi-agent orchestrator setup, once it's sanitized for the public.

## 🪃 Model Boomerang

Claude Code quietly strands sessions on fallback models: a safety-classifier flag re-runs one request on Opus and the whole session stays there; manual `/model` detours and limit downgrades do the same. Boomerang treats every fallback as an **excursion** with a lifecycle:

```
detected ──▶ open ──▶ resolved ──▶ closed
 switch      trigger    item        back on
 observed    item       fixed       primary
             pinned
```

The statusline is the sensor (the only place Claude Code continuously exposes the live model ID); four hooks brief Claude mid-turn to identify and log the trigger item, then nag for the one-keystroke `/model` revert — or block new prompts on the fallback entirely (opt-in enforce mode) — the moment the item is verified fixed. You get a truthful ledger of what keeps knocking you off your primary model, and a perfectly-timed way home.

**The honest limit:** Claude Code has no programmatic way to switch a live session's model — no hook, CLI, or settings edit touches a running session — so the final keystroke is yours. Harnesses that own their own routing can complete the boomerang automatically; the lifecycle ports cleanly.

### Install

```
git clone https://github.com/GWCCORP/.claude gwc-dotclaude
cp -r gwc-dotclaude/skills/model-boomerang "$HOME/.claude/skills/"
```

Then wire the sensor + hooks into `~/.claude/settings.json` (replace `<HOME>` with your absolute home path):

```
{
  "statusLine": {
    "type": "command",
    "command": "node \"<HOME>/.claude/skills/model-boomerang/scripts/statusline.js\""
  },
  "hooks": {
    "SessionStart":     [ { "hooks": [ { "type": "command", "command": "node \"<HOME>/.claude/skills/model-boomerang/scripts/hook.js\"" } ] } ],
    "UserPromptSubmit": [ { "hooks": [ { "type": "command", "command": "node \"<HOME>/.claude/skills/model-boomerang/scripts/hook.js\"" } ] } ],
    "PostToolUse":      [ { "matcher": "*", "hooks": [ { "type": "command", "command": "node \"<HOME>/.claude/skills/model-boomerang/scripts/hook.js\"" } ] } ],
    "Stop":             [ { "hooks": [ { "type": "command", "command": "node \"<HOME>/.claude/skills/model-boomerang/scripts/hook.js\"" } ] } ]
  }
}
```

Set your primary model prefix (default `claude-fable`):

```
node "$HOME/.claude/skills/model-boomerang/scripts/boomerang.js" config --primary claude-fable
```

Requires Node.js. No dependencies, no network; state stays in `~/.claude/boomerang/`.

### CLI

```
boomerang.js status                       live excursions + config
boomerang.js open --item "..."            pin the trigger item
boomerang.js resolve                      item fixed — start the revert nag
boomerang.js close                        stay on the fallback, close the ledger
boomerang.js snooze [--minutes N]         pause reminders
boomerang.js log                          closed excursions with durations
boomerang.js config --primary <prefix> --enforce on|off
```

### Orchestrators too

The same question — *which item knocked this agent off its primary model, and did it come back?* — applies to multi-agent orchestrators. `skills/model-boomerang/scripts/maoe-report.js` reads a MAOE run database and prints per-agent excursions with trigger task and recovery proof (`references/maoe.md`); the pattern ports to any orchestrator that logs `(task, model, used_fallback)`.