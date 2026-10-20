---
title: "Claude Skills 深度实测:能力包与软编排的完整指南"
source: "https://www.axtonliu.ai/newsletters/ai-2/posts/claude-skills-capability-package-soft-orchestration-guide"
author:
  - "[[重构个体：AI时代如何打造个人竞争力]]"
published: 2026-01-01
created: 2026-05-13
description: "使用 Claude Skills 三周的深度实测，揭示能力包型和软编排型两种用法。discussion-organizer 5 分钟整理 50 条笔记，srt-workflow 2 分钟字幕转文章，还有完整的 5A+ 上手指南和 MAPS 框架分析。"
tags:
  - "clippings"
status: processed
---
## Claude Skills 深度实测：能力包与软编排的完整指南 | AI 精英周刊 029

> 三周深度使用 Claude Skills 后发现，它真正的价值在于把判断逻辑资产化。能力包型(discussion-organizer)封装四层判断逻辑，5 分钟完成原本需要 3-4 小时的笔记整理;软编排型(srt-workflow)通过 Task 工具调度独立 sub-agents，数分钟将字幕转为结构化文章。
> 
> 核心区别：官方编排在单个 Claude 实例内执行，软编排跨多个独立 agents 协作(仅 Claude Code 支持)。附完整 5A+ 上手指南和 MAPS 框架分析。

Claude Skills 发布三周了。这三周我几乎每天都在用，越用越确信一件事:**这是一个有机会改变 Agent 格局的东西。** 也是在当前 AI 喧嚣当中，我认为最值得大家认真关注的一项技术。

很多人一听 Skills，会先有三个问号:

- 这东西到底是干嘛的
- 会不会又是一个只会炒概念的功能
- 和 Agent Builder、PROJECT、MCP 等等有什么关系

我先说结论，再慢慢展开。

对我这种天天写 Prompt、做自动化的人来说，Claude Skills 有三个让我眼前一亮的点:

- 第一， **不用写一行代码** ，就可以把非常复杂的判断逻辑，封装成一个可复用的模块。
- 第二， **原本要花四五个小时反复人工判断的工作** ，现在五分钟就能跑完，而且结果稳定、一致。
- 第三，它不是孤零零的一个功能，而是可以和 **MCP、工具调用、多 Agent 协作** ，拼成一整套工作流。

如果用一句最简单的话来概括就是:

> **Prompt 告诉 AI 做什么。**  
> **Skills 告诉 AI 如何判断、以及整个流程要怎么跑。**

本文我会用两个真实案例，带你看清楚:

- Skills 到底是什么
- 为什么我觉得它是当前最值得关注的 Agent 技术
- 不会写代码，也可以今天看完就动手用起来

## 什么是 Claude Skills?

到底什么是 Skills?

官方的定义是:

> **技能是由指令、脚本和资源组成的文件夹，Claude 会动态加载它们以提升在专业任务上的表现。**

这里面其实一个比较重要的特点是" **可重复** "的方式工作，所以，我们可以把 Skills 的定义简化为:

**"可复用的指令包"**

因此，很多人听到 Skills 时的第一反应都是:这不就是一个保存 Prompt 的地方吗?

这句话没错，但是完全没有体现出 Skills 厉害的地方。对我来说，更准确的理解是:

> **Skills 让你把自己的判断逻辑和处理流程，写成一个 Claude 能反复执行的模块。**

## Skill 的结构

这个模块是什么样呢?就是一个文件夹，里面有:

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/sites/2148203597/images/e0f3a5-4fc0-dc8-562d-c41b6dfddfe6_CleanShot2025-11-18at22.54.08.png)

1. **一个核心的 SKILL.md** ，用 Markdown 写清楚:
	- 这个 Skill 要解决什么问题
		- 判断标准是什么
		- 分几步走
		- 每一步需要用到哪些工具、哪些子能力
	**SKILL.md 这个文件的重点，是名称和描述** ，Claude 就是根据名称和描述来判断什么时候调用这个 Skill 的。
2. **一些资源文件** (可选):
	- 固定的模板
		- 参考文档
		- 示例
		- 甚至代码

## Prompt vs Skill:真正的区别在哪?

**那你可能会想:**

