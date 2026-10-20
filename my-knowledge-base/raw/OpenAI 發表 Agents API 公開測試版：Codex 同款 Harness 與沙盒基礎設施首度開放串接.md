---
title: "OpenAI 發表 Agents API 公開測試版：Codex 同款 Harness 與沙盒基礎設施首度開放串接"
source: "https://www.aiposthub.com/openai-agents-api-public-beta/"
author:
  - "[[Philo]]"
published: 2026-09-11
created: 2026-09-15
description: "OpenAI 於 2026 年 9 月 10 日推出 Agents API 公開測試版，開發者可用單一 API 呼叫建立雲端代理，直接沿用 Codex 背後的 Harness 與沙盒基礎設施。本文解析架構設計、多代理協作、情境壓縮、程式化工具呼叫與九大沙盒合作夥伴。"
tags:
  - "clippings"
status: processed
---
過去一年，「打造一個能長時間自主運作的 AI 代理」聽起來簡單，做起來卻異常繁瑣。開發者得自己設計情境管理機制，避免對話超出模型的上下文視窗；得自己寫工具路由邏輯，讓代理在數十個工具中挑出正確的一個；還得自己維運一套沙盒環境，讓代理能安全地讀寫檔案、執行程式碼、跑上數小時甚至數天而不中斷。每一項都不難，但加總起來，往往是一個團隊在打造「產品」之前，得先重新發明一遍「打造代理所需的基礎設施」。

2026 年 9 月 10 日，OpenAI 官方部落格宣布推出 **Agents API 公開測試版（Public Beta）** ，直接把支撐 Codex 與 ChatGPT for Work 服務數百萬用戶的同一套 Harness（代理執行框架）與運行基礎設施，透過一支簡單、彈性的 API 開放給所有開發者串接。換句話說，開發者不需要再重造輪子，而是直接站上 OpenAI 自己拿來打造 Codex 的那套地基上，蓋自己的應用。

我的判斷是正面偏樂觀。Agents API 最大的價值不在於又一個「呼叫模型」的介面，而是把 OpenAI 內部驗證過的情境壓縮、工具搜尋、多代理協作等工程解法，直接以版本化的方式提供給外部開發者，且不額外收費、只按 Token 與工具用量計價，門檻明顯低於自建。但目前仍是公開測試版，Harness 行為會隨模型更新而演進，正式導入生產環境的團隊，仍需持續關注版本相容性與尚未成熟的邊界案例。

## Agents API 是什麼？一支 API 呼叫建立完整代理

在 OpenAI 的敘事裡，這次發表的核心邏輯很清楚：有用的代理，需要一個強大的 Harness 來管理情境、有效率地使用工具、協調子代理（Subagents）；同時也需要能讓代理連續運作數天、可讀寫檔案、可執行程式碼、可保存中間結果的基礎設施。過去這兩件事都得靠開發者自己拼裝，現在 OpenAI 把它們封裝進一支 API。

透過 Agents API，開發者只要在單一 API 呼叫中指定任務、模型、工具與執行環境，就能建立一個生產就緒（Production-ready）的代理：

```javascript
import OpenAI from "openai";

const client = new OpenAI();

const session = await client.beta.agents.sessions.create({
  agent: {
    model: "gpt-6-astra",
    tools: [
      {
        type: "mcp",
        server_label: "observability",
        transport: {
          type: "http",
          server_url: "https://observability.example.com/mcp",
        },
      },
    ],
    multi_agent: { enabled: true, max_concurrent_subagents: 3 },
  },
  vault_ids: ["vault_YOUR_VAULT_ID"],
  environment: {
    type: "openai_hosted",
    capability_directories: ["/workspace/capabilities/skills"],
  },
  input:
    "Investigate service-api's elevated 5xx rate over the last 30 minutes. " +
    "Delegate deployment, error, and dependency analysis to subagents. " +
    "Save findings, evidence, and recommended mitigation in /workspace/outputs.",
});
```

這段範例示範了一個典型場景：交代代理去調查某個服務過去 30 分鐘內 5xx 錯誤率異常升高的原因，並要求它把部署、錯誤、依賴關係三個面向的分析工作分派給子代理平行處理，最後把調查結果、證據與建議的緩解方案存進指定的工作目錄。整段邏輯只用一支 API 呼叫描述完畢，模型選用 `gpt-6-astra` ，並掛載一支透過 MCP（Model Context Protocol）串接的觀測工具。

