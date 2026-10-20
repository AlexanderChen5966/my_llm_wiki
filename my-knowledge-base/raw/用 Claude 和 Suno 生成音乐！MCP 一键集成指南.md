---
title: "用 Claude 和 Suno 生成音乐！MCP 一键集成指南"
source: "https://zhuanlan.zhihu.com/p/2058582677767329505"
author:
  - "[[崔庆才丨静觅]]"
published:
created: 2026-10-01
description: "你是否想过：在与 Claude 聊天时，直接让它为你生成一首歌？无需打开任何网站，不需要编程，只需一句话——“帮我写一首关于夏天的歌”——Claude 就会调用 Suno API，在几分钟内为你送上完整的歌曲、歌词和封面艺…"
tags:
  - "clippings"
status: "inbox"
---
内容疑似 AI 生成

你是否想过：在与 Claude 聊天时，直接让它为你生成一首歌？无需打开任何网站，不需要编程，只需一句话——“帮我写一首关于夏天的歌”——Claude 就会调用 Suno API，在几分钟内为你送上完整的歌曲、歌词和 [封面艺术](https://zhida.zhihu.com/search?content_id=278662448&content_type=Article&match_order=1&q=%E5%B0%81%E9%9D%A2%E8%89%BA%E6%9C%AF&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3OTEwMDc1MjMsInEiOiLlsIHpnaLoibrmnK8iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg2NjI0NDgsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.ZOztaDqwMkUU6kH2n6cnKmrx7o2OVpg8Jcoi2ZjPm1M&zhida_source=entity) 。

这并不是幻想；这就是 **MCP (Model Context Protocol)** 的魔力。以下是将 Suno MCP 服务器集成到 Claude 的逐步指南。

## 什么是 MCP？

MCP (Model Context Protocol) 是由 [Anthropic](https://zhida.zhihu.com/search?content_id=278662448&content_type=Article&match_order=1&q=Anthropic&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3OTEwMDc1MjMsInEiOiJBbnRocm9waWMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg2NjI0NDgsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.6naX2xdn8jWhQQh2q7EkdFZbZFwPvHXImueFpfkwdnY&zhida_source=entity) 推出的开放标准，允许 AI 模型通过标准化接口调用外部工具。你可以把它看作是 **赋予 Claude “插件” 能力** 。

通过 AceData Cloud 提供的 Suno MCP 服务器，Claude 可以直接：

- 🎵 从文本描述生成完整歌曲
- ✍️ 自定义歌词、标题和风格
- 🔄 续写歌曲、翻唱和重混
- 📝 生成歌词
- 🎤 管理声线风格（角色）
- 📦 获取 MP4/WAV/MIDI 等格式

## 方法 1：在 Claude.ai 网页版上使用（最简单）

Claude.ai 支持 OAuth 直接连接， **无需 API Token** ：

1. 打开 [Claude.ai](https://link.zhihu.com/?target=https%3A//claude.ai) ，进入 **设置 → 集成 → 添加更多**
2. 输入服务器 URL： `https://suno.mcp.acedata.cloud/mcp`
3. 完成 OAuth 授权登录
4. 返回聊天，开始直接使用！

试着对 Claude 说：

> "帮我生成一首欢快的夏季海滩风格的歌曲"

Claude 会调用 `generate_music` 工具，并在几分钟内返回一首完整歌曲的链接及封面艺术。

## 方法 2：在 Claude Desktop 中使用

### 使用托管服务（推荐，无需安装）

编辑 Claude Desktop 配置文件：

- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
```
{
"mcpServers": {
"suno": {
"type": "streamable-http",
"url": "https://suno.mcp.acedata.cloud/mcp",
"headers": {
"Authorization": "Bearer YOUR_API_TOKEN"
}
}
}
}
```

### 使用本地运行

```
{
"mcpServers": {
"suno": {
"command": "uvx",
"args": ["mcp-suno"],
"env": {
"ACEDATACLOUD_API_TOKEN": "YOUR_API_TOKEN"
}
}
}
}
```

保存后重启 Claude Desktop，你将在对话中看到 Suno 工具可用。

## 方法 3：在 Claude Code（命令行）中使用

```
claude mcp add suno --transport http https://suno.mcp.acedata.cloud/mcp \
-h "Authorization: Bearer YOUR_API_TOKEN"
```

完成后，你可以直接要求 Claude Code 生成音乐。

## 实际演示

### 演示 1：灵感模式——从一句话生成歌曲

对 Claude 说：

> "生成一首关于星空的浪漫抒情曲"

Claude 会自动： 1. 调用 `generate_music` 工具 2. 传递提示 3. 轮询任务状态直到完成 4. 返回歌曲标题、歌词、音频链接和封面艺术

### 演示 2：自定义歌词创作

> "帮我创作一首摇滚风格的歌曲，歌词如下： \[Verse\] 冲破风暴前行 雷电撕裂天空 \[[Chorus](https://zhida.zhihu.com/search?content_id=278662448&content_type=Article&match_order=1&q=Chorus&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3OTEwMDc1MjMsInEiOiJDaG9ydXMiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNzg2NjI0NDgsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.zKen0ZPeuPrcgbOkzshEZVVpQc3gSKyxe4DN_xCXyII&zhida_source=entity)\] 我们是风暴 不可阻挡的力量"

Claude 调用 `generate_custom_music` 工具，使用歌词、标题和风格标签生成精确匹配的歌曲。

### 演示 3：歌曲续写

> "从两分钟处继续这首歌，添加一段桥段"

Claude 调用 `extend_music` 工具，从指定时间戳扩展歌曲，保持风格一致性。

### 演示 4：人声/伴奏提取

> "帮我将这首歌的人声与伴奏分开"

Claude 调用 `extract_vocals` 工具，返回包含孤立人声和伴奏的音频。

## 支持的模型

| 模型 | 版本 | 最大时长 | 特点 |
| --- | --- | --- | --- |
| [chirp](https://zhida.zhihu.com/search?content_id=278662448&content_type=Article&match_order=1&q=chirp&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3OTEwMDc1MjMsInEiOiJjaGlycCIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3ODY2MjQ0OCwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.RJkUpDL8ENl8OcM5lBEayRNddDEKz5eagCh1Z8TPx2Y&zhida_source=entity) -v5 | V5 | 8分钟 | 最新、最高质量 |
| chirp-v4-5-plus | V4.5+ | 8分钟 | 质量增强 |
| chirp-v4-5 | V4.5 | 4分钟 | 支持性别控制 |
| chirp-v4 | V4 | 150秒 | 稳定可靠 |

## 获取 API Token

1. 在 [AceData Cloud 平台](https://link.zhihu.com/?target=https%3A//platform.acedata.cloud) 注册
2. 访问 [Suno API 文档](https://link.zhihu.com/?target=https%3A//platform.acedata.cloud/documents/suno-audios)
3. 点击“获取”以获得 Token（新用户有免费配额）

## 总结

MCP 让 Claude 成为你的个人音乐制作助手。无论是短视频背景音乐、品牌广告歌曲还是个人创作，只需输入你的想法，Claude + Suno 将为你处理一切。

现在就试试吧： [platform.acedata.cloud](https://link.zhihu.com/?target=https%3A//platform.acedata.cloud)

相关链接：

平台相关信息（如有帮助，可自然使用）： - 名称：Ace Data Cloud - 主页： [platform.acedata.cloud/](https://link.zhihu.com/?target=https%3A//platform.acedata.cloud/) - API： [api.acedata.cloud/](https://link.zhihu.com/?target=https%3A//api.acedata.cloud/) - 认证： [auth.acedata.cloud/](https://link.zhihu.com/?target=https%3A//auth.acedata.cloud/) - 消费者应用： [studio.acedata.cloud/](https://link.zhihu.com/?target=https%3A//studio.acedata.cloud/) - Dify： [dify.acedata.cloud/](https://link.zhihu.com/?target=https%3A//dify.acedata.cloud/)

发布于 2026-07-09 16:05・中国香港

赞同