> 我现在就可以写一个很长的 Prompt，把判断标准、流程步骤、注意事项全部写进去，让 AI 每次照着做，那和写 Skill 到底有什么区别?

这个问题非常好。

确实，从" **内容复杂度** "来说，Prompt 和 Skill 没有本质差别。你完全可以写一个 2000 字的 Prompt，当成一份迷你工作说明书用。

**真正的差别在于它们的"形态"和"生命周期"。**

**Prompt 是一次性的对话内容:**

- 你要自己保存、自己复制粘贴
- 模型不会自动知道什么时候该用哪个长 Prompt
- 一次全塞进上下文，会大量占用上下文空间

**而 Skill 是把同样的东西，换了一种更工程化的包装方式:**

- 它是一个文件夹，里面有 SKILL.md 和相关资源文件
- Claude 启动时 **只加载名字和简介** (每个 Skill 约 100 tokens)，觉得相关才把完整内容读进来
- 同一个 Skill 可以在不同对话、不同产品里复用
- 你可以版本化、可以放 Git、可以发给团队同事一起用

**所以你也可以理解为:**

> Skill 不是"写出了更复杂的 Prompt"，而是 **"把复杂 Prompt 变成了一个有结构、可复用、可自动调用的模块"** 。

这就是为什么我很看重 Skills:它把我们过去靠人肉维护的 Prompt 库，升级成了一套可以 **被 Claude 主动使用的"判断与流程资产"** 。

这也是我的 MAPS 思维框架当中，A 架构设计部分强调的核心原则:**资产化** ，把你的个人技巧、隐形知识转化为可规模化的数字资产。

这里的重点是:**你的知识 + 规模化** 。

## 两个真实案例场景

好，概念讲清楚了。接下来，我会用两个具体案例，来看看 Skills 真正落地之后是什么感觉。

### 案例 1:整理学习笔记

假设你在学习 Claude Skills 这个新技术，边看文档边记笔记，记了一堆碎片化的内容:

- 有些是官方文档的关键信息
- 有些是社区讨论里的经验
- 有些是你自己的思考和疑问
- 还有些可能是误解或者过时信息

你想把这些笔记整理成:

- 哪些是已验证的核心知识
- 哪些是有价值的实践经验
- 哪些需要进一步验证
- 哪些可以直接丢掉

如果手工整理，你要:

- 一条条读
- 判断价值和可信度
- 分类打标签
- 改写成结构化笔记
- 至少 3-4 小时

但用 **discussion-organizer** 这个 Skill，5 分钟搞定。

### 案例 2:把字幕文件转成文章

你录了一段 7 分钟的讲座，用工具生成了 SRT 字幕文件。

现在你想把它变成一篇可以发布的文章，需要:

- 把碎片化的字幕合并成段落
- 修正语音识别错误
- 添加标题和结构
- 如果是英文，翻译成中英对照版

手工做的话，至少 1-2 小时。

但用 **srt-workflow** 这个 Skill，2 分钟全自动跑完。

这两个案例，其实代表了 Skills 的两种典型用法:

- **案例 1:能力包型** (官方设计)
- **案例 2:软编排型** (我的扩展用法)

在看具体操作之前，我们先理解一下这两种模式的区别。

## 两种使用模式的架构对比

在官方文档里，Skills 有三种用途:

- 知识包(如品牌规范)
- 能力包(如 PDF 处理)
- 单 Skill 内的工作流（比如 SOP）

今天我要展示的，是两种 **实际使用场景**:

### 第一种:能力包型(官方设计)

**代表案例:discussion-organizer**

这种 Skill 的特点是:

- 封装一套 **判断逻辑** 和 **处理规则**
- Claude 读取后，在 **当前对话** 中应用这套规则
- 不调度其他 Agent，只是让 Claude "按规矩做事"

比如 discussion-organizer:

- 教会 Claude 怎么判断内容的价值和可信度
- 怎么提取关键信息
- 用什么措辞风格整理笔记

### 第二种:软编排型(我的扩展用法)

**代表案例:srt-workflow**

这种 Skill 的特点是:

- 不自己干活，而是定义一套 **SOP(标准操作流程)**
- 主 Claude 读取 SOP 后，用 **Task 工具调度多个独立的 sub-agents**
- Sub-agents 之间通过 **文件传递** 上下文，不走主 Claude

