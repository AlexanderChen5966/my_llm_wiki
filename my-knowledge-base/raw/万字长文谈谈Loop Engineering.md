---
title: "万字长文谈谈Loop Engineering"
source: "https://juejin.cn/post/7663708874660364324"
author:
  - "[[周末程序猿]]"
published: 2026-07-19
created: 2026-07-20
description: "万字长文谈谈Loop Engineering，Google 工程师**Addy Osmani**发表了一篇博客，将这个概念正式命名为**Loop Engineering**（循环工程）..."
tags:
  - "clippings"
status: processed
---
## 一、什么是 Loop Engineering

### 1.1 引爆点：两句话，百万次转发

2026 年 6 月初，AI 编程社区被两句话点燃：

> **Peter Steinberger** （OpenClaw 创始人）："You shouldn't be prompting coding agents anymore. You should be designing loops that prompt your agents."

> **Boris Cherny** （Anthropic Claude Code 负责人）："I don't prompt Claude anymore. I have loops running that prompt Claude and figuring out what to do. My job is to write loops."

这两句话在 X（Twitter）上获得了超过 500 万次浏览。随后，Google 工程师 **Addy Osmani** 发表了一篇博客，将这个概念正式命名为 **Loop Engineering** （循环工程），并给出了清晰的定义框架。开发者 **Cobus Greyling** 则推出了开源参考实现 `loop-engineering` （MIT 协议，7K+ Star），将理念变成了可直接落地的工具集。

那么，Loop Engineering 到底是什么？

### 1.2 核心定义

**Loop Engineering** 是一种系统设计方法论：你不再亲自给 AI Agent 写提示词，而是设计一套自动化循环系统，让系统自己去发现工作、分配任务、验证结果、持久化状态，并在必要时交还给人。

用 Addy Osmani 的话说：

> Loop engineering is replacing yourself as the person who prompts the agent. You design the system that does it instead.

杠杆点已经从"打磨单条 prompt"移到了"设计编排 Agent 的控制系统"。

### 1.3 AI 工程的四次范式跃迁

过去两年，AI 开发的重心经历了四次清晰的跃迁：

| 阶段 | 核心问题 | 人的角色 | 典型产物 |
| --- | --- | --- | --- |
| Prompt Engineering | 怎么问 AI？ | 提示词工匠 | 精心打磨的 prompt 模板 |
| Context Engineering | 给 AI 什么信息？ | 上下文组织者 | AGENTS.md、CLAUDE.md、规则文件 |
| Harness Engineering | 如何组织 AI 的能力？ | 环境搭建者 | 工具链、MCP 连接器、权限配置 |
| Loop Engineering | 如何让 AI 持续创造结果？ | 系统架构师 | 自动化循环、状态文件、验证链 |

每一次跃迁，人的角色都在向上移动：从"操作工"到"监工"再到"架构师"。Loop Engineering 是这条演进路线的当前终点——你不再是那个守在聊天框前不断输入指令的人，你是那个设计自动化循环结构的人。

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/f6bea72751c34546aba97f4b1d381a0c~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1785067411&x-signature=XuZMYp%2BmBbsbODwsK%2FPwbhqxihM%3D)

### 1.4 Agent Loop vs Loop Engineering

很多人把 **Agent Loop** 和 **Loop Engineering** 混为一谈——以为在终端里敲 `/loop 1d` 就算"做 loop 工程"了。其实不然：

| 维度 | Agent Loop | Loop Engineering |
| --- | --- | --- |
| 本质 | 运行机制 / 产品功能 | 系统设计 / 工程方法论 |
| 你在做什么 | 启动一个会重复的 Agent 任务 | 设计发现→执行→验证→交接的完整系统 |
| 粒度 | 一次递归目标 + 调度 | 模式、技能、状态 schema、安全策略、成本模型 |
| 成功标准 | "它又在跑了" | "它跑得对、跑得省、出事能停、人能看懂它干了什么" |
| 典型产物 | /loop 1d... 一条命令 | STATE.md + Skills + Worktree 策略 + Verifier + Checklist |

打个比方： **Agent Loop** 像 `while (!done) { agent.run(); }` ——循环语句本身。 **Loop Engineering** 像写整个 `main()` ：输入从哪来、状态存哪、谁写谁验、超时怎么办、日志打哪、什么时候 `break` 叫人。

### 1.5 三层概念关系

参考库的 concepts 文档把几层概念捋得很清楚：

```
代码解读复制代码Harness = 单次 Agent 运行的环境（工具、权限、规则）
Loop    = Harness + 调度 + 状态 + 验证链
Loop Engineering = 设计并运营上述 Loop 系统的工程实践
```

| 概念 | 关注点 | 类比 |
| --- | --- | --- |
| Agent Harness Engineering | 一次会话里 Agent 能用什么、知道什么 | 单个工位的工具箱 |
| Agent Loop | 让 Agent 按节奏反复跑 | 传送带的运转 |
| Loop Engineering | 整条产线如何发现任务、分工、质检、交接 | 工厂设计与 SOP |

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/4b360ce98919438ca7f1b8cee04d1f32~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1785067411&x-signature=VZ3M5V29L4yIO5MsgEATrYS%2BNrw%3D)

## 二、Loop Engineering 的六大构件

一个能真正"无人值守"跑起来的 Loop，不是一条长 prompt，而是六个部分精密协作的系统。

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/83290111869a47eda8a4dd00e254b4e5~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1785067411&x-signature=2or5htwWckACMoqC7yPeHtJ%2Bum4%3D)

### 2.1 Automations / Scheduling（自动化调度）—— 心跳

没有调度，你只有一个一次性的 Agent 运行。调度是 Loop 的心跳。

**常见实现方式：** `/loop` （Grok、Claude Code）—— 定时重复执行 `/schedule` —— 在特定时间点触发

- GitHub Actions cron —— 团队级定时任务 `/goal` —— 运行直到可验证条件为真
- 自定义 harness 调度器

**关键属性：** 间隔时间、是否立即触发、单次还是重复、是否持久化（重启后仍存活）。

### 2.2 Worktrees（工作树）—— 安全并行

当两个 Agent 同时编辑同一个文件时，你会得到合并地狱。Git worktree（或等效的隔离检出）给每个 Agent 自己的工作目录——共享历史但不共享工作树。

在 Grok 中：传递 `isolation: "worktree"` 给子 Agent。在 Claude Code 中：使用 `--worktree` 或专用会话。

**清理很重要** ——Loop 应该在任务完成或移交时删除 worktree。 `loop-worktree` CLI 使这变得机械化：每次尝试一个 worktree，在 manifest 中跟踪，在拒绝或升级时清理。

### 2.3 Skills（技能）—— 意图的持久记忆

一个 Skill（通常是 `SKILL.md` + 可选的脚本/参考文件）编码了：

- 项目约定
- "我们不这样做，因为 X 事故"
- 构建/测试/lint 命令
- 审查标准
- 领域知识

没有 Skills，Loop 每次运行都从零开始重新推导一切——这就是 **Intent Debt（意图债）** 。Skills 是偿还意图债的方式：约定写一次，每次运行都读。

### 2.4 Plugins & Connectors（MCP）—— 连接真实世界

一个只能读文件系统的 Loop 是受限的。连接器让 Loop 能够：

- 读写 Linear / Jira 工单
- 发送 Slack / Discord 消息
- 查询数据库或内部 API
- 创建 GitHub 分支和 PR
- 触发部署或 runbook

