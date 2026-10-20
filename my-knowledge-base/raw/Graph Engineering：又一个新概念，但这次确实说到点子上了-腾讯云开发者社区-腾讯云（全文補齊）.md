---
title: "Graph Engineering：又一个新概念，但这次确实说到点子上了-腾讯云开发者社区-腾讯云（全文補齊）"
source: "https://cloud.tencent.com/developer/article/2715098"
author: "老周聊架构"
published: "2026-07-24"
created: 2026-09-12
description: "與同名原檔為同一篇文章。原始 Obsidian Web Clipper 擷取僅抓到頁面導覽列（首頁／MCP廣場／返回腾讯云官網），正文完全遺失，僅 978 bytes。本檔透過 WebFetch 重新抓取，正文完整補齊，供後續知識庫整理使用。"
tags:
  - "clippings"
status: processed
---

> 補齊說明：`Graph Engineering：又一个新概念，但这次确实说到点子上了-腾讯云开发者社区-腾讯云.md`（原檔）因 Web Clipper 擷取失敗，僅存導覽列骨架（978 bytes）。依 CLAUDE.md「NEVER modify or delete files in raw/」規則，原檔不可改寫，故另存本檔案保留完整正文。原檔與本檔為同一篇文章，處理時請以本檔為準。
>
> 本檔內容已於 2026-09-12 整理進 [[graph-engineering-node-edge-state]]（FSM 框架、AutoGen/CrewAI/LlamaIndex Workflows 對照、觀察性、三重退出條件、Reducer 範例）與 [[ai-engineering-terms-genealogy]]（作為「轉載幾輪變定論」現象的具體例證，發布日 2026-07-24 晚於該條目查證基準日 2026-07-21 三天），status 已標記 processed。

# Graph Engineering：又一个新概念，但这次确实说到点子上了

作者：老周聊架构
发布时间：2026-07-24 12:36:13

---

哈喽大家好，我是「老周聊架构」的主理人老周。

AI 圈造词能力一直是世界顶尖水平。上周刚消化完 **Harness Engineering**，这周 **Graph Engineering** 来了。我的第一反应跟大多数人一样：又是换皮？

但仔细想了一下，这次不一样——不是在炒概念，是工程化走到这里，真的需要「图」来描述问题了。

我们先把这条演进链捋一遍，搞清楚每一代在解决什么问题，再说图是怎么自然长出来的。

---

### 五代工程，一条清晰的脉络

**第一代：Prompt Engineering（提示词工程）**

最早大家接触 LLM，研究的问题很简单：怎么写一条好 prompt，让模型输出更好的结果？

这阶段的心智模型是「咒语」——你得摸清楚模型的脾气，用对魔法词汇，它才给你想要的答案。Chain-of-Thought、few-shot、角色扮演、思维链……全是这个时期的产物。

**局限在哪里**：它只管「输入怎么写」，对于复杂任务，单条 prompt 撑不住。你没法用一句话让模型帮你完成一个需要检索资料、多轮推理、调用外部工具的任务。

**第二代：Context Engineering（上下文工程）**

意识到 prompt 只是上下文的一部分，真正影响模型输出的是**它能看到什么**。

于是问题从「怎么写」升级到「给什么看」：RAG 检索到的文档、对话历史、工具返回值、系统状态、用户画像——这些都是上下文的组成部分。Context Engineering 研究的是如何精心构造这个「信息包」，让模型在最合适的信息环境下做决策。

**局限在哪里**：它解决了信息输入的问题，但没有解决「谁来驱动执行」的问题。构造好上下文之后，模型调用一次就结束了，复杂任务需要多次调用、多步协作，这套框架管不了。

**第三代：Harness Engineering（驾驭工程）**

把模型包在一个完整的运行环境里。不只是输入输出，而是整套脚手架：工具调度、权限管控、验证逻辑、错误处理、可观测性、Human-in-the-Loop……

我之前写过一篇专门讲这个，12 个组件，把 Agent 从「能跑的玩具」变成「能上线的生产系统」。

**局限在哪里**：Harness 解决了单个 Agent 的生产化问题，但它默认的运行模型还是「一个 Agent 跑一个任务」。当任务复杂到需要多个 Agent 分工协作，Harness 给不出结构上的答案。

**第四代：Loop Engineering（循环工程）**

这一代开始真正处理「长时间自动运行」的问题。