### 关键对比

让我说清楚这两种模式的本质区别:

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/sites/2148203597/images/453a584-3b30-b07c-f4b6-c4fc4b8ddce7_CleanShot2025-11-18at22.56.37.png)

**官方的"编排型 Skill"是这样的:**

Skill 定义流程:"第一步做A，第二步做B，第三步做C"

Claude 自己执行: A → B → C

这是 **单个 Claude 实例** 按步骤执行。

**我的"软编排"是这样的:**

Skill 定义 SOP:"先分段(segmenter)，再审查(reviewer)，最后定稿(finalizer)"

主 Claude 读取 SOP → 用 Task 工具调用 segmenter agent → segmenter 执行，写 segments.json → 主 Claude 用 Task 工具调用 reviewer agent → reviewer 读 segments.json，执行，写 review.json → 主 Claude 用 Task 工具调用 finalizer agent → finalizer 读 review.json，执行，写 article.md

这是 **跨多个独立 agents** 的协调编排。

你可以这样理解:

**能力包型:**

- Skill 既定义规则，Claude 也按这个规则执行
- 声明与执行在同一个 Claude 实例

**软编排型:**

- Skill 只定义 SOP 规范
- 主 Claude 是"项目经理"，看着 SOP 决定叫谁干活
- Sub-agents 是"专业执行团队"，各司其职
- Task 工具是"打电话叫人"的调度工具

**为什么要这样做?**

软编排的优势:

1. **独立上下文**:每个 sub-agent 有自己的上下文窗口，不会互相污染
2. **Token 优化**:通过文件传递，不用把全文粘贴到主上下文
3. **可测试性**:每个 agent 可以独立测试和优化
4. **可组合性**:Sub-agents 可以被多个 Skill 复用

### 重要说明

需要明确两点:

**1\. 软编排不是官方标准用法**

这是我在实践中摸索出来的工程化方案。

官方的"编排型 Skill"是在 **单个 Skill 内** 编排各种能力，比如:

- 调用其他 Skills
- 使用 MCP 工具
- 执行 Python/JS 脚本
- 组合多种工具调用

但关键是:都在 **同一个 Claude 实例的上下文内** 执行。

而我的软编排是:**跨多个独立的 sub-agents** ，每个都有自己的上下文窗口，通过文件传递协作。

**2\. 软编排只能在 Claude Code 使用**

因为它依赖 **Task 工具** 来调度 sub-agents，而这个能力目前只有 Claude Code 支持。

所以如果你想用软编排，必须在 Claude Code 环境下。

## 两个案例详细演示

好，架构理解了。现在我们来看具体操作。

### 案例 1:discussion-organizer(能力包型)

**场景回顾:**

我有一个 Markdown 文件，里面是我学习 Claude Skills 时记的笔记，有 50 多条碎片化的内容，包括官方文档摘录、社区讨论、个人思考，比较杂乱。

我要用 discussion-organizer 这个 Skill 来整理它。

**Step 1:触发 Skill**

我直接在 Claude Code 里输入:

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/sites/2148203597/images/31bf16c-bcb6-afef-2dc1-2164b4cc07d_2025-11-18_23-01-24_1.png)

```markdown
请帮我整理以下这些讨论记录和学习笔记
```

注意，我 **没有说** "用 discussion-organizer"。

Claude 看到"整理笔记"这个意图，自动匹配到了这个 Skill。

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/sites/2148203597/images/035beec-81ea-3653-613f-b0c124636ac_2025-11-18_23-02-45.png)

**Step 2:Skills 在做什么**

这个 Skill 的核心，是封装了一套 **四层判断逻辑**:

**1\. 价值判断(Value Assessment)**

每条讨论会被评分:

- ⭐⭐⭐⭐⭐ 核心洞察
- ⭐⭐⭐ 有用补充
- ⭐ 常规讨论
- ❌ 无价值噪音

**2\. 可信度判断(Credibility Check)**

- ✅ 已验证(有官方文档/数据支持)
- ⚠️ 需验证(合理推断，但缺证据)
- ❌ 可疑(与已知事实矛盾)

**3\. 提取粒度(Granularity)**

- 🔥 直接引用(原话有价值)
- 📝 改写提炼(去掉口语化)
- 🗂️ 归类打标签(只记录主题)

