---
title: n8n：用視覺化工作流重現 Supervisor × Multi-Agent 架構
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10396549
type: article
status: processed
asset_value: 62
---

LangChain 程式碼描述的 Supervisor × Multi-Agent 架構，搬到 n8n 的視覺化節點圖中會是什麼樣子？n8n 定位為「流程協作與執行層」，補完 LangChain 在「語意推理與 Agent 管理層」之外的自動化串接能力。

## T（核心論點）

n8n 是開源無程式碼/視覺化工作流自動化平台，與 LangChain 並非競品而是互補關係：LangChain 負責語意推理與 Agent 管理（思考層），n8n 負責流程協作與執行串接（行動層，串接 Gmail、Slack、資料庫等外部服務）。同一套 Supervisor × Multi-Agent 架構可以分別用程式碼（LangChain）或節點圖（n8n）實作。

## B（適用場景）

**適用：**
- 需要與大量外部服務（Email、Slack、CRM、資料庫）整合，n8n 內建數百個現成節點比手寫 API 整合快
- 團隊中有非工程背景成員需要參與、檢視或調整自動化流程
- 想快速驗證 Multi-Agent 架構原型，不想寫大量 boilerplate 程式碼

**不適用：**
- 需要複雜自訂邏輯、版本控制、單元測試的核心系統（程式碼仍是更可靠的選擇）
- 對延遲或效能有嚴格要求的場景（視覺化平台有額外開銷）

## R（實作模式）

### Docker 安裝

```bash
docker run -it --rm --name n8n -p 5678:5678 \
  -e GENERIC_TIMEZONE="Asia/Taipei" \
  -e TZ="Asia/Taipei" \
  -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true \
  -e N8N_RUNNERS_ENABLED=true \
  -v n8n_data:/home/node/.n8n \
  docker.n8n.io/n8nio/n8n
```

啟動後透過 `localhost:5678` 進入視覺化編輯介面。

### 架構對照：LangChain 程式碼 → n8n 節點

延續 [[supervisor-multi-agent-langchain]] 的 Tool Calling Pattern，在 n8n 中重新搭建：

| LangChain 概念 | n8n 對應節點 |
|----------------|-------------|
| 使用者輸入 | When Chat Message Received（觸發節點） |
| Supervisor Agent | Supervisor Agent 節點（使用 gemini-2.5-flash） |
| 子 Agent 包裝成 @tool | Call n8n Workflow Tool（呼叫子工作流） |
| WeatherAgent / PlannerAgent / AdvisorAgent | 各自獨立的 n8n 子工作流 |
| 子 Agent 入口 | When Executed by Another Workflow（子工作流觸發節點） |
| 最終輸出整合 | Send a Message in Gmail（寄送結果） |

### 架構流程

```
[Chat 輸入] → [Supervisor Agent (Gemini)]
                    ├─→ [Call Workflow: WeatherAgent 子工作流]
                    ├─→ [Call Workflow: PlannerAgent 子工作流]
                    └─→ [Call Workflow: AdvisorAgent 子工作流]
                              ↓
                    [Supervisor 整合回覆] → [Gmail 寄送]
```

每個子 Agent 是獨立的 n8n workflow，用「When Executed by Another Workflow」節點作為入口接收 Supervisor 傳入的參數，執行完畢回傳結果給 Supervisor 工作流。

## C（關鍵行動）

- [ ] 系統需要整合多個外部服務（Email、通知、資料庫）時，優先評估 n8n 現成節點，而非手寫 API 整合
- [ ] 把每個專家 Agent 拆成獨立 n8n 子工作流，用「Call n8n Workflow Tool」串接，對應 LangChain 的 @tool 包裝模式
- [ ] LangChain 與 n8n 不是二選一：複雜推理留給 LangChain/LangGraph，外部服務串接交給 n8n

## 相關連結

- [[supervisor-multi-agent-langchain]] — Day 24：本篇重新實作的原始 LangChain 架構
- [[multimodal-multi-agent-extension]] — Day 25：同系統的多模態擴充版本
- [[langchain-mcp-integration]] — MCP 與 LangChain 整合（另一種工具串接方式對照）
- [[synthesis-multi-agent]] — Multi-Agent 系統知識精粹（本主題 7 篇整合）
