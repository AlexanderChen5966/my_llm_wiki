---
title: "AGENTS.md 深度解析：为 AI 代理而生的 README"
source: "https://juejin.cn/post/7647845266864209970"
author:
  - "[[周末程序猿]]"
published: 2026-06-07
created: 2026-07-20
description: "如果你同时用 Cursor、Claude Code、Copilot、Codex 写代码，大概率被一件事困扰过：每换一个工具，就要重新「教」AI 一遍项目怎么构建、怎么测试、代码风格是什么。AGENTS"
tags:
  - "clippings"
status: processed
---
> AGENTS.md 它为什么出现、怎么写、怎样才算写得好
> 
> 基于 agents.md 官方规范、GitHub 对 2,500+ 仓库的实证分析及行业资料整理 · 2026

如果你同时用 Cursor、Claude Code、Copilot、Codex 写代码，大概率被一件事困扰过：每换一个工具，就要重新「教」AI 一遍项目怎么构建、怎么测试、代码风格是什么。AGENTS.md 就是为了终结这种重复劳动而生的开放标准。本文由浅入深，讲清楚它为什么出现、怎么写、以及怎样才算写得好。

## 一、从一个真实的痛点说起

2024 年以来，AI 编码代理经历了一场「寒武纪大爆发」。OpenAI Codex、Anthropic Claude Code、Cursor、GitHub Copilot、Google Jules、Aider、Cline……几乎每周都有新工具。但繁荣背后藏着一个尴尬的协调问题： **标准碎片化** 。

每个工具都发明了自己的「项目指令文件」：

结果就是项目根目录变成了一个塞满规则文件的「垃圾抽屉」（junk drawer）。这些文件内容高度重复——都在讲同一个项目的同一套构建命令和编码规范，却必须维护好几份、放在好几个地方。更糟的是它们无法跨工具迁移：你在用 Cursor 的同事精心写的规则，到了用 Claude Code 的你这里完全读不到。

> **核心矛盾** ：项目知识（构建步骤、测试命令、编码约定）本应只有一份，却被工具的私有格式撕裂成了 N 份。

## 二、AGENTS.md 是什么