**4\. 措辞风格(Tone Normalization)**

- 把口语化表达转成书面语
- 统一术语(AI Agent → Agent)
- 去除情绪化词汇

**Step 3:输出结果**

Claude 跑完之后，生成了一个结构化的 Markdown 文件:

```markdown
# Claude Skills 学习笔记 - 整理版

## 🔥 核心知识 (⭐⭐⭐⭐⭐)

### 1. Skills 的渐进式披露机制 ✅已验证
**笔记原文**: "Skills 不会一次性把所有内容都加载进来"
**验证结果**: 确认。Claude 启动时只加载 metadata(name + description)，
每个 Skill 约 100 tokens。触发后才加载完整 SKILL.md。
**来源**: Anthropic 官方工程博客

### 2. Skills 与 Prompt 的本质区别 ✅已验证
**核心要点**:
- Prompt 是一次性指令，需要手动复制粘贴
- Skills 是可复用模块，Claude 自动识别触发
- Skills 支持资源文件、版本管理、团队共享
**来源**: 官方文档 + 实践验证

---

## 📝 实践经验 (⭐⭐⭐)

### 1. Skill 触发机制的注意事项 ⚠️待验证
**经验**: description 写得不够清楚会导致不触发
**建议**:
- 在 description 中明确触发词
- 用排除词标明不适用场景
- 必要时手动指定 Skill 名称

### 2. 软编排 vs 官方编排的区别
**笔记**: 我用 Skills 编排多个 sub-agents
**待验证**: 这是官方设计还是扩展用法?

---

## 🗂️ 待整理 (⭐)

- Skills 不跨平台同步
- 需要 Pro 以上账户
```

**关键效果:**

- ✅ 原本 50 多条碎片笔记，变成了 3 个清晰分类
- ✅ 每条都打上了价值评分和可信度标签
- ✅ 已验证 vs 待验证清晰标注
- ✅ 措辞统一，可以直接作为知识库使用
- ⏱️ **用时: 数分钟** (手工至少 3-4 小时)

**这就是"能力包型 Skill"的典型场景:**

- 封装复杂的判断逻辑
- Claude 按这套规则处理内容
- 不调度其他 Agent
- 输出高度一致、可复用

### 案例 2:srt-workflow(软编排型)

**场景回顾:**

我有一个 7 分钟讲座的 SRT 字幕文件(英文)，要把它转成一篇可发布的文章。

我要用 **srt-workflow** 这个 Skill，让它自动跑完 4 个阶段。

**⚠️ 重要提示**:这个案例只能在 **Claude Code** 使用，因为需要调用 Task 工具来编排 sub-agents。Web 版不支持。

**Step 1:触发 Skill**

我在 Claude Code 里输入:

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/sites/2148203597/images/518166-f43-e5d3-46e8-0410e7e534a3_2025-11-18_23-06-04.png)

```markdown
把这个字幕文件转成文章:@AI Obsidian Mind Map.srt
```

Claude 识别意图，加载 srt-workflow Skill。

**Step 2:软编排在做什么**

srt-workflow 不是一个 Skill 自己干活，而是定义了一套 **4 阶段 SOP**:

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/sites/2148203597/images/1cc1d33-66d-84a5-63d0-83701046e861_CleanShot2025-11-18at23.08.10.png)

```markdown
阶段 0: subtitle-segmenter(字幕分段)
  ↓ 输出 segments.json
阶段 1: subtitle-segment-reviewer(质量审查)
  ↓ 输出 segments_reviewed.json
阶段 2: content-finalizer(定稿)
  ↓ 输出 article_draft.md
阶段 3: pg-style-translator(翻译，条件触发)
  ↓ 输出 article_cn.md
```

每个阶段都是一个 **独立的 sub-agent**:

- 有自己的上下文窗口
- 有自己的工具白名单
- 有自己的判断标准

主 Claude 的角色是:

- 读取 SOP
- 用 Task 工具按顺序调用 sub-agents
- 检查每个阶段的输出
- 决定下一步走哪条分支

**Step 3:执行过程**

Claude 依次调用了 4 个 agents:

**阶段 0: subtitle-segmenter**

- 正在分段字幕文件…
- 输入: 74 条字幕
- 输出: 22 个语义段落
- 文件: segments.json