最早的 Agent 是个简单 while 循环：感知环境 → 思考下一步 → 执行动作 → 再感知。Loop Engineering 是在外面再套一层父循环——Agent 不只是执行单次任务，而是持续感知环境变化，自主规划，减少人工干预，直到达成目标。

这就是最近很火的「长任务 Agent」背后的结构：Agent 自己检查结果是不是符合目标，不符合就继续跑，直到满意或者达到退出条件。

**局限在哪里**：while 循环是单线程的。它能描述「一个 Agent 持续执行」，但描述不了「多个 Agent 并行协作、互相依赖、结果互传」的场景。

这时候，图出现了。

---

### 为什么是图？从一个真实任务说起

让我用一个真实的例子把这个问题说透。

我最近在做一个内容生产的大任务，拆开来大概是这样的结构：

- **Agent A** 理解用户需求，产出结构化的任务描述（目标、约束、输出格式）
- **Agent B** 拿到任务描述，去做资料检索，构建上下文包（RAG + 网络搜索 + 历史案例）
- **Agent C** 基于上下文包做核心内容生成，产出草稿
- **Agent D** 质检裁判——判断草稿是否达标
- 达标：流程结束，输出结果
- 不达标：**打回重做，回到 Agent B**，重新检索，重新生成

你能用 while 循环描述这个结构吗？

不能。

while 循环描述的是一个节点在时间轴上的重复。但这里是**四个不同的节点，有明确的数据流向，有条件分支，还有回环**。

你能用 DAG（有向无环图）描述吗？

也不能。DAG 要求无环——CI/CD 流水线、Spark 数据处理管道，是 DAG，因为每一步走完就走完了，不会回头。但我的场景里，D 判断不合格要回到 B——这就是一个环，DAG 建模不了。

你能用简单的树形结构描述吗？

还是不能。树形结构没有「汇聚」概念——Agent C 需要同时接收来自 A 和 B 的输出，树形描述不了多对一的关系。

**唯一能自然描述这个场景的，是有向图（Directed Graph）。**

节点是 Agent，有向边是数据流，条件边是分支逻辑，回边是重试环路。图天然能表达：并行、串行、汇聚、分支、循环——这些正是真实多 Agent 任务所需要的全部拓扑元素。

这不是强行套概念，是**现实逼出来的结构选择**。

---

### 图 vs DAG vs FSM：说清楚三个概念

这三个概念经常混用，我来把它们说清楚。

**DAG（Directed Acyclic Graph，有向无环图）**

有方向、无环。数据从上游流向下游，不会回头。

适合场景：编译依赖、数据处理管道、批处理流水线。特点是可以做拓扑排序，执行顺序确定，推理简单。

**不适合 Agent 编排的原因**：真实任务里，失败重试、质检回环、多轮协商，全都要求有环。强行用 DAG，要么你把「重试」建模成一堆重复的节点（极其丑陋），要么你在节点内部偷偷做循环（等于在 DAG 外面藏了个状态机，欺骗自己）。

**有向图（Directed Graph，DG）**

有方向、允许有环。是 DAG 的超集。

Agent 编排用的就是这个：节点是 Agent 或处理步骤，边是数据流或控制流，允许回边（回环），允许条件边（不同条件走不同路径）。

**FSM（Finite State Machine，有限状态机）**

有限状态机是有向图的一种特殊形式，强调「状态」和「状态转移」。

节点 = 状态，边 = 转移条件，系统在有限个状态之间切换，直到到达终态。

为什么说 Graph Engineering 更像 FSM？

因为它不只是描述「数据怎么流」，更描述「系统处于什么状态、在什么条件下切换到下一个状态」。Agent D 判断合格，系统从「待审核」状态切换到「完成」状态；判断不合格，系统从「待审核」切换回「待检索」状态——这就是一个 FSM 的状态机描述。

FSM 这个概念有几十年历史了，用在 UI 状态管理（XState）、游戏 AI（行为树）、网络协议（TCP 状态机）都是经典场景。现在轮到了 LLM Agent 编排。

**概念不新，但用对地方，就是对的工具。**

---

### 现在的工具怎么实现

这套思路已经有成熟的工程实现，我挑几个主流的说。

**LangGraph**

LangChain 团队出的，也是目前最主流的 Graph Engineering 框架之一。核心抽象就三个：节点（Node）、边（Edge）、状态（State）。

代码示例：

