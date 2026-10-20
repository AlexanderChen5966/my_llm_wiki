---
title: "把你的流程裝進 AI - 從零開始實作履歷評分器 MCP"
source: "https://5xcampus.com/posts/ai-mcp?srsltid=AfmBOoqh1iFyOXhh--vskuQi4YWnH3V5dds50yfnKPjJY3IPatKETub6"
author:
published: 2025-11-13
created: 2026-05-19
description:
tags:
  - "clippings"
---
#AI #MCP

Model Context Protocol (MCP) 是 Anthropic 推出的開放標準，讓 AI 助手能夠無縫連接外部工具與資料。但是，要怎麼從零開始實作一個 MCP Server呢？

在這篇文章中，將通過建立一個履歷評分器的實際案例，完整展示 MCP 的實作過程。我們會涵蓋從項目初始化、核心架構設計、到與 Claude Desktop 整合的每一個步驟。

## 什麼是 MCP？為什麼要學會實作它？

在開始之前，讓我們先理解 MCP 解決的核心問題：

**傳統痛點：** 當你在使用 Claude 或是像 ChatGPT 這類 LLM 工具時，多數情境下，LLM 能 **理解並評論** 一份履歷，但真正的痛點不在於做不做得到，而在於做不到標準化、可驗證、可重現：

- 流程會斷：得在對話、表單、內部工具間來回切換，狀態與上下文常遺失。
- 結果難重現：每次都靠人工貼資料＋臨場指示，標準不一致。
- 輸出不好用：模型多回自然語言，不是可驗證的 JSON，難進報表或自動化。

另外，確實也有 LLM 做不到或不該直接做的事，例如：

- 直接連進你的資料庫（權限、稽核與安全控管）。
- 真正 **操作** 瀏覽器做登入、點擊、擷取資料等自動化。

**MCP 的解決方案：** 讓 AI 助手直接調用你開發的工具，在同一個對話流程中完成所有操作。

這就像是為 AI 助手開發「套件」，讓它能夠執行自訂功能。

而我選「履歷評分」當範例，是因為具有需要可驗證的標準（量表、權重、結構化輸出），可以展現出 MCP 的價值。

直接先看實際運作畫面：