**阶段 1: subtitle-segment-reviewer**

- 正在审查分段质量…
- 发现 6 个 ASR 错误:
	- "two-use" → "tool use"
		- "latent discourse" → "later in this course"
- 建议添加 5 个标题
- 文件: segments\_reviewed.json

**阶段 2: content-finalizer**

- 正在生成定稿文章…
- 应用结构建议
- 修正术语错误
- 文件: article\_draft.md

**阶段 3: pg-style-translator**

- 检测到英文内容
- 正在翻译为中英对照版…
- 保留英文技术术语，添加中文注释
- 文件: article\_cn.md

**Step 4:最终结果（输出内容仅为示例）**

生成了两个文件:

1. **article\_draft.md** (英文原版)
2. **article\_cn.md** (中英对照版)

打开 `article_cn.md` ，你会看到:

```markdown
# 构建 AI Agent 工作流的四大设计模式

在构建 LLM agent(大语言模型智能体)工作流时，我们的核心方法是把不同的构建模块
组合起来，形成强大的复合系统...

## 模式一:反思(Reflection)—— 让 AI 自我纠错

Reflection(反思)的核心思路是:让 LLM 生成内容后，再让它自己审查这个内容，
找出问题，然后重新生成...

## 模式二:工具使用(Tool Use)—— 给 AI 接上'外挂'

Tool use(工具使用)让 LLM 可以调用外部工具。比如做网页搜索、查数据库、
或者执行代码...
```

**关键效果:**

- ✅ 原始字幕 74 条 → 结构化文章 2500 字
- ✅ 自动修正 6 个 ASR 错误
- ✅ 添加 1 个 H1 + 5 个 H2 标题
- ✅ 中英对照，技术术语保留英文 + 中文注释
- ⏱️ **用时:约 2 分钟** (手工至少 1-2 小时)

**这就是"软编排型 Skill"的典型场景:**

- Skill 定义 SOP，不自己执行
- 主 Claude 按 SOP 调度多个 sub-agents
- Sub-agents 通过文件传递上下文
- 整个流程自动化、可回滚、可测试

## 使用边界与限制

好，两个案例演示完了。你可能会想:这玩意我能用吗?有什么限制?

### 使用 Skills 的前置条件

**1\. 付费账户**

- Free 用户不能用 Skills
- 需要 **Pro、Team 或 Enterprise** 计划

**2\. 代码执行权限**

- 必须在设置里打开 **Code Execution**
- Skills 依赖文件系统工具和代码执行能力

**3\. 支持的平台**

**基础 Skills(能力包型):**

- ✅ Claude.ai(Web、Desktop)
- ✅ Claude Code

**软编排 Skills(调度 sub-agents):**

- ✅ Claude Code\*\*

**重点**:本文演示的 **srt-workflow 案例只能在 Claude Code 使用** 。

### 关键限制

**限制 1:Skills 不跨平台同步**

- Web 上创建的 Skill，不会自动同步到 Claude Code
- 建议用 Git 管理你的 Skills 文件夹

**限制 2:触发机制依赖 Claude 理解**

- Skills 是根据 `description` 自动触发的
- 如果 description 写得不够清楚，可能不会被触发
- 或者多个 Skills description 重叠，可能触发错误

**解决方案:**

- 在 description 中明确触发条件
- 用排除词标明"不适用场景"
- 必要时可以手动指定 Skill 名称

**限制 3:软编排的并行度上限**

- 官方文档限制:**最多 10 个并行 sub-agents**
- 大多数场景用不到这么多
- 如果真的需要更高并行度，建议用硬编排(例如使用 Make 自动化工作流平台)

### 什么情况不适合用 Skills?

- ❌ 一次性任务(不会重复执行)
- ❌ 非常简单的指令(直接说就行)
- ❌ 需要频繁调试的流程(触发不可控)
- ❌ 需要精确控制的后端服务(用 API + 硬编排，例如使用 Make 自动化工作流平台)

## MAPS 复盘:从 Skills 看 AI 工作流的演进

好，限制说清楚了。现在你应该对 Skills 有完整的理解了。

最后，我用 MAPS 框架帮你复盘一下，然后给你一套立刻能动手的行动步骤。

