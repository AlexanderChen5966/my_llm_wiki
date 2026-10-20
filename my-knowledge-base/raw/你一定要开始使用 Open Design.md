---
title: "你一定要开始使用 Open Design"
source: "https://zhuanlan.zhihu.com/p/2045845136085611469"
author:
  - "[[姚同学挖掘优质数字内容，打破信息不对称。]]"
published:
created: 2026-06-08
description: "出去旅行了一周，回到家里，目光重新聚焦到 AI 上，总结起来如下： 1️⃣ 谷歌 gemini 3.5 flash 模型拉了，不太好用，期待六月的 gemini 3.5 pro 上线； 2️⃣ Hermes 发布了桌面端，大家都在从 CLI 转回 GUI，用…"
tags:
  - "clippings"
status: processed
---
5 人赞同了该文章

出去旅行了一周，回到家里，目光重新聚焦到 AI 上，总结起来如下：

1️⃣ 谷歌 gemini 3.5 flash 模型拉了，不太好用，期待六月的 gemini 3.5 pro 上线；

2️⃣ Hermes 发布了桌面端，大家都在从 CLI 转回 GUI，用了挺久 Codex 发现确实比 Claude Code 更容易上手。

3️⃣ [Open Design](https://zhida.zhihu.com/search?content_id=276207420&content_type=Article&match_order=1&q=Open+Design&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODEwNTgyNzUsInEiOiJPcGVuIERlc2lnbiIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3NjIwNzQyMCwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.dpDs_f9o8ACU0_S9K6gRaYRmO2kcvl5Kj9L6FwANKPA&zhida_source=entity) 这个伟大的开源项目更新到了 0.9 版本，它是 Figma 和 [Claude Design](https://zhida.zhihu.com/search?content_id=276207420&content_type=Article&match_order=1&q=Claude+Design&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODEwNTgyNzUsInEiOiJDbGF1ZGUgRGVzaWduIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6Mjc2MjA3NDIwLCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.-PM-kHewVQ5Gk168a3Or_OjgB2fgWLmGyTr_mZUTdQc&zhida_source=entity) 的 Agent 原生替代，同样也是桌面客户端优先。

今天还是重点讲一下 Open Design，我实在是太喜欢这个这产品了，在 x 上和它们维护人员互动也是一件非常开心的事情。

![](https://pic4.zhimg.com/v2-b4b068c6a93ed94ca85d535af6a89019_1440w.jpg)

我在 Mac 上已经安装好了 [Codex CLI](https://zhida.zhihu.com/search?content_id=276207420&content_type=Article&match_order=1&q=Codex+CLI&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODEwNTgyNzUsInEiOiJDb2RleCBDTEkiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzYyMDc0MjAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.iXSlAGmMw_p_whsf-XT1dRz9S7qwYx2B533mijvJCmE&zhida_source=entity) 和 Claude Code，然后通过 Open Design 跑了一轮原型。整套链路花了不到十分钟，输出的原型水平和 Claude Opus 4.8 已经不分上下。

人生苦短，一起来用 Open Design 吧。

## Claude Design 把门槛抬太高了

Claude Design 是 [Anthropic](https://zhida.zhihu.com/search?content_id=276207420&content_type=Article&match_order=1&q=Anthropic&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODEwNTgyNzUsInEiOiJBbnRocm9waWMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzYyMDc0MjAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.TK38emiMrcNHKDRTiYi9gptrwqIv8kARLi9dBPaccUc&zhida_source=entity) 在 4 月 17 日发布的，跑在 Opus 4.8 上，让 Claude 不再写废话，直接交付原型、PPT、海报这种可视化成品。

它一上线就出圈，但同时也意味着几件不友好的事：闭源、付费（Pro 或 Max 才能用）、只跑在云上、绑死 Anthropic 自家模型、用的是 Anthropic 内部的 Skill 集合。

对一个常年在不同 AI 工具间切换的人来说，这种锁死的设计哲学有点违和。

![](https://pic4.zhimg.com/v2-c24f28fb99bcda4d21b72cab8d584dd1_1440w.jpg)

特别是大陆用户，光是付费这一步就够折腾。Pro 月费要走外卡，网络条件也要自己安排。整套门槛之下，能跑通 Claude Design 的人，本身已经不是这个工具的核心受众了。

## Open Design 用 60 天补上漏洞

5 月初，nexu-io 团队推出了 Open Design。

定位是 Claude Design 的开源替代。Apache-2.0、本地优先、Vercel 可部署、每一层都 BYOK。最关键的是它做了一件聪明事：不自带 Agent，而是扫描你 PATH 上已有的 coding CLI，把它们直接当成设计引擎。

![](https://pic2.zhimg.com/v2-bc230799590c530d16eec5161efb76eb_1440w.jpg)

它能识别 16 种 CLI，包括 Claude Code、Codex CLI、Cursor Agent、Gemini CLI、OpenCode、Qwen、Qoder、Copilot CLI 等等。没装任何 CLI 也可以走 BYOK 模式，直接对接 OpenAI、Anthropic、Azure、Gemini 这些标准接口。实在太全面了。

![](https://pic2.zhimg.com/v2-941f0404d0728665385c8fa516307a71_1440w.jpg)

我这里建议大家直接安装 Kimi CLI 或 Hermes，无需任何额外设置。

## 我的链路长这样

我这套链路有四个关键节点。

**第一节点是 Codex CLI** 。OpenAI 官方的 coding CLI，npm 全局装一下就行，配置过程没什么坑。

**第二节点是 [CC Switch](https://zhida.zhihu.com/search?content_id=276207420&content_type=Article&match_order=1&q=CC+Switch&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODEwNTgyNzUsInEiOiJDQyBTd2l0Y2giLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzYyMDc0MjAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.JojQYcGoXJctNPqE--jujeegjVRvbY1YFF0KoMuH30E&zhida_source=entity)** 。轻量的模型切换工具，原本是给 Claude Code 用的，现在被很多人用来跨 CLI 管理多个 endpoint。我用它给 Codex 注入自定义的 baseUrl 和 API key。

**第三节点是转发模型** 。把 [GPT-5.5](https://zhida.zhihu.com/search?content_id=276207420&content_type=Article&match_order=1&q=GPT-5.5&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODEwNTgyNzUsInEiOiJHUFQtNS41IiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6Mjc2MjA3NDIwLCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.49dgELvF7qYYrBDRP9UuPifp3BvN38QC440p-yy-Qr4&zhida_source=entity) 接到 CC Switch 里。中转的价值在于成本可控、省去外卡这一步、网络条件也不用单独安排。这条链路任何一个中转服务理论上都能替代，关键是要有这一层。

![](https://pic3.zhimg.com/v2-2dd32301beae6c82f5ba2de3ac55e676_1440w.jpg)

**第四节点是 Open Design** 。pnpm tools-dev 起服务，浏览器打开后会自动识别我 PATH 上的 Codex。Skill 默认选了 web-prototype，设计系统选了 Neutral Modern。

整套配齐之后，本机环境长这样：Codex 跑在终端层，CC Switch 在 endpoint 层做转发， [TokenGo](https://zhida.zhihu.com/search?content_id=276207420&content_type=Article&match_order=1&q=TokenGo&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODEwNTgyNzUsInEiOiJUb2tlbkdvIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6Mjc2MjA3NDIwLCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.DVRTf1L-keF6gv3R8LfzTQwaG5kJGQ6CSDH1HZFizK8&zhida_source=entity) 在 API 层兜底，Open Design 在 UI 层把这些 agent 当原料喂进去。

## 怎么用上呢？（重点）

1️⃣ 安装 Codex Cli，在终端运行下面的指令

```
npmi-g@openai/codex
```
![](https://pica.zhimg.com/v2-5ccd019d63744e16dafce00ca8f49120_1440w.jpg)

```
https://open-design.ai
```

6️⃣ 在 Open Design 中通过 Codex Cli 使用 GPT5.5 模型。

7️⃣ 愉快地用 Open Design 开始创作吧。

## 最后补充

我的判断是「原型设计+ PPT 制作」这条工作流，到 2026 年底之前会全面进入「本地 daemon + 多 CLI」这种架构。Claude Design 这种「全云端 + 单模型」的形态，未来很可能只是众多入口之一，而不是默认。

开源，依旧是这个世界上最可爱的模式。你已经在用 Open Design 了吗？

发布于 2026-06-04 12:31・北京