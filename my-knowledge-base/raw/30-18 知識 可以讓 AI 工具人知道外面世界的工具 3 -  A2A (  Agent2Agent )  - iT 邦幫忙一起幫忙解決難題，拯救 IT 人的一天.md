---
title: "30-18: [知識] 可以讓 AI 工具人知道外面世界的工具 3 -  A2A (  Agent2Agent )  - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10391363"
author:
  - "[[iThome]]"
published:
created: 2026-06-22
description: "上一篇文章中咱們討論完 MCP，接下來我們應該會發現一直有和他比較的名詞，那就是A2A這就是我們今天的主題。 🚀 A2A ( Agent2Agent ) 的起源與幹啥用的 A2A ( Agent2Ag..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 18

0

[AI & Data](https://ithelp.ithome.com.tw/2025ironman/ai-and-data)

### 30 天從 0 至 1 建立一個自已的 AI 學習工具人系列 第 18 篇

## 30-18: \[知識\] 可以讓 AI 工具人知道外面世界的工具 3 - A2A ( Agent2Agent )

- 分享至

上一篇文章中咱們討論完 MCP，接下來我們應該會發現一直有和他比較的名詞，那就是 `A2A` 這就是我們今天的主題。

## 🚀 A2A ( Agent2Agent ) 的起源與幹啥用的

A2A ( Agent2Agent ) 協定是 Google 在 2025 年時提出的，記憶中應該下面這個連結:

[https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/?utm\_source=chatgpt.com](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/?utm_source=chatgpt.com)

它主要的目的就是如同它的名稱:

> 讓 Agent 與 Agent 可以互相進行的協助

**🤔 那和 MCP 的差別是?**

- MCP 是給 Agent 可以標準化裝備遠端 Tools 與取得 Context 的協定 ( MCP Server 可以沒有 AI )。
- A2A 是點 Agent 可以和其它 Agent 協作的協定

簡單的概念差別就是這樣。

然後這段話是 Google 官方提到和 MCP 的關係，概念上和上面說的差不多:

> A2A is an open protocol that complements Anthropic's Model Context Protocol (MCP), which provides helpful tools and context to agents. Drawing on Google's internal expertise in scaling agentic systems, we designed the A2A protocol to address the challenges we identified in deploying large-scale, multi-agent systems for our customers. A2A empowers developers to build agents capable of connecting with any other agent built using the protocol and offers users the flexibility to combine agents from various providers. Critically, businesses benefit from a standardized method for managing their agents across diverse platforms and cloud environments. We believe this universal interoperability is essential for fully realizing the potential of collaborative AI agents.

然後這張 Google 官方的圖，我自已覺得解釋的很到位。

![https://ithelp.ithome.com.tw/upload/images/20251002/20089358vQUAcgFIiG.png](https://ithelp.ithome.com.tw/upload/images/20251002/20089358vQUAcgFIiG.png)  
[圖片來源: Google A2A 官方](https://a2a-protocol.org/latest/topics/what-is-a2a/#understanding-the-agent-stack-a2a-mcp-agent-frameworks-and-models)

## 🚀 A2A Protocl 設計原則與協定大綱

**🤔設計原則 A2A design principles**

先來說一下這個 Protocol 這個的 5 個設計原則，它們會和我們下面詳細的協定內容息息相關，以下為官方原文:

[https://a2a-protocol.org/latest/topics/what-is-a2a/#key-design-principles-of-a2a](https://a2a-protocol.org/latest/topics/what-is-a2a/#key-design-principles-of-a2a)

- Simplicity: A2A 採用很多現有存在的技術，例如 HTTP、JSON-RPC 或 SSE，這個可以加速開發者使用這個協定，不需要在自已寫個輪子或是研究新技術。
- Enterprise Readiness: 這個協定是已經可以給企業使用，因為他在設計時有包含所有企業在意的 Authentication、Authorization、Security、Privacy、Tracing 與 Monitoring。
- Asynchronous: 支援 Agent 情境很常見的長時間任務，然後它可以透過 Streaming 與 Push Notifications 來進行通知。
- Modality Independent: 允許 Agent 可以用不同類型的 Content 來進行溝通，例如語音也可。
- Opaque Execution: Agent 可以在不暴露內部所有相關的知識或上下文來進行有效的協作，可以保護知識智慧財產，也能提升安全性。( **🤔但是 Agent 間的上下文，我們要如何處理呢? 等等會提** )

雖然這個和第一版的設計原則有些地方不太一樣，但大至還是差不多，下面這是第一版的，差別應該是在 Embrace agentic capabilities 這個，其它 4 個都一樣，但這個有部份是包含在 Opaque Execution，所以也沒差多少。

[https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/?utm\_source=chatgpt.com](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/?utm_source=chatgpt.com)

**🤔A2A 協定大綱**

整份 Specification 在下面這個連結中，然後我們先摘要一下重點，然後接下來再開始說說每個部份。

[https://a2a-protocol.org/latest/specification/](https://a2a-protocol.org/latest/specification/)

- Transport
- Authentication and Authorization
- Agent Card
- Message 與 Task

## 🚀 Transport

A2A 總共支援以下 3 種傳輸協定，也就是說你的 server 實作時，會選擇其中一種來實作，然後接下來 client 就是會用這個模式來和你溝通。

- JSON-RPC 2.0 Transport
- gRPC Transport
- HTTP+JSON/REST Transport

並且他們每一種在 method 上都有一些要求規範，大部份都是 {category}/{action} 這種的概念，然後轉化成不同 Transport，如下表:

![https://ithelp.ithome.com.tw/upload/images/20251002/20089358qWhPQET9Dj.png](https://ithelp.ithome.com.tw/upload/images/20251002/20089358qWhPQET9Dj.png)  
[圖片來源: 官方](https://a2a-protocol.org/latest/specification/#356-method-mapping-reference-table)

**🤔 順到問一下那 client 端，要如何知識 server 支援什麼 transport?**

> The Agent Card

等等會提到。

## 🚀 Authentication and Authorization

[https://a2a-protocol.org/latest/specification/#4-authentication-and-authorization](https://a2a-protocol.org/latest/specification/#4-authentication-and-authorization)

先說一下，因為有很多新手朋朋們會搞錯，所以簡單說明一下這兩個字的差別:

- Authentication ( 驗證 ): 你是誰
- Authorization ( 授權 ): 你可以幹啥

然後還記得我們在 MCP 寫的嗎? 它事實上只要求 Authorization 這塊要用 OAuth 2.0。

然後在 A2A 這裡是兩個都要，然後它們在 A2A 的差別方式為

Client 透過 AgentCard 中的 authentication 欄位發現伺服器所需的驗證機制

但是他的確在 Authorization 這裡，沒有像 MCP 一樣要求如果要實作一定要 Oauth 2.0

## 🚀 Agent Card

[https://a2a-protocol.org/latest/specification/#5-agent-discovery-the-agent-card](https://a2a-protocol.org/latest/specification/#5-agent-discovery-the-agent-card)

> 它應該是整個 A2A 的核心，它定議了你的 Agent 會什麼、如何溝通之類的

當我們在建立 A2A server 時，會需要先在某個地方，來讓其它 Agent 可以知道你的資訊，然後這個地方官方是建議放在下面這個地方 ( 但不強制 ):

> https://{server\_domain}/.well-known/agent-card.json

然後裡面這個至少需要以下的資訊:

- protocolVersion: 這個 Agent 遵守的 A2A 版本
- name: 你這個 Agent 叫啥
- description: 你這個 Agent 的自我介紹
- url: 這個 Agent 所在的位置
- version: Agent 的版本號
- capabilities: 是否支援一些可選的功能，有 streaming、pushNotifications、stateTransitionHistory、extensions。
- defaultInputModes: 預設情況下，你的 skill 所能支援的 input MIME types，例如 \["application/json", "text/plain"\]。
- defaultOutputModes: 預設情況下，你的 skill 所能支援的 output MIME types，例如 \["application/json", "image/png"\]。
- skills: 就是你這個 Agent 實際上可幹啥的地方。

然後以下為官方一個完整的 Agent Card，然後 skills 的範例事實上就很像我們 MCP 的 tool 那的東西。

```json
{
  "protocolVersion": "0.2.9",
  "name": "GeoSpatial Route Planner Agent",
  "description": "Provides advanced route planning, traffic analysis, and custom map generation services. This agent can calculate optimal routes, estimate travel times considering real-time traffic, and create personalized maps with points of interest.",
  "url": "https://georoute-agent.example.com/a2a/v1",
  "preferredTransport": "JSONRPC",
  "additionalInterfaces" : [
    {"url": "https://georoute-agent.example.com/a2a/v1", "transport": "JSONRPC"},
    {"url": "https://georoute-agent.example.com/a2a/grpc", "transport": "GRPC"},
    {"url": "https://georoute-agent.example.com/a2a/json", "transport": "HTTP+JSON"}
  ],
  "provider": {
    "organization": "Example Geo Services Inc.",
    "url": "https://www.examplegeoservices.com"
  },
  "iconUrl": "https://georoute-agent.example.com/icon.png",
  "version": "1.2.0",
  "documentationUrl": "https://docs.examplegeoservices.com/georoute-agent/api",
  "capabilities": {
    "streaming": true,
    "pushNotifications": true,
    "stateTransitionHistory": false
  },
  "securitySchemes": {
    "google": {
      "type": "openIdConnect",
      "openIdConnectUrl": "https://accounts.google.com/.well-known/openid-configuration"
    }
  },
  "security": [{ "google": ["openid", "profile", "email"] }],
  "defaultInputModes": ["application/json", "text/plain"],
  "defaultOutputModes": ["application/json", "image/png"],
  "skills": [
    {
      "id": "route-optimizer-traffic",
      "name": "Traffic-Aware Route Optimizer",
      "description": "Calculates the optimal driving route between two or more locations, taking into account real-time traffic conditions, road closures, and user preferences (e.g., avoid tolls, prefer highways).",
      "tags": ["maps", "routing", "navigation", "directions", "traffic"],
      "examples": [
        "Plan a route from '1600 Amphitheatre Parkway, Mountain View, CA' to 'San Francisco International Airport' avoiding tolls.",
        "{\"origin\": {\"lat\": 37.422, \"lng\": -122.084}, \"destination\": {\"lat\": 37.7749, \"lng\": -122.4194}, \"preferences\": [\"avoid_ferries\"]}"
      ],
      "inputModes": ["application/json", "text/plain"],
      "outputModes": [
        "application/json",
        "application/vnd.geo+json",
        "text/html"
      ]
    },
    {
      "id": "custom-map-generator",
      "name": "Personalized Map Generator",
      "description": "Creates custom map images or interactive map views based on user-defined points of interest, routes, and style preferences. Can overlay data layers.",
      "tags": ["maps", "customization", "visualization", "cartography"],
      "examples": [
        "Generate a map of my upcoming road trip with all planned stops highlighted.",
        "Show me a map visualizing all coffee shops within a 1-mile radius of my current location."
      ],
      "inputModes": ["application/json"],
      "outputModes": [
        "image/png",
        "image/jpeg",
        "application/json",
        "text/html"
      ]
    }
  ],
  "supportsAuthenticatedExtendedCard": true,
  "signatures": [
    {
      "protected": "eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UiLCJraWQiOiJrZXktMSIsImprdSI6Imh0dHBzOi8vZXhhbXBsZS5jb20vYWdlbnQvandrcy5qc29uIn0",
      "signature": "QFdkNLNszlGj3z3u0YQGt_T9LixY3qtdQpZmsTdDHDe3fXV9y9-B3m2-XgCpzuhiLt8E0tV6HXoZKHv4GtHgKQ"
    }
  ]
}
```

**🤔 然後上面那題 client 端，要如何知識 server 支援什麼 transport?**

就是看 `preferredTransport` 這個欄位。

## 🚀 Message 與 Task

在 A2A 基本上除了 Agent Card 以下，還有以下二種主要的溝通元素，那就是:

- Message: 一次性的對話，但通常是用來傳送資料，但不一定會是最後的答案。
- Task: 有狀態與ID的工作單位，用來支援長時間的任務（可多輪互動、可串流、可推播）。

這兩個很重要的原因就在於，如果要實作 A2A 支援的功能，那大部份都是和這兩個有關，我簡單貼一下官網說要實作的東西，是不是都和這兩個有關?

**🤔 一定要實作的方法**

- message/send - Send messages and initiate tasks
- tasks/get - Retrieve task status and results
- tasks/cancel - Request task cancellation

**🤔 可選的要實作的方法**

- message/stream - Streaming message interaction (requires capabilities.streaming: true)
- tasks/resubscribe - Resume streaming for existing tasks (requires capabilities.streaming: true)
- tasks/pushNotificationConfig/set - Configure push notifications (requires capabilities.pushNotifications: true)
- tasks/pushNotificationConfig/get - Retrieve push notification config (requires capabilities.pushNotifications: true)
- tasks/pushNotificationConfig/list - List push notification configs (requires capabilities.pushNotifications: true)
- tasks/pushNotificationConfig/delete - Delete push notification config (requires capabilities.pushNotifications: true)
- agent/authenticatedExtendedCard - Retrieve authenticated agent card (requires supportsAuthenticatedExtendedCard: true)

**🤔 上面提到的 Agent 與 Agent 的上下文要如何處理呢?**

在 A2A 會用 contextId 識別，那些是同一個事情，然後送請求時，會帶這它。也就是說每個 Agent 會透過這個唯一編號，來在各 Agent 管理這份相關的上下文，所以這也代表其它 Agent 不會拿到你的上下文。

[https://a2a-protocol.org/latest/topics/life-of-a-task/#example-follow-up-scenario](https://a2a-protocol.org/latest/topics/life-of-a-task/#example-follow-up-scenario)

## 🚀 溝通流程情境範例

上面都是在說文謅謅的東西，這裡來實際上根據以上的東西，用情境範例來理解它整個溝通流程與結構。

**🤔 情境 1: 發現 Agent 與進行最簡單的 say hello ~**

![https://ithelp.ithome.com.tw/upload/images/20251002/2008935875EFoEXDeD.png](https://ithelp.ithome.com.tw/upload/images/20251002/2008935875EFoEXDeD.png)

這裡就是大部份最開始的情境，都會先透過上面提到的 `Agent Card` 來知道其它 Agent 的資訊。

**🤔 情境 2: 請幫我整理這 30 天的學習資料，然後寄信到這個 email 信箱 ( 通知版本 )**

![https://ithelp.ithome.com.tw/upload/images/20251002/200893586v8n1hi4LY.png](https://ithelp.ithome.com.tw/upload/images/20251002/200893586v8n1hi4LY.png)

整個流程大概就會如下:

1. 馬克 Agent 會根據 A2A 協定，發送 `message/send` 裡面說了我要做啥。
2. 接下來 Hahow Agent 會乖乖的做事情。
3. 然後完成後，會打 pushNotificationConfig 中的位置來進行通知。

然後 message/send payload 大概會長的如下:

```json
{
  "jsonrpc": "2.0",
  "id": "req-001",
  "method": "message/send",
  "params": {
    "message": {
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "請幫我整理這 30 天的學習資料，然後寄信到 user@example.com"
        }
      ],
      "messageId": "6dbc13b5-bd57-4c2b-b503-24e381b6c8d6"
    },
    "configuration": {
      "pushNotificationConfig": {
        "url": "https://my-app.com/webhook/a2a-notifications",
        "token": "secure-client-token-xyz",
        "authentication": {
          "schemes": ["Bearer"]
        }
      }
    }
  }
}
```

**🤔 情境 3: 請幫我整理這 30 天的學習資料，然後寄信到這個 email 信箱 ( 輪詢 )**

![https://ithelp.ithome.com.tw/upload/images/20251002/200893581RxRhVHi6z.png](https://ithelp.ithome.com.tw/upload/images/20251002/200893581RxRhVHi6z.png)

然後這個是如果我們不用通知，而是用輪詢的情況下的流程 ( 現實中還是有可能，例如你們服務封閉，外網是打不進去的 )。

1. 馬克 Agent 會根據 A2A 協定，發送 `message/send` 裡面說了我要做啥，payload 大約如下。
```json
// 1. 發送訊息 (message/send)
{
  "jsonrpc": "2.0",
  "id": "req-001",
  "method": "message/send",
  "params": {
    "message": {
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "請幫我整理這 30 天的學習資料，然後寄信到 user@example.com"
        }
      ],
      "messageId": "6dbc13b5-bd57-4c2b-b503-24e381b6c8d6"
    }
  }
}

// Server 回應
{
  "jsonrpc": "2.0",
  "id": "req-001",
  "result": {
    "id": "363422be-b0f9-4692-a24d-278670e7c7f1",
    "contextId": "c295ea44-7543-4f78-b524-7a38915ad6e4",
    "status": {
      "state": "working"
    },
    "kind": "task"
  }
}
```
2. 再來根據我們上面取得到的 taskId 來輪詢這個 task 的狀態，最後到了某一天終於完成了，它們的 payload 如下:
```json
// 2. 查詢任務狀態 (tasks/get)
{
  "jsonrpc": "2.0",
  "id": "req-002",
  "method": "tasks/get",
  "params": {
    "id": "363422be-b0f9-4692-a24d-278670e7c7f1"
  }
}

// Server 回應 - 進行中
{
  "jsonrpc": "2.0",
  "id": "req-002",
  "result": {
    "id": "363422be-b0f9-4692-a24d-278670e7c7f1",
    "contextId": "c295ea44-7543-4f78-b524-7a38915ad6e4",
    "status": {
      "state": "working"
    },
    "kind": "task"
  }
}

// Server 回應 - 完成
{
  "jsonrpc": "2.0",
  "id": "req-003",
  "result": {
    "id": "363422be-b0f9-4692-a24d-278670e7c7f1",
    "contextId": "c295ea44-7543-4f78-b524-7a38915ad6e4",
    "status": {
      "state": "completed"
    },
    "artifacts": [
      {
        "artifactId": "9b6934dd-37e3-4eb1-8766-962efaab63a1",
        "name": "learning_summary.pdf",
        "parts": [
          {
            "kind": "text",
            "text": "學習報告已整理完成並寄送至 user@example.com"
          }
        ]
      }
    ],
    "kind": "task"
  }
}
```

## 🚀 小總結

整體來說，我自已覺得整個 A2A 協定比 MCP 協定還簡單不少，當初在看 MCP 時還花比較多時間理解，但這個只是我自已的感覺。

## 🚀 參考資料

- [A2A 官網](https://a2a-protocol.org/latest/)

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10391362)

[

30-17: \[實作-8\] 將我們的學習記錄，記錄到 Notion 之 2 ( MCP Client )

](https://ithelp.ithome.com.tw/articles/10391362)[下一篇](https://ithelp.ithome.com.tw/articles/10392437)

[

30-19: \[實作-9\] 讓我們的 AI 工具人可以與其它 Agent 溝通 - A2A 的實作

](https://ithelp.ithome.com.tw/articles/10392437)

系列文

[30 天從 0 至 1 建立一個自已的 AI 學習工具人](https://ithelp.ithome.com.tw/users/20089358/ironman/8560) 共 30 篇

目錄

1. 26
	[30-26: \[知識\] AI Application Evaluation ( 2 ) 之 Ragas 評分指標 ( Langfuse 版本 )](https://ithelp.ithome.com.tw/articles/10395711)
2. 27
	[30-27: \[知識\] AI Application Evaluation ( 3 ) 之 Experiments 篇](https://ithelp.ithome.com.tw/articles/10395659)
3. 28
	[30-28: \[實作\] 我們要如何評估與優化課程問與答的功能品質呢? ( 還需研究 )](https://ithelp.ithome.com.tw/articles/10396365)
4. 29
	[30-29: \[知識\] 選擇 AI Model 時要考慮那些呢? ( 2025 年版 )](https://ithelp.ithome.com.tw/articles/10396755)
5. 30
	[30-30: Junior AI Application Engineer 的學習指南 - 透過實作 AI 學習工具人](https://ithelp.ithome.com.tw/articles/10397188)
[完整目錄](https://ithelp.ithome.com.tw/users/20089358/ironman/8560)

[![.](https://itadstatic.ithome.com.tw/B9/1781230970_6a2b6d7a22557.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2606B9003)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/0045b58c-9293-4ce2-b294-f4c734ef5a92_small.jpg)

[後疫情時代，數位工作場域的趨勢演變與資安策略](https://itplus.ithome.com.tw/vod-page/349?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/16/60254a11-2a07-4888-a7f3-ef00d038ca9b_small.png)

[DevOps on Atlassian Cloud](https://itplus.ithome.com.tw/vod-page/537?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/14/d2d98f48-4d73-4a83-bada-665ac4d78c26_small.png)

[如何透過 juju 簡化 K8s 服務的操作](https://itplus.ithome.com.tw/vod-page/924?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

28 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/10/14/108145c8-1e53-4e7b-8537-41e497984dbc_small.png)

[復盤： 將經驗轉化成能力](https://itplus.ithome.com.tw/vod-page/972?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

64 分