```python
from langgraph.graph import StateGraph, END
from typing import TypedDict

# 定义全局状态结构
class AgentState(TypedDict):
    task: str
    context: list[str]
    draft: str
    quality_score: float
    retry_count: int

# 定义各个节点（每个节点是一个函数）
def agent_a_understand(state: AgentState) -> AgentState:
    """需求理解：把用户输入转成结构化任务"""
    structured_task = llm.invoke(f"结构化以下任务：{state['task']}")
    return {"task": structured_task}

def agent_b_retrieve(state: AgentState) -> AgentState:
    """资料检索：构建上下文包"""
    context = rag.search(state["task"], top_k=5)
    return {"context": context, "retry_count": state.get("retry_count", 0)}

def agent_c_generate(state: AgentState) -> AgentState:
    """核心生成：基于上下文产出草稿"""
    draft = llm.invoke(build_prompt(state["task"], state["context"]))
    return {"draft": draft}

def agent_d_judge(state: AgentState) -> AgentState:
    """质检裁判：打分判断是否合格"""
    score = evaluator.score(state["draft"], state["task"])
    return {"quality_score": score, "retry_count": state["retry_count"] + 1}

# 条件边：D 的结果决定走哪条路
def should_retry(state: AgentState) -> str:
    if state["quality_score"] >= 0.85:
        return "end"           # 合格，结束
    if state["retry_count"] >= 3:
        return "end"           # 超过最大重试次数，强制结束
    return "retry"             # 不合格，打回重做

# 构建图
graph = StateGraph(AgentState)
graph.add_node("understand", agent_a_understand)
graph.add_node("retrieve",   agent_b_retrieve)
graph.add_node("generate",   agent_c_generate)
graph.add_node("judge",      agent_d_judge)

# 连边
graph.set_entry_point("understand")
graph.add_edge("understand", "retrieve")
graph.add_edge("retrieve",   "generate")
graph.add_edge("generate",   "judge")

# 条件边：judge 根据结果决定走 end 还是 retry（回到 retrieve）
graph.add_conditional_edges(
    "judge",
    should_retry,
    {"end": END, "retry": "retrieve"}   # retry 时回到 retrieve，形成环路
)

app = graph.compile()
result = app.invoke({"task": "帮我分析这份合同的风险条款", "retry_count": 0})
```

几个关键设计值得注意：

1. **全局状态（State）是图的核心** 所有节点共享同一个状态对象，节点的输入输出都是对状态的读写，这解决了节点间数据传递的问题。

2. **条件边（Conditional Edge）实现分支** `add_conditional_edges` 接收一个函数，根据当前状态返回下一步走哪个节点——这就是 FSM 里的状态转移条件。

3. **环路靠条件边实现** `"retry": "retrieve"` 这一行，就是那条回边，让图从 judge 回到 retrieve，形成环路。

4. **退出条件必须明确** `retry_count >= 3` 是硬性出口，防止死循环。这不是可选的，是必须设计的。

**LlamaIndex Workflows**

事件驱动模型，节点之间通过事件（Event）而不是直接边来通信。更松耦合，节点不需要知道谁会消费自己的输出，只负责发出事件。

适合异步场景、节点解耦要求高的系统。

**AutoGen（微软）**

不用图来显式建模，而是让 Agent 之间直接对话——一个 Agent 可以调用另一个 Agent，结果在对话里传递。拓扑结构隐式存在于对话流程里。

灵活性高，但可读性和可调试性相对弱。适合对话式协作，不适合需要精确控制流程的场景。

**CrewAI**

用「团队」（Crew）+ 「任务」（Task）的概念来描述多 Agent 协作。背后是流程图，支持顺序执行和层次化执行。

对不懂图论的人更友好，代码更声明式，适合快速搭原型。

---

### 工程落地，有几个坑要提前踩

理解了概念，落地的时候还有几个实际问题要处理。

**坑一：状态管理的写冲突**

单循环 Agent，状态是串行的，不存在并发问题。图里一旦有并行节点（比如 A 和 B 同时跑），两个节点可能同时写状态——谁的写入算数？

LangGraph 的解法是 Reducer：你可以为每个状态字段定义一个合并函数，指定并行写入时如何合并。比如 `context` 字段的 Reducer 是 `operator.add`，两个节点写入的列表会被合并而不是覆盖。

**坑二：环路的出口设计**

设计含环的图，第一件事不是画节点，是想好出口在哪里。

出口通常有三种：

- **质量阈值** score >= 0.85 就退出
- **最大迭代次数** retry_count >= N 强制退出
- **超时** 整条链路超过 X 秒强制退出

