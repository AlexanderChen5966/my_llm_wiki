---
title: "Anthropic 的 AI Native 实战指南"
source: "https://juejin.cn/post/7678278576718299179"
author:
  - "[[周末程序猿]]"
published: 2026-08-27
created: 2026-09-01
description: "2026-08-21 Anthropic 应用 AI 团队发布了一份完整的实践指南，系统阐述了如何将 AI 融入软件开发生命周期（SDLC）的每一个环节。这份指南的核心观点是：代码已经不再是瓶颈了。"
tags:
  - "clippings"
status: "processed"
---
![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/e5b0fc33b9064acaad4590bc22334acb~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1788413376&x-signature=wTSZYKkcNw1dygzJtl8y2Vi1uNw%3D) 2026-08-21 Anthropic 应用 AI 团队发布了一份完整的实践指南，系统阐述了如何将 AI 融入软件开发生命周期（SDLC）的每一个环节。这份指南的核心观点是： **代码已经不再是瓶颈了** 。过去一年里，AI 写代码的速度已经快到难以想象，但代码周围的流程（规划、评审、部署）还停留在原来的节奏上。

指南围绕六个阶段（Plan、Design、Build、Test、Deploy、Maintain）展开，每个阶段都给出了具体的实施步骤、治理框架和度量指标。贯穿全文的一条主线是： **每个阶段结束时写入一个版本控制的产物，下一个阶段读取它** 。

从 `intent.md` 到 `spec.md` 到 `plan.md` ，再到代码、评审、部署，最后事故记录又写回 `intent.md` ，形成完整的循环。提交链本身就是审计记录：谁提了什么需求，Agent 产出了什么，谁批准了它。

> 原文： *The AI-Native SDLC Playbook* · 作者：Louis Claxton（Anthropic 应用 AI 团队） · 2026-08-21

## 代码不再是瓶颈

企业已经开始用 AI 以一年前不可想象的速度来写代码了，但代码周围的流程并没有跟上同样的节奏。

传统 SDLC 有六个阶段：规划、设计、构建、测试、部署、维护。它的设计初衷是为了在一个「编写和实现代码是最耗时、最昂贵的环节」的时代里最大化效率。而现在情况已经变了。

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/a53c9d6dc44b4cae83fcbdd9b3888004~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1788413376&x-signature=3tXOrt4i1rthYf1fvP2jjb8Kg%2Fs%3D)

**构建不再是约束，真正占时间的是它周围那些以人类速度运行的环节。**

构建阶段已经被压缩到了几个小时，而规划、评审、部署这些环节还维持着原来的时长。结果就是整个流程变成了沙漏形，中间快、两头慢。

## 什么是 AI Native SDLC

AI Native SDLC 是一套重新构想的流程，它保留了传统流程的控制目标，但换了全新的执行方式。流程不再是线性的，而是变成了一个循环，AI 被嵌入到了每一个环节中。

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/d196bb01a6ac41c18bdbf561515085b5~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1788413376&x-signature=lTgor8GJY26IxklFH3AH4R0GSBU%3D)

传统流程和 AI Native 流程的对比：

| 阶段 | 传统 SDLC | AI Native SDLC |
| --- | --- | --- |
| 规划 | 需求由委员会收集，经过研讨会和层层签批，手工撰写 | Claude 从源头综合痛点，写入 `intent.md` （人类可读、机器可执行） |
| 设计 | 分析师写规格，设计师再解读 | 需求和设计压缩在与 Agent 的一次工作会话中完成，组织标准以 Skills 形式嵌入，版本化存储在 git 中 |
| 构建 | 测试和代码手写，文档事后补 | 测试和代码由 AI 生成，组织知识维护在版本化的、机器可读的 `CLAUDE.md` 文件和 Skills 中 |
| 测试 | QA 门禁设在阶段边界 | 持续 Evals 贯穿整个实现过程 |
| 部署 | 人类审查每一行代码，治理依赖评审周期 | 多层 Agent 评审，人类审查保留给受监管和关键代码。治理由 Hooks 作为审批门禁来强制执行 |
| 维护 | 人类盯着生产环境找 bug | Agent 监控线上部署，任何突破控制带的异常都会被诊断并写回循环 |