MCP（Model Context Protocol）已成为通用基板，为一个工具编写的连接器通常可以在另一个工具中工作。

### 2.5 Sub-agents（子 Agent）—— Maker / Checker 分离

这是可靠 Loop 最重要的结构模式。

写代码的 Agent 是评判自己工作的糟糕法官。第二个 Agent（有时使用更强的模型，总是使用不同的指令）执行验证。

**常见分离模式：**

- Explorer → Implementer → Verifier
- Implementer → Security reviewer
- Implementer → Test writer + runner

在无人值守的 Loop 中，Verifier 是让你（人类）能够放心走开的关键。

### 2.6 Memory / State（记忆/状态）—— 跨会话的脊柱

模型在独立的轮次或会话之间没有长期记忆。Loop 必须读写持久化的东西：

- 仓库中的 `STATE.md` 或 `LOOP-STATE.json`
- Linear 看板或 GitHub Project 的专用区域
- 小型数据库行

**好的状态回答三个问题：**

1. 我们当前在做什么？
2. 上次我们尝试了什么，结果如何？
3. 什么在等待人类？

状态文件通常是 Loop 产生的最重要的产物。

### 2.7 构件组装顺序

一个最小可行 Loop 通常从以下开始：

```
代码解读复制代码调度 + 一个 Skill（分诊）+ 状态文件
```

然后逐步添加：

- 当你开始做修改时 → 添加 Worktree 隔离
- 当 Loop 自主行动时 → 添加子 Agent 验证
- 当你想让它驱动工单和 PR 时 → 添加连接器

最好的 Loop 是每个新构件只在前一个版本证明了其价值（和失败模式）后才添加的。

## 三、LangChain 视角：四层 Loop 模型

如果说 Addy Osmani 的六大构件回答了"Loop 由什么组成"，那么 LangChain 团队在《The Art of Loop Engineering》中提出的四层 Loop 模型则回答了"Loop 如何层层叠加、持续进化"。Swyx 将这种层层叠加的艺术称为 **Loopcraft** ——"the art of stacking loops"。

这四层 Loop 不是并列关系，而是 **自底向上、逐层增强** 的堆叠结构。每一层都建立在下一层之上，外层 Loop 可以"伸手进去"修改内层 Loop 的配置。

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/0cb598c31dde4a71a12bb1245ae3dd8d~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1785067411&x-signature=v2ZQhjAEbuVTGcNl4cf%2FV1cGeJA%3D)

### 3.1 Loop 1：Agent 循环 —— 最基础的"模型调用工具"

这是所有 Agent 的起点：给 LLM 上下文，让它调用工具，循环直到任务完成。

```
代码解读复制代码from langchain.agents import create_agent

# 最基础的 Agent Loop：模型 + 工具 = 能行动的 Agent
agent = create_agent(
    model="claude-sonnet-4-20250514",
    tools=[read_file, write_docs, open_pr],
    system_prompt="你是一个文档助手，负责改进项目文档。"
)

# 一次调用，Agent 内部自动循环调用工具直到完成
result = agent.invoke({
    "messages": [{"role": "user", "content": "更新 README 中的安装说明"}]
})
```

这就是 LangChain `create_agent` 给的东西。选一个模型，接入工具，你就有了一个能工作的 Agent Loop。以 LangChain 内部的文档 Agent 为例：它接收文档改进请求 → 模型规划并起草修改 → 用工具 clone 仓库、读写文件、打开 PR。

**但问题也很明显：** Agent 的产出不一定正确或一致。它可能在第一次尝试时就出错，而你没有机制去发现。

### 3.2 Loop 2：验证循环 —— "写完不算完，验过才算"

当一致性很重要时，在 Agent Loop 外面包一层验证循环：检查输出，不合格就带着反馈重试。

```
代码解读复制代码from langchain.agents import create_agent
from deepagents.middleware import RubricMiddleware

# RubricMiddleware 实现验证循环：
# Agent 产出 → Grader 按评分标准检查 → 不合格就带反馈重试
agent = create_agent(
    model="claude-sonnet-4-20250514",
    tools=[read_file, write_docs, open_pr],
    middleware=[
        RubricMiddleware(
            rubric="""
            1. 所有链接必须可解析
            2. 所有 CI 检查必须通过
            3. diff 范围不能超出请求的修改
            """,
# 最多重试 3 次

        )
    ]
)
```

验证循环的核心是 **Grader（评分器）** ——它按评分标准检查 Agent 的输出。Grader 可以是确定性的（如运行测试、检查链接），也可以是 Agentic 的（LLM-as-a-Judge，用另一个模型来评判）。

对于文档 Agent：Grader 在每次尝试后运行测试，检查所有链接是否可解析、CI 是否通过、diff 是否在请求范围内。这些错误类型不需要人工审查就能自动捕获。

**代价：** 验证循环增加了延迟和每次运行的成本。但当质量比速度更重要时——大多数生产场景都是如此——这个代价是值得的。

### 3.3 Loop 3：事件驱动循环 —— "Agent 不再是你手动调用的东西"

前两层 Loop 自动化了"做"和"验"，但 Agent 仍然需要你手动触发。事件驱动循环把 Agent 接入你的生态系统——一个新文档到达、一个定时器触发、一个 webhook 到来——Agent 自动运行。

```
代码解读复制代码from langsmith import deployment

# LangSmith Deployment 支持 cron 和 webhook 触发
# 将 Agent 部署为持续运行的后台服务
deployment.serve(
    agent=agent,
    triggers=[
        # Cron 触发：每天早上 9 点运行
        deployment.CronTrigger(schedule="0 9 * * *"),
        # Webhook 触发：Slack 消息到达时运行
        deployment.WebhookTrigger(
            source="slack",
            channel="#docs-plz"
        )
    ]
)
```

LangChain 的文档 Agent 就是通过 **Fleet** （无代码 Agent 构建器）的 channels 和 schedules 来实现事件驱动：每当 `#docs-plz` Slack 频道有消息，就触发文档 Agent 运行。

**关键转变：** Agent 不再是你手动调用的东西——它是持续运行在更大系统中的组件。这也是 OpenClaw 中 "heartbeats" 概念的精髓：把你的 Agent 变成一个始终在线、主动出击的助手。

### 3.4 Loop 4：爬山循环 —— "自动化改进本身"

前三层 Loop 自动化了工作。第四层——也是最重要的一层—— **自动化了改进** 。

```
代码解读复制代码from langsmith import Engine

# LangSmith Engine 分析生产 Trace，自动改进 Harness 配置
engine = Engine(
    agent=agent,
    analysis_prompt="""
    分析最近的 Agent 运行 Trace，找出：
    1. 哪些 prompt 导致了低质量输出？
    2. 哪些工具调用经常失败？
    3. Grader 的评分标准是否需要调整？
    """,
    auto_improve=True  # 自动应用改进
)

# Engine 持续监控，发现问题自动提交改进 PR
engine.monitor()
```

每一次 Agent 运行都会产生 **Trace** ：模型做了什么、调用了哪些工具、Grader 给了什么反馈。这些 Trace 包含关于"什么有效、什么无效"的高价值信号。爬山循环运行一个分析 Agent 来处理这些 Trace，用发现来重写 Harness 配置——包括 prompt 调整、工具配置优化、Grader 标准改进。

**关键动作：** 返回箭头不只是回到顶层——它 **伸手进去直接更新 Agent Loop** 。外层 Loop 的每一次循环都让内层 Loop 更有效。

对于更进一步的团队，爬山循环还可以：