2025 年 8 月，由 **OpenAI（Codex）、Amp、Google（Jules）、Cursor、Factory** 等一线厂商联合发起，推出了 [AGENTS.md](https://link.juejin.cn/?target=https%3A%2F%2Fagents.md%2F "https://agents.md/") ——一个简单、开放、供应商中立的代理指令文件标准。它现已由 Linux 基金会下的 Agentic AI Foundation 托管。

官方对它最精炼的定位是一句话：

> **「A README for agents」** ——一份写给 AI 代理看的 README。

这个类比极具传播力。 `README.md` 是写给人看的：项目简介、快速上手、贡献指南。而 `AGENTS.md` 装的是代理需要、但塞进 README 会显得啰嗦的细节：精确的构建步骤、测试命令、命名约定、安全红线。两者刻意分开，体现了它的三条设计哲学：

- **关注点分离** ：README 保持简洁面向人类，AGENTS.md 信息密集面向机器。
- **互操作与开放** ：代理无关（agent-agnostic），目标是「一个文件，适配所有代理」，避免厂商锁定。
- **极致简洁** ：就是标准 Markdown，没有强制字段、没有 schema、没有新依赖。会写 README 就会写它。

「仅仅是 Markdown」并非偷懒，而是一项战略选择：把采纳门槛降到最低，优先追求网络效应，让整个生态先就「同一个文件名、同一个位置」达成共识——这是建立事实标准的经典打法。截至目前，已有 **超过 6 万个** 开源项目采用了 AGENTS.md。

## 三、为什么需要它：四个层次的理解

「减少重复文件」只是表面。深入看，AGENTS.md 的价值有四层，从浅到深：

### 第 1 层：统一入口，消除碎片

最直接的好处——把 N 份私有规则文件收敛成一份。换工具不再需要重新「教」AI，知识可在团队、工具间无缝流转。

### 第 2 层：实用的上下文管理

当前 LLM 受限于上下文窗口大小，且每个 token 都是成本与延迟。AGENTS.md 提供了一个 **集中、专门** 的位置存放项目指令，相当于给提示词「降噪」，把宝贵的 token 用在最相关的信息上，从而提升代理响应的准确性与相关性。它本质上是给当代 LLM 配的一份「项目长期记忆 / 操作手册」。

### 第 3 层：倒逼更好的工程文化（Forcing Function）

这是最被低估的一层。传统的 `CONTRIBUTING.md` 常被人类忽略，写得再烂也没人立刻「报错」。但 AGENTS.md 不一样——它会被 AI 立即阅读并执行， **代理表现的好坏，直接反映了你指令质量的高低** 。

为了让代理给出「具体命令」，团队被迫标准化并清晰记录构建/测试流程；为了让代理行为可靠，团队被迫保持文档与代码同步。于是 AI 成了一个不知疲倦、绝对客观的「文档消费者」，通过工作表现持续给文档质量打分。这个反馈回路反过来推动团队的 DevOps 与文档成熟度——而这些清晰文档同样惠及新加入的人类成员。

### 第 4 层：代理技术栈中的「知识与上下文层」

把它放进整个 AI 代理技术栈看会更清楚。用「心智—神经系统—身体」来类比：

| 层 | 角色 | 对应组件 |
| --- | --- | --- |
| 心智（推理与知识） | 提供先验知识与行为准则 | **AGENTS.md** = 项目长期记忆 / 操作手册；LLM = 推理引擎 |
| 神经系统（编排） | 协调推理、感知与行动的循环 | LangChain / LangGraph / AutoGen / CrewAI 等框架 |
| 身体（行动与感知） | 对外部世界产生实际影响 | Function Calling / Tool Calling / MCP |

AGENTS.md 既不是框架，也不是执行引擎，它是 **静态、声明式的上下文** ——在代理的编排循环开始之前，预先「设定」好它的心智。理解这一定位，对设计代理系统的架构师至关重要：它决定了控制、逻辑与安全措施应该放在技术栈的哪一层。

## 四、怎么写：最小可用版本

规范本身没有任何强制字段，纯标准 Markdown。最简单的做法：在 **仓库根目录** 创建 `AGENTS.md` ，写上你会告诉一个新同事的东西即可。官方的最小示例长这样：

```markdown
代码解读复制代码# AGENTS.md

## Setup commands
- Install deps: \`pnpm install\`
- Start dev server: \`pnpm dev\`
- Run tests: \`pnpm test\`

## Code style
- TypeScript strict mode
- Single quotes, no semicolons
- Use functional patterns where possible
```

官方建议覆盖这些「该被记下来」的内容：

- **项目概述与结构** ：核心功能、关键目录用途（如「前端在 `/webapp` ，API 在 `/server` 」）。
- **设置 / 构建 / 测试命令** ：可直接复制粘贴的 shell 命令。 *这是最关键的部分* ——代理会据此自我验证、甚至自动跑测试。
- **编码约定与风格** ：如「Python 遵循 PEP8」「单引号、无分号」。
- **架构原则** ：MVC、微服务、Redux 数据流等既定模式。
- **提交与 PR 规范** ：如 commit / PR 标题格式 `[<project>] <Title>` 。
- **安全注意事项** ：如「所有数据库查询必须用参数化 SQL 防注入」。

> 关于行为冲突： **离被编辑文件最近的 AGENTS.md 优先级最高** ；而用户在对话中的显式指令，可以覆盖一切。如果你列了测试命令，代理会主动执行并尝试修复失败，直到全绿。

## 五、怎样才算写得好

会写不等于写得好。GitHub 工程师 Matt Nigh 分析了公开仓库里 **2,500+ 份 AGENTS.md** ，得出了一条非常清晰的分水岭：失败的文件几乎都因为 **太含糊** 。

> 「You are a helpful coding assistant」毫无用处； 「You are a test engineer who writes tests for React components, follows these examples, and **never modifies source code** 」才有用。

### 5.1 高质量文件的六个共性

| 原则 | 怎么做 |
| --- | --- |
| **命令前置** | 把可执行命令放在靠前的章节，带上参数： `npm test` 、 `pytest -v` ，而不只是工具名。 |
| **代码示例胜过解释** | 一段展示风格的真实代码，胜过三段文字描述。直接「show，don't tell」。 |
| **明确边界** | 告诉 AI 什么 *绝对不能碰* （密钥、vendor 目录、生产配置）。「Never commit secrets」是出现最多的有效约束。 |
| **技术栈要具体** | 写「React 18 + TypeScript + Vite + Tailwind」，不要写「一个 React 项目」。带上版本与关键依赖。 |
| **覆盖六大核心区** | 命令、测试、项目结构、代码风格、git 工作流、边界——全覆盖就进入第一梯队。 |

### 5.2 三档边界（Three-tier Boundaries）

GitHub 总结的最佳实践里，最实用的是用三档标签界定权限——它能直接防止破坏性操作：

- ✅ **Always 总是做**
- ⚠️ **Ask first 先问再做**
- 🚫 **Never 绝不做**

### 5.3 一份「写得好」的完整范例

下面是 GitHub 官方给出的、用于定义「文档代理」人格的范例（带 YAML frontmatter）：

```markdown
代码解读复制代码---
name: docs_agent
description: Expert technical writer for this project
---

You are an expert technical writer for this project.

## Your role
- You are fluent in Markdown and can read TypeScript code
- Your task: read code from \`src/\` and update docs in \`docs/\`

## Project knowledge
- **Tech Stack:** React 18, TypeScript, Vite, Tailwind CSS
- **File Structure:**
  - \`src/\`   – Application source code (you READ from here)
  - \`docs/\`  – All documentation (you WRITE to here)
  - \`tests/\` – Unit, integration, Playwright tests

## Commands you can use
Build docs: \`npm run docs:build\`        (checks for broken links)
Lint markdown: \`npx markdownlint docs/\` (validates your work)

## Boundaries
- ✅ Always do:  Write new files to \`docs/\`, follow style examples, run markdownlint
- ⚠️ Ask first:  Before majorly rewriting existing docs
- 🚫 Never do:   Modify code in \`src/\`, edit config files, commit secrets
```

它好在哪？ **角色清晰** （它是谁、会什么、做什么）、 **命令可执行且前置** 、 **技术栈带版本** 、 **用真实代码示例代替抽象描述** 、 **三档边界防止误删误改** 。

### 5.4 维护层面的四条铁律

- **明确且简洁** ：避免歧义。普遍建议单文件控制在 **~150 行内** ，别让重要信号被淹没。
- **命令用反引号包裹** ：给代理一个明确信号去解析、复制、执行，无需猜测。
- **与代码同步更新** ：把它当代码对待，纳入 code review。构建/规范一变，它就得跟着变。
- **链接而非复制** ：维持单一信息源。需要详细文档就放链接，别把 README 整段搬过来。

### 5.5 一条来自实战的心法

> 别想着一次写完美。 **从最小开始 → 测试 → 当代理犯错时再补细节。** 最好的 AGENTS.md 是迭代长出来的，不是前期规划出来的。

## 六、进阶：Monorepo 与嵌套发现

单一根级文件对大型 monorepo 不够用。AGENTS.md 设计了 **层次化发现机制** ：你可以在任意子目录放置嵌套的 `AGENTS.md` ，代理会自动读取离当前工作目录 **最近** 的那一份，就近覆盖上层的通用指令——和 `.gitignore` 、`.eslintrc` 的就近原则一致，非常符合直觉。

```
代码解读复制代码my-monorepo/
├── AGENTS.md            # 全局通用约定
├── apps/
│   ├── web/
│   │   └── AGENTS.md    # React + Vite 专属命令
│   └── api/
│       └── AGENTS.md    # Go 测试流程专属
└── packages/
    └── ui/
        └── AGENTS.md    # 组件库规范
```

这避免了根文件因塞满所有子项目指令而臃肿。一个常被引用的数据： **OpenAI 自家主仓库一度包含多达 88 份 AGENTS.md** ，充分说明该机制在超大型项目中的实用性。

## 七、生态与「标准之战」

AGENTS.md 势头很猛，但标准化之路并不平坦。这是一场经典的「开放联盟 vs. 私有生态」之争。

| 标准 / 文件名 | 主要支持者 | 格式 | 关键特性 | 互操作性 |
| --- | --- | --- | --- | --- |
| `AGENTS.md` | OpenAI、Google(Jules)、Cursor、Amp、Factory、Zed、Aider、opencode… | 标准 Markdown | 通用开放标准，支持层次化发现 | 高 |
| `CLAUDE.md` | Anthropic | Markdown | 针对 Claude 优化，支持 `@import` | 低（专有） |
| `GEMINI.md` | Google (Gemini CLI) | Markdown | 针对 Gemini 优化 | 低（专有） |
| `.cursor/rules` | Cursor（高级） | 带 frontmatter 的 Markdown | 基于路径/描述的高级规则匹配 | 低（专有） |

Anthropic 的 `CLAUDE.md` 和 Google 的 `GEMINI.md` 仍在坚守自家格式，这是普遍标准化的主要障碍。但来自开发者的实用主义压力非常强：Claude Code 仓库里那条「请支持 AGENTS.md」的 issue 收获了大量点赞，社区还自发搞出了变通方案——

**实用变通：兼容多工具的两种姿势**

```bash
代码解读复制代码# 方案 A：符号链接，让旧工具也能读到同一份内容
mv CLAUDE.md AGENTS.md && ln -s AGENTS.md CLAUDE.md

# 方案 B：在 CLAUDE.md 里 import（CLAUDE.md 内容只有一行）
@AGENTS.md
```

各家工具的接入也很轻量，例如：

```yaml
代码解读复制代码# Aider —— .aider.conf.yml
read: AGENTS.md
```
```json
代码解读复制代码// Gemini CLI —— .gemini/settings.json
{ "context": { "fileName": "AGENTS.md" } }
```

> opencode / 多数工具：在 TUI 里运行 `/init` 即可自动生成或更新。

这种自下而上的压力，很可能最终倒逼坚守者至少把 AGENTS.md 作为备选方案纳入支持。

## 八、安全：一个新的攻击面

> ⚠️ **必读** ：AGENTS.md 的内容会被代理直接加载、并成为 system prompt 的一部分。这意味着它天然是\*\*提示注入（prompt injection）\*\*的载体。

设想一个攻击场景：攻击者向某个公开仓库提交了恶意 `AGENTS.md` 。当你毫无防备地在该项目上运行 AI 代理时，代理读取了这份文件，其中的指令可能诱导它：

- **数据泄露** ：读取 `.env` 、私钥等敏感文件并外发到攻击者服务器。
- **任意命令执行** ：若代理有 shell 权限，可能被诱导执行反弹 shell 等危险命令。
- **供应链攻击** ：在代码里植入难以察觉的后门。

「仅仅是 Markdown」在这里是把双刃剑：它没有任何内置安全模型、沙箱或权限系统，安全责任被完全转移给开发者与运行时。缓解策略：

- **视为代码** ：强制 code review + 版本控制，任何改动都要审。
- **审查第三方** ：对来自不可信仓库的 AGENTS.md，授权代理前先人工通读。
- **最小权限** ：文件里绝不放 API key、密码、连接串等敏感数据。
- **用强化代理** ：让代理在沙箱中运行，对危险操作要么拒绝、要么请求显式授权。
- **反向利用** ：用它主动写入安全规则（如「SQL 必须参数化」），引导代理生成更安全的代码。

## 九、争议与局限：它是终点吗？

客观看待，社区对 AGENTS.md 也有不少质疑，值得了解：

- **文档重复** ：对 AI 重要的信息，对人类（尤其新人）往往同样重要。维护 README + AGENTS.md 两套，有信息不同步风险，违背单一信息源。
- **「保姆式照管」反功能** ：有人认为，手把手把指令喂给一个本应足够聪明、能自行推断的 AI，与「AI 简化工作」的承诺背道而驰。
- **单文件结构受限** ：复杂项目里，扁平的单文件不够用；社区呼吁 `.agents/` 目录式的多文件按需加载。相比之下 Cursor 的多文件规则系统更先进。
- **Token 成本与延迟** ：每行字都在每次交互消耗 token，是真金白银和响应延迟。
- **可靠性** ：LLM 非确定性使代理几轮后可能「忘记」指令，难以构建完全可靠的自动化。

更深的哲学分歧是： **README.md vs. AGENTS.md，到底该不该分** ？一种前瞻判断是—— **AGENTS.md 很可能是一项「过渡性 / 脚手架」技术** 。它是为应对 *当代* LLM 局限（上下文窗口有限、推理不完美）而生的务实方案。当未来模型能直接理解组织良好的整个代码库（README、注释、源码、设计文档）时，专门的指令文件或许就不再必要。

但这恰恰调和了争论双方：它在 **当下** 确实有效，而批评者对 **未来** 的判断也可能正确。它所倡导的原则——标准化、关注点分离、明确指令——正在为下一代更复杂的代理协作（ `PLAN.md` 、 `ARCHITECTURE.md` 、规范驱动开发、A2A 协议）铺路。

**参考来源**

- [agents.md 官方规范](https://link.juejin.cn/?target=https%3A%2F%2Fagents.md%2F "https://agents.md/")
- [github.com/agentsmd/ag…](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fagentsmd%2Fagents.md "https://github.com/agentsmd/agents.md")
- [GitHub Blog：How to write a great agents.md — Lessons from over 2,500 repositories](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.blog%2Fai-and-ml%2Fgithub-copilot%2Fhow-to-write-a-great-agents-md-lessons-from-over-2500-repositories "https://github.blog/ai-and-ml/github-copilot/how-to-write-a-great-agents-md-lessons-from-over-2500-repositories")