## 六大实践

这份指南的核心是六个实践（Plays），按六个非线性阶段分组：Plan、Design、Build、Test、Deploy、Maintain，合起来覆盖完整的生命周期。

每个实践都包含：发生了什么变化、如何开始、实施步骤、治理考量，以及度量方法。

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/1d6258ab392e4091aa6791d84a9cd44f~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1788413376&x-signature=uifegzwomcmayppfhYD4O3XxfVw%3D)

这六个实践是模块化的，企业可以根据自身需要优先改造不同的阶段。

## 阶段一：规划，用 intent.md 捕获意图

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/9bc02fd5a32343298ccaf43b402d27db~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1788413376&x-signature=2AfSYl7f8x4IUYTULikuFlQA2uU%3D)

**传统做法：** 想法经过需求积压、用户故事、精化会议等流程，然后工程团队才开始行动。

**AI Native做法：** 发起人直接和 Claude 头脑风暴，产出 `intent.md` ，一份用发起人自己的语言写成的原始规格。

**流程：**

1. 发起人用自然语言向 Claude 描述问题
2. 反复讨论直到具体化
3. Claude 按组织模板将结果写成 `intent.md`
4. 发起人纠正理解偏差
5. 提交到共享的版本控制仓库

**intent.md 示例：**

```shell
代码解读复制代码# 意图：理赔状态自助查询
作者：J. Ortiz（理赔运营）。状态：草稿。

## 问题
客户打电话到联络中心询问理赔进度。
客服约 1/3 的通话时间花在纯状态查询上。

## 预期成果
客户在门户网站上可以看到理赔状态、下一步操作和预计日期。

## 受影响的用户和系统
理赔客服、门户团队、理赔核心 API。

## 约束
门户会话中不引入新的 PII。仅使用现有认证。

## 待确认问题
第三方损失评估师是否也需要访问权限？
```

**治理：** 产品负责人审批，决策以 git 历史中的合并或关闭评审记录下来。

**度量指标：**

- **先导指标：** 从第一次对话到提交 `intent.md` 的时间
- **滞后指标：** 被接受进入阶段二的 `intent.md` 存活率

## 阶段二：设计，需求和设计合并为一次会话

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/7fae2760c72b40969119abc852c0daff~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1788413376&x-signature=VkQgrvTI%2F01p1CfOVy%2BesIAU63A%3D)

**传统做法：** 分析师把需求正式化，设计师再反过来解读。两个阶段，又慢又有信息损耗。

**AI Native做法：** 两者合并在一次 prompt 会话中完成。Claude 读取 `intent.md` ，在组织 Skills 的约束下产出规格。

**流程：**

1. 产品负责人打开会话，加载组织 Skills
2. 附上 `intent.md` ，要求标记出关注点
3. 产品负责人对照原始想法审查规格
4. 与策略负责人一起解决标记出的关注点
5. 将 `spec.md` 连同 `intent.md` 一起提交
6. 产品负责人决定是否推进到构建阶段

**Prompt 模板：**

> 读取附件中的 intent.md，为将其集成到我们现有代码库中产出需求和设计规格。应用你可用的 Skills，使方案符合我们的品牌指南、安全策略和 UX 标准。将规格完整地写成 spec.md，可以直接交给工程团队。明确描述任何关注点，尤其是策略之间存在矛盾的地方。

**治理：** 规格撰写过程中实时应用组织策略。Skills、Prompts 和 Skill 版本都记录在版本控制中。

**度量指标：**

- **先导指标：** `intent.md` 和 `spec.md` 提交之间的耗时
- **滞后指标：** 构建开始后的需求返工次数

