---
title: "microsoft/SkillOpt: SkillOpt is a text-space optimizer that trains reusable natural-language skills for frozen LLM agents through trajectory-driven edits, validation-gated updates, and deployable best_skill.md artifacts."
source: "https://github.com/microsoft/SkillOpt"
author:
  - "[[microsoft]]"
published:
created: 2026-06-30
description: "SkillOpt is a text-space optimizer that trains reusable natural-language skills for frozen LLM agents through trajectory-driven edits, validation-gated updates, and deployable best_skill.md artifacts. - microsoft/SkillOpt"
tags:
  - "clippings"
status: processed
---
## SkillOpt: Executive Strategy for Self-Evolving Agent Skills

*Train agent skills like you train neural networks — with epochs, (mini-)batchsize, learning rates, and validation gates — but without touching model weights.*

[![microsoft%2FSkillOpt | Trendshift](https://camo.githubusercontent.com/8f7e3457ddf58dbf34da260d45930d9612206f518d456b1078f714628b3ffde4/68747470733a2f2f7472656e6473686966742e696f2f6170692f62616467652f7472656e6473686966742f7265706f7369746f726965732f33383439382f6461696c793f6c616e67756167653d507974686f6e)](https://trendshift.io/repositories/38498?utm_source=trendshift-badge&utm_medium=badge&utm_campaign=badge-trendshift-38498)> 📖 **For installation, data preparation, training/eval commands, the full configuration reference, and framework internals, see the [Documentation & Reproduction Guide](https://microsoft.github.io/SkillOpt/docs/guideline.html)** (rendered on GitHub Pages).

---

## News 🔥🔥🔥

- **\[2026-06-15\]** 😴 **SkillOpt-Sleep (preview)** — a nightly offline self-evolution companion for local coding agents (Claude Code / Codex / Copilot): review past sessions, replay recurring tasks, and consolidate validated skills behind a held-out gate. See **[`docs/sleep/README.md`](https://github.com/microsoft/SkillOpt/blob/main/docs/sleep/README.md)** for what it is, how to use it, and results.
- **\[2026-06-03\]** 🎉 **[gbrain](https://github.com/garrytan/gbrain), [gbrain-evals](https://github.com/garrytan/gbrain-evals/blob/main/docs/benchmarks/2026-06-03-skillopt.md), and [darwin-skill](https://github.com/alchaincyf/darwin-skill) have all integrated SkillOpt.**
- **\[2026-06-02\]** 🎉 **SkillOpt [v0.1.0](https://github.com/microsoft/SkillOpt/releases/tag/v0.1.0) is now available on [PyPI](https://pypi.org/project/skillopt/)!** Install with `pip install skillopt`. This initial release includes the full training loop (rollout → reflect → aggregate → select → update → evaluate), multi-backend support (OpenAI / Azure / Claude / Qwen / MiniMax), six built-in benchmarks, and WebUI dashboard.

---

## Overview

Modern agent skills are usually hand-crafted, generated one-shot by a strong LLM, or evolved through loosely controlled self-revision — none of which behaves like a deep-learning optimizer for the skill itself, and none of which reliably improves over its starting point under feedback.

**SkillOpt treats the skill document as the trainable state of a frozen agent**, and trains it with the discipline that makes weight-space optimization reproducible. A separate optimizer model turns scored rollouts into bounded add / delete / replace edits on a single skill document; a candidate edit is accepted only when it strictly improves a held-out validation score. A textual learning-rate budget, a rejected-edit buffer, and an epoch-wise slow / meta update make skill training stable while adding **zero inference-time model calls** at deployment.

The deployed artifact is a compact `best_skill.md` (typically 300–2,000 tokens) that runs against the unchanged target model. Across **six benchmarks, seven target models, and three execution harnesses** (direct chat, Codex CLI, Claude Code CLI), SkillOpt is best or tied-best on **all 52 evaluated (model, benchmark, harness) cells** and on GPT-5.5 lifts the average no-skill accuracy by **+23.5 points in direct chat, +24.8 inside the Codex agentic loop, and +19.1 inside Claude Code**. Optimized skill artifacts transfer across model scales, between Codex and Claude Code harnesses, and to nearby benchmarks without further optimization.

For the full method, ablations, and per-cell results see the [paper](https://arxiv.org/abs/2605.23904); for a visual walkthrough of the loop see the [project page](https://microsoft.github.io/SkillOpt/); for deeper API / backend / benchmark docs see [`docs/`](https://github.com/microsoft/SkillOpt/blob/main/docs).

## 🎬 Demo Video

64c8f76086bed7bd7a5ce664a7a14f40\_raw.mp4<video src="https://private-user-images.githubusercontent.com/29210256/597425176-eb12d3bc-371c-467f-904d-91b61f339ed7.mp4?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODI3ODU0MDIsIm5iZiI6MTc4Mjc4NTEwMiwicGF0aCI6Ii8yOTIxMDI1Ni81OTc0MjUxNzYtZWIxMmQzYmMtMzcxYy00NjdmLTkwNGQtOTFiNjFmMzM5ZWQ3Lm1wND9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjA2MzAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYwNjMwVDAyMDUwMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTc3ZTBiZTlkOTExN2E4YzQ5OTU1Njk4NDcwNDU3OTRlM2JkNTA1YTNhMmVlZTBhMTg2MTY2YWY0MzNhNmRmY2YmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT12aWRlbyUyRm1wNCJ9.Vu88nX2tHGwL6uOPVTzNCM088PaiemN8A-MZmQnOMIg" controls="controls"></video>

[**▶ Watch the full demo on YouTube**](https://youtu.be/JUBMDTCiM0M)

---

## Extensibility & WebUI

### Adding a new backend

A backend = a chat / exec target (e.g. `openai_chat`, `claude_chat`, `qwen_chat`, `minimax_chat`, `codex_exec`, `claude_code_exec`). See [`docs/guide/new-backend.md`](https://github.com/microsoft/SkillOpt/blob/main/docs/guide/new-backend.md) for the full contract; in short you add a `skillopt/model/<name>_backend.py` module, register it in `skillopt/model/common.py` + `backend_config.py`, and wire it through the router in `skillopt/model/__init__.py`. `qwen_backend.py` and `minimax_backend.py` are good templates.

### Adding a new benchmark

A benchmark = a `skillopt/envs/<name>/` package with a `dataloader.py`, a `rollout.py`, and an `initial.md` seed skill. See [`docs/guide/new-benchmark.md`](https://github.com/microsoft/SkillOpt/blob/main/docs/guide/new-benchmark.md) for the full contract; the simplest reference is `skillopt/envs/searchqa/`.

### WebUI

Launch the monitoring dashboard (optional):

```
pip install -e ".[webui]"
python -m skillopt_webui.app
```

| Flag | Default | Description |
| --- | --- | --- |
| `--port` | 7860 | Server port |
| `--host` | `0.0.0.0` | Bind address |
| `--share` | off | Create a public Gradio share link |

---

## Citation

```
@article{yang2026skillopt,
  title={Skillopt: Executive strategy for self-evolving agent skills},
  ={Yang, Yifan and Gong, Ziyang and Huang, Weiquan and Yang, Qihao and Zhou, Ziwei and Huang, Zisu and Li, Yan and Gao, Xuemei and Dai, Qi and Liu, Bei and others},
  journal={arXiv preprint arXiv:2605.23904},
  year={2026}
}
```