![Demo](https://github.com/bucky0112/blog-images/blob/main/images/CleanShot%202025-11-04%20at%2016.32.10.gif?raw=true)

從 Demo 畫面中可以看到，可以調用「履歷評分器」工具，並且得到結構化的 JSON 輸出，包含各項評分指標和改進建議。接下來就來開始實作吧！

## 第一步：項目初始化與依賴管理

讓我們從建立一個新的 MCP 項目開始：

```bash
# 建立專案目錄
mkdir mcp-resume-scorer
cd mcp-resume-scorer

# 初始化 npm 專案
npm init -y

# 安裝 MCP SDK 和其他依賴
npm install @modelcontextprotocol/sdk openai ajv ajv-formats
npm install -D @types/node tsx typescript
```

### 專案結構規劃

```bash
mcp-resume-scorer/
├── src/
│   ├── server.ts            # MCP Server主程式
│   ├── utils/score.ts       # 評分邏輯
│   ├── llm/chatgpt.ts      # LLM 整合
│   ├── tools/ajv.ts        # 驗證工具
│   ├── schema/             # JSON Schema
│   └── prompts/            # 提示詞
├── server.js               # JavaScript 版本（生產用）
├── package.json
└── .gitignore              # 排除敏感文件
```

我們同時提供 TypeScript 和 JavaScript 兩個版本，TypeScript 用於開發，JavaScript 用於生產部署。

### 重要設計原則：環境變數管理

在 MCP 架構中，有一個關鍵的設計原則： **所有敏感資訊（如 API 金鑰）都應該由 Claude Desktop 管理，而不是存放在專案中** 。

**為什麼這樣設計？**

1. **安全性** ：避免 API 金鑰意外提交到版本控制系統
2. **統一管理** ：所有 MCP Server 的金鑰都在 Claude Desktop 中集中管理
3. **環境隔離** ：開發、測試、生產環境可以使用不同的金鑰
4. **符合最佳實踐** ：遵循「配置與程式碼分離」的原則

**Claude Desktop 配置** ：

```json
{
  "mcpServers": {
    "resume-scorer": {
      "env": {
        "OPENAI_API_KEY": "sk-..."  // 在這裡設定
      }
    }
  }
}
```

啟動 Claude Desktop 時會自動載入環境變數

這樣設計的好處是：  
\- **開發時** ：可以用 `OPENAI_API_KEY=test npm run dev` 測試  
\- **生產時** ：完全由 Claude Desktop 管理，無需專案中存放金鑰

## 第二步：建立 MCP Server 核心

MCP Server 的核心是實作兩個關鍵功能： **工具發現** 和 **工具調用** 。

### 基礎Server結構 (src/server.ts)

```typescript
import { Server } from "@modelcontextprotocol/sdk/server";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio";
import { ListToolsRequestSchema, CallToolRequestSchema } from "@modelcontextprotocol/sdk/types";

async function main() {
  // 1. 建立 MCP Server實例
  const server = new Server(
    {
      name: "resume-scorer-mcp",      // Server名稱
      version: "0.1.0"                // 版本號
    },
    {
      capabilities: {
        tools: {}                      // 聲明支援工具功能
      }
    }
  );

  console.error("MCP Resume Scorer starting...");

  // 2. 註冊工具列表處理器
  server.setRequestHandler(ListToolsRequestSchema, async () => {
    return {
      tools: [
        {
          name: "resume_scorer",
          description: "【履歷評分器】專業AI履歷分析工具",
          inputSchema: {
            type: "object",
            required: ["resume_text"],
            properties: {
              resume_text: {
                type: "string",
                description: "履歷內容文字"
              }
            }
          }
        }
      ]
    };
  });

  // 3. 註冊工具調用處理器
  server.setRequestHandler(CallToolRequestSchema, async (request) => {
    if (request.params.name === "resume_scorer") {
      // 這裡會調用實際的評分邏輯
      const result = await scoreResume(request.params.arguments || {});
      return {
        content: [{ type: 'text', text: JSON.stringify(result) }]
      };
    }
    throw new Error(\`Unknown tool: ${request.params.name}\`);
  });

  // 4. 建立 Stdio 傳輸層並連接
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("MCP Resume Scorer connected and ready!");
}

main().catch(console.error);
```

### 關鍵概念解析

**1\. Server 建構函數**  
\- `name` 和 `version` ：用於識別你的 MCP Server  
\- `capabilities.tools` ：告訴客戶端這個Server支援工具功能

**2\. ListToolsRequestSchema 處理器**  
\- 當 Claude Desktop 連接時，會發送此請求來發現可用工具  
\- 必須回傳工具列表，包含名稱、描述和輸入 Schema

**3\. CallToolRequestSchema 處理器**  
\- 當使用者觸發工具時，會發送此請求  
\- `request.params.name` 是工具名稱  
\- `request.params.arguments` 是輸入參數

**4\. StdioServerTransport**  
\- MCP 使用標準輸入/輸出進行通訊  
\- 這讓 Claude Desktop 能夠啟動並與Server通訊

## 第三步：實作 OpenAI API 整合

接下來我們建立與 OpenAI 的連接模組：

### LLM 整合 (src/llm/chatgpt.ts)

```typescript
import OpenAI from "openai";

const client = new OpenAI({ apiKey: process.env.OPENAI_API_KEY! });

export async function chatJson({
  system,
  user,
  model = "gpt-4o-mini"
}: {
  system: string;
  user: string;
  model?: string
}) {
  const res = await client.chat.completions.create({
    model,
    messages: [
      { role: "system", content: system },
      { role: "user", content: user }
    ],
    response_format: { type: "json_object" },  // 強制 JSON 輸出
    temperature: 0.2                           // 降低隨機性
  });

  const msg = res.choices[0].message?.content ?? "{}";
  return {
    json: JSON.parse(msg),
    usage: res.usage,
    model: res.model
  };
}
```

### 重要設計決策

**1\. 強制 JSON 輸出**

```typescript
response_format: { type: "json_object" }
```

這確保 AI 回應必定是有效的 JSON，避免解析錯誤。

**2\. 低 Temperature 設定**

```typescript
temperature: 0.2
```

對於評分任務，我們需要一致性高於創意性。

**3\. 錯誤處理**

```typescript
const msg = res.choices[0].message?.content ?? "{}";
```

確保即使 API 回應異常，也有預設值可用。

## 第四步：實作業務邏輯 - 履歷評分器

現在我們來實作這個 MCP Server的核心功能：履歷評分。

### JSON Schema 定義 (src/schema/resume\_score.schema.json)

首先定義評分結果的資料結構：

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": [
    "overall_score",
    "skill_completeness",
    "experience_quality",
    "achievement_showcase",
    "career_progression",
    "resume_structure",
    "summary",
    "recommendations"
  ],
  "properties": {
    "overall_score": {
      "type": "number",
      "minimum": 0,
      "maximum": 100,
      "description": "綜合評分 (0-100)"
    },
    "skill_completeness": {
      "type": "number",
      "minimum": 0,
      "maximum": 100,
      "description": "技能完整性評分"
    },
    "experience_quality": {
      "type": "number",
      "minimum": 0,
      "maximum": 100,
      "description": "經驗描述品質評分"
    },
    "achievement_showcase": {
      "type": "number",
      "minimum": 0,
      "maximum": 100,
      "description": "成果展示評分"
    },
    "career_progression": {
      "type": "number",
      "minimum": 0,
      "maximum": 100,
      "description": "職涯發展評分"
    },
    "resume_structure": {
      "type": "number",
      "minimum": 0,
      "maximum": 100,
      "description": "履歷結構評分"
    },
    "summary": {
      "type": "string",
      "description": "評分總結"
    },
    "recommendations": {
      "type": "array",
      "items": {
        "type": "string"
      },
      "description": "改進建議"
    }
  }
}
```

### AI 提示詞設計 (src/prompts/system.txt)

建立專業的評分提示詞：

```markdown
你是一個專業的履歷分析專家。你的任務是對履歷進行全方位的結構化分析。