用我的 MAPS 框架来看 Skills，你会发现它触及了四个维度:

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/sites/2148203597/images/1de53a-683-bf75-4d68-237f3168f84c_CleanShot2025-11-18at23.13.00.png)

**M - Mindset(思维)**

- 从"写 Prompt"到"写资产"
- 把隐性知识显性化、模块化
- 不再是"我知道怎么做"，而是"我把它封装成了可复用的模块"

**A - Architecture(架构)**

- Skills 作为中间层:连接 Prompt 和 Agent
- 软编排:声明层(Skill) + 执行层(Sub-agents)分离
- 资产化:你的判断逻辑变成可版本化、可共享的数字资产

**P - Prompt(提示工程)**

- Prompt 不再是临时指令，而是封装在 Skill 里的规范
- 渐进式披露:只在需要时加载，节省 token
- 可测试性:每个 Skill 可以独立迭代优化

**S - Systems(系统思维)**

- Skills 不是孤立功能，而是可以和 MCP、工具调用、多 Agent 协作
- 从单点工具到完整工作流
- 可组合性:Sub-agents 可以被多个 Skill 复用

**这就是为什么我说 Skills 有机会改变 Agent 格局:**

它不是简单的"功能更新"，而是提供了一套 **从 Prompt 到 Agent 的中间层抽象** ，让我们可以:

- 把判断逻辑资产化
- 把工作流模块化
- 把个人经验可复用化

---

## 本期会员专属福利：discussion-organizer Skill

在进入快速上手指南之前，本期为 AI 精英圈会员提供一个特别福利： **我实际使用的 discussion-organizer Skill 完整包** 。

### 这个 Skill 能帮你做什么？

这是我日常用来整理学习笔记和讨论记录的工具，可以：

✅ **自动分类和评估** ：将混乱的笔记按 TBRC 框架组织成结构化素材库

✅ **事实核查机制** ：主动验证内容可信度，区分已验证事实、合理观点和推测内容

✅ **完整工作流程** ：包含 8 个阶段的标准操作流程（SOP）

✅ **质量控制标准** ：内置质量检查清单，确保输出的一致性和准确性

### Skill 包内容

完整的 Skill 包含：

1. **SKILL.md** （376 行）：核心工作流程和判断逻辑
2. **sop-complete.md** （1100+ 行）：详细的标准操作流程指南
3. **templates.md** ：TBRC 和洞察库的输出模板

### 使用场景

适合以下场景：

- 整理技术学习笔记
- 提炼会议/直播讨论记录
- 为教程和文章准备结构化素材
- 建立个人知识库

### 如何获取

AI 精英圈会员可以在「精英圈 PRO 资源库」下载完整的 Skill 文件包。

**重要提醒** ：此 Skill 包含完整的工作流程设计和判断标准，仅供 AI 精英圈付费会员内部使用，不要外传。

---

## 5A+ 快速启动:今天就能动手

如果你想今天就开始用 Skills，跟着这个 5A+ 框架走:

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/sites/2148203597/images/06d2aaa-4a58-32b4-16ca-5fa30850fc47_CleanShot2025-11-18at23.17.42.png)

### A1 - Aim(目标)

问自己:

- 我有哪些任务，每周要重复 3 次以上?
- 这些任务里，哪些需要复杂的判断逻辑?

### A2 - Acquire(获取)

首先， **你得知道不同平台的安装方法**:

**Claude Web / Claude Desktop(上传 ZIP 文件)**

1. 将 Skill 文件夹打包成 **ZIP 文件** ，扩展名为.skill
2. 打开 Claude，进入 **Settings > Capabilities**
3. 确保开启了 **Code execution and file creation**
4. 滚动到 **Skills** 部分
5. 点击 **"Upload skill"** 按钮
6. 选择你打包好的 ZIP 文件上传

**Claude Code(拷贝文件夹)**

Claude Code 不需要上传，直接把 Skill 文件夹放到指定目录:

- **个人 Skills** (跨所有项目可用):
	- 放到 `~/.claude/skills/` 目录
		- 例如:`~/.claude/skills/discussion-organizer`
- **项目 Skills** (团队共享):
	- 放到项目根目录的 `.claude/skills/` 文件夹
		- 例如:`your-project/.claude/skills/discussion-organizer`