三种最好组合使用。只有质量阈值没有次数限制，容易死循环；只有次数限制没有质量判断，可能过早截断。

**坑三：节点的错误边界**

单个节点挂了怎么办？整条图要不要重跑，还是从挂的节点重试？

LangGraph 支持在编译时配置错误处理策略，也支持 Checkpointing——把每个节点执行后的状态持久化，失败时从 checkpoint 恢复，不用从头跑。

对于耗时长、成本高的图，Checkpointing 是必选项，不是锦上添花。

**坑四：可观测性**

图比循环更难调试，原因很简单：一次执行可能走 A→B→C→D→B→C→D→END 这样的路径，也可能走 A→B→C→D→END，路径不确定。

没有追踪，你不知道这次执行走了哪条路、在哪个节点花了多少时间、哪个节点的输出质量有问题。

这就是为什么我上一篇花整篇文章讲 Langfuse——Observability 在单 Agent 场景是重要的，在多 Agent 图里是**不可妥协的**。你得能在 trace 里看到完整的节点执行顺序、每个节点的输入输出、走了哪条条件边。

**坑五：节点粒度**

图里节点的粒度是个需要仔细拿捏的设计决策。

节点太粗：A 节点做了「理解需求 + 检索 + 生成」三件事，图退化成两三个节点的线性结构，失去图的表达价值——出了问题你也不知道是哪步出的。

节点太细：每个小操作都是一个节点，图变成有几十个节点的意大利面，维护噩梦，图结构反而比代码更难读。

合理的粒度原则：**每个节点有且只有一个清晰的职责，它的输入和输出契约能用一句话说清楚。** 如果你描述一个节点需要说「它做 A，然后做 B，还会做 C」，就应该拆成三个节点。

---

### 为什么现在才出现这个概念

有人可能会问：FSM 是几十年前的东西，图算法是 CS 基础课，为什么现在才有 Graph Engineering 这个词？

因为之前不需要。

早期的 LLM 应用，任务简单，一问一答。一个 prompt 进去，一个回答出来。没有多步推理，没有工具调用，更没有多 Agent 协作。Prompt Engineering 就够了。

后来任务复杂了一点，需要给模型提供更多上下文，Context Engineering 出来了。再后来需要调工具、接系统，Harness Engineering 出来了。再后来需要长时间自动运行，Loop Engineering 出来了。

**每一代概念的出现，都是被真实任务的复杂度推出来的。**

现在的情况是：AI 的能力强到足以承担「有相当复杂度的真实工作任务」了。一个真实的工作任务，不是线性的——它有分工、有并行、有依赖、有质检、有重做。这种结构，只有图能自然描述。

工具层面也成熟了。LangGraph、AutoGen、CrewAI、LlamaIndex Workflows 这些框架，都在用图来做 Agent 编排，而且在生产环境里跑起来了。框架收敛了，概念自然就来了。

**Graph Engineering 不是有人拍脑袋造出来的词，是大量工程师踩了同一类坑之后，集体意识到需要一个统一的描述框架。**

名字是新的，问题是真实的，工具是现成的——这种组合，才是一个概念真正站得住脚的条件。

---

### 小结

把这一代代工程范式串起来，你会看到一条清晰的演进方向：

**从「人来控制」到「结构来控制」。**

- **Prompt Engineering**：人精心设计输入，控制单次输出。
- **Context Engineering**：人设计信息环境，控制模型的认知范围。
- **Harness Engineering**：人设计运行框架，控制 Agent 的行动边界。
- **Loop Engineering**：人设计循环逻辑，模型自主持续执行。
- **Graph Engineering**：人设计协作拓扑，多个模型在图中分工完成复杂任务。

每一步，人退一步，结构进一步。

Graph Engineering 的意义不只是「用图来描述 Agent 流程」，而是**把软件工程里成熟的状态机思想，系统性地引入到 LLM Agent 的设计里**——这是两个领域的一次正式握手。

FSM、DAG、有向图——这些概念在经典软件里早就有了。它们能撑住几十年，是因为它们描述的是真实问题的本质结构。现在 LLM Agent 的复杂度到了，这些工具自然被拿过来用。

不是旧酒新瓶，是旧工具遇到了新问题，刚好合适。

**你现在的多 Agent 系统是什么拓扑？线性、树形还是有环图？有没有在真实任务里用过 LangGraph？评论区聊聊。**

— 完 —

---

**标签**：系统、graph、工具、模型、设计
