---
title: "OfficeCLI ：AI Agent 时代的命令行 Office 套件"
source: "https://zhuanlan.zhihu.com/p/2059427039145469807"
author:
  - "[[勇哥AI笔记]]"
published:
created: 2026-07-16
description: "单独做 PPT 的技能 Skill 有不少，但是我们日常还会经常遇到要处理 Office 三件套：Word、Excel、PPT 的场景。 对于创建 Word 和 Excel 我一般用 WordBuddy 连通腾讯文档，让它把输出的 Markdown 写到腾讯文档。 …"
tags:
  - "clippings"
status: processed
---
4 人赞同了该文章

单独做 PPT 的技能 Skill 有不少，但是我们日常还会经常遇到要处理 Office 三件套：Word、Excel、PPT 的场景。

对于创建 Word 和 Excel 我一般用 WordBuddy 连通腾讯文档，让它把输出的 Markdown 写到腾讯文档。

但是读取的场景，就需要让 AI 写代码去获取里面的数据了。

现在从AI工具到AI优先：未来最重要的用户可能不是人，这种趋势下，很多开发和运维软件都有了CLI [命令行](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E5%91%BD%E4%BB%A4%E8%A1%8C&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLlkb3ku6TooYwiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.dvDpUpy2uAIsFW-YY4nao9i5biMVqmO0LPzvRIHjVuA&zhida_source=entity) 化工具，就是为了让 AI 能够操作。

**今天介绍的 [OfficeCLI](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=OfficeCLI&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiJPZmZpY2VDTEkiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.SePMu2EIJjdNGkcIp6VukdbC01AU2XjMr2lK2Jr7pqE&zhida_source=entity)** 就是 [iOfficeAI](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=iOfficeAI&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiJpT2ZmaWNlQUkiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.5-uf9ejXPXc1w-PwdsMZRa1jkp2WjdC4u3Pwvs0Jj_U&zhida_source=entity) 团队推出的开源命令行 Office 套件，让任何 AI Agent 通过一行命令完全掌控 Word、Excel、PowerPoint 的读取、创建、编辑与 [自动化](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E8%87%AA%E5%8A%A8%E5%8C%96&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLoh6rliqjljJYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.quDZYnjNKXuKk_PhUbEzR6lLjOUaXvwfDdlgwoqhHNo&zhida_source=entity) 。

