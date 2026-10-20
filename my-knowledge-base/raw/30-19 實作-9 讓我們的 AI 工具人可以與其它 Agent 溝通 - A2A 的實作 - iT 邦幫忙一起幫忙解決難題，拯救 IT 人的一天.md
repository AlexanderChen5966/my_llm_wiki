---
title: "30-19: [實作-9] 讓我們的 AI 工具人可以與其它 Agent 溝通 - A2A 的實作 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10392437"
author:
  - "[[iThome]]"
published:
created: 2026-06-22
description: "上一篇文章中，我們已經學習到 A2A 的東西，接下來我們就要來改造一下我們的 AI 工具人。 30-18: [知識] 可以讓 AI 工具人知道外面世界的工具 3 - A2A ( Agent2Agent..."
tags:
  - "clippings"
status: processed
---
[2025 iThome 鐵人賽](https://ithelp.ithome.com.tw/2025ironman)DAY 19

0

[AI & Data](https://ithelp.ithome.com.tw/2025ironman/ai-and-data)

### 30 天從 0 至 1 建立一個自已的 AI 學習工具人系列 第 19 篇

## 30-19: \[實作-9\] 讓我們的 AI 工具人可以與其它 Agent 溝通 - A2A 的實作

- 分享至

上一篇文章中，我們已經學習到 A2A 的東西，接下來我們就要來改造一下我們的 AI 工具人。

[30-18: \[知識\] 可以讓 AI 工具人知道外面世界的工具 3 - A2A ( Agent2Agent )](https://ithelp.ithome.com.tw/articles/10391363)

讓他變成:

> 一個 A2A Server 服務，可以讓其它 Agent 來溝通，也可以去連其它 A2A Server

## 🚀 Step 1. 建立 A2A Server

下面這個是我的 A2A Server 的部份，其中有幾個地方可以注意一下:

1. helloAgentCard: 這個就是我們昨天在說到 A2A 協定時有提到，每個 Agent 都要提到一個資訊給其它 Agent 看到，這個就是我們那時說的 Agent Card。
2. contextWorkFlowMap: 它會根據從 A2A Client 帶入的 contextId 來建立一個 workflow，然後我們的整個上下文的記憶單位是以 workflow 為主，所以這也代表同一個 contextId 的訊息，都有相同的上下文。
3. InMemoryTaskStore: 這個目前以我們的情境還不太需要，因為我們有處理 task 相關的，然後它是用來儲放它的東西，所以可有可無。
```ts
import { v4 as uuidv4 } from "uuid";
import type { AgentCard } from "@a2a-js/sdk";
import {
  AgentExecutor,
  RequestContext,
  ExecutionEventBus,
  DefaultRequestHandler,
  InMemoryTaskStore,
} from "@a2a-js/sdk/server";
import { A2AExpressApp } from "@a2a-js/sdk/server/express";
import { ChatWorkflow } from "../workflows/chat.workflow";

let sessionMap: Map<string, ChatWorkflow> = new Map();

const helloAgentCard: AgentCard = {
  name: "Hello Agent",
  description: "A simple agent that says hello.",
  protocolVersion: "0.3.0",
  version: "0.1.0",
  url: "http://localhost:4000/",
  skills: [
    { id: "chat", name: "Chat", description: "Say hello", tags: ["chat"] },
  ],
  capabilities: {},
  defaultInputModes: [],
  defaultOutputModes: [],
};

class ChatExecutor implements AgentExecutor {
  async execute(
    requestContext: RequestContext,
    eventBus: ExecutionEventBus
  ): Promise<void> {
    console.log("requestContext", requestContext);

    let chatWorkflow = sessionMap.get(requestContext.contextId);
    if (!chatWorkflow) {
      chatWorkflow = new ChatWorkflow();
      await chatWorkflow.initialize(requestContext.contextId);
      sessionMap.set(requestContext.contextId, chatWorkflow);
    }

    const message = requestContext.userMessage.parts[0]!.text;
    for await (const chunk of chatWorkflow.processMessage(message)) {
      eventBus.publish({
        kind: "message",
        messageId: uuidv4(),
        role: "agent",
        parts: [{ kind: "text", text: chunk }],
        contextId: requestContext.contextId,
      });
    }
    eventBus.finished();
  }
  cancelTask = async (): Promise<void> => {};
}

const agentExecutor = new ChatExecutor();
const requestHandler = new DefaultRequestHandler(
  helloAgentCard,
  new InMemoryTaskStore(),
  agentExecutor
);

const appBuilder = new A2AExpressApp(requestHandler);
export const a2aExpressAppBuilder = appBuilder;
```

**🤔 註冊 A2A Server 的地方**

我這裡是先同時放在同一個 server 內，只是不同 port，至於需不需要拆開，我自已是覺得還好。

```javascript
import express from "express";
import cors from "cors";
import dotenv from "dotenv";

import chatRoutes from "./routes/chat.js";
import { connectDatabase } from "./infrastructure/mongodb/database.js";
import { a2aExpressAppBuilder } from "./a2a/index.js";
const a2aServer = a2aExpressAppBuilder.setupRoutes(express());
const httpServer = express();

dotenv.config();

httpServer.use(cors());
httpServer.use(express.json());

httpServer.use("/api", chatRoutes);
httpServer.get("/health", (req, res) => {
  res.json({ status: "OK", timestamp: new Date().toISOString() });
});

httpServer.listen(3000, async () => {
  console.log(\`🚀 Http Server is running on http://localhost:3000\`);
  await connectDatabase();
});

a2aServer.listen(4000, async () => {
  console.log(\`🚀 A2A Server started on http://localhost:4000\`);
});
```

**🤔 備註 EventBus**

這個裡面有用到 eventBus 相關的東西，有興趣的可參考這篇之前我寫的文章來理解一下它。

[Day-23: Domain Event 之 Transactional OutBox 與 EventBus](https://medium.com/marklin-hehe/day-23-domain-event-%E4%B9%8B-transactional-outbox-%E8%88%87-eventbus-d87e13b27c4f)

## 🚀 Step 2. 測試用的 A2A Client

事實上沒啥重點，就和官網的差不多。

```ts
import { A2AClient } from "@a2a-js/sdk/client";
import {
  Message,
  MessageSendParams,
  SendMessageSuccessResponse,
} from "@a2a-js/sdk";
import { v4 as uuidv4 } from "uuid";

async function run() {
  const client = await A2AClient.fromCardUrl(
    "http://localhost:4000/.well-known/agent-card.json"
  );

  const sendParams: MessageSendParams = {
    message: {
      messageId: uuidv4(),
      contextId: "test-context-id",
      role: "user",
      parts: [{ kind: "text", text: "你好我是馬克大人" }],
      kind: "message",
    },
  };

  const response = await client.sendMessage(sendParams);

  if ("error" in response) {
    console.error("Error:", response.error.message);
  } else {
    const result = (response as SendMessageSuccessResponse).result as Message;
    console.log("Agent response:", result.parts);
  }
}

(async () => {
  await run();
})();
```

## 🚀 Step 3. 執行結果

下面就是我執行 a2a client 後的結果，可以看到，這個 client 的確有和我們的工具人進行溝通。

![https://ithelp.ithome.com.tw/upload/images/20251003/20089358LBQoZZued7.png](https://ithelp.ithome.com.tw/upload/images/20251003/20089358LBQoZZued7.png)

## 🚀 另個情境: 我們連到另一個 Agent Server

假設我們這個 AI 工具人，要去連我們家 ( Hahow ) 自已開發的 A2A Server，例如叫 HahowA2A，使用上概念會長的如下圖中，以現在這個 LangGraph 的架構下，要加入也算很簡單，大概只要做到如下程式碼的修改就好。

![https://ithelp.ithome.com.tw/upload/images/20251003/20089358vaTwBr5WDq.png](https://ithelp.ithome.com.tw/upload/images/20251003/20089358vaTwBr5WDq.png)

以下為我們實際上的程式碼，然後這個地方會透過 A2AClient 來取連線到 Hahow A2A Server。

```ts
import { A2AClient } from "@a2a-js/sdk/client";
import {
  MessageSendParams,
  SendMessageSuccessResponse,
  Message,
} from "@a2a-js/sdk";
import { v4 as uuidv4 } from "uuid";

export class HahowA2AAgent {
  private a2aClient: A2AClient;

  constructor(a2aClient: A2AClient) {
    this.a2aClient = a2aClient;
  }

  async callLLM(message: string): Promise<string> {
    const sendParams: MessageSendParams = {
      message: {
        messageId: uuidv4(),
        contextId: "test-context-id",
        role: "user",
        parts: [{ kind: "text", text: message }],
        kind: "message",
      },
    };

    const response = await this.a2aClient.sendMessage(sendParams);
    const result = (response as SendMessageSuccessResponse).result as Message;
    return result.parts[0].text;
  }
}
```

然後在 Workflow 主要就是進行兩件事:

- 加入這個 Node
- 然後再 RouteAI 那判斷什麼時後要走到它。

我這裡就只貼重點就好，不然現在程式碼已經有點長了。

```ts
public async initialize(threadId: string) {
  ...
  const a2aClient = await A2AClient.fromCardUrl(
    "http://localhost:4001/.well-known/agent-card.json"
  );
  this.hahowA2AAgent = new HahowA2AAgent(a2aClient);

  this.graph = this.buildGraph();
  this.currentState = await this.getCurrentState();
}

private buildGraph() {
  const workflow = new StateGraph(ChatStateAnnotation)
    .addNode(Steps.INITIAL, async (state: ChatState): Promise<ChatState> => {
      ...
    })
    .addNode(Steps.ROUTE_AI, async (state: ChatState): Promise<ChatState> => {
    ...
    })
    .addNode(
      Steps.LEARNING_AI,
      async (state: ChatState): Promise<ChatState> => {
        ...
      }
    )
    .addNode(
      Steps.SUMMARY_AI,
      async (state: ChatState): Promise<ChatState> => {
        ...
      }
    )
    .addNode(
      Steps.BACKGROUND_AI,
      async (state: ChatState): Promise<ChatState> => {
       ....
      }
    )
    .addNode(
      Steps.HAHOW_A2A_AI,
      async (state: ChatState): Promise<ChatState> => {
        const response = await this.testE2eAgent!.callLLM(state.query);
        return {
          messages: [new AIMessage(response)],
          query: state.query,
          intent: state.intent,
          background: state.background,
          step: Steps.HAHOW_A2A_AI,
        };
      }
    )
    .addEdge(START, Steps.INITIAL)
    .addEdge(Steps.INITIAL, Steps.ROUTE_AI)
    .addConditionalEdges(Steps.ROUTE_AI, (state: ChatState) => {
      if (!state.intent) {
        return END;
      }
      if (state.intent === Intent.SUMMARY) {
        return Steps.SUMMARY_AI;
      }
      if (state.intent === Intent.HAHOW) {
        return Steps.HAHOW_A2A_AI;
      }
      return Steps.BACKGROUND_AI;
    })
    .addEdge(Steps.SUMMARY_AI, END)
    .addConditionalEdges(Steps.BACKGROUND_AI, (state: ChatState) => {
      return Steps.LEARNING_AI;
      if (state.background) {
        return Steps.LEARNING_AI;
      }
      return END;
    })
    .addEdge(Steps.LEARNING_AI, END);

  if (!this.checkpointSaver) {
    throw new Error("Checkpoint saver is not initialized");
  }
  return workflow.compile({
    checkpointer: this.checkpointSaver,
  });
}
```

## 🚀 小總結

這篇文章中我們已經讓我們的 AI 工具人變成了一個可以讓其它 Agent 來連的 A2A Server，並且也可以連到其它的 A2A Server 來增加我們這個工具人的功能。

不過先說好，這個基本上是最簡單的版本，如果還要進行到 Agent 與 Agent 的更多互動，可能就還需要花時調整了。

提外話，寫到這篇後真的發現用 LangGraph 來開發真的輕鬆不少，要加個 A2A 也很簡單，不過它真很多東西都還沒更新上 1.0.0 啊……

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10391363)

[

30-18: \[知識\] 可以讓 AI 工具人知道外面世界的工具 3 - A2A ( Agent2Agent )

](https://ithelp.ithome.com.tw/articles/10391363)[下一篇](https://ithelp.ithome.com.tw/articles/10392948)

[

30-20: \[知識\] Retrieval-Augmented Generation 之 核心概念與總覽

](https://ithelp.ithome.com.tw/articles/10392948)

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