## 阶段三：构建，没有被接受的计划，就不开始实现

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/0c7d5a4a482146e4b74b32a22982f7a4~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1788413376&x-signature=Rz1Ar95P65FM8tlaz5YunMMqkQs%3D)

### Claude Code 的 Plan 模式

**传统做法：** 工程师读完设计文档就开始写代码，实现计划只存在于工程师的脑子里。

**AI Native做法：** 工作从 Claude 在 Plan 模式下产出的书面计划开始。Plan 模式下 Claude 可以读取代码库但不能修改。

**流程：**

1. 工程师在 Plan 模式下启动 Claude 会话
2. 提供 `intent.md` 和 `spec.md`
3. Claude 创建计划，列出需要修改的文件、工作顺序和验证测试
4. 工程师质询计划（什么可能出问题？风险最高的步骤？有其他方案吗？）
5. 反复迭代，直到一个新人仅凭计划就能实现
6. 将已批准的计划作为 `plan.md` 提交
7. 接受计划，让 Claude 开始实现
8. 实现过程中偏离计划时，在同一个 commit 中更新 `plan.md`

**plan.md 示例：**

```shell
代码解读复制代码# 计划：理赔状态自助查询（来自 intent.md 2026-06-02）

## 需要修改的文件
portal/src/claims/StatusPanel.tsx（新增），claims-api/routes/status.py，
claims-api/tests/test_status.py

## 工作顺序
1. 在现有认证之后添加状态接口
2. 针对接口开发面板
3. 接入门户导航

## 风险
理赔核心 API 限速 50 rps，面板需要做缓存

## 验证
test_status.py 覆盖四种理赔状态；截图与批准的设计稿一致
```

### CLAUDE.md——组织知识文件

放在仓库根目录的文件，提供一个新人入职时需要的全部上下文：

```markdown
代码解读复制代码# 支付服务

## 命令
- 构建：make build
- 测试：make test（单元），make itest（集成，需要 docker）
- Lint：make lint（CI 中运行，推送前必须修复）

## 约定
- Java 21，Spring Boot 3。不引入新的 Lombok
- 金额永远用 BigDecimal，不用 double
- 每个接口都需要集成测试，放在 src/itest

## 架构
- api/ 放 REST 控制器，core/ 放领域逻辑，adapters/ 与外部系统通信
- Kafka 事件定义在 schemas/ 下，不要编辑生成的类

## Claude 容易犯的错
- 不要升级依赖版本，这由平台团队管
- 遗留的 v1/ 包已冻结，改动放到 v2/
```

### Skills 作为组织知识

版本控制的、组织级别的策略。一个安全 API 审查 Skill 的示例：

```yaml
代码解读复制代码---
name: secure-api-review
description: 应用 API 安全标准。在创建或修改面向外部的接口、
  审查 API 代码或生成 OpenAPI 规格时使用
---
# 安全 API 审查

当你创建或修改 API 接口时：
1. 认证：每个接口都需要网关 JWT，
   /health 之外不允许匿名路由
2. 输入验证：根据 OpenAPI schema 验证请求体，
   拒绝未知字段
3. 审计：每个状态变更接口都必须发出审计事件，
   包含操作者、操作、实体、时间戳
4. 数据分类：schema 中标记为 pii 的字段
   不得出现在日志或错误消息中

运行 scripts/check-endpoints.sh 并在摘要中包含其输出
```

### Hooks 作为构建时护栏

确定性的控制机制，可以阻止对受保护路径的编辑、在修改后运行格式化器和 linter，或者防止凭据出现在 diff 中。

### 并行会话与子 Agent

一个工程师可以用独立的 git worktree 同时驱动多个独立的工作流。子 Agent 处理有明确范围的重复性任务，每个子 Agent 有自己的上下文窗口。

**治理：** 设计评审发生在代码生成之前。Plan 模式强制执行这一点，因为工程师接受计划之前 Claude 无法修改文件。

**度量指标：**

