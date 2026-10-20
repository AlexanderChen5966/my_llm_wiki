---
title: "实战：Graph Engineering 到底在 Engineering 什么？"
source: "https://zhuanlan.zhihu.com/p/2064791235676606842"
author:
  - "[[居丽叶]]"
published:
created: 2026-09-03
description: "大家好，我是居丽叶。 过去这段时间，Agent 圈造词的速度确实有点快。 前段时间大家还在讲 Harness Engineering：怎样用工具、权限、沙箱和上下文把 Agent 管住。紧接着，Loop Engineering 又开始刷屏：怎样让 Age…"
tags:
  - "clippings"
status: processed
---
7 人赞同了该文章

大家好，我是居丽叶。

过去这段时间，Agent 圈造词的速度确实有点快。

前段时间大家还在讲 [Harness Engineering](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=Harness+Engineering&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiJIYXJuZXNzIEVuZ2luZWVyaW5nIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6Mjc5ODU2MDM0LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.15mA-TEc_zXObkS18DJ7C1moxpqepOS8M6qinYUm6gY&zhida_source=entity) ：怎样用工具、权限、沙箱和上下文把 Agent 管住。紧接着，Loop Engineering 又开始刷屏：怎样让 Agent 根据反馈持续执行。现在，Graph Engineering 又来了。

很多人看到这里，第一反应大概是：图、 [状态机](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E7%8A%B6%E6%80%81%E6%9C%BA&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLnirbmgIHmnLoiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.YnyItJCDP_uSG6Ug0t8MLZgzOVGwdHjPkkjF-_dRd3c&zhida_source=entity) 、工作流早就存在，这次又把哪项旧技术换了个名字？

但如果因此武断地把 Graph Engineering 当成纯粹的营销词，也会错过 Agent 工程正在发生的一次 [重心迁移](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E9%87%8D%E5%BF%83%E8%BF%81%E7%A7%BB&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLph43lv4Pov4Hnp7siLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.i04yb4XzQJn0pOfKZO79z5TBY_GWN2n7rOt5ycPSLls&zhida_source=entity) 。今天越来越多的系统开始同时运行多个 Agent，处理任务依赖、并行执行、证据核验、局部失败、人工审批和中断恢复。

这些问题都存在于模型之外，也很难只靠单个 Agent 内部的循环解决。

这篇文章主要讲三件事：

1. Graph Engineering 到底是什么；
2. 为什么说名词在变， [工程能力](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E5%B7%A5%E7%A8%8B%E8%83%BD%E5%8A%9B&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLlt6XnqIvog73lipsiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.IkEJaMUsCtqmwsuDjVqo1iYaS1tO7flgnhny8Yj8RCo&zhida_source=entity) 却在螺旋式上升；
3. 同一个股票研究 Agent，采用 [固定拓扑](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E5%9B%BA%E5%AE%9A%E6%8B%93%E6%89%91&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLlm7rlrprmi5PmiZEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.D0l5Av5vd7dqqCBaBwzq-EL79hJ7sH7I2BNLX05tEqA&zhida_source=entity) 、单 Agent Loop、进一步引入 Graph Engineering，分别会长成什么样。

## 先把 Graph Engineering 说清楚

我比较认同下面这个定义：

> **Graph Engineering，是把模型调用、Agent Loop、确定性代码、工具和人工决策组织成一张显式执行图，并设计它们之间的状态传递、 [动态调度](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E5%8A%A8%E6%80%81%E8%B0%83%E5%BA%A6&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLliqjmgIHosIPluqYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.wdeLBF8REoxdQQGr4WF1tzsU_JvyrvQdlpl_Z9eArqk&zhida_source=entity) 、并行汇合、结果验证、中断恢复和责任边界。**

这里的 Graph，主要指 [控制图](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E6%8E%A7%E5%88%B6%E5%9B%BE&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLmjqfliLblm74iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.llNV4GHyXjwnJo28u2LAoeWzHhDYJ6JKYwyiv2_KZX4&zhida_source=entity) 。