重启 Claude Code 后生效。

---

好，现在你知道怎么安装了。接下来的问题是:**从哪里获取 Skills?**

有三种方式:

**方式 1:从官方和社区获取**

注意，由于 Skill 需要代码执行权限，因此对于第三方的 Skills 大家要谨慎使用：

Anthropic 的官方 Skills，既可以用来学习，还可以用来把他们放到 Claude Code 里用:

- [https://github.com/anthropics/skills](https://github.com/anthropics/skills)

Awesome 系列，这是 Skills:

- [https://github.com/BehiSecc/awesome-claude-skills](https://github.com/BehiSecc/awesome-claude-skills)

**方式 2:用 skill-creator 快速转换(最简单)**

这是 Anthropic 官方提供的一个 Skill，专门用来把你现有的 Prompt 或者技能经验转成 Skill。

具体操作:

- 激活 skill-creator

在 Claude Desktop 里打开 skill-creator skill

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/sites/2148203597/images/3224460-d635-2fd5-641-5d4edda20e_CleanShot2025-11-18at23.22.55.png)

- **创建 Skill**

直接在 Claude 里说:

用 skill-creator 帮我创建一个 Skill，目标是。。。

![](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/sites/2148203597/images/61dd6e6-e3e-046-a48e-54d41cb52dfe_CleanShot2025-11-18at23.23.49.png)

- **Claude 自动生成**
	skill-creator 会帮你生成:
	- SKILL.md(包含完整的 YAML frontmatter)
		- 自动补充 description(触发词)
		- 建议 allowed-tools
		- 优化格式和结构

**为什么推荐用 skill-creator?**

- ✅ 零学习成本，不用手写 YAML
- ✅ 自动生成规范的文件结构
- ✅ 智能补充你可能遗漏的部分
- ✅ 5 分钟就能把常用 Prompt 变成 Skill

**方式 3:手写(适合高级用户)**

- 自己创建文件夹和 SKILL.md
- 完全自定义结构和逻辑
- 适合复杂场景

### A3 - Attempt(尝试)

- 选一个最简单的场景，创建第一个 Skill
- 不要贪多，先做一个能跑起来的
- 测试:看 Claude 能不能自动识别并触发

### A4 - Adjust(调整)

- 如果没触发，调整 description
- 如果输出不稳定，优化判断标准
- 如果 token 消耗太高，考虑文件传递

### A5 - Apply(应用)

- 跑通一个 Skill 后，复制这个模式
- 逐步建立你的个人 Skills 库
- 条件允许的话，分享给团队

### +(进阶)

- 如果你想做软编排，学习创建 Sub-agents
- 如果你想做生产环境，研究 Claude Agent SDK
- 持续关注官方文档更新

## 结语

本文展示了 Skills 的基础用法。如果你想深入掌握:

- 如何写出高质量的判断逻辑
- 软编排的完整工程化方法论
- 如何把 Skills 用在真实项目里

可以关注我的 **MAPS 课程** 和 **AI 精英圈** ，正在规划更完整的 Skills 直播。

其实你可能注意到了，Skills 重点用的就是 **Markdown 和 YAML** ，这正是我在 MAPS 课程的基础技术章节里重点教的内容，还包括 JSON、XML、API 这些。

当初设计这个课程时，很多人觉得"学这些基础干嘛"。但现在你看，这些恰恰是 AI 时代最需要掌握的技术基础。

这也验证了我一直强调的:**AI 时代不是不需要技术基础，而是需要恰到好处的技术基础，既不会眉毛胡子一把抓，也不会知其然不知其所以然。**

[![旗舰课程 MAPS™ 训练营](https://kajabi-storefronts-production.kajabi-cdn.com/kajabi-storefronts-production/file-uploads/themes/2156870510/settings_images/23a3eb-0a7-d375-e087-471c55cca_a2e26503-cd1e-4000-adc7-d8655929c339.jpg)](https://www.axtonliu.ai/resource_redirect/landing_pages/2151451780)

## 这篇文章解决了你的一个“点”

但真正的竞争力，来自于将无数个“点”连接成“系统”的能力。旗舰课程 **MAPS™ 训练营** 专为此而设，带你从解决零散问题，到构建一个能为你持续创造价值的AI强大引擎。