- **先导指标：** 首次通过就合并的变更比例；从计划批准到 PR 合并的时间
- **滞后指标：** 每个变更的返工次数；合并后的 diff 与提交的 `plan.md` 匹配程度

## 阶段四：测试，每次会话都要自检工作

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/3d9c92e7e2d14905b6dbb71e6df164b1~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1788413376&x-signature=Ds9XGnYyBjm0taAxTTjZq5702aU%3D)

**传统做法：** 代码能用的信号来得很晚（CI、测试、生产环境），Agent 生成的代码必须由人完整审查。

**AI Native做法：** 会话有办法在人类看到之前自检工作。Claude 反复迭代直到检查通过。

**流程：**

1. 把工作检查封装成一条命令： `make test` 或 `npm test`
2. 在 `CLAUDE.md` 的命令部分列出，并给出健康输出的示例
3. 设定可量化的目标：「所有测试通过」「截图与设计稿一致」「接口用新字段返回 200」
4. 修 bug 时：先写失败测试，让 Claude 把 bug 复现为测试，确认测试失败，提交测试，然后才让 Claude 修复（且不能修改测试）
5. UI 相关的工作：给 Claude 浏览器/截图工具和设计稿，反复迭代（通常 2-3 轮）
6. 把验证变成 `CLAUDE.md` 中「完成」指令的一部分
7. 用 Hook 阻止 Agent 在修复任务中编辑测试文件

**CLAUDE.md 验证配置示例：**

```
代码解读复制代码## 验证工作

- 构建：make build（必须以 "Build succeeded" 结束）
- 测试：make test（全部通过，不得跳过或删除失败的测试）
- Lint：make lint（零警告）

在报告任何任务完成之前运行以上全部三项，并粘贴输出。
如果测试失败，修代码，不修测试。
```

### CI 中的持续 Evals

Evals 是 AI Native世界里的阶段门禁 QA。在 Agent 配置变更（模型切换、prompt 改写、Skill 更新）时运行。

**流程：**

1. 平台工程师收集 20-50 个带有预期结果的真实任务
2. 将每个写成 eval：prompt + 定义可接受结果的检查项（测试通过、lint 干净、行为不变、策略遵循）
3. 套件按计划运行，也在 `CLAUDE.md` 、Skills 或 Hooks 变更时运行
4. 配置变更需要通过 eval 门禁
5. 每个生产事故都变成一条 eval，成为永久的回归测试

**CI 工作流示例：**

```bash
代码解读复制代码name: Agent evals
on:
  pull_request:
    paths: ['CLAUDE.md', '.claude/**']
  schedule:
    - cron: '0 2 * * *'
jobs:
  evals:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install -g @anthropic-ai/claude-code
      - name: Run eval suite
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          for eval in evals/*.json; do
            claude -p "$(jq -r '.prompt' $eval)" \
              --allowedTools "Read,Edit,Bash(make test)" \
              --output-format json > result.json
            ./evals/check.sh "$eval" result.json
          done
```

**治理：** Evals 提供了跟得上 Agent 产出速度的 QA 门禁。通过率阈值作为合并检查强制执行；运行结果记录以便随时间比较。

**度量指标：**

- **先导指标：** Eval 通过率随时间的变化；从生产事故到永久 eval 的时间
- **滞后指标：** CI 中捕获的回归 vs. 逃逸到生产环境的回归

## 阶段五：部署，评审双向运行

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/175df84c28bf4e5d82cec39b0ed1c5e0~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1788413376&x-signature=S7bmkBFV%2BubqyZwgST%2BM9tQ6fxQ%3D)

### AI 在 PR 评审循环中的角色

Claude 既给出评审，也接受评审。它按策略审查传入的 PR，同时也处理自己 PR 上收到的评审意见。

**传统做法：** 评审能力按人类产出的速度规划。PR 等待审查人；质量随负载波动。

**AI Native做法：** 所有 PR 都获得一致的、按严重程度排序的评审。人类的注意力转向行为和风险。