![动图封面](https://pic3.zhimg.com/v2-b6d55f922f41d66231e88475fb464ac0_b.jpg)

内置 HTML 渲染引擎形成「渲染→查看→修复」闭环：Agent 不再盲目写 XML，而是真正「看见」文档长什么样。

内置 [MCP Server](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=MCP+Server&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiJNQ1AgU2VydmVyIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6Mjc4ODI4MTM5LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.0-rDbZYblqn3Q5iq3b7Jrn15_mG1xFaUWg21uO9jovQ&zhida_source=entity) 可一键注册到 [Claude Code](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=Claude+Code&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiJDbGF1ZGUgQ29kZSIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3ODgyODEzOSwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.Ofcuw1BdBFa0pivKgTQsWveIRVbaKursh-X5E9SZ7X0&zhida_source=entity) / [Cursor](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=Cursor&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiJDdXJzb3IiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.IFWE9itP2N9u-PFjpRbrcXQH-ldEe_5Ib5ZvPd63Qbo&zhida_source=entity) / [VS Code Copilot](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=VS+Code+Copilot&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiJWUyBDb2RlIENvcGlsb3QiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.50lVfeWo9sKRPC7_yJ1DvEZc4pQTW3IGQGEbtCG9yh4&zhida_source=entity) / [LM Studio](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=LM+Studio&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiJMTSBTdHVkaW8iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.v79IoJemj5CRKyPhmpFL5AHjrWKCEm7RXMSF6cCoPcI&zhida_source=entity) ，安装后自动检测已知 AI 工具目录并写入 SKILL.md 技能文件，Agent 读取即自主掌握全部命令。

---

## 二、关键特性

### 三层渐进式架构

OfficeCLI 把文档操作按 [抽象层次](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E6%8A%BD%E8%B1%A1%E5%B1%82%E6%AC%A1&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLmir3osaHlsYLmrKEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.ITW5tasyhjx47InMxwbb4Y_-BdpBm9rEEkophkRWGyc&zhida_source=entity) 分为三级，Agent 根据需求选取最合适的级别。

L1 读取层提供 [语义化](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E8%AF%AD%E4%B9%89%E5%8C%96&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLor63kuYnljJYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.9bVzI_mRW6nt9ZT1nJGnS3ruyqw-DBwLr5jZSlZgOj0&zhida_source=entity) 视图，命令 `view` 支持大纲、文本、标注、统计、问题诊断和 HTML 预览等多种输出模式。

L2 DOM 层是结构化元素操作的核心，命令从 `get` （获取元素及子元素，支持 `--depth` 与 `--json` 结构化输出）、 `query` （CSS 风格查询，如 `[attr=value]` 、`:contains()` 、`:has()` ）、 `set` （修改属性）、 `add` （新增元素或 `--from` 克隆）到 `remove` （删除）、 `move` （支持 `--to` / `--index` / `--after` / `--before` 多种定位）、 `swap` （交换两个元素）一应俱全。

L3 原始 XML 层通过 `raw` （查看）与 `raw-set` （使用 XPath 修改）直接访问底层 [OOXML](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=OOXML&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiJPT1hNTCIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3ODgyODEzOSwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.ks92Rlccss25hymM2deNvvqNFrno45cUVbGk0AGj38M&zhida_source=entity) 数据，作为任何上层 API 覆盖不到的万能降级方案。

### 内置引擎

**渲染引擎** 从零实现 [高保真](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E9%AB%98%E4%BF%9D%E7%9C%9F&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLpq5jkv53nnJ8iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.ARCFcPUtkRm0f8q05DxaWYLBwF9aSLAihik3Na0hPHw&zhida_source=entity) HTML 渲染，覆盖形状、图表（ [趋势线](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E8%B6%8B%E5%8A%BF%E7%BA%BF&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLotovlir_nur8iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.CozyLJIveMjEWNNgdbuDkCwz5dLAFjpi1qHZ1VDDOig&zhida_source=entity) 、误差线、 [瀑布图](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E7%80%91%E5%B8%83%E5%9B%BE&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLngJHluIPlm74iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.lCNu6KY37blYojhSqESv-rJH9PXsFHbDESY19NGOvtc&zhida_source=entity) 、蜡烛图、迷你图等）、OMML 公式到 LaTeX/KaTeX 的自动转换、3D`.glb` 模型、变形转场和形状特效。

三种输出模式分工明确：

`view html` 生成资源内联的独立 HTML 文件，可在任意浏览器打开；

`view screenshot` 产生每页 PNG 截图供 [多模态](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E5%A4%9A%E6%A8%A1%E6%80%81&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLlpJrmqKHmgIEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.5ZNs3KatVeCcywvpebgOgsHn5RiAMX4AEaqckZoZzlc&zhida_source=entity) Agent 直接读取； `watch` 启动本地 HTTP 服务（默认 `http://localhost:26315` ），每次 `add` / `set` / `remove` 操作后浏览器自动刷新。

让 Office CLI 好用的闭环：生成、渲染、检查、重复，每一步完全无头运行，AI 不再盲目写 XML 而是真正知道幻灯片长什么样再交还给你」。

**公式与透视引擎** 内置 350+ Excel 函数（写入时自动计算、命令返回时结果已就绪）和原生 OOXML 透视表（支持多字段、日期分组、 `showDataAs` 、排序、calculated fields 等）。

**模板合并** 通过 `merge` 命令替换 `{{key}}` 占位符，覆盖段落、 [单元格](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E5%8D%95%E5%85%83%E6%A0%BC&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLljZXlhYPmoLwiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.DO2nEXV_UvpEvp6rhwf4dTpX24Y9yquyVr_6JDGnkB4&zhida_source=entity) 、形状、页眉页脚及图表标题，配合 JSON 数据可批量生成同一模板的多份文档。

**往返转储** 支持 `dump` （ [序列化](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E5%BA%8F%E5%88%97%E5%8C%96&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLluo_liJfljJYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.RmHcHexlUk4mI1P-xLmzVisX7fs7oqo9001QWMyUUGc&zhida_source=entity) 文档或子树为可重放 batch JSON）和 `batch` （重放），实现单次打开/保存周期内的多操作批处理。

### Agent 友好设计

确定性 JSON 输出（ `--json` ）使 Agent 解析零歧义。路径寻址 `/slide[1]/shape[2]` 无需理解 OOXML [命名空间](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLlkb3lkI3nqbrpl7QiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.roNyW_iDAJN_r5ajHPqu0FhSMwt1p0pFBKByzItrBKY&zhida_source=entity) 。

自我修复工作流通过 `validate` （OpenXML 模式校验）+ `view issues` （结构化错误码与修正建议）让 Agent 自动重试直到通过。

驻留模式（ `open` / `close` ）把文档保持在内存中，连续操作近零延迟。

批量模式（ `batch` ）在单次打开/保存周期内执行多条操作，高效稳定。

全命令行帮助 `officecli help <格式> <元素>` 提供属性、别名、示例与机器可读 [schema](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=schema&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiJzY2hlbWEiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.j8W2H8sf50fY7EmF2vI9FlbyIzn4sWujinNwwNjMBw8&zhida_source=entity) ，Agent 可自主学习全部能力。

### 支持的格式

完整支持 Word（`.docx` ）、Excel（`.xlsx` ）、PowerPoint（`.pptx` ）三种格式的读、改、建全操作。

通过插件可扩展至`.doc` 、`.hwpx` 和 PDF 导出。

图片格式支持 PNG / JPG / GIF / SVG，Excel 支持 CSV / TSV 导入。

---

## 三、快速开始

### 安装（三种方式）

**方式 1：一键脚本（推荐）**

```
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/iOfficeAI/OfficeCLI/main/install.sh | bash
# Windows (PowerShell, 管理员)
irm https://raw.githubusercontent.com/iOfficeAI/OfficeCLI/main/install.ps1 | iex
```

**方式 2： [包管理器](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E5%8C%85%E7%AE%A1%E7%90%86%E5%99%A8&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLljIXnrqHnkIblmagiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.EKKj2uLLYMeGLeK4455ACcly7RmLRLfPwrjOQYBaHJk&zhida_source=entity)**

```
brew install officecli     # macOS / Linux
scoop install officecli    # Windows
npm install -g @officecli/officecli # 全平台（前端开发者）
```

**方式 3：手动下载**  
从 GitHub Releases 按平台下载 [二进制](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E4%BA%8C%E8%BF%9B%E5%88%B6&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLkuozov5vliLYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.PlfP4bmy8tsH0BKJqelLsdKXfczJgVqiwrsclpCWg0s&zhida_source=entity) （如 `officecli-mac-arm64` ），加入 PATH。

**验证**

```
officecli --version
```

**给 AI Agent 一行上手**

```
curl -fsSL https://officecli.ai/SKILL.md
```

Agent 读取技能文件并自动完成安装与配置。

Claude Code 用户可以将技能文件保存到本地： `curl -fsSL https://officecli.ai/SKILL.md -o ~/.claude/skills/officecli.md` 。

**如果是你自己用，** 推荐图形界面安装 AionUi 桌面应用，用自然语言描述即可生成文档；

如果是给 Agent 用，则用命令行 CLI 方式，下载 Release 后运行 `officecli install` 。

### 快速开始

生成一份 PPT 的步骤，只需要三条命令：

```
officecli create deck.pptx
officecli watch deck.pptx                # 浏览器打开 http://localhost:26315
officecli add deck.pptx / --typeslide --prop title="Hello, World!"
```

每一条命令执行后浏览器即时刷新，Agent 可以「看见」每一步的产物。

![](https://pic3.zhimg.com/v2-e80f1e77a4e9d3f571e477dab12db908_1440w.jpg)

需要检查最终效果时，直接 `view screenshot` 渲染成图片——整个过程没有打开过任何 Office 窗口。

---

## 四、使用示例

### 1\. PPT 创建与编辑

```
officecli create deck.pptx
officecli add deck.pptx / --typeslide --prop title="Q4 Report"--prop background=1A1A2E
officecli add deck.pptx&#x27;/slide[1]&#x27;--typeshape \
 --prop text="Revenue grew 25%"--prop x=2cm --prop y=5cm \
 --prop font=Arial --prop size=24 --prop color=FFFFFF

# 大纲视图快速浏览结构
officecli view deck.pptx outline

# HTML 渲染预览（独立文件，浏览器直接打开，无需服务器）
officecli view deck.pptx html

# 结构化 JSON 供 Agent 解析
officecli get deck.pptx&#x27;/slide[1]/shape[1]&#x27;--json
```

三条命令在零点几秒内生成一个真正能打开的 PPT，渲染成图片让 Agent 直接「看」到结果。

然后进入「生成→渲染→检查→重复」的闭环，直到幻灯片完全符合预期。

### 2\. Word 修改与驻留模式

```
officecli open report.docx               # 加载到内存
officeclisetreport.docx /body/p[1]/r[1] --prop bold=true
officecli close report.docx               # 保存并释放
```

### 3\. Excel 透视表

```
officecli add sales.xlsx&#x27;/Sheet1&#x27;--typepivottable \
 --propsource=&#x27;Data!A1:E10000&#x27;--prop rows=&#x27;Region,Category&#x27;\
 --prop cols=Quarter --prop values=&#x27;Revenue:sum,Units:avg&#x27;\
 --prop showDataAs=percentOfTotal
```

### 4\. 模板合并

```
officecli merge invoice-template.docx out-001.docx&#x27;{"client":"Acme","total":"$5,200"}&#x27;
officecli merge q4-template.pptx q4-acme.pptx data.json
```

### 5\. 批量模式（JSON 一次执行）

```
echo&#x27;[{"command":"set","path":"/slide[1]/shape[1]","props":{"text":"Hello"}},
   {"command":"set","path":"/slide[1]/shape[2]","props":{"fill":"FF0000"}}]&#x27;\
 | officecli batch deck.pptx --json
```

### 6\. 端到端自愈工作流

```
officecli create report.pptx
officecli add report.pptx / --typeslide --prop title="Q4 Results"
officecli add report.pptx&#x27;/slide[1]&#x27;--typeshape --prop text="Revenue:$4.2M"--prop x=2cm --prop y=5cm --prop size=28
officecli validate report.pptx             # 校验 OpenXML 模式
officecli view report.pptx issues --json        # 返回结构化错误码，Agent 自动重试
```

### 7\. MCP 注册

```
officecli mcp claude
officecli mcp cursor
officecli mcp vscode
```

---

## 五、使用场景

可以从数据库或 API 自动化生成报告，批量处理文档（查找替换、样式更新），

在 CI/CD 流水线中构建 [文档自动化](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E6%96%87%E6%A1%A3%E8%87%AA%E5%8A%A8%E5%8C%96&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLmlofmoaPoh6rliqjljJYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.AvAJPZLlAW4pFUk3bW6z0vxQVSuvENyWALrEci7LHog&zhida_source=entity) （如从测试结果生成文档），

或在 Docker 及容器化环境中实现无头 Office 自动化。

可以根据用户提示自动生成含图表、透视表的演示文稿或报表，

从文档中抽取 [结构化数据](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E7%BB%93%E6%9E%84%E5%8C%96%E6%95%B0%E6%8D%AE&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLnu5PmnoTljJbmlbDmja4iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.U9uWI19hXHeVq7toYYv0BU_hoVpN5HiGbb9g2mpMUOw&zhida_source=entity) 到 JSON，并在交付前通过 `validate` + `view issues` 进行质量检查。

任何能运行 shell 命令的编程助手（Claude Code、Cursor、GitHub Copilot 等），现在都能在任何 [操作系统](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLmk43kvZzns7vnu58iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.o-8uUUpu0w0htY_r6l5BHBEXO-JMQsqq6vk6k6OFwpw&zhida_source=entity) 上、不额外安装任何东西的情况下产出一份可直接交付的文档。

**如果你是团队工作，** 可以克隆文档模板批量填入数据（合同、周报、汇报文件）、在 CI/CD 中自动化文档校验。

典型落地包括 Claude / Cursor 智能文档助手自动生成周报与合同、企业批量报表自动化、后端服务嵌入 Office 自动化（Linux 服务器、GitHub Actions、 [云函数](https://zhida.zhihu.com/search?content_id=278828139&content_type=Article&match_order=1&q=%E4%BA%91%E5%87%BD%E6%95%B0&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODQzNTk2MDIsInEiOiLkupHlh73mlbAiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg4MjgxMzksImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.eibOy3qh284SBJwwF22HHOD1gWUuYa0jLAV6GvtR9Wc&zhida_source=entity) 等无 GUI 环境均可部署）。

不适合的场景：极度复杂的旧版`.doc` 格式处理、重度依赖 VBA 宏的场景。

下面是用它完成的PPT、Word、Execel 演示。

PPT：

![动图封面](https://picx.zhimg.com/v2-442a0c6e0a8165672809301c81584d61_b.jpg)

![动图封面](https://pic1.zhimg.com/v2-cad3863d77f8d175e5890c7a0de8e830_b.jpg)

![动图封面](https://pic1.zhimg.com/v2-c2bd656d2a01822db1d6ca09cbf435f6_b.jpg)

![动图封面](https://pic4.zhimg.com/v2-e3bf2eeb6db549122cf8d100298efc8b_b.jpg)

![动图封面](https://pica.zhimg.com/v2-3bf07711b87502ade2bad93940f6e93e_b.jpg)

![动图封面](https://pic4.zhimg.com/v2-c8ea20e944690d0b319c44d58691cfe3_b.jpg)

Word：

![动图封面](https://pic1.zhimg.com/v2-03aa4442ba1282255845c5fddb662ffa_b.jpg)

![动图封面](https://pic2.zhimg.com/v2-f899c174b3655c4f45fbfb9fc94ca6c3_b.jpg)

![动图封面](https://pic2.zhimg.com/v2-8186605212a1946fd3cb174c163a88b3_b.jpg)

Excel：

![动图封面](https://pic2.zhimg.com/v2-413fd4affe929823bf582e0269d306eb_b.jpg)

你有处理 Office 的需求吗？

欢迎使用这个神器后，来评论区留言。

发布于 2026-07-12 00:01・北京

赞同 4