- 对使用开源模型的团队，将 Trace 结果作为 RL 微调的训练信号
- 改进辅助上下文（记忆、检索到的 Skills）
- 自动 A/B 测试不同的 prompt 策略

### 3.5 四层 Loop 总览

| Loop | 做什么 | 影响 | LangChain 原语 |
| --- | --- | --- | --- |
| 1\. Agent 循环 | 模型反复调用工具直到任务完成 | 自动化工作 | `create_agent` |
| 2\. 验证循环 | Agent 产出按评分标准检查，不合格带反馈重试 | 确保质量和正确性 | `RubricMiddleware` |
| 3\. 事件驱动循环 | 事件触发 Agent 运行，更新真实系统 | 规模化自动工作 | LangSmith Deployment (cron/webhook) 或 Fleet channels |
| 4\. 爬山循环 | 生产 Trace 喂给分析 Agent，改进 Harness 配置 | Harness 持续改进 | LangSmith Engine |

**LangChain 团队的核心观点：** Loop 1 和 Loop 2 我们已经思考了很久。但重点应该转向 Loop 3 和 Loop 4——把 Agent 嵌入你的生态系统，让它们根据你的标准持续改进。这才是价值复利产生的地方。

Satya Nadella 对此有一个精辟的总结："那些早期构建学习循环的公司——人类判断力和 token 资本共同复利——将建立起难以复制的优势。"

### 3.6 四层模型与六大构件的关系

LangChain 的四层模型和 Addy Osmani 的六大构件是 **互补视角** ，而非竞争关系：

| 六大构件 | 在四层模型中的位置 |
| --- | --- |
| Scheduling（调度） | Loop 3 的核心——cron/webhook 触发 |
| Worktrees（工作树） | Loop 1/2 的隔离基础——并行安全 |
| Skills（技能） | Loop 1 的上下文注入 + Loop 4 的优化目标 |
| MCP Connectors（连接器） | Loop 1 的工具层 + Loop 3 的事件源 |
| Sub-agents（子 Agent） | Loop 2 的 Maker/Checker 分离 |
| State/Memory（状态） | 跨所有 Loop 的持久化脊柱 |

**简单来说：六大构件是"零件清单"，四层模型是"组装说明书"。**

## 四、如何设计一个 Loop

### 4.1 Loop 的解剖结构

一个完整的 Loop 运行周期包含以下阶段：

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/a71d9410380a418692b3d145b83e1f76~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1785067411&x-signature=2cKTlhl5mBr2thz%2BrOHrV1PXh9Y%3D)

### 4.2 自主级别：L0 → L1 → L2 → L3

Loop Engineering 强调 **分阶段放权** ，绝不一步到位。完整的自主级别从 L0 草稿开始：

| 级别 | 描述 |
| --- | --- |
| L0 Draft | Loop 设计文档 + 手动触发，验证流程正确性 |
| L1 Report Only | 分诊 → 写入状态，不自动执行任何操作 |
| L2 Assisted | 小型自动修复 + Verifier 验证 |
| L3 Unattended | 无需人工监控，自主运行 |

**L0 Draft 是大多数团队忽略的起点：** 你先写一份 LOOP.md 设计文档，手动运行每个步骤，确认分诊逻辑、状态读写、升级触发都符合预期。L0 阶段不涉及任何自动化——你在验证"这个 Loop 的设计本身是否正确"。

**关键原则：永远不要在新模式的生产仓库上跳过 L1。** 从 L0 到 L3，每一级都必须在前一级稳定运行后才推进。

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/27b12036019849f785bed836bc6856db~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1785067411&x-signature=Zfj%2FQ56cDNM1QFYrhI%2FnHJS89sI%3D)

### 4.3 Loop 设计的前提

在启用生产 Loop 之前，必须通过以下 10 个维度的检查：

#### 1\. 目的与范围

- 单一明确目标——一句话：这个 Loop 完成什么？
- 明确的非目标——这个 Loop \*\* 不会\*\* 做什么？
- 监控范围——哪些仓库、分支、PR 或工单？
- 分阶段上线——先只报告，再小步行动？

#### 2\. 调度

- 选择节奏——间隔匹配紧急程度
- 是否立即触发——启动时运行，还是等待间隔？
- 持久化——如果需要，是否在会话/工具重启后存活？
- 自我清理——监控列表为空时 `scheduler_delete` ？

#### 3\. Skills

- 分诊 Skill 存在，输出格式紧凑
- 操作 Skill（minimal-fix 等）匹配项目约定
- Skill 描述无聊且具体（良好的自动触发）

#### 4\. Maker / Checker 分离

- Implementer 和 Verifier 是分离的（Agent、模型或指令）
- Implementer **不能** 标记自己的工作为"完成"
- Verifier 在隔离环境（worktree）中运行测试后才批准

#### 5\. 状态 / 记忆

- 状态文件或看板 schema 已文档化
- Loop 每次运行开始时读取先前状态
- Loop 写入结果、时间戳、最后操作
- 每次运行清理已解决/已合并/已关闭的项目

#### 6\. 人工交接

- 升级触发器明确（最大尝试次数、风险路径、模糊性）
- 路径黑名单——auth、payments、secrets、infra
- 通知规则——仅在需要人类行动时 ping

#### 7\. 连接器（MCP）

- 连接器最小权限（读 vs 写）
- Loop 可以打开/更新 PR 或工单（如果行动），而不仅仅是建议
- Bot 身份在 PR 评论上清晰可见

#### 8\. 成本与限制

- Token 预算已估算 `loop-budget.md` 包含每日上限和终止开关 `loop-run-log.md` 用于追加式运行历史
- 每个项目每次运行的最大迭代次数
- 每天最大自动 PR 数

#### 9\. 可观测性

- 记录每次运行：开始时间、发现项目、采取行动、升级
- 成功指标已选择
- 团队可以在不阅读聊天日志的情况下检查状态文件

#### 10\. 安全

- 没有明确允许列表的情况下不自动合并
- 密钥/env 文件在黑名单中
- Flake 处理——不要仅用重试来"修复"间歇性测试

### 4.4 Loop 设计的栏栅

如果 Loop 如果中断，在继续之前停止并修复，参考样例：

- 同一个 PR 有超过 3 次自动修复尝试但没有进展
- Verifier 和 Implementer 是同一个 Agent 会话
- 没有状态文件——Loop 每次运行都失忆
- 每次运行都通知，无论是否有发现
- 自动合并启用但没有路径允许列表

### 4.5 Loop 设计的反模式

以下反模式是 **设计阶段就必须避免** 的错误——它们不是运行时故障，而是架构层面的根本缺陷。启用无人值守 Loop 之前，逐条自查：