OpenAI 負責代管與維護整套 Harness，但執行環境的選擇權在開發者手上：可以用 OpenAI 代管的沙盒、放進自己的基礎設施，或交給合作的沙盒服務商。這個設計的用意是讓開發者把心力放在真正讓自己代理與眾不同的地方——工具、知識庫與工作流程，而不是重新打造代理運行的地基。

## 選擇代理的執行環境：九大沙盒合作夥伴

![Agents API 沙盒合作夥伴：Modal、Cloudflare、Daytona、Blaxel、Runloop、Vercel、Oracle、E2B 與 DigitalOcean](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/openai-agents-api-sandbox-partners.png)

Agents API 提供與九大沙盒生態系服務商的一級整合。 圖片來源： OpenAI 官方公告 。

不同的工作負載，需要不同的運算、儲存與部署方式。Agents API 讓開發者可以挑選最符合應用需求的沙盒環境。OpenAI 表示，已與生態系中的多家服務商建立一級整合（First-class Integration），目前公布的九大合作夥伴為：

| 類別 | 合作夥伴 |
| --- | --- |
| 雲端運算與沙盒平台 | Modal、Daytona、Blaxel、Runloop、E2B |
| 邊緣與雲端基礎設施 | Cloudflare、DigitalOcean、Oracle |
| 前端與部署平台 | Vercel |

這些整合涵蓋的能力包括：

- **部署彈性** ：可選擇完全代管的環境，或部署在企業自有的 VPC（虛擬私有雲）內。
- **檔案與密鑰儲存機制** ：依據不同服務商，採用各自的檔案系統與機密（Secret）管理方式。
- **運算規格客製化** ：可依成本、冷啟動速度與效能需求，挑選不同的 CPU、GPU 與記憶體配置組合。

## OpenAI 代管沙盒：直接沿用 Codex 同款基礎設施

對於希望快速上手、又要能有效擴展的開發者，OpenAI 同步推出了「OpenAI 代管沙盒（OpenAI Hosted Sandbox）」選項。這項服務直接借用支撐 Codex 與 ChatGPT 背後的同一套沙盒基礎設施：由 OpenAI 負責建置與維運沙盒環境，讓代理擁有一個安全、高效能的空間來執行程式碼、操作檔案並產出成果。

這些代管沙盒可以彈性配置，載入開發者自訂的檔案、套件、Skills 與外掛，讓代理具備完成任務所需的一切資源，開發者不需要額外自建容器或維運底層運算資源。

## 隨模型演進的 Harness：三項近期強化重點

要跟上模型能力的每一次進步，往往意味著開發者得重新調整自己的 Harness，這會佔用原本該用來改善應用本身的時間。Agents API 的解法是：隨每一次模型發布，提供版本化的能力存取；OpenAI 會持續維護並優化這套 Harness，讓開發者的代理在每次升級後都能拿到效能提升，而不必自己重寫底層邏輯。以下是官方點出的三項近期強化：

### 1\. 情境管理：讓代理能撐過更長的工作階段

要支援模型連續工作數小時，OpenAI 建置了情境管理機制，協助代理在較長的工作階段（Session）中，持續保留關鍵資訊。當一次工作階段逼近情境上限（Context Limit）時，Agents API 會自動壓縮（Compact）較早的情境內容，同時保留代理後續作業所需的資訊。這代表開發者可以打造橫跨多個情境視窗的工作流程，而不必自行實作壓縮邏輯。

### 2\. 工具搜尋與程式化工具呼叫：更有效率地使用大量工具

Agents API 協助代理找到正確工具，並有效率地使用它們。「工具搜尋（Tool Search）」會依需求動態載入相關的工具定義，有助於降低 Token 消耗與成本，同時維持模型的快取效益。當工具就緒後，「程式化工具呼叫（Programmatic Tool Calling）」讓代理可以平行執行呼叫、串接相關操作，並直接在程式碼中篩選或彙整結果，如此一來，代理能處理大量資料，卻只把真正相關的結果帶回情境視窗中。Agents API 支援 MCP、自訂函式，以及網路搜尋等內建工具：

```json
"agent": {
  "tools": [
    {
      "type": "mcp",
      "server_label": "openai_docs",
      "transport": {
        "type": "http",
        "server_url": "https://developers.openai.com/mcp"
      }
    }
  ]
}
```

### 3\. 多代理協作：讓子代理平行拆解任務

透過多代理（Multi-agent）支援，Agents API 能把複雜任務拆解成獨立的子任務，交派給多個子代理平行處理。每個子代理維持自己獨立的情境，有助於專注在被指派的工作上；而主代理則負責協調各子代理的工作並彙整結果。官方指出，這項機制能加速研究、分析與程式撰寫等適合平行處理的任務，且不需要開發者自行打造協調邏輯：

