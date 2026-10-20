---
title: A2A（Agent2Agent）協定：設計原則、Agent Card 與 TypeScript 實作
date: 2026-06-22
tags: [ai/agent]
source: https://ithelp.ithome.com.tw/articles/10391363
type: article
status: processed
asset_value: 65
---

A2A（Agent2Agent）是 Google 2025 年提出的開放協定，補充 MCP 處理 Agent 間溝通的需求：MCP 是 Agent 裝備工具的協定，A2A 是 Agent 呼叫其他 Agent 的協定。

## T（核心論點）

**A2A vs MCP 差別：**
- **MCP**：Agent 標準化裝備遠端 Tools 與取得 Context（MCP Server 可以沒有 AI）
- **A2A**：Agent 與 Agent 之間協作，保護各自的內部上下文

兩者互補，不是取代關係（Google 官方明確說明）。

## B（適用場景）

- 建立多 Agent 系統，讓不同服務的 Agent 可以互相呼叫
- 需要跨組織/跨廠商的 Agent 協作（每個 Agent 不暴露內部狀態）
- 長時間非同步任務（支援 Streaming + Push Notifications）

## R（完整內容）

### 五個設計原則

| 原則 | 說明 |
|------|------|
| Simplicity | 採用 HTTP/JSON-RPC/SSE 等現有技術 |
| Enterprise Readiness | 內建 Authentication/Authorization/Security/Privacy/Tracing |
| Asynchronous | 支援長時間任務，透過 Streaming 和 Push Notifications 通知 |
| Modality Independent | 允許語音等不同類型 Content 溝通 |
| Opaque Execution | Agent 不暴露內部上下文，保護知識智慧財產 |

### 協定結構

**Transport（三選一）：**
- JSON-RPC 2.0
- gRPC
- HTTP+JSON/REST

**Agent Card（每個 A2A Server 的名片）：**

放在 `https://{server}/.well-known/agent-card.json`，包含：
- `protocolVersion`、`name`、`description`、`url`、`version`
- `capabilities`：`streaming`、`pushNotifications`、`stateTransitionHistory`
- `skills`：定義這個 Agent 能做什麼（類似 MCP 的 tool）
- `preferredTransport`：告訴 client 用哪種 Transport

**Message vs Task：**
- `message/send`：一次性對話或發起任務
- `tasks/get`：查詢有狀態任務的進度
- `tasks/cancel`：取消任務
- 選填：`message/stream`（串流）、`tasks/pushNotificationConfig/set`（設定推播）

**上下文管理：**
用 `contextId` 識別同一件事情。每個 Agent 用 contextId 管理自己的上下文記憶，不會洩漏給其他 Agent。

### TypeScript 實作（`@a2a-js/sdk`）

#### A2A Server 核心結構

```typescript
import { AgentExecutor, RequestContext, ExecutionEventBus,
         DefaultRequestHandler, InMemoryTaskStore } from "@a2a-js/sdk/server";
import { A2AExpressApp } from "@a2a-js/sdk/server/express";

const agentCard: AgentCard = {
  name: "Hello Agent",
  protocolVersion: "0.3.0",
  url: "http://localhost:4000/",
  skills: [{ id: "chat", name: "Chat", description: "Say hello" }],
  capabilities: {},
  // ...
};

class ChatExecutor implements AgentExecutor {
  async execute(requestContext: RequestContext, eventBus: ExecutionEventBus) {
    // contextId 管理上下文（同一 contextId = 同一對話歷史）
    let chatWorkflow = sessionMap.get(requestContext.contextId);
    if (!chatWorkflow) {
      chatWorkflow = new ChatWorkflow();
      await chatWorkflow.initialize(requestContext.contextId);
      sessionMap.set(requestContext.contextId, chatWorkflow);
    }
    const message = requestContext.userMessage.parts[0]!.text;
    for await (const chunk of chatWorkflow.processMessage(message)) {
      eventBus.publish({ kind: "message", role: "agent",
                         parts: [{ kind: "text", text: chunk }],
                         contextId: requestContext.contextId, /* ... */ });
    }
    eventBus.finished();
  }
}

const appBuilder = new A2AExpressApp(new DefaultRequestHandler(
  agentCard, new InMemoryTaskStore(), new ChatExecutor()
));
```

#### A2A Client 呼叫另一個 Agent

```typescript
import { A2AClient } from "@a2a-js/sdk/client";

// 透過 Agent Card 發現對方
const client = await A2AClient.fromCardUrl(
  "http://other-agent:4001/.well-known/agent-card.json"
);

const response = await client.sendMessage({
  message: {
    messageId: uuidv4(),
    contextId: "test-context-id",  // 共享上下文 ID
    role: "user",
    parts: [{ kind: "text", text: "你好" }],
    kind: "message",
  },
});
```

#### LangGraph 整合模式

在 StateGraph 中加入 A2A Node：
```typescript
.addNode(Steps.EXTERNAL_A2A, async (state) => {
  const response = await externalA2AAgent.callLLM(state.query);
  return { messages: [new AIMessage(response)], ...state };
})
.addConditionalEdges(Steps.ROUTE_AI, (state) => {
  if (state.intent === Intent.EXTERNAL) return Steps.EXTERNAL_A2A;
  return Steps.LOCAL_AI;
})
```

### 三種溝通流程

1. **Agent Card 發現 + Hello**：Client 讀 Agent Card → `message/send`
2. **長時間任務（Push Notification）**：`message/send` → A2A Server 非同步做事 → 完成後推播通知
3. **長時間任務（輪詢）**：`message/send` 取得 taskId → 定期 `tasks/get` 查詢狀態

## C（關鍵行動）

- [ ] 評估系統中是否需要 Agent 間通訊（多 Agent 系統才需要 A2A）
- [ ] 用 `@a2a-js/sdk` 快速驗證：Server（AgentCard + AgentExecutor）+ Client（fromCardUrl）
- [ ] 結合 LangGraph StateGraph：把外部 A2A Agent 包裝成一個 Node

## 相關連結

- [[ai-skills-mcp-plugins-agents-comparison]] — MCP vs Skills vs Agent：A2A 是第五個概念
- [[react-multi-mcp-agent]] — ReAct × Multi-MCP：類似的多工具協調模式
- [[supervisor-multi-agent-langchain]] — LangGraph Supervisor 多 Agent 架構