- **节点** 表示一个执行单元，可以是普通函数、模型调用、带工具的 Agent、Verifier，也可以是人工审批；
- **边** 表示执行关系，例如顺序、条件分支、并行、循环和等待；
- **State** 保存跨节点共享的任务事实，例如计划、证据、预算、错误和审批结果；
- **Controller** 决定下一步继续分发、重新规划、转人工、生成结果还是安全终止。
![](https://picx.zhimg.com/v2-8d066efa3179dc1022a8f9c0e71b1cdd_1440w.jpg)

它和 **GraphRAG 里的知识图谱** 要分开。 [知识图谱](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=2&q=%E7%9F%A5%E8%AF%86%E5%9B%BE%E8%B0%B1&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLnn6Xor4blm77osLEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MiwiemRfdG9rZW4iOm51bGx9.Lx-o8scXI8wgL-MfnFcIj2Bruv2EB4MjDSv_9qHH5L0&zhida_source=entity) 描述实体之间的关系，控制图描述系统下一步运行谁。Agent Trace 也可能画成图，但 Trace 用于回放一次执行，控制图负责驱动执行。

Graph Engineering 也不等于 LangGraph。LangGraph 是一种实现框架；普通 Python、Temporal、Airflow 或其他 [工作流引擎](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E5%B7%A5%E4%BD%9C%E6%B5%81%E5%BC%95%E6%93%8E&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLlt6XkvZzmtYHlvJXmk44iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.ytn2ikx3iX6oVy5AKw94L7FmhP7KNL6Es9MzgB0NEwU&zhida_source=entity) 同样可以承载图工程。反过来，项目里调用了 `StateGraph` 、注册了几个节点，也不能自动证明这套系统已经做好了 Graph Engineering。

真正要看的是： **图有没有承担系统可靠性的责任** 。

任务是否会根据输入改变？并行结果怎样合并？一条结论有没有证据？部分节点失败以后重跑哪里？程序退出后能否继续？哪些决定留给模型，哪些决定必须由规则或人来做？

如果这些问题还没有进入设计，代码即使画成了图，也可能只是一条换了外观的固定流水线。

## Graph 和传统 Workflow 差在哪里？

节点、边、条件路由、状态和 checkpoint 都有很长的工程历史。Graph Engineering 沿用了工作流、状态机和 [分布式调度](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E5%88%86%E5%B8%83%E5%BC%8F%E8%B0%83%E5%BA%A6&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLliIbluIPlvI_osIPluqYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Ax8LnZ8XFGsZuvpCVeLCotDdqVB2iNRytzh-LlfYDDs&zhida_source=entity) 中的大量思想。

变化主要来自 Agent 节点的行为。

传统工作流节点执行 SQL、HTTP 请求或确定性脚本，成功和失败相对容易判断。Agent 节点可能正常返回一份语言流畅的报告，其中却混入了错误数字、遗漏条件或没有来源的推断。进程成功，只能说明模型给出了输出；任务是否合格，还需要规则、测试、证据或独立 Reviewer。

传统节点的职责通常提前定义。Agent 节点收到的任务可能是“研究这家公司最近的经营风险”，它会自行检索、选择工具、拆分子任务，并根据中间结果调整方向。

路由也可能带有语义判断。财务指标超过阈值可以交给代码判断；一条新闻属于经营风险、监管风险还是市场噪声，更适合由模型分类。

因此，一张 Agent Graph 通常同时包含两类控制：

- 确定性控制：权限、预算、依赖、schema、停止条件、人工审批；
- 概率性控制：任务拆解、语义路由、研究策略、冲突解释。
![](https://pic4.zhimg.com/v2-8a3abbc9d53fef1087a5eedb67a36d41_1440w.jpg)

Graph Engineering 的工作，就是决定这两类控制怎样组合。

## 名词在变，技术真的在进步吗？

从 Harness Engineering 到 Loop Engineering，再到 Graph Engineering，很容易被理解成一串互相取代的新概念。

我更愿意把它们看成不同尺度的工程对象：

| 工程对象 | 主要关注的问题 |
| --- | --- |
| Harness Engineering | 模型在什么工具、权限、沙箱和日志环境里工作 |
| Loop Engineering | 执行以后怎样验证、反馈、重试和停止 |
| Graph Engineering | 多个 [执行单元](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=2&q=%E6%89%A7%E8%A1%8C%E5%8D%95%E5%85%83&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLmiafooYzljZXlhYMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MiwiemRfdG9rZW4iOm51bGx9.LfzqVQ6Fvy-OsodOGcLk6XxBdaGubOm0CPfjyaxXFlo&zhida_source=entity) 怎样依赖、并行、汇合、恢复和交接控制权 |

更准确地说，它们是一组逐层展开的包含关系。

Loop Engineering 要持续执行，离不开 Harness 提供的工具、权限、 [沙箱](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=3&q=%E6%B2%99%E7%AE%B1&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLmspnnrrEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MywiemRfdG9rZW4iOm51bGx9.Ivxmi3EbcOujfxV7IvGo9aPjANdJueUz8tZeD7LQwBA&zhida_source=entity) 、上下文和日志环境；Graph Engineering 编排的某个节点，又可以是一只运行 ReAct Loop 的 Agent。Prompt 和 Context 仍在更里层，负责一次模型调用中的指令表达与信息供给。

工程对象随着系统尺度向外扩展：从模型怎样在受控环境里工作，到单个 Agent 怎样根据反馈持续行动，再到多个执行单元怎样协作。

![](https://pic1.zhimg.com/v2-2f0c3761650ec1f5d91608a162bcb3f4_1440w.jpg)

所以，Graph Engineering 把 Loop Engineering 纳入了更大的控制尺度。Loop 负责节点内部的探索与纠错，Graph 负责节点之间的依赖、并行、汇合与恢复。

Josh C. Simmons 在《We Are Entering the Graph Engineering Phase》中提出，过去两年的 Agent 大多可以概括为“模型套在 [while loop](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=while+loop&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiJ3aGlsZSBsb29wIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6Mjc5ODU2MDM0LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.Tdua-Psz-pYM5zV1EIw0JgBE6ng9B5BDxNUmqM0xQwQ&zhida_source=entity) 里”。当单个步骤逐渐可靠， [工程瓶颈](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E5%B7%A5%E7%A8%8B%E7%93%B6%E9%A2%88&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLlt6XnqIvnk7bpoogiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.BwrKi3JDWOSRPwZ2JJh8DV2BP0jKWfZ69qXm4fnxW18&zhida_source=entity) 会向多个执行单元之间的并行、协作、恢复和审批移动。\[1\]

LangChain 则承认 Graph Engineering 带有新 Buzzword 的成分，同时强调图适合表达可预测的业务骨架；面对开放研究等很难提前确定路线的任务，过度预编排反而会压缩 Agent 的探索空间。\[2\]

**新名词经常重新包装旧思想，但旧思想正在处理新的执行对象。**

技术的发展原本就很少沿着直线前进。我们会重复遇到调度、状态、验证和容错，只是每次进入下一圈时，系统规模、自动化程度和不确定性都提高了。

这就是我理解的“螺旋式上升”。

## 用同一个股票 Agent 看三种工程路线

下面用 [知识星球](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E7%9F%A5%E8%AF%86%E6%98%9F%E7%90%83&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLnn6Xor4bmmJ_nkIMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.eWrvc3g6-dFeH2A97o7y3mLaofqaZXG9vZWcUTb-FC8&zhida_source=entity) 里面的股票 Agent 项目来举例子。

用户输入：

> 请研究 [贵州茅台](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E8%B4%B5%E5%B7%9E%E8%8C%85%E5%8F%B0&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLotLXlt57ojIXlj7AiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.V1gVir2ASALgz-c3aUtcqoj62pp-u64w9dWt3QZEdKI&zhida_source=entity) 当前的 [基本面](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E5%9F%BA%E6%9C%AC%E9%9D%A2&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLln7rmnKzpnaIiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.PcUkylGE0rYRTFjJilclgeVBlGLVAA5DpG9c5QY1fv4&zhida_source=entity) 、估值、技术走势和新闻风险，所有结论都要给出证据；如果 [数据冲突](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E6%95%B0%E6%8D%AE%E5%86%B2%E7%AA%81&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLmlbDmja7lhrLnqoEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.aVEX9NapposhEmA4H52gu6VaC3LPXS9K9NKk7dN93iE&zhida_source=entity) ，先让我确认。

![](https://picx.zhimg.com/v2-e1bdca77a7e0c6050a21ac69042a6edd_1440w.jpg)

下面用三种方式实现它。

## 路线一：固定拓扑的 Graph-shaped Workflow

这条路线对应我之前开发的股票 Agent 。

它使用了 LangGraph，四个分析 Agent 并行执行，再统一进入 Summary Agent，形成固定的 `fan-out / fan-in` （并行分发 / 结果汇聚）。项目中的主要代码如下：

```
workflow = StateGraph(AgentState)

workflow.add_node("start_node", lambda state: state)
workflow.add_node("fundamental_analyst", fundamental_agent)
workflow.add_node("technical_analyst", technical_agent)
workflow.add_node("value_analyst", value_agent)
workflow.add_node("news_analyst", news_agent)
workflow.add_node("summarizer", summary_agent)

workflow.set_entry_point("start_node")

# 固定 fan-out：每次都启动四个分析节点
workflow.add_edge("start_node", "fundamental_analyst")
workflow.add_edge("start_node", "technical_analyst")
workflow.add_edge("start_node", "value_analyst")
workflow.add_edge("start_node", "news_analyst")

# 固定 fan-in：四路结果全部交给总结节点
workflow.add_edge("fundamental_analyst", "summarizer")
workflow.add_edge("technical_analyst", "summarizer")
workflow.add_edge("value_analyst", "summarizer")
workflow.add_edge("news_analyst", "summarizer")
workflow.add_edge("summarizer", END)

app = workflow.compile()
```
![](https://pica.zhimg.com/v2-123d4fc19dd6392e30c4a1dcce9a9384_1440w.jpg)

开发者提前决定要运行哪些 Agent、它们怎样连接，LangGraph 负责 [fan-out](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=3&q=fan-out&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiJmYW4tb3V0IiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6Mjc5ODU2MDM0LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjMsInpkX3Rva2VuIjpudWxsfQ.INjqLFTw-q8lHNq8Er8zUBnsIXDrCdLNLCed5RwEe7Q&zhida_source=entity) 和 [fan-in](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=3&q=fan-in&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiJmYW4taW4iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MywiemRfdG9rZW4iOm51bGx9.YKN2D_H0xHPnZU7mhTX2KZno5TAzizRBOJB1RH8hbbI&zhida_source=entity) 。任务无论怎样变化，运行的始终是同一张图。

因此，这种实现更接近 Graph-shaped Workflow：代码已经有图的形状，系统可靠性仍主要依赖各个 Agent 自己完成任务。使用了 LangGraph，不会自动获得动态规划、 [证据门](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E8%AF%81%E6%8D%AE%E9%97%A8&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLor4Hmja7pl6giLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.dVYfzdPjNYgF0XVsuGmVNP00U2El_7fYcDEoRT1PMHM&zhida_source=entity) 、局部恢复和人工接管。

这种实现对于路径固定的业务落地而言，往往是收益最高的选择。

它的优势在于：

- 代码短，开发速度快；
- [数据流](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E6%95%B0%E6%8D%AE%E6%B5%81&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLmlbDmja7mtYEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Zw_4ZMIrLlGjJtMNV47QOYMrtsvW76MfCQMQ4Ugwlc8&zhida_source=entity) 容易理解；
- 调试时顺着调用栈就能定位；
- 任务种类固定时，维护成本低。

但也存在一些问题。

用户只问技术走势，系统仍会把四类分析全部跑一遍；估值节点也不会显式等待基本面证据；某个分支失败，程序要么整条重跑，要么靠额外的异常代码继续拼报告。模型写出一个数字以后，系统也很难回答数字来自哪里、是否过期、有没有经过复核。

一旦任务需要等待人工确认，或者进程中断后继续，固定流程会逐渐堆出大量 `if/else` 、状态字段和 [补偿逻辑](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E8%A1%A5%E5%81%BF%E9%80%BB%E8%BE%91&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLooaXlgb_pgLvovpEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.XMy4vEQ6lh92aZ-TfSCTyRzDZ9dlQ11O8HoTr76_pno&zhida_source=entity) 。那时，系统实际上已经出现了图，只是图藏在 [调用栈](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=2&q=%E8%B0%83%E7%94%A8%E6%A0%88&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLosIPnlKjmoIgiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MiwiemRfdG9rZW4iOm51bGx9.SA7mDyvElxOO-qG4wbNMTFTD-LkCANebmmDAfs-QO_g&zhida_source=entity) 和条件语句里。

### 这条路线适合什么场景？

任务短、路径稳定、调用次数少、失败后整条重跑也能接受时，直接写流程通常更划算。

Agent 项目启动阶段最常见的问题，往往是数据源、Prompt 和验收标准还没有跑通。此时先画一张复杂图，不会自动提高结果质量。

## 路线二：单 Agent Loop

第二种做法，是去掉固定的多节点编排，把任务交给一只带工具的 Agent。下面是一段简化后的 Loop-only 实现：

```
messages = [HumanMessage(content=user_query)]
tool_registry = {
    "get_financials": get_financials,
    "get_prices": get_prices,
    "search_news": search_news,
}

for step in range(MAX_STEPS):
    action = await agent.plan(
        messages,
        tools=list(tool_registry.values()),
    )

    if action.type == "tool_call":
        observation = await tool_registry[action.name](**action.args)
        messages.extend([action.message, observation])
        continue

    if action.type == "finish":
        verdict = verifier.check(action.report)
        if verdict.passed:
            return action.report

        # 证据不足，把反馈放回下一轮
        messages.append(
            HumanMessage(content=verdict.feedback)
        )

raise BudgetExceeded("Agent 已达到最大执行轮数")
```
![](https://pic2.zhimg.com/v2-d4a15885c900a235f226f6658f8c0c0d_1440w.jpg)

这只 Agent 可以自己决定先看财报还是先查新闻；发现 EPS 缺失后补查数据；看到异常消息后继续检索公告；最后由 Verifier 判断证据是否足够，不足就把反馈送回下一轮。

Loop 给系统带来了真实的适应性。

面对“最近为什么跌”“这家公司最大的风险是什么”一类开放问题，开发者很难提前穷举全部路线。让 Agent 根据观察结果继续探索，通常比固定流程更自然。

Loop 的优势在于：

- 路径开放，适合探索型任务；
- Agent 可以根据中间结果修改计划；
- 新工具接入后，不必为每种组合重新画边；
- 代码形态接近模型原生的 Plan—Act—Observe。

它的代价来自“所有事情都挤在一个循环里”。

基本面、 [技术面](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E6%8A%80%E6%9C%AF%E9%9D%A2&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLmioDmnK_pnaIiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.OuD_7uQG5nI8pQmdqnsgAtnOXv0oHOlLx5Q-ohN7GsQ&zhida_source=entity) 和新闻分析怎样真正并行？估值任务是否依赖基本面先完成？某个工具调用成功、另一个调用失败时，重启循环会不会重复消耗？人工审批以后，系统应该从哪一步继续？

如果这些状态主要保存在一长串 Messages 中，执行时间越长，恢复和审计越困难。Loop 可以决定下一步做什么，却不擅长天然表达多个执行单元之间的 [依赖关系](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLkvp3otZblhbPns7siLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.8oNrJGqI9fmCp3XlOwP2FjuJH7iyF_lts41FEvU_7H8&zhida_source=entity) 。

### 这条路线适合什么场景？

路径高度开放、任务主要由一只 Agent 完成、并行和跨角色依赖较少时，Loop 很有价值。

深度研究、代码探索、资料调查都可能先采用这条路线。只要加上验证、预算和停止条件，它就能比固定流水线灵活很多。

## 路线三：股票 Agent 可以怎样引入 Graph Engineering

第三种路线，是在固定图的基础上继续优化，把稳定控制和开放探索分层。

下面讨论的是一组可参考的设计思路。

如果把 Graph Engineering 引入股票 Agent，可以先保留一张稳定控制图：

```
请求解析
→ 混合 Planner 生成任务 DAG
→ Plan Policy 校验
→ Scheduler 找出依赖已满足的任务
→ 动态并行 Worker
→ Evidence Verifier
→ Controller
   ├─ 继续分发后继任务
   ├─ 缺少证据时重新规划
   ├─ 冲突时等待人工处理
   ├─ 证据通过后生成报告
   └─ 无法恢复时安全终止
```

外层控制图保持稳定，每次运行的研究任务图可以根据问题变化。

例如用户只问技术走势，确定性 Planner 可以只生成一个 `technical` 任务；标准综合研究会先并行执行 `fundamental` 、 `technical` 和 `news` ，等 `fundamental` 完成后再启动依赖它的 `valuation` ； [深度研究](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=2&q=%E6%B7%B1%E5%BA%A6%E7%A0%94%E7%A9%B6&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLmt7HluqbnoJTnqbYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MiwiemRfdG9rZW4iOm51bGx9.v7TVTTeff4Z2RiRwadZoBQ7kj2La5w4esrjX51elpxA&zhida_source=entity) 还可以增加 `rag` 与 `cross_check` 。

![](https://picx.zhimg.com/v2-3e38c0d4b6520b944057c497c8a419e7_1440w.jpg)

Planner 可以根据问题生成结构化任务 DAG，也可以在模型不可用时回退到确定性计划。无论计划来自谁，都要经过策略层检查：

- task capability 是否来自白名单；
- 依赖是否存在环；
- 工具、Token 和任务数量是否超出预算；
- 模型有没有尝试生成任意工具名或代码。

Scheduler 不需要提前知道会产生多少个任务。它只读取当前计划，找到依赖已经满足的节点，再动态分发。简化后的思路如下：

```
ready_tasks = plan.find_ready_tasks(completed=state["completed"])

dispatches = [
    Send("research_worker", {"task": task})
    for task in ready_tasks
]

return Command(goto=dispatches)
```
![](https://pic4.zhimg.com/v2-992d321fec22aa9e9ff722495b8a3fb3_1440w.jpg)

每个 Worker 内部仍然可以运行自己的 Agent Loop。Graph 负责 Worker 之间的依赖、fan-out 与 fan-in，Loop 负责单个 Worker 怎样使用工具完成任务。

状态也可以逐步从自由文本升级为结构化对象，分别记录任务计划、依赖、预算、研究结果、证据来源和控制决策。这样一条结论引用了什么数据、哪个任务已经完成、失败后应该重跑哪里，都会更容易追踪。

再往前一步，可以增加独立的证据门：检查来源、时效、checksum、数值冲突和引用完整性。证据通过后生成报告；出现冲突时暂停执行，把批准、修改计划或终止的选择交给人。

![](https://pic2.zhimg.com/v2-7743d30e095385ab93f3f29759f571d5_1440w.jpg)

如果任务还需要跨进程恢复，可以配合 [checkpointer](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=checkpointer&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiJjaGVja3BvaW50ZXIiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.RpF-PheHxmC4fFO6q0jx7Lrfbpq3xVStX1ghfGjxc8A&zhida_source=entity) 保存执行位置，让程序恢复后继续未完成任务，减少对数据源和模型的重复调用。

这些优化思路指向同一个目标：让 Graph 承担更多可靠性责任，把不确定的模型行为装进一套可验证、可恢复、有预算的执行制度。

### 这条路线的收益和代价

Graph Engineering 的收益主要出现在复杂任务上：

- 可以显式表达依赖、并行和汇合；
- 失败能够限制在局部；
- State 和 checkpoint 支持中断恢复；
- 验证、预算和人工审批拥有清楚的位置；
- 执行轨迹更容易评测与审计。

代价在于：

- 需要设计状态 schema 和 [reducer](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=reducer&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiJyZWR1Y2VyIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6Mjc5ODU2MDM0LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.XIGfXtrjeH8iyO5Ct297vh97EfgI9h-3NKEjB6g3wvE&zhida_source=entity) ；
- 动态任务、重试、恢复会增加测试组合；
- 节点过细时，系统会变得难读；
- 所有判断都画成边，会压缩 Agent 的自主空间；
- 简短任务使用这套架构，投入很可能超过收益。

Graph Engineering 追求的从来都不该是“边越多越专业”。每一条边都应当对应一项真实约束。

Graph 并不要求所有任务路径都提前写完。稳定主图可以只保留入口、计划校验、调度、验证、checkpoint 和人工接管；本次任务需要哪些 Worker、依赖怎样组织，可以由 AI 在运行时生成。

同时，把所有编排权都交给模型也会产生新的风险。它可能跳过证据验证，创建循环依赖，反复调用昂贵工具，或者绕过人工确认。

更稳妥的做法，是让 AI 生成受约束任务图，让系统掌握道路规则。

## 三种路线放在一起比较

| 维度 | 固定拓扑 | 单 Agent Loop | Graph Engineering |
| --- | --- | --- | --- |
| 路径 | 人提前写好顺序流程 | Agent 根据观察持续决定下一步 | 稳定控制图 + 运行时任务 DAG |
| 自主性 | 低 | 高 | [分层控制](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E5%88%86%E5%B1%82%E6%8E%A7%E5%88%B6&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLliIblsYLmjqfliLYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Wzew9ck5LKyqBSH8AQsz4H9rCFzG-Yjoh-Lk1fog7YQ&zhida_source=entity) |
| 并行与依赖 | 依靠手写并发和条件 | 单循环里较难表达 | 图中显式表达 |
| 状态 | 局部变量或自由文本 | 对话历史与外部 Memory | 结构化 State + checkpoint |
| 验证 | 流程末尾集中检查 | 每轮可以加入 Verifier | 独立验证节点和控制分支 |
| 局部恢复 | 较弱 | 依赖 Loop 自己记住进度 | checkpoint 与任务状态支持 |
| 开发成本 | 低 | 中 | 高 |
| 适合场景 | 短任务、固定流程、快速 Demo | 开放探索、单 Agent 长任务 | 多角色、并行依赖、审批与恢复 |

这里没有绝对正确的路线。

工程判断体现在：你能否用最低的 [结构成本](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E7%BB%93%E6%9E%84%E6%88%90%E6%9C%AC&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLnu5PmnoTmiJDmnKwiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.3tghB5GsCnbSe6xjcrLT8dKqEqYYkffbX9MH-ItIRzg&zhida_source=entity) ，换到当前任务真正需要的可靠性。

## 写在最后

经常有同学问我：

> Agent 岗位真正的技术壁垒在哪里？普通的工作流搭建和工具调用，会不会快速同质化，变成前两年的 Java？

答案是一定会的，花无百日红。

框架调用的门槛一定会降低。今天要手写的工作流，明天可能一句话就能生成；今天还需要开发者配置的工具，下一版模型也许会直接帮你选择。

真正拉开差距的能力，分上下两层，一层靠近业务，一层靠近底层架构：

**往业务层看** ：拿到模糊、零散的需求，你能不能拆解成一套可落地、可校验的 Agent 执行流程？分得清哪些环节交给模型自主判断，哪些必须靠固定规则卡死？哪里允许模型自由探索，哪些风险必须把控制权交还给人工审核？

**往底层架构看** ：一套充满不确定性的 Agent 系统，该怎么搭建完整评估标准？线上出现异常案例，如何快速定位问题、修复漏洞？程序报错时，怎么把故障范围锁死在局部，不影响整体流程？大量并发请求、高频工具调用时，怎么做流量限流、 [背压](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E8%83%8C%E5%8E%8B&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLog4zljosiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.xkxT8RmL1th3ZBsinRsMAdxQ6Feby1oHMPW3_f-RAXY&zhida_source=entity) 管控？后续换成更强的新模型，如何平滑迭代整套链路，不用推翻原有架构全部重写？

Graph Engineering 值得关注，正是因为它把这些系统问题推到了台前，它开始奖励真正懂系统的人：状态机、幂等、 [故障域](https://zhida.zhihu.com/search?content_id=279856034&content_type=Article&match_order=1&q=%E6%95%85%E9%9A%9C%E5%9F%9F&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODg1NzkwMjYsInEiOiLmlYXpmpzln58iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzk4NTYwMzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Hb_EPI0OsW8uVl-RQjIMdQYFJkruh4I2yZiHpAqtyeo&zhida_source=entity) 、backpressure、checkpoint、可观测性。

这件事其实挺有意思。当下AI行业最热的方向，开始重新靠近分布式计算中那些最古老的纪律。

从 Harness Engineering 到 Loop Engineering，再到 Graph Engineering，工程抽象正在一层层向外扩展。我们需要死磕单次模型调用、打磨prompt的场景越来越少，对整体架构设计的要求却越来越高。

---

## 参考资料

\[1\] Josh C. Simmons, [We Are Entering the Graph Engineering Phase](https://link.zhihu.com/?target=https%3A//www.drjoshcsimmons.com/writing/we-are-entering-the-graph-engineering-phase)

\[2\] Sydney Runkle, Harrison Chase, [3 Years of Graph Engineering with LangGraph](https://link.zhihu.com/?target=https%3A//www.langchain.com/blog/3-years-of-graph-engineering-with-langgraph)

\[3\] Ksenia Se, [Is Graph Engineering Real? Why Everyone Is Talking About It](https://link.zhihu.com/?target=https%3A//www.turingpost.com/p/is-graph-engineering-real-why-everyone-is-talking-about-it)

编辑于 2026-07-26 19:18・北京

赞同 7