```json
"agent": {
  "model": "gpt-6-astra",
  "multi_agent": {
    "enabled": true,
    "max_concurrent_subagents": 3
  }
}
```

## 官方示範影片：Harness 如何驅動實際代理任務

在發表公告中，OpenAI Developer Experience 工程師 Charlie Guo 透過一支約 2 分 18 秒的影片，說明 Agents API 背後的設計理念，以及它如何讓開發者直接沿用 Codex 的 Harness 打造自己的代理應用：

<iframe src="https://player.vimeo.com/video/1225684862?h=f6a80ae7b1&amp;badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" title="OpenAI Agents API 公開測試版官方介紹影片" width="1280" height="720" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write; encrypted-media; web-share" allowfullscreen=""></iframe>

OpenAI Developer Experience 工程師 Charlie Guo 介紹 Agents API 公開測試版。影片來源： [OpenAI 官方 Vimeo](https://vimeo.com/1225684862/f6a80ae7b1?ref=aiposthub.com) 。

## 開源基礎：Codex Harness 的核心邏輯完全公開

Agents API 背後運行的，是開源的 Codex Harness，這讓開發者能直接檢視協調模型呼叫、工具與情境管理的核心邏輯。在這個架構下，OpenAI 負責操作與維護這套 Harness，開發者則可以自由檢視、學習其公開的程式碼庫，理解代理在背後究竟是怎麼「思考」與「行動」的——這與許多「黑盒」代理框架的做法明顯不同。

## 客戶怎麼說：從評測分數到延遲表現的實測回饋

在官方發布頁面上，包括 Ciridae、Long Lake、WithCoverage、SafetyKit、Dwelly、Hypha、deepsense.ai、Nash.ai 在內的多家企業分享了導入經驗。其中 Ciridae 技術長 Jack Weissenberger 的說法頗具代表性：

> 「透過 Agents API，我們的評測分數從 0.71 提升到 0.85。API 中的子代理支援非常出色，大幅加快了我們的工作流程。過去在舊架構中觀測與協調子代理相當繁瑣，但新的 API 讓我們的延遲降低了 4 倍。我們花了很長時間試圖優化這個問題，而子代理工作流程本身就帶來了巨大的開箱即用提升。」

這類回饋呼應了 Agents API 的核心賣點：多代理協調與情境管理這類「工程硬骨頭」，一旦被框架層直接解決，團隊就能把心力重新聚焦在評測分數、產品邏輯與使用者體驗上。

## 定價與開始使用方式

Agents API 目前以公開測試版形式，向所有開發者開放。官方明確表示，使用 Agents API **沒有額外收費** ——開發者只需依照官方定價頁面公布的費率，為代理實際使用的 Token 與工具用量付費，不會因為改用 Agents API 而產生額外的框架使用費。

開發者可以透過 Agents API 總覽（Overview）文件深入了解架構設計，或直接依照快速入門指南（Quickstart）操作，將支撐 Codex 的 Harness 引入自己的代理應用中。OpenAI 也表示，在公開測試階段，團隊會根據開發者的實際回饋快速迭代，並持續朝正式全面開放（General Availability）的方向推進；官方也鼓勵開發者回報哪些地方運作良好、哪些地方仍卡關，以及在正式將代理送上生產環境時，還需要哪些能力。

## 對開發者與企業團隊的實際意義

Agents API 的發表，某種程度上反映了 OpenAI 對「代理基礎設施」這個市場區塊的判斷：與其讓每個開發者各自摸索情境管理、工具路由與多代理協調的最佳實務，不如把 OpenAI 自己在 Codex 與 ChatGPT for Work 上驗證過的解法直接產品化。

對正在評估是否要投入資源自建代理框架的團隊來說，這意味著幾個實際的考量點：

- **降低前期工程投入** ：情境壓縮、工具搜尋、多代理協調等原本需要團隊自行設計與維運的能力，現在可以直接透過設定參數啟用。
- **執行環境保有彈性** ：無論是完全交給 OpenAI 代管、部署在自有 VPC，或串接第三方沙盒服務商，都不需要更換底層的 Harness 邏輯。
- **仍需留意測試版風險** ：公開測試版意味著行為可能隨著模型與 Harness 更新而調整，將關鍵業務流程遷移至 Agents API 前，建議先在非核心場景驗證穩定性與延遲表現。

隨著愈來愈多企業開始把「代理」而非「單次問答」當作 AI 導入的核心單位，Agents API 這類把底層工程能力產品化的動作，很可能成為接下來一年 AI 基礎設施競爭的重要戰場。