| # | 反模式 | 为什么致命 | 正确做法 |
| --- | --- | --- | --- |
| 1 | 同一个 Agent 实现并验证 | 自己写的代码自己检查，等于没检查。模型会无意识地偏向自己的实现 | Implementer 和 Verifier 必须是不同 Agent、不同模型或不同指令 |
| 2 | 没有 Early Exit | 监控列表为空时仍然全量扫描，每天烧掉数百万 token 无用功 | 空列表在 ~3-5k token 内退出，不要"以防万一"多跑一遍 |
| 3 | 状态文件形同虚设 | 写了状态但下次运行不读，或者状态 schema 不清晰，等于每次从零开始 | 状态文件必须文档化 schema，每次运行开头强制读取上次状态 |
| 4 | 无分支允许列表的自动合并 | Loop 可以直接合并到 `main` ，一旦出错直接污染主干 | 自动合并必须配置分支和路径双重允许列表 |
| 5 | 用重试"修复" Flake 测试 | 间歇性失败测试被重试掩盖，真正的 bug 被埋藏 | Flake 测试应标记为 `@flake` 并排除，或升级给人类处理 |
| 6 | 每次运行都发通知 | 团队很快学会忽略 Loop 消息，真正需要关注时也错过 | 只在需要人类行动时 ping——"发现 3 个高优 Issue"比"运行完成"有价值 |
| 7 | 无 Token 预算上限 | 一个失控的 Loop 可以在 48 小时内烧掉 800 万 token | 设置每日 token 上限，80% 时切换为报告模式，100% 时终止 |
| 8 | Verifier 与 Implementer 共享会话 | 上下文污染——Verifier 看到了 Implementer 的推理过程，丧失独立性 | Verifier 必须在全新会话/worktree 中运行，只看代码 diff |
| 9 | 无限修复循环 | 同一个 Issue 反复修复失败，Loop 永不停止 | 每项最多 3 次尝试，超过后自动升级给人类 |
| 10 | 跳过 L1 直接上 L3 | 从"只报告"直接跳到"自动合并"，中间没有任何验证 | 必须经历 L0→L1→L2→L3，每一级稳定 1-2 周才推进 |

**最重要的反模式是 [#1](https://link.juejin.cn/?target=) 和 [#8](https://link.juejin.cn/?target=)** ——它们本质上说的是同一件事： **独立性是验证的前提** 。一个看过实现过程的验证者，无论多强，都已经丧失了独立判断的能力。

### 4.6 Loop 设计的事故响应流程

当 Loop 在生产中出事（例如自动合并了错误代码、烧爆了预算、修改了不该碰的文件），按以下四步流程处理：

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/f31e17e0d45a40ce9f231a0911948f68~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1785067411&x-signature=majuDybiRY7M9HhfIoAElxcB5A4%3D)

**第 1 步：立即停止**

执行 `loop-pause-all` （全局暂停所有 Loop）或 `scheduler_delete` （删除特定 Loop 的调度任务）。这不是"建议暂停"——是 **立刻、马上、现在** 停止。

**第 2 步：根因分析**

不要急于修复。先问三个问题：

1. 哪个安全机制本该阻止这件事但没生效？
2. 这个 Loop 在哪个级别运行？是否跳级了？
3. 状态文件和运行日志说了什么？

**第 3 步：修复并降级**

修好根因后， **不要** 恢复原级别。将 Loop 降级至少一级：

- L3 出事 → 降回 L2，人工闸门重新启用
- L2 出事 → 降回 L1，只报告不行动
- L1 出事 → 回到 L0，重新审视设计

**第 4 步：记录教训**

在 `stories/` 目录下写一份诚实的事后复盘，格式参考 `why-we-killed-ci-sweeper.md` 。这份文档的价值不在于"我们学到了什么"——而在于 **下一个团队不需要再踩同样的坑** 。

## 五、Loop 模式设计

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/888088853a324338b63d72679357a0f8~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1785067411&x-signature=QCtyUU7qN6LWpB6%2FnjUWKAfH0tQ%3D)

### 5.1 Daily Triage（每日分诊）

**目标：** 每天早上（或活跃时段）获得优先排序的、可操作的待办事项图景——无需手动检查 CI、issues、PR 和聊天。

**典型周期：** 调度触发 → 分诊 Skill 摄入 CI 失败（24h）、开放 issues/工单、最近提交、先前 STATE.md → 高优先级项目追加到状态并建议下一步行动 → 清理已解决项目 → 记录运行后复盘。

**最佳入门 Loop。** 先跑 1-2 周只报告模式，测量分诊准确率后再启用 L2。

### 5.2 PR Babysitter（PR 保姆）

**目标：** 减少人类在 PR 审查、CI、rebase 和合并上花费的时间，同时保持人类在判断席位上。

**典型周期：** 发现团队开放的 PR → 对每个 PR 运行分诊 → CI 红了就生成修复 → 有审查评论就提出最小补丁 → 准备就绪就标记"ready to merge" → 模糊或高风险就升级给人。

### 5.3 CI Sweeper（CI 清扫者）

**目标：** 快速响应 main 或活跃分支上的 CI 失败——诊断、提出最小修复，无法自信解决时升级。

**关键安全措施：** 分类 flake vs 真实回归 vs 基础设施；flake 不自动修复；同一失败最多 3 次尝试后升级； `loop-guard` 熔断器防止无限循环。

#### 5.3.1 真实失败案例：Why We Killed Our CI Sweeper

`loop-engineering` 记录了一次真实事故——这正是 CI Sweeper 被标记为"L2 谨慎模式"的原因：

| 维度 | 详情 |
| --- | --- |
| **触发条件** | `loop 5m` 在 main 分支红色期间运行，恰逢 flaky 迁移分支 |
| **损失** | 48h 烧掉 ~800 万 token；11 个 PR 中 3 个是症状修复，1 个破坏生产配置（人工拦截） |
| **根因 1** | 没有 L1 阶段——直接跳到自动修复 |
| **根因 2** | Verifier 与 Implementer 在同一会话（一半的运行） |
| **根因 3** | 没有分支允许列表——扫到了已知 CI 红色的 feature 分支 |
| **根因 4** | 没有每日预算——调度器从不暂停 |
| **事后处理** | 删除所有调度任务 → 切换为 `main` 失败事件驱动 → 先只报告 1 周 → Verifier 独立子 Agent → 每日 200 万 token 上限 → 分支允许列表仅 `main` |

**教训提炼：** 这个案例暴露了 Loop Engineering 中最危险的认知偏差——"自动化 = 安全" **。实际上，自动化程度越高，需要的安全机制越严格。CI Sweeper 的模式文档之所以存在，正是因为有人付出了真金白银的代价。**

**记住：** L2 不是 L1 的升级版，而是 L1 加上安全网之后的谨慎延伸。

### 5.4 Dependency Sweeper（依赖清扫者）

**目标：** 自动化补丁和低风险 CVE 依赖更新，减少"依赖过期焦虑"。

**典型周期：** 调度触发（建议每周一次，非高峰时段）→ 扫描 `package.json` / `requirements.txt` / `Cargo.toml` → 过滤：仅补丁版本 + 低风险 CVE → 对每个候选依赖创建独立 Worktree → 运行 `npm update` / `pip install --upgrade` → 执行测试套件 → 测试通过则创建 PR，失败则记录原因并跳过。

**关键安全措施：**

- **前 30 天仅补丁 + 低风险 CVE** ——主版本和黑名单包（如 `auth` 、 `crypto` 相关）需要人工闸门
- **每个依赖独立 PR** ——避免一个 PR 改 20 个包导致无法定位回归
- **锁定文件变更必须可审查** ——如果 `package-lock.json` diff 超过 500 行，升级给人类
- **不碰 peer dependency** ——这类变更几乎总是需要人工判断

**为什么从补丁开始：** 补丁版本更新是最"无聊"的工作——收益明确（修 bug、堵漏洞），风险极低（SemVer 保证向后兼容）。让 Loop 处理无聊的事，人类处理需要判断的事。

### 5.5 Changelog Drafter（变更日志起草者）

**目标：** 从合并的 PR 和提交自动生成发布说明草稿，消除"发布前赶写 changelog"的痛苦。

