---
title: "🛠️ Harness Engineering — Quick Actionable Guide 🤖"
source: "https://dev.to/truongpx396/harness-engineering-quick-actionable-guide-2b93"
author:
  - "[[Truong Phung]]"
published: 2026-04-20
created: 2026-05-22
description: "Distilled from the Learn Harness Engineering course by WalkingLabs, which synthesizes harness... Tagged with ai, llm, agents, webdev."
tags:
  - "clippings"
status: processed
---
*Distilled from the [Learn Harness Engineering](https://github.com/walkinglabs/learn-harness-engineering) course by WalkingLabs, which synthesizes harness engineering theory and practice from [OpenAI](https://openai.com/index/harness-engineering/), [Anthropic](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents), and industry practitioners.*

> **The model is smart. The harness makes it reliable.**  
> — A well-harnessed agent spent \\$200/6hrs and built a working game. Without a harness, same model spent \\$9/20min and produced garbage. The model didn't change. The harness did.

**📊 Hard numbers:**

- 📉 SWE-bench Verified top agents: ~50-60% pass rate — on *curated* tasks with clear descriptions. In real repos? Even lower.
- 🏗️ OpenAI's million-line experiment: 3 engineers + Codex → ~1,500 PRs over 5 months, ~3.5 PRs/person/day — but only after investing heavily in harness infrastructure.
- 📄 A team added `AGENTS.md` to a FastAPI project: same model went from failing all 3 runs to succeeding all 3 runs, with ~60% better context efficiency.
- 🚀 A TypeScript + React project went from 20% success rate (bare repo) → 100% (full harness). Model never changed.

---

## 📑 Table of Contents

## 🔍 What Is Harness Engineering?

Harness engineering is **building a complete working environment around an AI coding agent** so it produces reliable results. It's NOT about writing better prompts. It's about designing the **system** the model operates inside.

```
You → give task → Agent reads harness files → Agent executes
                                                |
                                      harness governs every step:
                                      ├── Instructions: what to do, in what order
                                      ├── Scope: one feature at a time
                                      ├── State: progress log, feature list, git history
                                      ├── Verification: tests, lint, type-check
                                      └── Lifecycle: init at start, clean state at end
                                                |
                                                v
                                      Agent stops ONLY when
                                      verification passes
```

---

## 🧩 The 5 Subsystems of a Harness

Every effective harness has exactly five parts:

| # | Subsystem | Job | Key Files |
| --- | --- | --- | --- |
| 1 | 📋 **Instructions** | Tell the agent what to do, in what order, what to read first | `AGENTS.md`, `CLAUDE.md`, `docs/` |
| 2 | 💾 **State** | Track what's done, in-progress, and next. Persisted to disk so the next session picks up exactly where the last left off | `claude-progress.md`, `feature_list.json`, `git log` |
| 3 | ✅ **Verification** | Only passing tests count as evidence. Agent cannot declare victory without proof | tests, lint, type-check, smoke runs |
| 4 | 🎯 **Scope** | Constrain agent to ONE feature at a time. No overreach. No half-finishing three things | feature boundaries, definition of done |
| 5 | 🔄 **Session Lifecycle** | Initialize at start. Clean up at end. Leave a clean restart path | `init.sh`, handoff notes, clean commits |

---

## 🚀 Quick Start: Your Minimal Harness (Do This Today)

Drop these 4 files into your project root:

```
YOUR PROJECT ROOT
├── AGENTS.md              ← the agent's operating manual
├── init.sh                ← runs install + verify + health check
├── feature_list.json      ← what features exist, which are done
├── claude-progress.md     ← what happened each session
└── src/                   ← your actual code
```

### 1\. AGENTS.md — The Operating Manual

```
# Agent Instructions

## Before Starting Any Work
1. Run \`./init.sh\` to verify environment health
2. Read \`claude-progress.md\` for context from last session
3. Read \`feature_list.json\` to see what's done and what's next
4. Check \`git log --oneline -10\` for recent changes

## Rules
- Work on exactly ONE feature at a time
- Never declare "done" without passing tests
- Run the full test suite before committing
- Update \`claude-progress.md\` after every session
- Update \`feature_list.json\` when a feature status changes
- Commit only when the project is in a clean, resumable state

## Verification Checklist
- [ ] All tests pass
- [ ] Linter passes
- [ ] Type-check passes
- [ ] Feature works as specified
```

### 2\. init.sh — Environment Health Check

```
#!/bin/bash
set -e
echo "=== Installing dependencies ==="
npm install
echo "=== Running tests ==="
npm test
echo "=== Type checking ==="
npx tsc --noEmit
echo "=== Environment healthy ==="
```

### 3\. feature\_list.json — Machine-Readable Scope

```
{
  "features": [
    { "id": "F001", "name": "User login", "status": "done", "tests": "src/auth.test.ts" },
    { "id": "F002", "name": "Document import", "status": "in-progress", "tests": "src/import.test.ts" },
    { "id": "F003", "name": "Search", "status": "not-started", "tests": null }
  ]
}
```

### 4\. claude-progress.md — Session Memory

```
# Progress Log

## Session 3 — 2026-04-20
- Completed: F001 (user login) — all tests passing
- In progress: F002 (document import) — parser done, validation pending
- Blocked: none
- Next session should: finish F002 validation logic, then run full test suite
```

---

## 📐 The 12 Core Principles

### Principle 1: Strong Models ≠ Reliable Execution

**Problem:** Models ace benchmarks but fail on real multi-file engineering tasks.  
**Why:** Real tasks need multi-step coordination, not one-shot answers. Agents fail at five specific layers:

1. **Task specification** — vague requirements → agent guesses wrong
2. **Context provision** — implicit conventions not written down → agent violates them
3. **Execution environment** — missing deps, wrong versions → agent wastes context on setup
4. **Verification feedback** — no tests → agent says "done" when it's not
5. **State management** — no progress tracking → next session starts from zero

**Action:** When things fail, **fix the harness first, not the model.** Attribute every failure to one of these five layers. One `AGENTS.md` file might be more effective than upgrading to a more expensive model.

> **Diagnostic Loop:** Execute → observe failure → attribute to a specific harness layer → fix that layer → re-execute. After a few rounds, your harness gets stronger and agent performance stabilizes.

### Principle 2: A Harness Is 5 Subsystems, Not a Better Prompt

**Problem:** People think "better prompt = better results."  
**Why:** Prompts don't persist state, verify work, or control scope.  
**Action:** Implement all 5 subsystems: instructions, state, verification, scope, lifecycle.

**Real data:** A team added subsystems one at a time to a TypeScript + React project:

- Stage 1 (bare repo): 20% success rate
- Stage 2 (+AGENTS.md): 60%
- Stage 3 (+verification commands): 80%
- Stage 4 (+progress files): 80-100%

**The feedback/verification subsystem has the lowest investment and highest ROI.** Start there.

> **Constrain, don't micromanage.** Use executable rules, not step-by-step instructions. OpenAI: "enforce invariants, don't micromanage implementation."
> 
> **Harness debt is real.** Harness rots like code does. Audit regularly — remove outdated rules, update stale docs.

### Principle 3: The Repo Is the Single Source of Truth

**Problem:** If the agent can't see it in the repo, it doesn't exist. Your Slack history, Jira tickets, Confluence pages, verbal agreements — the agent sees **none** of it.  
**Why:** Agents don't remember conversations. They read files. They can't ask a colleague.  
**Action:** Put everything the agent needs INTO the repo: instructions, progress, feature definitions, docs.

**Cold-Start Test** — open a fresh agent session (no verbal context) and see if it can answer:

1. What is this system?
2. How is it organized?
3. How do I run it?
4. How do I verify it?
5. What's the current progress?

If it can't answer → your repo has blind spots. Where the map is blank, the agent guesses — and guessing creates bugs.

**ACID Principles for Agent State:**

- **Atomicity** — Each logical operation gets one git commit. If it fails midway, `git stash` to roll back.
- **Consistency** — All tests pass, lint reports zero errors before committing. Inconsistent states don't get committed.
- **Isolation** — Multiple agents use separate progress files or git branches.
- **Durability** — Cross-session knowledge must be persisted to files. What's only in memory doesn't count.

**Place knowledge near code.** A 50-line `ARCHITECTURE.md` in `src/api/` is more useful than a 500-page Confluence doc nobody maintains.

### Principle 4: Split Instructions — Don't Use One Giant File

**Problem:** One massive instruction file overwhelms the agent's context. A team's `AGENTS.md` grew from 50 → 600 lines. Their agent success rate dropped from 72% to 45%.  
**Why:** Three killers:

- **Lost in the Middle Effect** (Liu et al., 2023): LLMs use info in the middle of long texts far less effectively than at the beginning or end. A critical rule at line 300 of 600 will likely be ignored.
- **Context budget eaten alive**: A 600-line file consumes 10-20K tokens before the agent even starts working.
- **Priority conflicts**: Hard constraints, soft guidelines, and historical notes all look identical. The agent can't distinguish.

**Action:** Keep `AGENTS.md` at **50-200 lines** as a routing file. Split details into topic docs:

```
AGENTS.md (50-200 lines — overview + hard constraints + links)
├── docs/api-patterns.md        ← read when adding endpoints
├── docs/database-rules.md      ← read when modifying DB operations  
├── docs/testing-standards.md   ← reference when writing tests
└── docs/deployment.md          ← read only for deployment tasks
```

**Rules for the entry file:**

- Put critical rules at the **top or bottom**, never the middle
- Max 15 non-negotiable hard constraints
- Every instruction should have: a source (why), applicability (when), and expiry (when to remove)
- Audit regularly — delete outdated rules like you delete unused dependencies

**Result:** After splitting, a team's success rate went from 45% → 72%, and security constraint compliance went from 60% → 95%.

### Principle 5: Persist Context Across Sessions

**Problem:** Session 2 starts fresh. Agent has no memory of Session 1. Tasks over 30 minutes see failure rates spike sharply without state.  
**Why:** Without persisted state, the agent re-does work, reverses deliberate decisions, or drifts from requirements like a game of telephone.

**Context Anxiety** (discovered by Anthropic): When agents sense context is running low, they rush — skipping verification, choosing simple solutions over correct ones. Like guessing on remaining exam questions when time is almost up.

**Action:** Use three continuity artifacts:

1. **`claude-progress.md`** — What's done, what's in progress, what's blocked, next steps
2. **`DECISIONS.md`** — Why option B was chosen over A, with date and reasoning. Prevents the next session from reversing deliberate choices.
3. **Git commits as checkpoints** — Commit after each atomic unit of work with clear messages explaining what and why.

**Key metric: Rebuild Cost** — how long a new session takes to reach an executable state. Good harnesses: ~3 minutes. Bad harnesses: 15-20 minutes.

**Real data:** A 12-feature blog system over 5 sessions:

- Without progress files: 58% features completed, 43% hidden defect rate
- With progress files: 100% features completed, 8% hidden defect rate, rebuild time reduced ~78%
```
WITHOUT STATE                        WITH STATE
Session 1: does work                 Session 1: does work, writes progress
Session 2: starts from zero          Session 2: reads progress, continues
Result: rework & drift               Result: steady forward progress
```

### Principle 6: Initialize Before Every Session

**Problem:** Agent starts coding in a broken environment (missing deps, failing tests). Code written before the test framework is configured = code without verification.  
**Why:** Initialization and implementation have different goals. Mixing them = doing both poorly. Anthropic's data: dedicated initialization phase → **31% higher feature completion** in multi-session scenarios.  
**Action:** Treat initialization as a **separate phase**. The first session does ONLY initialization — no feature code.

**Bootstrap Contract** — initialization is complete when four conditions are met:

1. ✅ Can start (`make setup` succeeds)
2. ✅ Can test (at least one example test passes)
3. ✅ Can see progress (task breakdown file exists)
4. ✅ Can pick up next steps (progress file is readable)

**Warm start >> Cold start.** Use project templates to preset standard structure. Starting from a template is 10x better than starting from an empty directory.

**Time invested in initialization is fully recovered in the next 3-4 sessions.**

### Principle 7: One Feature at a Time — No Overreach (WIP=1)

**Problem:** Agents try to do 3 things at once, finish none of them properly. Context capacity C divided by k tasks = each task gets C/k attention. When that drops below minimum, nothing finishes.  
**Why:** Overreach and under-finish amplify each other. More code written ≠ more features completed — Anthropic found they're **negatively correlated**. Agents using "small next step" (WIP=1) show **37% higher task completion**.  
**Action:** Enforce WIP=1 (Work-in-Progress Limit from Kanban). Write it explicitly in `AGENTS.md`:

```
## Work Rules
- Work on one feature at a time
- Only start the next feature after the current one passes end-to-end verification
- Don't "also refactor" feature B while implementing feature A
```

**Real data:** REST API with 8 features:

- Unconstrained: 5 features activated, ~800 lines across 12 files, 20% end-to-end pass → 37.5% completion by session 3
- WIP=1: 1 feature at a time, ~200 lines across 4 files, 100% pass → 87.5% completion by session 4

**"Do less but finish" always beats "do more but leave half-done."**

### Principle 8: Feature Lists Are Harness Primitives

**Problem:** Vague scope = vague results. "Shopping cart mostly done" — what does "mostly" mean? Which tests passed? Nobody knows.  
**Why:** Feature lists aren't memos — they're the **backbone** of the harness. The scheduler, verifier, and handoff reporter all depend on them.  
**Action:** Every feature entry must be a **triple**: `(behavior description, verification command, current state)`

```
{
  "id": "F03",
  "behavior": "POST /cart/items with {product_id, quantity} returns 201",
  "verification": "curl -X POST localhost:3000/api/cart/items -d '{\"product_id\":1}' | jq .status",
  "state": "not_started"
}
```

**State machine** — four states, controlled by the harness:

- `not_started` → `active` (agent picks it up)
- `active` → `passing` (only when verification command succeeds — **pass-state gating**)
- `active` → `blocked` (dependency issue)
- `passing` is **irreversible** — once verified, it stays verified

**Granularity rule:** Each feature should be completable in one session. "User can add items to cart" = good. "Implement the shopping cart" = too broad. "Create the name field on the Cart model" = too narrow.

**Result:** Structured feature lists show **45% higher completion rate** than free-form tracking, with zero duplicate implementations.

### Principle 9: Don't Let Agents Declare Victory Early

**Problem:** Agent says "done!" but tests fail, edge cases are broken, or code doesn't compile. Anthropic found that **agents confidently praise their own work** — you must separate "the person who does the work" from "the person who checks the work."  
**Why:** **Verification Gap** = the gap between the agent's confidence and actual correctness. This is the #1 failure mode.  
**Action:** Write an explicit **Definition of Done** for every task. Not "add a search feature" but:

```
Completion criteria:
- New endpoint GET /api/search?q=xxx
- Supports pagination, default 20 items
- Results include highlighted snippets
- All new code passes pytest
- Type checking passes (mypy --strict)
```

"Done" = verification passes. "The code looks fine" does NOT count. `curl returns 201` DOES count.

### Principle 10: Only Full-Pipeline Verification Counts

**Problem:** Agent runs one unit test. Claims everything works.  
**Why:** Partial verification misses integration issues, type errors, and regressions.  
**Action:** Require the full pipeline: `tests + lint + type-check + build + smoke run`. All must pass.

### Principle 11: Make the Agent's Runtime Observable

**Problem:** You can't fix what you can't see. Missing observability wastes **30-50% of session time** on redundant diagnosis.  
**Why:** Without observability: agents can't distinguish "correct" from "looks correct," retries become blind guesses, and evaluation becomes subjective.  
**Action:** Build **two layers** of observability:

**Layer 1 — Runtime signals:** Application lifecycle, feature path execution, errors with full context. The harness collects these automatically — don't rely on the agent to log its own actions.

**Layer 2 — Process observability:**

- **Sprint contracts** — before each task, define: what to change, what NOT to change, pass criteria, exclusions
- **Evaluator rubrics** — turn "is it good?" into structured scoring:

| Dimension | A | B | C | D |
| --- | --- | --- | --- | --- |
| Code correctness | All tests pass | Main flow passes | Partial pass | Build fails |
| Test coverage | Main + edge cases | Main flow only | Skeleton only | No tests |

**Real data:** A "dark mode" task — without observability: 3-4 blind retries, 45 minutes. With sprint contract + rubric: 1 iteration, 15 minutes. **3x efficiency.**

### Principle 12: Every Session Must Leave a Clean State

**Problem:** Agent leaves half-committed code, broken tests, uncommitted changes. "Clean up later" = never clean up.  
**Why:** Entropy grows by default (Lehman's Laws). Without cleanup, a 12-week project degrades:

|  | Week 1 | Week 12 (no cleanup) | Week 12 (with cleanup) |
| --- | --- | --- | --- |
| Build pass rate | 100% | 68% | 97% |
| Test pass rate | 100% | 61% | 95% |
| Session startup | 5 min | 60+ min | 9 min |

**Action:** Session completion = task passes verification **AND** clean state check passes. Five dimensions:

```
## Session Exit Checklist
- [ ] Build passes (npm run build)
- [ ] All tests pass (npm test)  
- [ ] Feature list + progress updated
- [ ] No debug code remaining (console.log, debugger, TODO)
- [ ] Standard startup path works (npm run dev)
```

**Quality Document** — maintain an active scorecard for each module (A/B/C/D). New sessions read it and know where to prioritize.

**Periodically simplify the harness.** As models improve, some constraints become unnecessary overhead. Monthly: disable one harness component, run benchmarks. If results don't degrade → remove it permanently.

---

## 🔄 The Agent Session Lifecycle (Follow This Every Time)

```
START
  1. Agent reads AGENTS.md
  2. Agent runs init.sh (install, verify, health check)
  3. Agent reads claude-progress.md (what happened last time)
  4. Agent reads feature_list.json (what's done, what's next)
  5. Agent checks git log (recent changes)

SELECT
  6. Agent picks exactly ONE unfinished feature
  7. Agent works ONLY on that feature

EXECUTE
  8. Agent implements the feature
  9. Agent runs verification (tests, lint, type-check)
  10. If verification fails → fix and re-run
  11. If verification passes → record evidence

WRAP UP
  12. Agent updates claude-progress.md
  13. Agent updates feature_list.json
  14. Agent records what's still broken or unverified
  15. Agent commits (only when safe to resume)
  16. Agent leaves clean restart path for next session
```

---

## ⚖️ Without Harness vs. With Harness

|  | Without Harness | With Harness |
| --- | --- | --- |
| 🟡 **Session start** | Agent starts fresh, no context | Agent reads progress, picks up where it left off |
| 🟡 **Scope** | Agent does random things | Agent works on one specific feature |
| 🟡 **"Done"** | Agent says "looks good" | Tests pass, lint clean, types check |
| 🟡 **Session end** | Half-committed mess | Clean state, progress logged, ready for next session |
| 🟡 **Your role** | Rescue & cleanup | Review & approve |
| 🟡 **Result** | You spend more time fixing than if you did it yourself | Agent does the work, you verify the result |

---

## 📝 Additional Templates Worth Adding

### DECISIONS.md — Prevent Next Session From Reversing Your Choices

```
# Design Decisions

## 2026-04-15: Use Redis for user preferences caching
- Reason: High read frequency (every API call), small data size
- Rejected: PostgreSQL materialized view (high change frequency)
- Constraint: Cache TTL of 5 minutes, active invalidation on write

## 2026-04-18: Use Vitest over Jest
- Reason: Native ESM support, faster execution
- Constraint: All test files use .test.ts extension
```

### Sprint Contract — For Complex Tasks

```
# Sprint Contract: Dark Mode Support

## Scope
- Modify the theme toggle component
- Update global CSS variables
- Add dark mode tests

## Verification Standards
- Visual regression tests pass
- Main flow E2E tests pass
- No flash of unstyled content (FOUC)

## Exclusions
- NOT handling print styles
- NOT handling third-party component dark mode
```

### Richer feature\_list.json — With Verification Evidence

```
{
  "features": [
    {
      "id": "F01",
      "behavior": "POST /api/register with {email, password} returns 201",
      "verification": "curl -X POST /api/register -d '{\"email\":\"test@example.com\"}' | jq .status",
      "state": "passing",
      "evidence": "commit abc123, test output log"
    },
    {
      "id": "F02",
      "behavior": "GET /api/search?q=xxx returns paginated results (default 20)",
      "verification": "pytest tests/test_search.py -x",
      "state": "active",
      "evidence": null
    }
  ]
}
```

---

## 🎯 Key Takeaways

1. 🔧 **The harness doesn't make the model smarter — it makes its output reliable**
2. 📂 **Everything the agent needs must live in the repo** (if it can't see it, it doesn't exist)
3. 🎯 **One feature at a time** — scope is the most underrated lever
4. ✅ **Never trust "done" without verification evidence** — tests must pass
5. 🧹 **Every session must leave a clean state** — the next session's success depends on it
6. 💾 **State must persist to disk** — memory dies between sessions, files don't
7. 🔄 **Initialize before work, verify during work, clean up after work** — this is the lifecycle
8. 🔍 **When things fail, fix the harness first** — attribute failures to one of five layers, fix that layer, re-run
9. 1️⃣ **WIP=1** — finish one feature before starting the next. Less code, more completed features
10. ⚠️ **Harness debt is real** — audit and simplify regularly. Rules that helped last month may be unnecessary overhead today
11. 🏁 **"Do less but finish" beats "do more but leave half-done"** — always

---

## 🔬 How to Diagnose Harness Quality

**Isometric Model Control:** Keep the model fixed. Remove harness components one at a time. Measure which removal causes the biggest performance drop. That's your bottleneck — focus your effort there.

| Remove this... | If performance drops significantly... |
| --- | --- |
| AGENTS.md | Your instructions are critical |
| Verification commands | Your feedback loop is carrying the weight |
| Progress files | Your state management is essential |
| Feature list | Your scope control is doing the heavy lifting |
| init.sh | Your initialization is preventing cascading failures |

---

## 📚 References

- [OpenAI: Harness Engineering](https://openai.com/index/harness-engineering/)
- [Anthropic: Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
- [Anthropic: Harness Design for Long-Running Apps](https://www.anthropic.com/engineering/harness-design-long-running-apps)
- [LangChain: The Anatomy of an Agent Harness](https://blog.langchain.com/the-anatomy-of-an-agent-harness/)
- [Thoughtworks: Harness Engineering](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html)
- [HumanLayer: Skill Issue — Harness Engineering for Coding Agents](https://www.humanlayer.dev/blog/skill-issue-harness-engineering-for-coding-agents)
- [Lost in the Middle: How Language Models Use Long Contexts (Liu et al., 2023)](https://arxiv.org/abs/2307.03172)
- [Course Website](https://walkinglabs.github.io/learn-harness-engineering/en/)

## \- GitHub Repo

> If you found this helpful, let me know by leaving a 👍 or a comment!, or if you think this post could help someone, feel free to share it! Thank you very much! 😃[MongoDB](https://dev.to/mongodb)Promoted

[![MongoDB Atlas image](https://media2.dev.to/dynamic/image/width=775%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fi.imgur.com%2FT7NQ2OL.jpeg)](https://www.mongodb.com/cloud/atlas/lp/try3?utm_campaign=display_dev.to-webdev_pl_flighted_atlas_tryatlaslp_prosp_gic-null_ww-all_dev_dv-all_eng_leadgen&utm_source=dev.to&utm_medium=display&utm_content=prototype&bb=263177)

## Build fast on MongoDB Atlas without the fear of outgrowing.

Don't let your database dictate your speed. With MongoDB Atlas, the same document model you use for your MVP handles global scale across AWS, Azure, and Google Cloud. Start free and stay fast as you grow.