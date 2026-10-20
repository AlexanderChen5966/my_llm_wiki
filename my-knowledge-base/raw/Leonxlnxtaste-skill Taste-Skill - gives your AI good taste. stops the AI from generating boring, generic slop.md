---
title: "Leonxlnx/taste-skill: Taste-Skill - gives your AI good taste. stops the AI from generating boring, generic slop"
source: "https://github.com/Leonxlnx/taste-skill"
author:
  - "[[Leonxlnx]]"
published:
created: 2026-06-09
description: "Taste-Skill - gives your AI good taste. stops the AI from generating boring, generic slop  - GitHub - Leonxlnx/taste-skill: Taste-Skill - gives your AI good taste. stops the AI from generating boring, generic slop"
tags:
  - "clippings"
status: processed
---
[![Taste Skill - Anti-slop Agent Skills for premium frontends](https://github.com/Leonxlnx/taste-skill/raw/main/assets/readme-banner.png)](https://github.com/Leonxlnx/taste-skill/blob/main/assets/readme-banner.png)

## Taste Skill

*The Anti-Slop Frontend Framework for AI Agents*

[![Taste Skill](https://github.com/Leonxlnx/taste-skill/raw/main/assets/taste-skill-logo.webp)](https://tasteskill.dev/ "Taste Skill - tasteskill.dev")

Portable **Agent Skills** that upgrade AI-built interfaces: stronger layout, typography, motion, and spacing instead of boilerplate-looking UIs. This repo also includes **image-generation skills** for reference boards (web, mobile, brand kits). Pair them with **ChatGPT Images** or similar generators, then hand the frames to Codex, Cursor, or Claude Code for implementation.

## Disclaimer

Taste Skill has no official token, coin, or crypto project. Any token using my name, image, or project is unaffiliated and not endorsed by me.

<sub><a href="#disclaimer">Disclaimer</a> · <a href="#installing">Install</a> · <a href="#skills">Skills</a> · <a href="#settings-taste-skill-only">Settings</a> · <a href="#examples">Examples</a> · <a href="#support-the-project">Sponsor</a> · <a href="#research">Research</a> · <a href="#common-questions">FAQ</a> · <a href="#license">License</a></sub>

## Feedback & Contributions

We would love your feedback. Suggestions and bug reports:

- Open a Pull Request or Issue on GitHub
- DM [@lexnlin](https://x.com/lexnlin) or [@blueemi99](https://x.com/blueemi99)
- Email us at [hello@tasteskill.dev](mailto:hello@tasteskill.dev)

## Installing

The [`npx skills add`](https://github.com/vercel-labs/agent-skills) CLI scans the `skills/` folder in this repo, so **all skills below (code and image-generation) install the same way.**

```
npx skills add https://github.com/Leonxlnx/taste-skill
```

Install a single skill by its **install name** (the `name:` field inside the SKILL frontmatter, not the folder name):

```
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"
```

You can also copy any `SKILL.md` into your project or paste it into ChatGPT / Codex conversations.

### Updating from the previous version

The default `taste-skill` (install name `design-taste-frontend`) is now **v2 (experimental)**, a substantial rewrite of the original v1. If you already have v1 installed, just re-run the install command and you will be upgraded:

```
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"
```

The install name did not change, so no script updates are needed. The newer SKILL.md replaces the older one in place.

If you depend on the exact behavior of v1 and want to pin to it explicitly:

```
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend-v1"
```

See [CHANGELOG.md](https://github.com/Leonxlnx/taste-skill/blob/main/CHANGELOG.md) for the full v1 to v2 diff and the rationale.

## Skills

Each skill does one job; you do not need all of them at once. **Implementation skills** output code. **Image-generation skills** output reference images only.

The `Install name` column is the exact value you pass to `--skill`.

| Skill (folder) | Install name | Description |
| --- | --- | --- |
| **taste-skill** | `design-taste-frontend` | 🆕 **v2 (experimental)** - substantial rewrite of the default skill. Reads the brief, infers the design language, tunes three dials (VARIANCE / MOTION / DENSITY). Brief inference, design-system map, hard em-dash ban, canonical GSAP code skeletons, redesign-audit protocol, strict pre-flight check. Actively iterating toward v2.0.0 stable. |
| **taste-skill-v1** | `design-taste-frontend-v1` | The original v1 of taste-skill, preserved for projects depending on its exact behavior. Use only if the v2 default breaks something specific in your workflow. |
| **gpt-tasteskill** | `gpt-taste` | Stricter variant for GPT/Codex: higher layout variance, stronger GSAP direction, aggressive anti-slop. |
| **image-to-code-skill** | `image-to-code` | Image-first pipeline: generate site references, analyze them, then implement the frontend to match. |
| **redesign-skill** | `redesign-existing-projects` | Existing projects: audit the UI first, then fix layout, spacing, hierarchy, styling. |
| **soft-skill** | `high-end-visual-design` | Polished, calm, expensive UI with softer contrast, whitespace, premium fonts, spring motion. |
| **output-skill** | `full-output-enforcement` | When the model ships half-finished work: full output, no placeholder comments. |
| **minimalist-skill** | `minimalist-ui` | Editorial product UI (Notion/Linear vibes), restrained palette, crisp structure. |
| **brutalist-skill** | `industrial-brutalist-ui` | Hard mechanical language: Swiss type, sharp contrast, experimental layout. |
| **stitch-skill** | `stitch-design-taste` | Google Stitch-compatible rules, including optional `DESIGN.md` export format. |

### Image generation skills

These produce design images only (no code). Use with ChatGPT Images, Codex image mode, or any agent that generates images.

| Skill (folder) | Install name | Description |
| --- | --- | --- |
| **imagegen-frontend-web** | `imagegen-frontend-web` | Website comps: hero, landing, multi-section with strong typography, spacing, anti-slop art direction. |
| **imagegen-frontend-mobile** | `imagegen-frontend-mobile` | Mobile screens and flows: iOS/Android/cross-platform, mockups, readable type, coherent sets. |
| **brandkit** | `brandkit` | Brand-kit boards: logo directions, palettes, type, identity applications across categories. |

### Which one should I use?

- Start with **taste-skill** for the safest general default. (Now v2 experimental - see what changed in the [CHANGELOG](https://github.com/Leonxlnx/taste-skill/blob/main/CHANGELOG.md).)
- If you depend on the exact behavior of the original taste-skill, install **taste-skill-v1** instead.
- Use **gpt-taste** when you want the stricter GPT/Codex-oriented rules and motion/layout enforcement.
- Use **image-to-code-skill** for image → analyze → code website workflows.
- Use **redesign-skill** to improve an existing codebase instead of greenfield styling.
- Add **soft-skill**, **minimalist-skill**, or **brutalist-skill** when the visual direction is already chosen.
- Add **output-skill** if the agent keeps truncating output.
- Use **imagegen-frontend-web**, **imagegen-frontend-mobile**, or **brandkit** when the deliverable is **images** (comps, flows, identity boards), then pass results to your coding agent.

### Image-first tip

For **image-to-code-skill**, state the pipeline in the prompt, e.g.: `follow the skill: generate images, then analyze, then code`.

### ChatGPT Images and Codex

Attach or paste **`imagegen-frontend-web`**, **`imagegen-frontend-mobile`**, or **`brandkit`** and ask for the frames you need, then feed the renders to Codex, Cursor, or Claude Code. Use **image-to-code-skill** when you want one workflow that both generates references and implements the site in code.

## Settings (taste-skill only)

Numbers at the top of the file are 1-10 dials:

- **DESIGN\_VARIANCE**: Layout experimentation (lower: centered/clean · higher: asymmetric/modern).
- **MOTION\_INTENSITY**: Animation depth (lower: hover · higher: scroll/magnetic).
- **VISUAL\_DENSITY**: Information per viewport (lower: spacious · higher: dense dashboards).

## Examples

Created with taste-skill:

[![](https://github.com/Leonxlnx/taste-skill/raw/main/examples/floria-top.webp)](https://github.com/Leonxlnx/taste-skill/blob/main/examples/floria-top.webp) [![](https://github.com/Leonxlnx/taste-skill/raw/main/examples/floria-bottom.webp)](https://github.com/Leonxlnx/taste-skill/blob/main/examples/floria-bottom.webp)

## Support the project

If Taste Skill helps you, consider sponsoring:

[Sponsor on GitHub](https://github.com/sponsors/Leonxlnx)

### Current Sponsors

[![dnakov](https://github.com/dnakov.png "dnakov")](https://github.com/dnakov) [![AkramReshad](https://github.com/AkramReshad.png "AkramReshad")](https://github.com/AkramReshad) [![ajmalaksar25](https://github.com/ajmalaksar25.png "ajmalaksar25")](https://github.com/ajmalaksar25) [![krikkkk](https://github.com/krikkkk.png "krikkkk")](https://github.com/krikkkk) [![navanchauhan](https://github.com/navanchauhan.png "navanchauhan")](https://github.com/navanchauhan) [![robinebers](https://github.com/robinebers.png "robinebers")](https://github.com/robinebers) [![JKc66](https://github.com/JKc66.png "JKc66")](https://github.com/JKc66) [![u2393696078-rgb](https://github.com/u2393696078-rgb.png "u2393696078-rgb")](https://github.com/u2393696078-rgb) [![a-human-created-this](https://github.com/a-human-created-this.png "a-human-created-this")](https://github.com/a-human-created-this) [![AtharvaJaiswal005](https://github.com/AtharvaJaiswal005.png "AtharvaJaiswal005")](https://github.com/AtharvaJaiswal005) [![ghughes7](https://github.com/ghughes7.png "ghughes7")](https://github.com/ghughes7) [![mccun934](https://github.com/mccun934.png "mccun934")](https://github.com/mccun934)[![Star History Rank](https://camo.githubusercontent.com/5ad78f6fece30abf7a9132613f43110411b99bf2e0036e53a4c65955a0cdd203/68747470733a2f2f6170692e737461722d686973746f72792e636f6d2f62616467653f7265706f3d4c656f6e786c6e782f74617374652d736b696c6c)](https://www.star-history.com/leonxlnx/taste-skill)## Research

Background writing that shaped these skills lives in [`research/`](https://github.com/Leonxlnx/taste-skill/blob/main/research).

## Common Questions

**How is this different from other AI design skills?**  
Multiple specialized variants, adjustable dials in key skills, anti-repetition rules informed by dedicated research. All are framework agnostic across major coding agents.

**Does it work with React, Vue, Svelte?**  
Yes. Rules target design intent, not a single framework API.

**What is SKILL.md?**  
A portable instruction file agents can load automatically; install via `npx skills add` or by copying into a repo or conversation.

**Do image-generation skills install with `npx skills add`?**  
Yes. They live under `skills/` alongside the code skills so the same CLI discovers them.

## License

[MIT License](https://github.com/Leonxlnx/taste-skill/blob/main/LICENSE) · Copyright (c) 2026 Leonxlnx