**典型周期：** 调度触发（建议每次发布前，或每周五下午）→ 拉取自上次发布以来的所有合并 PR → 按标签分类（ `bug` 、 `feature` 、 `breaking` 、 `docs` 、 `deps` ）→ 提取 PR 标题和描述中的关键信息 → 按分类生成 Markdown 草稿 → 标注需要人工补充的部分（如"这个 breaking change 的迁移指南需要你写"）。

**关键设计决策：**

- **只起草，不发布** ——最终版本永远由人类编辑和签署
- **标注置信度** ——对每个条目标注"自动提取"或"需要人工确认"
- **关联 Issue** ——自动链接相关的 Issue 编号，方便读者追溯
- **与 Post-Merge Cleanup 配合** ——Cleanup 清理代码，Drafter 记录变更，两者是天然搭档

**低风险、高价值。** 这是最适合作为第二个 Loop 的模式——在 Daily Triage 稳定后，Changelog Drafter 几乎零风险，但能显著减少发布摩擦。

### 5.6 Post-Merge Cleanup（合并后清理）

**目标：** 识别合并后可以清理的内容——死代码、过时注释、TODO 债务、未使用的导入。

**典型周期：** 调度触发（建议非高峰时段，如凌晨 2 点）→ 扫描最近合并的 PR 涉及的文件 → 检测模式：被注释掉的代码块、 `TODO` / `FIXME` 标记、未使用的导入、重复代码片段 → 对每个发现生成清理建议 → 创建单个"清理 PR"（而非每个发现一个 PR）。

**关键安全措施：**

- **非高峰时段运行** ——避免与活跃开发冲突
- **最低紧急程度** ——清理 PR 永远不阻塞任何东西
- **仅限最近合并的文件** ——不扫描整个代码库（范围过大容易引入回归）
- **不碰测试文件** ——测试中的"死代码"可能是故意保留的边界用例
- **单个清理 PR** ——方便一次性审查和合并，避免 PR 洪水

**为什么需要这个模式：** 代码库的熵增是必然的。每次合并都在增加复杂度——临时代码变成永久代码、注释过时、TODO 永远不做。Post-Merge Cleanup 是对抗熵增的自动化手段。

### 5.7 Issue Triage（Issue 分诊）

**目标：** 自动分类新 Issue——标记重复、建议优先级、路由到正确的团队。 **仅建议，不自动关闭。**

**典型周期：** Webhook 触发（新 Issue 创建时）→ 读取 Issue 标题和正文 → 与已有 Issue 做相似度匹配（检测重复）→ 按模板检查信息完整性（是否有复现步骤、环境信息、期望行为）→ 建议标签（ `bug` / `feature` / `question` / `duplicate` ）和优先级 → 如果信息不足，生成友好的追问评论 → 将分诊结果写入 STATE.md。

**关键设计决策：**

- **只建议，不执行** ——标签和关闭操作由人类确认
- **重复检测用相似度而非精确匹配** ——避免漏掉换了一种说法描述的同一个 bug
- **信息不足时追问而非关闭** ——"请补充复现步骤"比"信息不足已关闭"友好得多
- **路由建议基于历史数据** ——分析过去 3 个月每个标签的实际处理人，而非静态的 CODEOWNERS

**与 Daily Triage 的关系：** Issue Triage 是事件驱动的"实时分诊"，Daily Triage 是定时驱动的"全局分诊"。两者互补——Issue Triage 保证新 Issue 不被遗漏，Daily Triage 保证全局视角不被丢失。

### 5.8 模式选择与 LangChain 映射

上面七个模式覆盖了软件工程中最常见的自动化场景。选择哪个模式取决于你的 **痛点** 和 **风险承受能力** ——而不是"哪个模式更酷"。

**选择指南：**

| 你的痛点 | 推荐模式 | 起始级别 | 为什么从这个级别开始 |
| --- | --- | --- | --- |
| "每天早上不知道项目什么状态" | Daily Triage | L1 | 零风险，纯信息输出 |
| "PR 审查太花时间" | PR Babysitter | L1 → L2 | 先看 Agent 的建议质量，再开自动修复 |
| "CI 经常红，没人及时看" | CI Sweeper | L1 → L2 | **必须从 L1 开始** ——参考 5.3.1 事故案例 |
| "依赖更新总是忘记" | Dependency Sweeper | L2 仅补丁 | 补丁更新风险极低，适合直接 L2 |
| "发布说明写起来很痛苦" | Changelog Drafter | L1 | 零风险，纯文本生成 |
| "代码库越来越乱" | Post-Merge Cleanup | L1 | 先看清理建议是否合理 |
| "Issue 积压严重" | Issue Triage | L1 | 只建议不执行，零风险 |

**模式与 LangChain 四层模型的映射：**

每个模式都可以用 LangChain 原语实现，映射关系如下：

| 模式 | 核心 Loop 层 | LangChain 关键原语 | 说明 |
| --- | --- | --- | --- |
| Daily Triage | Loop 1 + Loop 3 | `create_agent` + `CronTrigger` | Agent 执行分诊，cron 定时触发 |
| PR Babysitter | Loop 1 + Loop 2 | `create_agent` + `RubricMiddleware` | Agent 修复 + Grader 验证 |
| CI Sweeper | Loop 1 + Loop 2 + Loop 3 | `create_agent` + `RubricMiddleware` + `WebhookTrigger` | CI 事件触发，修复后验证 |
| Dependency Sweeper | Loop 1 + Loop 3 | `create_agent` + `CronTrigger` | 定时检查 + 自动补丁 PR |
| Changelog Drafter | Loop 1 + Loop 3 | `create_agent` + `CronTrigger` | 定时生成发布说明草稿 |
| Post-Merge Cleanup | Loop 1 + Loop 3 | `create_agent` + `CronTrigger` | 非高峰定时清理 |
| Issue Triage | Loop 1 + Loop 3 | `create_agent` + `WebhookTrigger` | Issue 事件触发分诊 |

**进阶实践：** 当所有模式都稳定运行后，引入 Loop 4（LangSmith Engine）对所有模式的生产 Trace 进行统一分析。Engine 能发现跨模式的改进机会——例如 CI Sweeper 和 PR Babysitter 在重复修复同一类问题时，Engine 可以建议合并或调整分工。这就是"爬山循环"的真正价值：不是改进单个 Loop，而是优化整个 Loop 生态。

### 5.9 模式背后的设计原则

七个模式看似各不相同，但背后共享着五条设计原则。理解这些原则，你就能设计自己的模式，而不只是复制别人的。

**原则一：从 L1 开始，永远从 L1 开始。** 这不是保守，是工程纪律。L1 让你在零风险下验证 Agent 的判断质量。Daily Triage 跑两周只报告，你就能知道它的分诊准确率。CI Sweeper 的事故正是因为跳过了这一步。 **L1 不是"功能不完整"，而是"安全网就位前的必要观察期"。**

**原则二：Maker/Checker 分离不是可选项。** 七个模式中，凡是涉及代码修改的（PR Babysitter、CI Sweeper、Dependency Sweeper、Post-Merge Cleanup），都要求 Implementer 和 Verifier 是独立子 Agent。这不是过度设计——CI Sweeper 事故中，一半的运行 Verifier 和 Implementer 在同一会话，导致"自己验证自己"的盲区。 **AI 不能给自己的代码打分。**

**原则三：范围越小，信任越高。** Dependency Sweeper 只做补丁更新（范围极小）→ 可以直接 L2。CI Sweeper 要诊断任意 CI 失败（范围极大）→ 必须从 L1 开始。这不是双标，是风险与范围的匹配。 **设计模式时，先问"这个 Loop 的 blast radius 有多大"，再决定自主级别。**

