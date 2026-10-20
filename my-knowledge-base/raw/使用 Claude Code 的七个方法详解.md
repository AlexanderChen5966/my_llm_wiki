---
title: "使用 Claude Code 的七个方法详解"
source: "https://zhuanlan.zhihu.com/p/2059733450484199819"
author:
  - "[[炉石不传说]]"
published:
created: 2026-08-19
description: "一、概述Anthropic官方给出了七种调教Claude Code的方式，通过这七种方式可以更好的约束和自定义你的Claude Code。这七种方式分别是： CLAUDE.md文件、规则（Rules）、技能（Skills）、子智能体（Subagents）、钩…"
tags:
  - "clippings"
status: "processed"
---
[收录于 · AI之旅](https://www.zhihu.com/column/c_1627030882501476352)

## 一、概述

![](https://pic3.zhimg.com/v2-f08683f397f9504f691164822229d1ae_1440w.jpg)

Anthropic官方给出了七种调教 [Claude Code](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=Claude+Code&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiJDbGF1ZGUgQ29kZSIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3ODg2NjE2OSwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.nzIPOIdxrPYHbOYCecW88kgCqrTp6gpy-e7o12mmJWM&zhida_source=entity) 的方式，通过这七种方式可以更好的约束和自定义你的Claude Code。这七种方式分别是： **[CLAUDE.md](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=CLAUDE.md&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiJDTEFVREUubWQiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.XTj8Kt_-OzgjwE1grwoFArOnm6vn4aloEWIe0TTTcXQ&zhida_source=entity) 文件** 、 **规则（Rules）** 、 **技能（ [Skills](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=Skills&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiJTa2lsbHMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.tTaX5USWrD6mNLSupoGxeAwib3wuOm0K3RsravAxKQY&zhida_source=entity) ）** 、 **[子智能体](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E5%AD%90%E6%99%BA%E8%83%BD%E4%BD%93&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLlrZDmmbrog73kvZMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.68OlHPz77BL08w9b1CPB7l3i90bFg3Uvqz6cPZM0eFs&zhida_source=entity) （Subagents）** 、 **钩子（ [Hooks](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=Hooks&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiJIb29rcyIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3ODg2NjE2OSwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.HWzOy-xhE-4jp6_1i5gvdINkJ1yTOJW2uJZ-L31LVfE&zhida_source=entity) ）** 、 **输出风格（Output styles）** ，以及 **追加系统提示词（ [append-system-prompt](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=append-system-prompt&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiJhcHBlbmQtc3lzdGVtLXByb21wdCIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3ODg2NjE2OSwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.FZ8IFkMxikQp_kpTTFRye1fksmh-FLC3wzlsY7BCJlg&zhida_source=entity) ）** 。

![](https://pica.zhimg.com/v2-f2479dca6156d0fbe0bac7cc23cd5166_1440w.jpg)

Claude 的设计目标是按它按照你的工作方式来运行，而在 Claude Code 中，你又可以进一步来定制它的行为。

定制 Claude 行为一共有七种方式：CLAUDE.md 文件、Rules、Skills、Subagents、Hooks、Output styles，以及追加系统提示词（append-system-prompt）。

**每种方式都在控制三件事：**

- 指令会在什么时候进入上下文；
- 它在长会话压缩后是否还能保留；
- 它究竟有多强的指令权重。

下面这张表先快速对比七种方式的关键差异；后文再展开说明，帮助你判断一条指令该放在哪一层。

| 方法 | 加载时机 | 压缩行为 | 上下文成本 | 适用场景 |
| --- | --- | --- | --- | --- |
| CLAUDE.md（根目录） | 会话开始时加载，并在整个会话中持续保留 | 记忆化处理：会话内读取一次并缓存；压缩后清空缓存并重新读取 | 高。无论是否相关，每一行都会消耗 token | 构建命令、目录结构、monorepo 结构、编码规范、团队约定 |
| CLAUDE.md（子目录） | 按需加载；当 Claude 读取该子目录下文件时才加载 | 被压缩掉后，要等再次触达该子目录才会重新加载 | 低。只有处理相关子目录时才占用上下文 | 子目录特有的规范 |
| Rules | 会话开始加载（用户级 rule），或仅在命中文件路径时加载（path-scoped） | 压缩后会重新注入 | 中。除非做路径作用域，否则基本常驻 | 特定约束或规范，例如“所有 API handler 必须用 Zod 校验输入” |
| Skills | 会话开始只加载名称和描述；完整内容在调用 skill 时才加载 | 已调用的 skill 会在共享预算内重新注入；最早调用的会先被丢弃 | 低。正文只在调用时进入上下文，并受共享 token 预算限制 | 程序化流程，例如部署流程、发布检查清单 |
| Subagents | 会话开始加载名称、描述和工具列表；只有通过 Agent tool 调用时才加载正文 | 只有最终消息（摘要 + 元数据）会返回主会话 | 低。未调用前对主上下文几乎零成本，且在独立上下文窗口中运行 | 并行工作或需要隔离的支线任务，例如深度检索、日志分析、依赖审计 |
| Hooks | 在 [生命周期事件](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E4%BA%8B%E4%BB%B6&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLnlJ_lkb3lkajmnJ_kuovku7YiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.4n6cV6AnDLGT9rGEsPxl4N9aC2HbC3MLoflhLGUAzyU&zhida_source=entity) 发生时触发 | 完全绕过压缩 | 低。配置本身不在主上下文里，通常只有少量输出返回 | 确定性自动化，例如跑 linter、完成后发 Slack、阻止某些命令、在 PreCompact 备份聊天记录 |
| Output styles | 会话开始加载，并注入系统提示词 | 永不被压缩 | 高。占用上下文，而且会覆写默认系统提示词 | 重大角色切换，例如从代码助手切成通用助手 |
| 追加系统提示词 | 会话开始加载；通过 CLI flag 传入 | 永不被压缩，且只对当前这次调用生效 | 中。首轮请求后会被缓存 | 语气、回复长度、格式偏好 |

## 二、传递指令的七种方式

### 2.1 CLAUDE.md 文件

CLAUDE.md 是放在项目根目录的 Markdown 文件。它会在会话开始时加载进上下文，并在整个会话期间一直保留。

构建命令、目录布局、monorepo 结构、编码规范以及团队约定，都天然适合放在这里。

CLAUDE.md 有两类，加载方式也不同：

**始终加载：** 第一类是根目录 CLAUDE.md，可以来自 [共享仓库](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E5%85%B1%E4%BA%AB%E4%BB%93%E5%BA%93&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLlhbHkuqvku5PlupMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.QZWbo-bsn-wgAOMpeL7NQgHbO9AJBEQKedwsMvHPV1s&zhida_source=entity) ，也可以是保存在本地、只服务于你个人项目偏好的文件。这些文件都会在会话开始时加载，而且不会在长会话中逐渐丢失。Claude Code 在压缩对话时，会重新读取这些文件。

**按需加载：** 第二类是位于子目录中的 CLAUDE.md。例如 app/api/CLAUDE.md 不会在会话一开始就加载，而是只有在 Claude 读取 app/api 下的文件时才加载。它的压缩行为和 path-scoped rule 一样：一旦被压缩掉，就要等你再次触达这个子目录时才会回来。

当前 [工作目录](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E5%B7%A5%E4%BD%9C%E7%9B%AE%E5%BD%95&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLlt6XkvZznm67lvZUiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.SzFiq3AJlNbP2S751yGUFX9JS9Waz8E8ScruxXc4IDs&zhida_source=entity) 之下的所有子目录 CLAUDE.md，都会在 Claude 读取对应目录文件时按需加载。

![](https://pica.zhimg.com/v2-0efbe5e0b812110bfa448a2971f3f526_1440w.jpg)

图：当 Claude 读取某个目录中的文件时，当前工作目录之下该目录范围内的所有子目录CLAUDE.md都会加载。

在共享仓库里，CLAUDE.md 很容易像所有“没人真正负责的 [配置文件](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLphY3nva7mlofku7YiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Ko5snxsu2Lo-qkbMDr9jYy6FQhKUgcELAPwNAxTsPR0&zhida_source=entity) ”一样不断膨胀：每个团队都往里追加自己的要求，但很少有人清理旧内容。

问题在于，仓库里每位工程师的每个会话都会加载它的每一行——不管这些内容和当前任务是否相关。这会持续消耗 token，也会稀释那些真正重要指令的执行力度。随着文件变大，团队专属约定应该下沉到 path-scoped rules，流程性内容应该下沉到 skills，让它们只在需要时加载。

**Tip：** 尽量把 CLAUDE.md 控制在 200 行以内，明确 owner，并像审代码一样审它的变更。把它当成 [代码库](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E4%BB%A3%E7%A0%81%E5%BA%93&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLku6PnoIHlupMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.olYpRX2tYtAQDC5HRcBRt_M0756t-_CsBxWzoyMZ6MU&zhida_source=entity) 的“全局导览”或者“索引页”更合适：给 Claude 一个整体概览，并指向其他按需读取的说明文件。

在 [monorepo](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=3&q=monorepo&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiJtb25vcmVwbyIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3ODg2NjE2OSwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjozLCJ6ZF90b2tlbiI6bnVsbH0.RHwpjtlA4ltWoJnUWx9DuCBYUwq8B5KCz-_OF3tBS5c&zhida_source=entity) 中，建议每个团队目录都拥有自己的子目录 CLAUDE.md，这样团队只会加载自己的约定。 [开发者](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E5%BC%80%E5%8F%91%E8%80%85&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLlvIDlj5HogIUiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.6dRkLnCXwG-FsPh3Q-ns_MjSPzxksz2DFFvGQHk0vgA&zhida_source=entity) 还可以用 claudeMdExcludes 配置跳过那些自己根本不会接触的团队文件。

如果有一些标准必须对组织里的所有仓库都生效——例如安全策略、合规要求——可以通过 MDM 或 [配置管理](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLphY3nva7nrqHnkIYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.5OXQ8nKrbB2LeLkvzVZUL_tKI5xwrGqmnzVWNCB1Zqk&zhida_source=entity) ，将集中维护的 CLAUDE.md 下发到开发者机器上。这样的文件不能被个人设置排除。

*关于 CLAUDE.md 的更多背景，可参考官方文章： [claude.com/blog/using-c](https://link.zhihu.com/?target=https%3A//claude.com/blog/using-claude-md-files)*

### 2.2 规则 Rules

Rules 是放在.claude/rules/ 下的 Markdown 文件，用来给 Claude 提供更具体的约束或约定。

**不带 [作用域](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=2&q=%E4%BD%9C%E7%94%A8%E5%9F%9F&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLkvZznlKjln58iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MiwiemRfdG9rZW4iOm51bGx9.N__H5dpF20SClorhGbSKwJZ3EdLb_xk182SgWcilDyA&zhida_source=entity) 的 rule** ，行为上和 CLAUDE.md 很像：会在会话一开始就加载，并在压缩后重新注入。这意味着即使当前任务根本不需要它，它也会持续占用 token。

**带路径作用域的 rule** 则可以通过 paths 字段，只在相关文件被触达时才加载。

例如：一个只作用于 src/api/\*\* 的 rule，在“只写文档”的会话里就不会进入上下文。只有当 Claude 读取 src/api/ 中的文件时，它才会加载。

**示例：**

```
---
paths:
  - "src/api/**"
  - "**/*.handler.ts"
---
All API handlers must validate input with Zod before processing.
```

**Tip：** 如果某条约束是“文件相关的硬性约定”，比如“ [migrations](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=migrations&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiJtaWdyYXRpb25zIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6Mjc4ODY2MTY5LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.SMlXUPpEaADBxtrb-QkWXox_F_KN1t2Zy16DNOLHrQg&zhida_source=entity) 只能追加不能改写”，最适合把它写成 **rule** ，并通过 paths frontmatter 指定生效范围。只要这个约束涉及 [横切关注点](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E6%A8%AA%E5%88%87%E5%85%B3%E6%B3%A8%E7%82%B9&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLmqKrliIflhbPms6jngrkiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Lxct_xmoa3oq78h9bBnuvqwseCUIDlZngVY5fEl24O8&zhida_source=entity) ，或者同一种文件分布在代码库的多个角落（但又不是全局适用），就优先选择 path-scoped rule，而不是嵌套式 CLAUDE.md。

### 2.3 技能 Skills

Skills 存在于.claude/skills/ 下，是由指令、脚本和资源组成的目录。Claude 会按需动态加载它们。每个 skill 都有一个 SKILL.md 文件，其中包含名称、描述和正文。

会话开始时，只会加载 skill 的名称和描述；完整正文只有在 Claude 调用这个 skill 时才会加载——调用方式可以是 [斜杠命令](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E6%96%9C%E6%9D%A0%E5%91%BD%E4%BB%A4&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLmlpzmnaDlkb3ku6QiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.CqwPC8laP6t97X6lt2vsFi39LJ3F5PmVZpbysvbASFI&zhida_source=entity) （例如 /code-review），也可以是 Claude 根据任务自动匹配。

Skills 的触发是通过系统提示词完成的。

![](https://pic3.zhimg.com/v2-76bfeadb35648c847c7e2cccfdb7a9a2_1440w.jpg)

图：Skills 通过你的系统提示词触发。

例如，/ [code-review](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=2&q=code-review&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiJjb2RlLXJldmlldyIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3ODg2NjE2OSwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoyLCJ6ZF90b2tlbiI6bnVsbH0.t_DDqTTail1E-cSD0i2qFg2hnXxVNC05224VmKTBy38&zhida_source=entity) 是一个内置 skill。它会审查当前 diff，并输出发现的问题，但不会直接编辑文件。skill 把“审查流程”固化成了一套 playbook，因此你每次调用它时，Claude 都会遵循同样的结构化步骤。

在压缩时，Claude Code 会把“本次会话中已经调用过的 skills”按 [总预算](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E6%80%BB%E9%A2%84%E7%AE%97&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLmgLvpooTnrpciLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.JBp6IqbPzz4Dr5g0JjBlt15Tw7JNWsQwhevjSOoGdaw&zhida_source=entity) 重新注入。如果一个会话里调用了很多 skills，最早调用的那些会先被淘汰。

**Tip：** 一切“流程性”的指令——例如部署流程、发布 checklist、review 流程——都更适合写进 skill，而不是堆进 CLAUDE.md。

*Claude Code 自带一些 skills，你也可以编写自己的 custom skills。完整教程可参考： [claude.com/blog/complet](https://link.zhihu.com/?target=https%3A//claude.com/blog/complete-guide-to-building-skills-for-claude)*

### 2.4 子智能体 Subagents

Subagents 是放在.claude/agents/ 下的 Markdown 文件，用来定义专门处理某类支线任务的隔离助手。每个文件都使用 YAML [frontmatter](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=2&q=frontmatter&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiJmcm9udG1hdHRlciIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3ODg2NjE2OSwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoyLCJ6ZF90b2tlbiI6bnVsbH0.H7BdYM4iYdVHwltqUmqbtZInbLfVD3cKyCplR3OHa44&zhida_source=entity) （如 name、description，以及可选的模型和工具权限字段）开头，正文则会成为这个 subagent 的 system prompt。

Subagents 和 skills 有点像：会话一开始会加载它们的名称、描述和工具列表，但 agent 正文里的大块上下文不会自动进入 [主线程](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E4%B8%BB%E7%BA%BF%E7%A8%8B&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLkuLvnur_nqIsiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.Menl2hm3-BeQ5SQQWIodTnD-iAaKuZ9mCn_gzes5Wgc&zhida_source=entity) 。Claude 只有在通过 Agent tool 显式调用它们时，才会把一个 prompt 传给对应 subagent。

Claude Code 的上下文窗口承载着 Claude 在这个会话里“知道的全部内容”。下面这张交互时间线展示了哪些内容会进入上下文，以及它们会在什么时候加载。 **不仅 subagent 正文中的大段指令不会自动调用，它甚至根本不会进入父会话。**

![](https://pica.zhimg.com/v2-5a89577e496a85118d9209549ae9a8ee_1440w.jpg)

图：Claude Code 的上下文窗口承载着它在当前会话中知道的全部内容。这张交互时间线展示了哪些内容会进入上下文，以及它们会在什么时候加载。

subagent 会在一个全新的独立上下文窗口里运行，而返回主会话的只有它的最终消息——通常是许多子任务汇总后的结果——以及少量 [元数据](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=2&q=%E5%85%83%E6%95%B0%E6%8D%AE&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLlhYPmlbDmja4iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MiwiemRfdG9rZW4iOm51bGx9.JwXSHhJr-UgJ6GFmZjRrP7FXZGRPGwYz_KDu-NLlr2k&zhida_source=entity) 。

这种模式非常易于扩展：subagents 最多可以嵌套 5 层；而 [动态工作流](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E5%8A%A8%E6%80%81%E5%B7%A5%E4%BD%9C%E6%B5%81&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLliqjmgIHlt6XkvZzmtYEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.XU3LzrVIzymoq4OsaEE0t9bKUs9MkkBrjrbcWvLugoQ&zhida_source=entity) （dynamic workflows）甚至可以编排数十到数百个后台 agent，而不要求你手工把每个 subagent 架构细节都写出来。编排计划和中间结果保存在脚本变量中，而不是 Claude 的上下文窗口里，因此可以扩大规模而不牺牲指令精度。

**Tip：** 这种“隔离性”正是你选择 subagent 而不是 skill 的主要原因之一。如果某个支线任务——比如深度搜索、日志分析、依赖审计——会在主对话里产生大量之后不会再看的中间结果，就用 subagent。若你希望流程直接在主线程展开，方便你观察每一步、实时介入和调整，那就用 skill。

### 2.5 钩子 Hooks

Hooks 是用户自定义的命令、HTTP 端点或 LLM prompts。它们会在 Claude 生命周期中的特定事件上触发，从而用更“确定性”的方式控制 Claude 的行为，例如文件编辑、工具调用、会话开始等时刻。

![](https://pica.zhimg.com/v2-dd8b054df294d14b5a35147ec10928d8_1440w.jpg)

图：这张图展示了在 Claude Code 会话中，Hooks 可能触发的各个事件。

你可以在 settings.json、受管策略设置（managed policy settings），或者 skill / agent 的 frontmatter 中注册 hooks。

hooks 有多种类型：command、HTTP、mcp\_tool、prompt 和 agent。所有 hooks 都是被确定性触发的。前 3 种会确定性执行；后 2 种 prompt 和 agent 则仍然依赖 Claude 的判断来生成输出，而不是靠死规则返回固定结果。

Hooks 的上下文成本很低，因为真正的配置或指令并不常驻在主上下文窗口里。运行时由 harness 去执行处理器（command、http、mcp\_tool），或者为 prompt、agent 单独开模型窗口。

有些 hook 的输出会被保存回主上下文。例如，某个 blocking hook 的标准错误输出会进入上下文，这样 Claude 才知道为什么这次调用被拒绝了。

但绝大多数 hook 的输出都不会自动进入主窗口，除非配置里明确要求返回。比如，你如果在 PreCompact 事件中把聊天记录备份到另一个文件里，Claude 自己并不会知道那份备份最终写到了哪个文件。

这也是 hooks 和 CLAUDE.md、rules、skills 在本质上的区别。

*关于 hooks 的更多内容，可参考官方文章： [claude.com/blog/how-to-](https://link.zhihu.com/?target=https%3A//claude.com/blog/how-to-configure-hooks)*

**Tip：** 只要你希望某件事 **一定发生** ，就应该优先考虑 hook：比如编辑后跑 linter、完成任务后发 Slack、在命令执行前拦截特定操作。PreToolUse hook 可以检查任意工具调用，并通过返回码 2 直接拒绝执行。

它之所以上下文成本低，根本原因是：这类 hook 本质上是由 harness 执行的代码，而不是需要被 Claude 加载理解的“自然语言指令”。

### 2.6 输出风格 Output styles

Output styles 是放在.claude/output-styles/ 下的文件，用来把额外指令注入系统提示词。它们永远不会被压缩，会在每次会话开始时加载，并在会话内第一次请求后被缓存，因此它们的上下文成本属于中高水平。

因为 output styles 位于 system prompt 层，它们拥有目前为止所有方法里 **最高的指令权重** ，因此一定要谨慎使用。

**对 output style 的修改，会直接替换默认 output style** （除非你在 frontmatter 中设置 keep-coding-instructions: true）。

在 Claude Code 里，这意味着你可能会移除那些本来让 Claude 以“软件工程助手”身份工作的默认指令，其中包括很多关键行为，例如：

- 如何合理控制改动范围；
- 什么时候该加代码注释，什么时候该省略；
- 如何处理安全相关问题；
- 在宣布任务完成前，应该怎样验证结果，例如先跑测试。

默认情况下，自定义 output style 会把这些内容一并替换掉，于是 Claude Code 会更像一个通用助手，而不再是一个软件工程助手。

**Tip：** 在自己写 output style 之前，先看看内置样式。Proactive、Explanatory 和 Learning 已经覆盖了最常见的需求：更高自主性、教学模式、协作式编码。这样你往往不必自己维护一份 style 文件。

### 2.7 追加系统提示词 append-system-prompt

相比直接修改 output style，另一种更温和的方式是使用 append-system-prompt flag。修改 output style 可能会对 Claude 的行为造成很大、甚至意料之外的影响；而 append flag 只是 **在原始系统提示词上追加内容** 。

它不会改变 Claude 的基础角色，只是在默认角色之上叠加更多说明。

它也是在调用时动态传入的，只对这一次调用生效，不会像文件那样跨会话长期存在。

和其他传递指令的方法相比，追加系统提示词的上下文成本通常更高。它会增加输入 token；虽然 [prompt caching](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=prompt+caching&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiJwcm9tcHQgY2FjaGluZyIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3ODg2NjE2OSwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.fOpqf9p1vsXx-O3ZLgiXFBciEPo8jMavJ1uRlUS_D2Y&zhida_source=entity) 能在会话首轮之后降低一部分成本，但如果你同时要求 Claude 用更长、更啰嗦的风格回复，也会继续抬高输出 token。

**Tip：** 追加系统提示词最适合补充一些具体的编码标准、输出格式要求或领域知识。但也要注意，这种方法对遵循度有“ [边际递减效应](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E8%BE%B9%E9%99%85%E9%80%92%E5%87%8F%E6%95%88%E5%BA%94&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLovrnpmYXpgJLlh4_mlYjlupQiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.XIC9rLEiDVY9DFBjPq0r8IGSweDAXeWIE4srDAVwRkA&zhida_source=entity) ”：通常你附加的指令越多，Claude 对它们的执行反而会越松，尤其当这些指令彼此存在冲突时。

## 三、Claude Code 定制的快速建议

如果你发现自己正在做下面这些事，通常意味着这条指令放错地方了：

- **“每次 X 都必须做 Y”，但写在CLAUDE.md里。**

如果这个行为需要稳定、确定地发生——例如每次编辑后自动跑 prettier，或者完成任务后发 Slack——那应该写成 settings.json 里的 hook，而不是寄希望于模型“记得去做”。模型主动决定要不要调用格式化器，和格式化器被系统自动执行，是两件完全不同的事。

- **“绝对不要做某事”，但写在CLAUDE.md里。**

只要是“绝对不能发生”的事情，纯指令就不是对的工具。Claude 大多数时候会遵守，但在长会话、模糊情境、高压任务，或者文件里存在 prompt injection 时，它仍可能失手。真正的护栏必须是确定性的，应该用 hooks 和 permissions 来做。PreToolUse hook 可以检查调用并通过返回码 2 阻止它；而 managed settings 更进一步：它由管理员下发，用户本地配置无法覆盖，是组织级确定性护栏的唯一可靠方式。

- **把一个 30 行的操作流程写进CLAUDE.md。**

流程应该放进 skill。CLAUDE.md 用来存放那些 Claude 需要始终记住的“事实”，比如构建命令、monorepo 布局、团队约定；而部署 runbook 或安全审查 checklist 这类内容，应该放到.claude/skills/ 里，只在需要时加载正文。

- **写了一条只针对 API 的 rule，却没有做路径作用域。**

如果一条 rule 只适用于 src/api/\*\*，那就应该用 paths: 把它限定在对应范围。否则它和把同样内容直接塞进 CLAUDE.md 没区别：永远加载，永远消耗 token。

- **把个人偏好写进项目级CLAUDE.md。**

所有基于文件的方法，都有对应的用户级版本，会在每次 Claude Code 会话中加载，而不局限于某个仓库。个人偏好（例如“总是使用 [语义化](https://zhida.zhihu.com/search?content_id=278866169&content_type=Article&match_order=1&q=%E8%AF%AD%E4%B9%89%E5%8C%96&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODcyNzk2MTIsInEiOiLor63kuYnljJYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4NjYxNjksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.h0XzbGCa10Ei_8EWASWuQRsOnTb_Y-a0_NOcyklQmNE&zhida_source=entity) commit message”）应该写在本地用户级文件里；项目级文件只保留那些团队共有、且只对当前代码库生效的内容。

*原文参考链接： [claude.com/blog/steerin](https://link.zhihu.com/?target=https%3A//claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more)*

**by 2026-7-12 周日 晚上**

编辑于 2026-07-12 20:21・上海[豆包AI工作任务模式-本地文件智能管理与优化助手](http://www.doubao.com/download/desktop?ug_apk_token=NJ8a2&ad_platform_id=zhihu_feed_lead&ug_callback_url=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3Dcf6a2f3e-cdcc-4da2-854e-da5f03dd4c99%26os%3D3%26zid%3D1629%26zaid%3D3785712%26zcid%3D3805660%26cid%3D3805660%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26score%3D__EVENTSCORE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784532%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1ZY0gnWzUoISkYf1UXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYOsVXhSi6NpS&cb=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3Dcf6a2f3e-cdcc-4da2-854e-da5f03dd4c99%26os%3D3%26zid%3D1629%26zaid%3D3785712%26zcid%3D3805660%26cid%3D3805660%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26score%3D__EVENTSCORE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784532%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1ZY0gnWzUoISkYf1UXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYOsVXhSi6NpS&ug_semver=v1.0.0&spu=biz%3D0%26ci%3D3805660%26si%3D3de63755-3906-4f03-864f-43d3cdff0ea1%26ts%3D1787106816%26zid%3D1629)

[

解锁多元办公技能，支持在本地电脑完成文件分类、文档处理、表格分析与PPT制作等工作，助力简化日常办公流程。

](http://www.doubao.com/download/desktop?ug_apk_token=NJ8a2&ad_platform_id=zhihu_feed_lead&ug_callback_url=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3Dcf6a2f3e-cdcc-4da2-854e-da5f03dd4c99%26os%3D3%26zid%3D1629%26zaid%3D3785712%26zcid%3D3805660%26cid%3D3805660%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26score%3D__EVENTSCORE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784532%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1ZY0gnWzUoISkYf1UXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYOsVXhSi6NpS&cb=https%3A%2F%2Fsugar.zhihu.com%2Fplutus_adreaper_callback%3Fsi%3Dcf6a2f3e-cdcc-4da2-854e-da5f03dd4c99%26os%3D3%26zid%3D1629%26zaid%3D3785712%26zcid%3D3805660%26cid%3D3805660%26event%3D__EVENTTYPE__%26value%3D__EVENTVALUE__%26score%3D__EVENTSCORE__%26ts%3D__TIMESTAMP__%26cts%3D__TS__%26mh%3D__MEMBERHASHID__%26adv%3D784532%26ocg%3D0%26cp%3D0%26ocs%3D0%26aic%3D0%26atp%3D0%26ct%3D0%26ed%3DGiBNJgVzfCMmUW9XFyEvRA8xBGxJICwkOhh0FlwxKw1ZY0gnWzUoISkYf1UXISFcCiIEeh4yNyw9GGJBRCUlVFZ0DngJcmN3ehFkUwBkfwNZY1YkXSo-fX4DPhIMYOsVXhSi6NpS&ug_semver=v1.0.0&spu=biz%3D0%26ci%3D3805660%26si%3D3de63755-3906-4f03-864f-43d3cdff0ea1%26ts%3D1787106816%26zid%3D1629)

赞同