**流程：**

1. 启用托管的 Code Review 服务或在 CI 中运行 claude-code-action
2. 技术负责人把评审策略写成 `REVIEW.md` ：
```shell
代码解读复制代码# 评审指令

## 检查轮次
运行三轮检查，每轮标注类型：
- Bug：逻辑错误、边界条件遗漏、隐蔽的回归
- 安全：注入风险、认证漏洞、日志中的 PII
- 合规：变更是否符合 spec.md、plan.md、设计原则

## 什么是「重要」
「重要」仅用于会破坏行为、泄露数据或违反策略的发现。
风格和命名属于小建议。

## 控制小建议数量
每次评审最多报告五个小建议，其余汇总为数量。

## 不需要报告的
src/gen/ 下的生成文件，以及 CI 已经在检查的内容。
```

3\. 技术负责人设定人工阈值。评审发现不会自动批准或阻止合并 4. 在评审评论上 @claude，Claude 处理并推送修复 5. 评审发现反馈到 `CLAUDE.md` 6. 每月调优：评估发现的质量、控制小建议数量、排除生成路径

**治理：** 写代码的 Agent 无法批准自己的代码。评审策略统一应用于所有 PR。发现记录在 PR 历史中，形成审计记录。人类通过分支保护规则进行审批。

**度量指标：**

- **先导指标：** 首次评审响应时间；无需人类碰分支就解决的评论比例
- **滞后指标：** 合并前捕获的缺陷/漏洞 vs. 逃逸到生产的

### Hooks 作为审批门禁

Hooks 可以阻止、放行或暂停（等待审批）。发布门禁是最典型的场景。Hooks 还能阻止在没有变更工单的情况下编辑受保护路径，或者阻止 Agent 在修复任务中编辑测试文件。

**生产环境门禁 Hook 示例（.claude/hooks/production-gate.sh）：**

```bash
代码解读复制代码#!/bin/bash
# 生产部署需要具名的发布授权
cmd=$(jq -r '.tool_input.command' < /dev/stdin)
if [[ "$cmd" == *"deploy"* && "$cmd" == *"production"* ]]; then
   if [ -z "$RELEASE_APPROVAL" ]; then
     echo "Production deploys need a release authorization." >&2
     exit 2 # exit 2 阻止操作，消息发送给 Claude
   fi
fi
exit 0
```

### CI/CD 集成与部署

在流水线中以非交互模式运行 Claude，沙箱化执行，通过 MCP 集成暴露部署能力，预演回滚路径。

**流程：**

1. 从只读的判断步骤开始（分诊失败的构建、汇总不稳定的测试、起草变更日志）
2. 在现有门禁之后添加写入步骤（修复 lint、更新生成的文档、处理评审意见）
3. 执行沙箱化在容器中，使用短期限定作用域的令牌，没有长期的生产凭据
4. 通过 MCP 暴露部署能力，按环境限定作用域
5. 按环境分级自治权：开发环境（Agent 自由部署）、预发布（中间地带）、生产（Agent 准备，管理者授权，Hook 强制门禁）
6. 定期演练回滚，阶段六中控制带突破时会用到它

**流水线步骤示例：**

```arduino
代码解读复制代码- name: Triage failed build
 if: failure()
 run: >
   claude -p "Read the build log at out/build.log. Identify most
   likely cause, say whether failure looks flaky or real, and write
   three-line summary for PR thread." >> triage.md
```

**治理：** Agent 可以执行到生产门禁为止，但无法通过它。分支保护把 Agent 的写入变成 PR，没有直通 main 的路径。生产部署 Hook 在具名发布管理者授权之前阻止操作。按环境的权限层级设定 Agent 可以做多少事。

**度量指标：**

- **先导指标：** 不需要呼叫人类就完成分诊的流水线失败比例
- **滞后指标：** DORA 指标（CI/部署工具已经在产出这些数据）