**原则四：事件驱动优于定时轮询——但有前提。** Issue Triage 用 Webhook 触发（事件驱动），比定时扫描更及时且更省 token。但 CI Sweeper 的事故正是因为定时轮询 (`loop 5m`) 在错误的时间反复触发。 **事件驱动的前提是：你明确知道"什么事件值得响应"。如果不确定，先用定时 + L1 观察。**

**原则五：每个模式都要回答"什么时候升级给人类"。** 这不是"如果出问题了怎么办"的兜底方案，而是模式设计的核心约束。PR Babysitter 的升级条件是"情况模糊或高风险"，CI Sweeper 是"3 次尝试失败"，Dependency Sweeper 是"主版本或黑名单包"。 **没有明确定义升级条件的 Loop，不是一个完整的 Loop。**

## 六、示例代码（LangChain）

下面用 LangChain 生态来实现 Loop Engineering 的核心模式。LangChain 提供了从基础 Agent Loop 到事件驱动、验证循环、爬山循环的完整原语，让我们可以直接在框架层面构建 Loop，而不需要从零写调度器和状态管理。

### 6.1 项目结构

```perl
代码解读复制代码my-loop-project/
├── agent.py                # Agent 定义与配置
├── tools/
│   ├── github_tools.py     # GitHub PR/Issue 工具
│   └── ci_tools.py         # CI 状态查询工具
├── skills/
│   ├── triage.md           # 分诊 Skill（Markdown 格式）
│   └── minimal_fix.md      # 最小修复 Skill
├── middleware/
│   └── rubric.py           # 验证评分标准
├── deployment.py           # 事件驱动部署配置
└── engine_config.py        # 爬山循环（Engine）配置
```

### 6.2 Loop 1：Agent 循环 —— create\_agent

最基础的 Agent Loop：模型 + 工具 + 系统提示词。

```
代码解读复制代码# agent.py
from langchain.agents import create_agent
from langchain.chat_models import init_chat_model

# 初始化模型
model = init_chat_model("claude-sonnet-4-20250514")

# 定义工具
from tools.github_tools import (
    list_open_prs, get_pr_details, create_pr, add_pr_comment,
    get_ci_status, get_recent_commits
)

# 创建 Agent —— 这就是 Loop 1
agent = create_agent(
    model=model,
    tools=[
        list_open_prs,      # 列出开放 PR
        get_pr_details,     # 获取 PR 详情
        get_ci_status,      # 查询 CI 状态
        get_recent_commits, # 获取最近提交
        create_pr,          # 创建 PR
        add_pr_comment,     # 添加 PR 评论
    ],
    system_prompt="""你是一个 PR 保姆 Agent。
每天早上检查所有开放 PR 的状态：
1. 如果 CI 失败，分析原因并提出修复建议
2. 如果有未回复的审查评论，生成最小补丁
3. 如果一切就绪，标记为 ready-to-merge
4. 如果情况模糊或高风险，升级给人类
"""
)

# 单次调用
result = agent.invoke({
    "messages": [{"role": "user", "content": "检查所有开放 PR 的状态"}]
})
```

### 6.3 Loop 2：验证循环 —— RubricMiddleware

在 Agent Loop 外面包一层验证：产出 → 评分 → 不合格就带反馈重试。

```
代码解读复制代码# middleware/rubric.py
from deepagents.middleware import RubricMiddleware

# 定义评分标准
pr_review_rubric = RubricMiddleware(
    rubric="""
    对 Agent 的每次 PR 修复产出，按以下标准评分（每项 0-10 分）：

    1. 正确性（权重 40%）：修复是否真正解决了 CI 失败或审查意见？
       - 10 分：修复精确针对问题，不引入新错误
       - 0 分：修复与问题无关，或引入了新问题

    2. 最小性（权重 30%）：diff 是否尽可能小？
       - 10 分：只改了必要的行，无无关变更
       - 0 分：重构了无关模块，或改了超过 10 个文件

    3. 安全性（权重 30%）：是否触及了黑名单路径？
       - 10 分：未触及 auth/、payments/、secrets/ 等敏感路径
       - 0 分：触及了黑名单路径 → 立即升级

    总分低于 7 分 → 带反馈重试
    触及黑名单 → 不重试，直接升级给人类
    """,
    max_retries=3,  # 最多重试 3 次
    on_max_retries="escalate"# 超过重试次数后升级
)

# 将验证中间件挂载到 Agent
agent_with_verification = create_agent(
    model=model,
    tools=[list_open_prs, get_pr_details, get_ci_status,
           create_pr, add_pr_comment, get_recent_commits],
    middleware=[pr_review_rubric],
    system_prompt="你是一个 PR 保姆 Agent..."
)
```

### 6.4 Loop 3：事件驱动循环 —— LangSmith Deployment

把 Agent 部署为持续运行的后台服务，通过 cron 或 webhook 触发。

```
代码解读复制代码# deployment.py
from langsmith import deployment

# 部署 Agent，配置触发方式
deployment.serve(
    agent=agent_with_verification,
    name="pr-babysitter",
    triggers=[
        # Cron 触发：每 15 分钟检查一次 PR 状态
        deployment.CronTrigger(
            schedule="*/15 * * * *",
            input_template="检查所有开放 PR 的状态"
        ),
        # Webhook 触发：当有新 PR 或 CI 状态变更时立即响应
        deployment.WebhookTrigger(
            source="github",
            events=["pull_request.opened", "check_run.completed"]
        ),
    ],
    # 运行时配置
    runtime={
        "max_concurrency": 3,       # 最多 3 个并发运行
        "timeout_seconds": 600,     # 单次运行超时 10 分钟
        "error_policy": "escalate", # 出错时升级给人类
    }
)
```

### 6.5 Loop 4：爬山循环 —— LangSmith Engine

分析生产 Trace，自动改进 Harness 配置。

```
代码解读复制代码# engine_config.py
from langsmith import Engine

# Engine 持续分析 Agent 运行 Trace，自动改进
engine = Engine(
    agent=agent_with_verification,
    analysis_prompt="""
    分析最近 100 次 Agent 运行的 Trace，找出改进点：

    1. Prompt 问题：
       - 哪些系统提示词导致了低质量输出？
       - Agent 是否经常误解某些指令？

    2. 工具问题：
       - 哪些工具调用经常失败或超时？
       - 工具描述是否准确？

    3. Grader 问题：
       - 评分标准是否过于宽松/严格？
       - 是否有"通过验证但实际有问题"的案例？

    4. 模式问题：
       - 哪些类型的 PR 修复成功率最低？
       - 是否有反复出现的失败模式？

    对每个发现的问题，提出具体的改进建议。
    """,
    auto_improve=True,  # 自动应用非破坏性改进
    improvement_policy={
        "prompt_tweaks": "auto",       # prompt 微调自动应用
        "tool_config": "auto",         # 工具配置自动应用
        "rubric_changes": "review",    # 评分标准变更需人工审查
        "model_switch": "review",      # 模型切换需人工审查
    },
    schedule="0 2 * * 1",  # 每周一凌晨 2 点运行分析
)

# 启动 Engine 监控
engine.monitor()
```

### 6.6 Human-in-the-Loop：人工介入点

LangChain 将 Human-in-the-Loop 作为一等公民，在每一层 Loop 都可以插入人工审查：

