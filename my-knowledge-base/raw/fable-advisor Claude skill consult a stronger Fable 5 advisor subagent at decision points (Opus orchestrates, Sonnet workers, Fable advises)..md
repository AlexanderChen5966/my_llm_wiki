---
title: "echo-of-machines/fable-advisor: Claude skill: consult a stronger Fable 5 advisor subagent at decision points (Opus orchestrates, Sonnet workers, Fable advises)."
source: "https://github.com/echo-of-machines/fable-advisor"
author:
  - "[[echo-of-machines]]"
published:
created: 2026-07-08
description: "Claude skill: consult a stronger Fable 5 advisor subagent at decision points (Opus orchestrates, Sonnet workers, Fable advises). - echo-of-machines/fable-advisor"
tags:
  - "clippings"
status: processed
---
## fable-advisor — install & setup

This skill approximates the Claude API "advisor tool" (which is API-only and not available in Claude Code) by spawning a **Fable 5 subagent** as a second opinion at genuine decision points. The working split: **Opus orchestrates, Sonnet does workers, Fable advises.**

## Two-part design — read this before installing on a new machine

The advisor is made of **two** pieces, and only one of them lives in this folder:

1. **`SKILL.md`** (in this folder) — the consultation *procedure*: when to consult, how to package context (focused brief vs. read-only investigation), and how to weight the advice. This gives you **on-demand** use: invoke it with `/fable-advisor`.
2. **A rule in `~/.claude/CLAUDE.md`** (NOT in this folder) — the standing instruction that makes the advisor fire **automatically** at decision points. A skill can't self-trigger on that kind of always-on meta-behavior, so the automatic half must live in your global CLAUDE.md.

**Consequence:** installing this skill on a new machine gives you the on-demand skill, but the **automatic firing does NOT travel with it** — you must also add the CLAUDE.md rule below.

## Enable automatic firing (paste into ~/.claude/CLAUDE.md)

```
## Fable Advisor — auto-consult at decision points (OPUS-MAIN sessions ONLY)

Working setup: **Opus orchestrates, Sonnet does worker subagents, Fable advises.** The Claude
API "advisor tool" is API-only and NOT available in Claude Code, so the advisor is a
**Fable 5 subagent** driven by the \`fable-advisor\` skill.

**ONLY when the session's main model is Opus, proactively consult a Fable advisory subagent at
genuine decision points** — before committing to a non-obvious approach, when stuck, when
weighing a change of approach, and before declaring a substantial task done (make the deliverable
durable first). Invoke the **\`fable-advisor\`** skill for the procedure (focused-brief vs
read-only-investigation context modes, and how to weight the advice).

**Do NOT invoke it unless main is Opus.** If main is already Fable, Fable→Fable is pure waste
(nothing stronger to consult); if main is Sonnet/Haiku, this configured path doesn't apply.
Check the actual current model first.

**Restraint — rationing is the goal:** Fable is the priciest tier and drains the shared usage
pool fastest. Reserve consults for **high-value decisions on non-trivial tasks** — NOT
routine/reactive turns, trivial edits, or simple lookups; roughly one consult before committing
to an approach and one before declaring done.
```

Optional companion rule (if you also run the Sonnet-workers convention): in your CLAUDE.md's subagent-model policy, note that the Fable advisor is the one sanctioned non-Sonnet subagent (spawned with `model: "fable"`), so it doesn't conflict with an "always spawn workers on Sonnet" default.

## Install the skill itself

- **Claude Code / Desktop Code mode (filesystem):** put the `fable-advisor/` folder in `~/.claude/skills/` (or in a git-tracked skills dir that `~/.claude/skills` symlinks to). Claude Code discovers it at session start.
- **From the `.skill` package:** import `fable-advisor.skill` through Claude Desktop's Skills feature (opening the file surfaces a "Save skill" card).
- With just the skill installed, use it on demand via `/fable-advisor`. Add the CLAUDE.md rule above to get automatic firing.

## Requirements

- **Opus-main only.** The skill applies only when the session's main model is Opus — Fable advising Fable is waste, and the setup runs Opus as the orchestrator. (See the precondition in `SKILL.md`.)
- **Fable access** on your account/plan, and the **Anthropic API** — the advisor/Fable tier is not available via Bedrock, Google Cloud, or Foundry.