## 阶段六：维护，循环闭合

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/f72c713815c344549fcae184ca48a0ea~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1788413376&x-signature=Z9XqR90y3ghsh4pxgeTFrL%2F7U1M%3D)

**传统做法：** 维护是被动的。工单和事故等着人来处理，然后重启流程。

**AI Native做法：** 触发器（控制带突破、工单、频道消息、定时任务）直接调用 Claude，无需人介入。Claude 诊断问题，通过有门禁的路径执行操作，并把结果写成 `intent.md` ，进入上述各阶段的循环。

### 监控与闭环

确定性脚本监控生产环境，在控制带被突破时调用 Claude。

**流程：**

1. 服务负责人选择一个有稳定基线的指标（CI 测试失败率、部署后 5xx 率、PR 周期时间）
2. 编写检测脚本，使用滚动窗口的均值/标准差。采用规则（Western Electric 规则）来捕捉缓慢漂移和突增
3. 在版本控制的 `bands.yaml` 中定义响应层级：
4. 1σ：仅记录
5. 2σ：调用 Claude 进行只读诊断
6. 3σ：Claude 可以采取行动（提 PR、触发预批准的运维手册）
7. 触发层：定时工作流、来自监控栈的 webhook 或 Cron Job。Claude 以无状态方式运行（非交互的 CI 步骤或沙箱化容器中的 Agent SDK 服务）
8. Agent 将诊断写成 `intent.md` （异常与证据、建议的结果、受影响的系统、待确认问题）
9. 服务负责人或值班工程师分诊队列，路由给产品负责人或直接关闭
10. 修复上线后，为该事故添加 eval，防止回归

**bands.yaml 示例：**

```
代码解读复制代码metric: ci_test_failure_rate
baseline: rolling_30d
rules: western_electric
tiers:
  1sigma: { action: log }
  2sigma: { action: diagnose,
            tools: "Read,Grep,Bash(gh run view *)" }
  3sigma: { action: propose,
            routes: [pull_request, runbook:rollback-deploy] }
```

### Claude Tag 处理事故

Claude Tag（Slack 公开测试版）让 Claude 作为频道成员以自己的身份参与。每个事故都有了第一响应者，响应本身成为循环和记忆的一部分。

![图片](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/b6e2dc27f536455d8e3742e8956459fe~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5ZGo5pyr56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1788413376&x-signature=AC9DRnWNSC6ST99RJw4jGsdK4ZA%3D)

对话和组织知识留在频道中，任何团队成员都可以实时指导和执行响应。Claude 通过 MCP 访问验证指标是否回到基线，并在线程中确认，然后把事后复盘写入版本控制的经验教训文件。

不只是事故处理。小型的、边界清晰的修复以 PR 的形式通过评审门禁；更大的工作则写成 `intent.md` 进入阶段一。

**治理：** 层级边界从版本控制的配置中强制执行。权限和托管设置拒绝生产环境访问。调用、发现、分诊都带时间戳记录。服务负责人分诊和审批。运维手册预先批准。现有的 PR 评审门禁决定变更是否通过。

**度量指标：**

- **先导指标：** 从控制带突破到 `intent.md` 进入分诊队列的时间（对比过去的事故到事后复盘的时间）
- **滞后指标：** 转化为合并修复的发现比例；同类事故的重复率

## 关键产物与版本控制链

贯穿 AI Native SDLC 的主线是提交的产物。每个阶段结束时写入一个到版本控制中，下一个阶段从读取它开始：

- **intent.md** → 需求和设计环节读取它
- **spec.md** → Plan 模式读取它
- **plan.md** → 实现环节读取它
- **diff 和测试** → 评审环节读取它
- **带评审发现的 PR** → 部署环节
- **事故记录** → 写回新的 intent.md，循环闭合

人类对每一个需要判断力的决策负责。提交链就是审计记录：谁提了什么需求，Agent 产出了什么，谁批准了它。

## 遗留系统集成

对于流程产出的每一个产物，要指定一个系统作为唯一真实数据源（source of truth）：