```
代码解读复制代码from langchain.agents import create_agent
from deepagents.middleware import HumanInTheLoopMiddleware

agent_with_human = create_agent(
    model=model,
    tools=[create_pr, add_pr_comment, merge_pr],
    middleware=[
        # 在敏感操作前要求人工确认
        HumanInTheLoopMiddleware(
            require_approval_for=[
                "merge_pr",           # 合并 PR 必须人工确认
                "create_pr",         # 创建 PR 前人工确认（可选）
            ],
            auto_approve_for=[
                "add_pr_comment",    # 添加评论自动通过
            ],
# 24 小时未响应则自动拒绝

        ),
        pr_review_rubric,
    ],
    system_prompt="你是一个 PR 保姆 Agent..."
)
```

### 6.7 完整示例：Daily Triage Loop

将以上组件组装成一个完整的 Daily Triage Loop：

```
代码解读复制代码# daily_triage.py —— 完整的 Daily Triage Loop
from langchain.agents import create_agent
from langchain.chat_models import init_chat_model
from deepagents.middleware import RubricMiddleware
from langsmith import deployment

model = init_chat_model("claude-sonnet-4-20250514")

# 分诊 Agent：只读、只报告、不修改
triage_agent = create_agent(
    model=model,
    tools=[
        get_ci_status,
        list_open_prs,
        get_recent_commits,
        list_open_issues,
    ],
    system_prompt="""你是 Daily Triage Agent。
每天早上扫描项目状态：
1. 检查 CI 状态（过去 24 小时的失败）
2. 列出所有开放 PR 及其状态
3. 检查是否有过期依赖
4. 将发现写入 STATE.md

规则：
- 只报告，不自动执行任何操作
- 高优先级项目标记为需要人工关注
- 已解决的项目从 STATE.md 中清理
""",
    middleware=[
        RubricMiddleware(
            rubric="""
            分诊结果必须：
            1. 每个发现都有明确的优先级（high/medium/low）
            2. 每个发现都有建议的下一步行动
            3. 不遗漏任何 CI 失败或超过 3 天未活动的 PR
            """,
            max_retries=2
        )
    ]
)

# 部署为每天早上 9 点运行
deployment.serve(
    agent=triage_agent,
    name="daily-triage",
    triggers=[
        deployment.CronTrigger(
            schedule="0 9 * * 1-5",  # 工作日早上 9 点
            input_template="执行每日分诊，扫描 CI、PR、Issues 和依赖状态"
        )
    ]
)
```

## 七、Loop Engineering 进阶

### 7.1 成本管理

Token 成本是 Loop Engineering 最现实的约束。一个设计不当的 Loop 可能在一天内烧掉数百万 token。

#### 成本估算公式

```
代码解读复制代码每日 Token ≈ 每次运行 Token × 每天运行次数 × (1 + 子 Agent 倍数)
```

| 因素 | 影响 |
| --- | --- |
| 节奏（Cadence） | 线性乘数（5 分钟 vs 1 天 = 288× 运行次数/天） |
| 每次运行的子 Agent 数 | 每个 = 完整模型 + 工具往返 |
| 上下文大小 | 大型仓库 + 完整 CI 日志 = 昂贵的分诊 |
| Verifier 模型 | Verifier 上使用更强的模型 = 值得（无人值守时） |

#### 成本控制最佳实践

**分诊先行：** 先用廉价的分诊扫描（~5k token），只有发现可操作项目时才启动子 Agent **Early Exit：** 监控列表为空时在 ~3-5k token 内退出 **预算上限：** `loop-budget.md` 设置每日 token 上限，超限自动暂停 **熔断器：** 同一项目超过 3 次尝试自动升级，防止无限修复循环 **非高峰降速：** 夜间和周末使用更慢的节奏

#### 减速 / 暂停 / 杀死：三档应急标准

成本控制不只是"省钱"——当 Loop 行为异常时，你需要明确的应急标准。以下是生产级 Loop 的三档响应：

| 档位 | 触发条件 | 执行动作 | 恢复方式 |
| --- | --- | --- | --- |
| 🟡 减速 | 单次运行 token 超预算 50%，或连续 2 次运行无进展 | 节奏降为原来的 1/2，切换到更便宜的模型做分诊 | 下一轮运行恢复正常即自动升速 |
| 🟠 暂停 | 日 token 达到预算 80%，或 `loop-pause-all` 被激活 | 当前轮完成后退出，调度器标记为 paused | 人工执行 `loop-resume` 恢复 |
| 🔴 杀死 | 日 token 达到预算 100%，或检测到危险路径修改 | 立即终止当前运行，不等待完成 | 人工根因分析后重新从 L0 开始 |

**`loop-pause-all` 是全局紧急开关** ——执行后所有 Loop 在当前轮结束后立即退出。它不是"建议暂停"，而是硬性终止。配合 `loop-constraints.md` 中的 `If loop-pause-all is active, exit immediately` 规则，确保即使调度器来不及取消，Loop 自身也会主动退出。

### 7.2 状态同步

当多个 Loop 或人类同时修改状态时，会出现漂移。Codex CLI 的 `state` 子命令可以检测 `STATE.md` 和 `LOOP.md` 之间的不一致：

```perl
代码解读复制代码# 检查状态一致性
codex state diff --state STATE.md --loop LOOP.md

# 自动修复可安全合并的冲突
codex state sync --state STATE.md --loop LOOP.md --auto-resolve
```

**状态设计原则：**

- 一个模式一个状态文件，或清晰分隔的区域
- 每次运行必须清理已关闭/已合并的项目
- 人类覆盖必须记录在状态中
- 状态文件是 Loop 最重要的产物——它应该是团队可以不读聊天日志就能理解的

### 7.3 上下文管理（loop-context）

Codex CLI 内置了状态化记忆管理和长运行熔断器，通过 `context` 子命令实现：

```perl
代码解读复制代码# 注入历史状态到当前运行上下文
codex context inject --state STATE.md --since "24h"

# 修剪过时条目，防止状态文件无限增长
codex context prune --state STATE.md --older-than "7d"

# 检测同一失败的重复尝试，触发升级
codex context guard --state STATE.md --max-retries 3
```

它解决三个问题：

**1\. 上下文注入：** 在每次运行开始时注入相关的历史状态 **2\. 上下文修剪：** 清理过时条目，防止状态文件无限增长 **3\. 熔断器：** 检测同一失败的重复尝试，触发升级

### 7.4 Worktree 管理（loop-worktree）

Codex CLI 通过 `worktree` 子命令管理每次修复尝试的隔离 git worktree：

```
代码解读复制代码# 创建隔离 worktree
codex worktree create --run-id <id> --pattern <p>

# 锁定路径（防止多 Loop 冲突）
codex worktree lock --paths <globs> --owner <pattern>

# 解锁
codex worktree unlock --owner <pattern>

# 清理已完成或已放弃的 worktree
codex worktree cleanup --older-than "1h"
```

**关键原则：** 一次尝试一个 worktree，在 manifest 中跟踪，拒绝或升级时清理。

### 7.5 Goal Engineering（目标工程）

Goal Engineering 是 Loop Engineering 的伴侣概念。如果说 Loop 负责"发现和持续"，Goal 负责"聚焦和完成"：

> **Loops discover, goals finish.**

Goal 是一个可验证的停止条件——Agent 持续迭代直到条件满足：

```csharp
代码解读复制代码/goal All tests on main pass and lint is clean
/goal Keep working on this PR until CI is green and no blocking comments remain
```