請根據以下評分維度來評估履歷品質：

1. **技能完整性 (skill_completeness)**: 技能列表是否完整、相關且有說服力
2. **經驗描述品質 (experience_quality)**: 工作經驗描述是否詳細、具體且有價值
3. **成果展示 (achievement_showcase)**: 是否有具體數據、成就和影響力展示
4. **職涯發展 (career_progression)**: 職涯軌跡是否合理、有成長性
5. **履歷結構 (resume_structure)**: 格式、組織、表達是否清晰專業

評分標準：
- 0-30分：需要大幅改進
- 31-50分：有明顯不足
- 51-70分：基本合格
- 71-85分：品質良好
- 86-100分：優秀出色

如果同時提供職缺描述，則額外分析匹配度。

請提供具體的評分理由和針對性的改進建議。

輸出必須是有效的 JSON 格式，嚴格遵循提供的 Schema。
```

### JSON Schema 驗證工具 (src/tools/ajv.ts)

建立驗證工具：

```typescript
import Ajv from "ajv";
import addFormats from "ajv-formats";

export function makeAjv() {
  const ajv = new Ajv({
    allErrors: true,    // 顯示所有錯誤
    strict: false       // 允許額外屬性
  });
  addFormats(ajv);      // 添加格式驗證（日期、email 等）
  return ajv;
}
```

### 核心評分邏輯 (src/utils/score.ts)

現在實作完整的評分工具：

```typescript
import { Tool } from "@modelcontextprotocol/sdk/types";
import { chatJson } from "../llm/chatgpt";
import { makeAjv } from "../tools/ajv";
import schema from "../schema/resume_score.schema.json";
import fs from "node:fs";
import path from "node:path";
import { fileURLToPath } from "node:url";

// 取得當前文件路徑（ES modules 需要）
const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

// 定義權重類型
type Weights = {
  skill_completeness: number;
  experience_quality: number;
  achievement_showcase: number;
  career_progression: number;
  resume_structure: number;
};