**方案一：仓库作为唯一数据源**

Markdown 产物是权威记录，遗留系统引用 commit 中的文件。对工程主导的组织来说最干净：所有记录在一个工具中，有统一的时间戳权威。

**方案二：遗留系统作为唯一数据源**

Jira、ServiceNow 或需求工具持有权威记录，Markdown 产物是工作副本。Claude 在会话开始时读取记录，通过 MCP 连接器将结果写回。

**方案三：链接作为最低标准**

所有产物标注记录 ID，所有遗留记录包含 Markdown 文件的 commit SHA。作为过渡方案的良好起点，接受两个数据源并存。

## 控制与治理框架

通过 Managed Settings（托管配置）来设定组织级别的控制，工程师无法编辑或覆盖：

```json
代码解读复制代码{
    "permissions": {
      "deny": ["Read(.env*)", "Read(./secrets/**)", "WebFetch",
               "Bash(curl *)", "Bash(wget *)"],
      "allow": ["Bash(git *)", "Bash(make build)", "Bash(make test)",
                "Bash(make lint)"],
      "disableBypassPermissionsMode": "disable"
    },
    "allowManagedPermissionRulesOnly": true,
    "sandbox": {
      "enabled": true,
      "failIfUnavailable": true,
      "allowUnsandboxedCommands": false,
      "network": {
        "allowedDomains": ["git.internal.example.com",
                          "registry.npmjs.org"]
      },
      "credentials": {
        "files": [
          {"path": "~/.ssh", "mode": "deny"},
          {"path": "~/.aws/credentials", "mode": "deny"}
        ],
        "envVars": [{"name": "GITHUB_TOKEN", "mode": "deny"}]
      }
    },
    "allowManagedHooksOnly": true,
    "disableSideloadFlags": true,
    "allowManagedMcpServersOnly": true,
    "strictKnownMarketplaces": [
      {"source": "github", "repo": "example-corp/approved-plugins"}
    ],
    "requiredMinimumVersion": "2.1.193"
}
```

**每个配置项的作用：**

- `permissions.deny` 将密钥排除在上下文之外，阻止任意网络出口
- `permissions.allow` 预批准安全的内循环操作
- `disableBypassPermissionsMode` + `allowManagedPermissionRulesOnly` 阻止工程师自行放宽规则
- `sandbox` 通过操作系统级别的隔离弥补权限无法覆盖的空白
- `failIfUnavailable` 把沙箱变成门禁，如果沙箱无法初始化，Claude 会拒绝启动
- `credentials` 拒绝文件读取并从 shell 命令环境中剥离密钥
- `allowManagedHooksOnly` 确保只有受管理的 Hooks 才能运行
- `disableSideloadFlags` 确保 Skills、Agents 和 Hooks 只能通过审批的市场安装
- `allowManagedMcpServersOnly` 把工具表面变成白名单
- `requiredMinimumVersion` 强制使用经过评估的最低版本

## 实施顺序

这六个实践是模块化的，组织可以根据需要优先改造不同的阶段。依赖关系图中的箭头指示了前置条件：

- **阶段一：规划** 没有前置要求，可以从任何地方开始
- 其他阶段依赖于依赖图中箭头指示的前置阶段

人类的注意力集中在门禁上，审查的是 Agent 标记出来的内容，而不是从头开始每个阶段。

## 结语

模型和工具已经进化到了一个新的水平，组织不仅可以改变代码的生产方式，还可以改变整个软件开发生命周期。这种转变把人类的判断力保持在流程的核心位置，同时兼顾了大型企业组织的治理和合规要求。

**循环持续运转。**

**人类的判断力应始终在它之上。**

---

原文： [claude.com/blog/the-ai…](https://link.juejin.cn/?target=https%3A%2F%2Fclaude.com%2Fblog%2Fthe-ai-native-sdlc-playbook "https://claude.com/blog/the-ai-native-sdlc-playbook")