Goal 使用 **新鲜模型** 来判断停止条件是否满足——这是 Maker/Checker 思想的另一种应用。

### 7.6 Loop-Gate（循环闸门）

Codex CLI 的 `gate` 子命令从 `gate.yaml` 机械执行路径黑名单和自动合并允许列表：

```
代码解读复制代码# 检查是否可以自动合并
codex gate check --action auto-merge --paths <f1,f2,...>

# 检查路径是否在黑名单中
codex gate check --action deny --paths <f1,f2,...>
```

退出码约定：

- `0` — 通过，可以继续
- `2` — 升级给人类

这与 `codex context guard` 使用相同的约定，因此控制脚本可以链式调用两者。

#### Loop Constraints（约束机制）

`loop-constraints.md` 是比 `gate.yaml` 更高层的安全机制——它是 **自然语言编写的硬性规则** ，Loop 在每次运行开始时逐字读取并遵守。与 `gate.yaml` 的机械路径检查互补， `loop-constraints` 覆盖的是"意图层面"的约束。

典型的 `loop-constraints.md` 包含五个维度的规则：

| 维度 | 示例规则 | 为什么需要 |
| --- | --- | --- |
| Push & Merge | 永远不自动合并到 main；先创建草稿 PR | 防止未经审查的代码进入主干 |
| Paths | 永不编辑.env、auth/、payments/、secrets/ | 保护敏感配置和凭据 |
| Code | 每次修复前先跑测试；永不禁用测试来让 CI 变绿；每次运行最多 3 次尝试 | 防止虚假修复和无限循环 |
| Communication | 做之前先告诉我；未经批准不关闭 Issue/PR | 保持人类对关键决策的控制权 |
| Budget | token 达到 80% 日上限时切换为只报告； `loop-pause-all` 激活时立即退出 | 成本兜底 |

**约束是绑定的（binding）** ——Agent 必须遵守。它们不像系统提示词那样可以被"灵活理解"，而是作为硬性检查写入 Loop 的运行流程。配合 Codex CLI 的 `context guard` 机械执行（记录每次尝试到 `loop-ledger.json` ，重试前运行 `codex context guard --check` ），约束从"建议"变成了"不可绕过的护栏"。

### 7.7 多 Loop 协调

在一个仓库中运行多个 Loop 是正常的。没有边界地运行它们就是 Loop 互相打架的方式。

#### 核心原则

**1\. 一个分支一个所有者** —— 每小时最多一个 Loop 可以修改一个分支

**2\. 分离状态文件** —— `STATE.md` 用于分诊；模式特定文件用于操作 Loop

**3\. 分诊报告，操作 Loop 执行** —— L1 的 Daily Triage 从不与 CI Sweeper 修复竞争

**4\. 共享黑名单** —— 将相同的路径黑名单复制到每个 LOOP.md

**5\. 聚合 token 预算** —— 所有 Loop 共享一个预算上限

#### 冲突优先级

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/41a9fbd9658b42e7a35fe033bec3308c~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1785067411&x-signature=iTRp3vcn12um8gsKyY5kAHeaXAU%3D)

### 7.8 必须正视的三笔"债"

#### Intent Debt（意图债）

每个 session Agent 都是"冷启动"。团队约定、构建命令、"我们从不那样做"——若不写进 Skills / `AGENTS.md` ，每轮 loop 都在重新猜。 **Skills 是偿还意图债的方式。**

#### Comprehension Debt（理解债）

Loop 越快，仓库里"你写过但没读过"的代码越多。Loop 交付了，不代表你理解了。 **更快的 Loop 运送更多你没写过的代码——理解债增长，除非你阅读 Loop 做了什么。**

#### Cognitive Surrender（认知投降）

最危险的用法：把 Loop 当成逃避思考的按钮。Addy Osmani 警告：

> Build the loop. But build it like someone who intends to stay the engineer, not just the person who presses go.

**同一个 Loop 设计，可以加速真工程师，也可以加速"只会按 Go 的人"——区别在你有没有把判断力编码进 Skills 和 Verifier。**

### 7.9 十大常见失败模式

**1\. 无限修复循环：** 同一 PR 被自动修复 5+ 次，永不收敛。缓解：硬上限 3 次 → 升级给人

**2\. 状态腐烂：** STATE.md 引用已合并的 PR、已关闭的工单。缓解：每次运行清理

**3\. Verifier 表演：** Verifier "批准"但 CI 测试失败。缓解：Verifier 必须运行测试并报告输出

**4\. 通知疲劳：** 每 5 分钟 ping 一次；团队静音机器人。缓解：仅在需要人类决策时通知

**5\. Token 燃烧：** 账单飙升；Loop 在空或嘈杂的分诊上运行完整子 Agent 链。缓解：廉价分诊先行

**6\. 越界：** Loop 重构无关模块。缓解：路径黑名单 + "最小可能 diff"

**7\. 理解债螺旋：** 速度上升，但没人能解释最近的变更。缓解：非平凡 PR 强制人类审查

**8\. 认知投降：** "Loop 会处理"——对正确性或设计没有意见。缓解：每个模式中明确的人工闸门

**9\. 并行冲突：** 两个子 Agent 编辑相同文件。缓解：worktree 隔离 + 状态锁

**10\. 升级失败：** Loop 卡在重试中；人类从未被通知。缓解：连接器在升级时 ping

## 八、与未来展望

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/c2938e4c011f49c5891392cc8aed3b1b~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1785067411&x-signature=pseSW8vU3PiA1tbvt76htl%2BFzgA%3D)

#### 趋势一：从"写代码"到"设计系统"

工程师的角色正在从"代码生产者"转变为"系统设计者"。未来的高级工程师不是写得最多的人，而是设计出最好的 Loop 系统的人。Boris Cherny 的实践已经证明了这一点——他同时管理 15+ 个并行 Agent 实例，专注于代码审查和方向把控。

#### 趋势二：Loop 即代码（Loop as Code）

就像 Infrastructure as Code 改变了运维，Loop as Code 将改变软件开发。Loop 配置（LOOP.md、gate.yaml、loop-constraints.md）将成为项目的标准组成部分，就像 CI 配置一样。

#### 趋势三：多 Agent 编排标准化

MCP（Model Context Protocol）正在成为 Agent 间通信的通用基板。未来，不同厂商的 Agent 将能够通过标准化协议协作——一个 Claude Code Agent 做分诊，一个 Codex Agent 做修复，一个 Gemini Agent 做验证。

#### 趋势四：Goal Engineering 与 Loop Engineering 融合

Loop 负责"发现和持续"，Goal 负责"聚焦和完成"。两者的融合将产生更强大的自主开发系统——Loop 发现需要做的事情，Goal 确保每件事做到位。

#### 趋势五：安全与治理成为一等公民

随着 Loop 从 L1 走向 L3，安全机制将从"最佳实践"变为"硬性要求"。路径黑名单、自动合并允许列表、MCP 权限最小化、人工闸门——这些将成为任何生产 Loop 的标配。

#### 趋势六：成本优化自动化

未来的 Loop 系统将内置智能成本管理——根据预算自动调整节奏、在低价值任务上使用更便宜的模型、在关键验证上使用更强的模型。Token 预算将从静态配置变为动态优化。

#### 趋势七：从个人工具到团队基础设施

Loop Engineering 目前主要是个人实践（Boris 的 15 个并行 Agent），但正在快速演变为团队基础设施。共享的 Skills 库、团队级状态看板、跨项目的 Loop 模式——这些将成为工程组织的标准配置。