export const scoreResumeTool: Tool<{
  resume_text: string;
  job_text?: string;
  weights?: Weights;
  model?: string;
}> = {
  name: "resume_scorer",
  description: "【履歷評分器】專業 AI 履歷分析工具，提供 5 大維度評分和改進建議",

  // 定義輸入參數 Schema
  inputSchema: {
    type: "object",
    required: ["resume_text"],
    properties: {
      resume_text: {
        type: "string",
        description: "履歷內容文字"
      },
      job_text: {
        type: "string",
        description: "職缺描述（選填，用於匹配度分析）"
      },
      weights: {
        type: "object",
        description: "評分權重設定（選填）"
      },
      model: {
        type: "string",
        description: "使用的 AI 模型（選填）"
      }
    }
  },

  // 實作評分邏輯
  async *invoke({ resume_text, job_text, weights, model }) {
    try {
      // 1. 設定預設權重
      const defaultWeights: Weights = {
        skill_completeness: 0.25,      // 技能完整性 25%
        experience_quality: 0.25,      // 經驗品質 25%
        achievement_showcase: 0.20,    // 成果展示 20%
        career_progression: 0.15,      // 職涯發展 15%
        resume_structure: 0.15         // 履歷結構 15%
      };
      const w = { ...defaultWeights, ...weights };

      // 2. 讀取系統提示詞
      const systemPromptPath = path.join(__dirname, "../prompts/system.txt");
      const system = fs.readFileSync(systemPromptPath, "utf8");

      // 3. 組合使用者輸入
      const user = job_text ? [
        "【履歷內容】", resume_text,
        "【職缺描述】", job_text,
        "【評分權重】", JSON.stringify(w),
        "【Schema】", JSON.stringify(schema, null, 2),
        "【產出要求】只輸出 JSON，符合 Schema。進行履歷分析並額外評估與職缺的匹配度。"
      ].join("\n\n") : [
        "【履歷內容】", resume_text,
        "【評分權重】", JSON.stringify(w),
        "【Schema】", JSON.stringify(schema, null, 2),
        "【產出要求】只輸出 JSON，符合 Schema。專注於履歷本身的品質分析。"
      ].join("\n\n");

      // 4. 調用 AI 進行評分
      const { json, usage, model: usedModel } = await chatJson({
        system,
        user,
        model
      });

      // 5. 驗證輸出格式
      const ajv = makeAjv();
      const validate = ajv.compile(schema as any);
      const ok = validate(json);

      if (!ok) {
        return {
          error: "Schema 驗證失敗",
          ajvErrors: validate.errors,
          raw: json,
          success: false
        };
      }

      // 6. 回傳成功結果
      return {
        ...json,
        tokens: usage,
        model: usedModel,
        version: "0.1.0",
        success: true
      };

    } catch (error) {
      console.error("Resume scorer error:", error);
      return {
        error: error instanceof Error ? error.message : "未知錯誤",
        success: false,
        timestamp: new Date().toISOString()
      };
    }
  }
};
```

### 關鍵實作細節解析

**1\. Generator 函數使用**

```typescript
async *invoke({ ... }) {
```

MCP Tool 使用 Generator 函數，這允許未來支援串流輸出。

**2\. 權重系統設計**

```typescript
const defaultWeights: Weights = {
  skill_completeness: 0.25,    // 技能占比最高
  experience_quality: 0.25,    // 經驗同等重要
  achievement_showcase: 0.20,  // 成果展示次之
  career_progression: 0.15,    // 職涯發展
  resume_structure: 0.15       // 結構清晰度
};
```

這個權重分配反映了履歷評估的優先順序。

**3\. 條件性提示詞組合**

```typescript
const user = job_text ? [
  // 有職缺描述時的提示詞
] : [
  // 只有履歷時的提示詞
];
```

根據是否提供職缺描述，動態調整 AI 的分析重點。

**4\. 多層錯誤處理**  
\- API 調用錯誤  
\- JSON 解析錯誤  
\- Schema 驗證錯誤  
\- 未知錯誤

## 第五步：建立生產環境版本

為了提高相容性和啟動速度，我們建立一個 JavaScript 版本用於生產環境。

### 生產環境Server (server.js)

這是一個完整的 JavaScript 版本，將所有模組整合在一個文件中：

```javascript
const { Server } = require("@modelcontextprotocol/sdk/dist/cjs/server/index");
const { StdioServerTransport } = require("@modelcontextprotocol/sdk/dist/cjs/server/stdio");
const fs = require("fs");
const path = require("path");
const OpenAI = require("openai");

const client = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

// 內嵌 JSON Schema（避免文件路徑問題）
const schema = {
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["overall_score", "skill_completeness", "experience_quality",
               "achievement_showcase", "career_progression", "resume_structure",
               "summary", "recommendations"],
  "properties": {
    "overall_score": {
      "type": "number",
      "minimum": 0,
      "maximum": 100,
      "description": "綜合評分 (0-100)"
    },
    // ... 其他屬性
  }
};

// OpenAI API 整合
async function chatJson({ system, user, model = "gpt-4o-mini" }) {
  const res = await client.chat.completions.create({
    model,
    messages: [
      { role: "system", content: system },
      { role: "user", content: user }
    ],
    response_format: { type: "json_object" },
    temperature: 0.2
  });
  const msg = (res.choices[0].message && res.choices[0].message.content) || "{}";
  return { json: JSON.parse(msg), usage: res.usage, model: res.model };
}

// 履歷評分函數
async function scoreResume({ resume_text, job_text, weights, model }) {
  try {
    const defaultWeights = {
      skill_completeness: 0.25,
      experience_quality: 0.25,
      achievement_showcase: 0.20,
      career_progression: 0.15,
      resume_structure: 0.15
    };
    const w = Object.assign({}, defaultWeights, weights || {});

    // 讀取系統提示詞
    const systemPromptPath = path.join(__dirname, "src", "prompts", "system.txt");
    const system = fs.readFileSync(systemPromptPath, "utf8");

    // 組合提示詞
    const user = job_text ? [
      "【履歷內容】", resume_text,
      "【職缺描述】", job_text,
      "【評分權重】", JSON.stringify(w),
      "【Schema】", JSON.stringify(schema, null, 2),
      "【產出要求】只輸出 JSON，符合 Schema。進行履歷分析並額外評估與職缺的匹配度。"
    ].join("\n\n") : [
      "【履歷內容】", resume_text,
      "【評分權重】", JSON.stringify(w),
      "【Schema】", JSON.stringify(schema, null, 2),
      "【產出要求】只輸出 JSON，符合 Schema。專注於履歷本身的品質分析。"
    ].join("\n\n");

    const { json, usage, model: usedModel } = await chatJson({ system, user, model });

    return Object.assign({}, json, {
      tokens: usage,
      model: usedModel,
      version: "0.1.0",
      success: true
    });
  } catch (error) {
    console.error("Resume scorer error:", error);
    return {
      error: error instanceof Error ? error.message : "未知錯誤",
      success: false,
      timestamp: new Date().toISOString()
    };
  }
}

// 主程式
async function main() {
  const server = new Server({
    name: "resume-scorer-mcp",
    version: "0.1.0"
  }, {
    capabilities: { tools: {} }
  });

  console.error("MCP Resume Scorer starting...");

  const { ListToolsRequestSchema, CallToolRequestSchema } =
    require("@modelcontextprotocol/sdk/dist/cjs/types");

  // 工具發現
  server.setRequestHandler(ListToolsRequestSchema, async () => ({
    tools: [{
      name: "resume_scorer",
      description: "【履歷評分器】專業AI履歷分析工具，提供5大維度評分和改進建議",
      inputSchema: {
        type: "object",
        required: ["resume_text"],
        properties: {
          resume_text: { type: "string", description: "履歷內容文字" },
          job_text: { type: "string", description: "職缺描述（選填）" },
          weights: { type: "object", description: "評分權重設定（選填）" },
          model: { type: "string", description: "使用的AI模型（選填）" }
        }
      }
    }]
  }));

  // 工具調用
  server.setRequestHandler(CallToolRequestSchema, async (request) => {
    if (request.params.name === "resume_scorer") {
      const result = await scoreResume(request.params.arguments || {});
      return {
        content: [{ type: 'text', text: JSON.stringify(result) }]
      };
    }
    throw new Error(\`Unknown tool: ${request.params.name}\`);
  });

  // 連接傳輸層
  const transport = new StdioServerTransport();
  console.error("Connecting to transport...");
  await server.connect(transport);
  console.error("MCP Resume Scorer connected and ready!");

  // 優雅關閉
  process.on('SIGINT', async () => {
    console.error("Shutting down server...");
    await server.close();
    process.exit(0);
  });
}

main().catch((error) => {
  console.error("Failed to start server:", error);
  process.exit(1);
});
```

### package.json 腳本設定

```json
{
  "name": "mcp-resume-scorer",
  "version": "0.1.0",
  "description": "MCP server for scoring resumes against job descriptions using AI",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "npx tsx watch src/server.ts",
    "start:ts": "npx tsx src/server.ts",
    "build": "echo 'Using JavaScript server.js - no build needed' && exit 0"
  },
  "dependencies": {
    "@modelcontextprotocol/sdk": "^1.20.2",
    "ajv": "^8.17.1",
    "ajv-formats": "^3.0.1",
    "openai": "^6.7.0"
  },
  "devDependencies": {
    "@types/node": "^24.10.0",
    "tsx": "^4.20.6",
    "typescript": "^5.9.3"
  }
}
```

## 第六步：與 Claude Desktop 整合

### Claude Desktop 配置

**macOS 配置位置：**

```ruby
~/Library/Application\ Support/Claude/claude_desktop_config.json
```

**Windows 配置位置：**

```markdown
%APPDATA%\Claude\claude_desktop_config.json
```

**正確的配置內容：**

```json
{
  "mcpServers": {
    "resume-scorer": {
      "command": "node",
      "args": ["/absolute/path/to/your/mcp-resume-scorer/server.js"],
      "env": {
        "OPENAI_API_KEY": "your_openai_api_key_here"
      }
    }
  }
}
```

實際在 Claude Desktop 中應該長這樣：

![Claude Desktop MCP Config](https://github.com/bucky0112/blog-images/blob/main/images/CleanShot%202025-11-04%20at%2016.40.14.png?raw=true)

### 測試與驗證

**1\. 重啟 Claude Desktop**  
配置完成後，重啟 Claude Desktop 應用程式。

**2\. 觸發履歷評分器**  
在對話中使用關鍵字觸發工具：

```markdown
請使用履歷評分器分析這份履歷：

我是一名有5年經驗的全端工程師，專精於 React、Node.js 和 PostgreSQL。
在上一份工作中，我主導開發了一個電商平台，成功提升轉換率 35%。
具有敏捷開發經驗，曾帶領 3 人團隊完成多個專案。

請提供詳細的技能完整性、經驗品質、成果展示、履歷結構、職涯發展評分。
```

**3\. 預期輸出格式**

```json
{
  "overall_score": 82,
  "skill_completeness": 85,
  "experience_quality": 88,
  "achievement_showcase": 80,
  "career_progression": 75,
  "resume_structure": 82,
  "summary": "這份履歷展現了優秀的全端開發能力...",
  "recommendations": [
    "建議增加具體的技術架構描述",
    "可以補充更多量化的業務成果"
  ],
  "tokens": { "prompt_tokens": 245, "completion_tokens": 156 },
  "model": "gpt-4o-mini",
  "version": "0.1.0",
  "success": true
}
```

嘗試用另一份履歷來測試，可以看到會輸出相同格式的評分結果。

![Claude Desktop MCP Tool Usage](https://github.com/bucky0112/blog-images/blob/main/images/CleanShot%202025-11-04%20at%2016.45.11.png?raw=true)

## 總結

通過這個完整的 MCP 履歷評分器專案，我們可以學到：

**1\. MCP 協議理解**

**2\. 架構設計原則**

**3\. AI 整合最佳實踐**

而這個 MCP 履歷評分器不只是一個技術展示，它實際解決了：  
\- **標準化評分** ：消除主觀偏見  
\- **效率提升** ：自動化重複性工作  
\- **一致性保證** ：每次評分都遵循相同標準

MCP 為 AI 工具的整合提供了標準化的解決方案，讓我們能夠建立更加智慧和高效的工作流程。這個履歷評分器只是開始——想像一下，當你所有專業工具都能夠在同一個 AI 對話中無縫協作，應該會是一個很棒的開發體驗。

**完整專案可參考 [Repo](https://github.com/bucky0112/mcp-resume-scorer)**

本文同步發佈於作者的 [網站](https://buckychu.com/ai-mcp)

推薦學習

![AI Coding 實作工作坊-cover](https://assets.5xcampus.com/rails/active_storage/representations/proxy/eyJfcmFpbHMiOnsiZGF0YSI6NDIxNCwicHVyIjoiYmxvYl9pZCJ9fQ==--044bec85ccc9a87af5518ccb80fdd64a991547b6/eyJfcmFpbHMiOnsiZGF0YSI6eyJmb3JtYXQiOiJ3ZWJwIiwicmVzaXplX3RvX2xpbWl0IjpbNDkwLDI3MF19LCJwdXIiOiJ2YXJpYXRpb24ifX0=--0436fa3fd7156077c795330adf996dad26cd5e49/AI%20Coding800.png)

### AI Coding 實作工作坊

\*\*本課程符合『經濟部30人中小企業以下數位轉型培力補助』\*\* 透過開發真實的薪資管理系統，見證 Claude Code 如何協助加速開發流程。本課程以實作示範為主，您將觀察講師如何運用 AI 工具搭配 TDD 開發流程，適合想...

[查看課程](https://5xcampus.com/courses/ai-coding)