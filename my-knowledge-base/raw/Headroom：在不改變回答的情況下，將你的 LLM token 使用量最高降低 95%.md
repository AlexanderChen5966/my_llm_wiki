---
title: "Headroom：在不改變回答的情況下，將你的 LLM token 使用量最高降低 95%"
source: "https://codelove.tw/@tony/post/xXYYg3"
author:
published:
created: 2026-06-29
description: "如果你正在建構 AI Agent 或在正式環境中執行 LLM 流程，你一定很熟悉這種痛苦：工具輸出、日誌、RAG 區塊，以及對話歷史會迅速堆積。你很快就會發現，Token 消耗速度快到讓你的帳單儀表板看了就不舒服。[Headroom](https://github.com/chopratejas..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月28日](https://codelove.tw/@tony/post/xXYYg3 "2026-06-28 09:15:07")

## Headroom：在不改變回答的情況下，將你的 LLM token 使用量最高降低 95%

如果你正在建構 AI Agent 或在正式環境中執行 LLM 流程，你一定很熟悉這種痛苦：工具輸出、日誌、RAG 區塊，以及對話歷史會迅速堆積。你很快就會發現，Token 消耗速度快到讓你的帳單儀表板看了就不舒服。

[Headroom](https://github.com/chopratejas/headroom) 是一個直接解決這個問題的開源專案。它會在 AI Agent 讀取任何內容之前，就先把所有內容壓縮；而且在真實工作負載上宣稱可減少 60–95% 的 Token，同時保留準確性。

---

## 核心概念

Headroom 位於你的應用程式與 LLM 供應商之間。它會接收 Agent 即將送出的內容——例如一串工具呼叫結果、很長的日誌檔，或是 RAG 檢索結果——並依照內容類型使用多種策略之一來壓縮：

- **SmartCrusher** ：處理 JSON（陣列、巢狀物件、混合型別）
- **CodeCompressor** ：針對 Python、JS、Go、Rust、Java 和 C++ 使用感知 AST 的壓縮
- **Kompress-base** ：一個基於 HuggingFace 的模型，訓練於 agentic traces，用於散文與純文字
- **CacheAligner** ：穩定 prompt 前綴，讓供應商的 KV cache 能真正穩定命中
- **CCR（Content-Compressed Retrieval，內容壓縮式檢索）** ：將原始內容儲存在本地，讓 LLM 可按需取回——因此壓縮是完全可逆的

**ContentRouter** 會判斷它正在處理的是哪一類內容，然後自動選擇正確的壓縮器。你不需要自己去思考這件事。

關鍵點是：原始內容永遠不會被刪除。如果 LLM 需要某段內容的完整版本，它可以取回來。從這個意義上說，壓縮是無損的。

---

## 實際資料

以下是該專案在真實 Agent 工作負載上做的基準測試 Token 數：

| 工作負載 | 壓縮前 | 壓縮後 | 節省 |
| --- | --- | --- | --- |
| 程式碼搜尋（100 筆結果） | 17,765 | 1,408 | 92% |
| SRE 事故除錯 | 65,694 | 5,118 | 92% |
| GitHub issue 分流 | 54,174 | 14,761 | 73% |
| 程式碼庫探索 | 78,502 | 41,254 | 47% |

在準確率基準測試（GSM8K 數學、TruthfulQA、SQuAD v2、BFCL 工具使用）上，壓縮後的分數維持穩定，甚至略有提升。其直覺是：移除雜訊有助於模型聚焦在訊號上。

你可以用以下指令自行重現：

```bash
python -m headroom.evals suite --tier 1
```

---

## 安裝方式：三種使用方法

Headroom 提供三種整合模式。你可以依照自己的工作方式選擇最適合的。

### 方案 1：包裝既有 Agent（零程式碼修改）

```bash
pip install "headroom-ai[all]"
headroom wrap claude
```

就這樣。Headroom 會自動攔截 Claude Code、Codex、Cursor、Aider 或 Copilot CLI 的流量。你完全不需要改既有程式碼。

### 方案 2：即插即用的 Proxy

將 Headroom 以本機 Proxy 的方式在任意埠上執行：

```bash
headroom proxy --port 8787
```

然後把你原本的 OpenAI/Anthropic SDK 呼叫指向 `localhost:8787` ，而不是供應商的 URL。任何語言、任何框架都可以——除了更新 base URL 之外，不需要其他程式碼修改。

### 方案 3：內嵌函式庫

若你想要更細緻的控制，可以直接在 Python 或 TypeScript 中使用：

**Python：**

```python
from headroom import compress

messages = [{"role": "user", "content": your_giant_tool_output}]
compressed = compress(messages, model="claude-opus-4-6")
# compressed 保持相同結構，但 Token 數少很多
```

**TypeScript：**

```typescript
import { compress } from "headroom-ai";

const compressed = await compress(messages, { model: "claude-opus-4-6" });
```

**直接搭配 Anthropic SDK：**

```python
from anthropic import Anthropic
from headroom import withHeadroom

client = withHeadroom(Anthropic())
# 用法和正常一樣；壓縮會自動發生
```

**搭配 LangChain：**

```python
from headroom.integrations.langchain import HeadroomChatModel

llm = HeadroomChatModel(your_existing_llm)
```

**搭配 Vercel AI SDK：**

```typescript
import { wrapLanguageModel } from "ai";
import { headroomMiddleware } from "headroom-ai";

const model = wrapLanguageModel({
  model: yourModel,
  middleware: headroomMiddleware(),
});
```

需要 Python 3.10+。Node/TypeScript： `npm install headroom-ai` 。

---

## MCP Server 模式

如果你使用 MCP 用戶端（例如 Claude Desktop 等），可以把 Headroom 安裝成 MCP Server：

```bash
headroom mcp install
```

這會提供三個 MCP 工具： `headroom_compress` 、 `headroom_retrieve` 和 `headroom_stats` 。你的 AI Agent 可以在工具呼叫迴圈中直接使用它們。

---

## 跨 Agent 記憶

有一個容易被低估的功能：跨 Agent 共用記憶。如果你同時執行 Claude 和 Codex，Headroom 可以提供它們一個共用的壓縮上下文儲存區，並自動去重。

```python
from headroom.memory import SharedContext

ctx = SharedContext()
ctx.put("current_task", task_description)

# 在另一個 Agent 的 session 中
task = ctx.get("current_task")
```

這在多 Agent 流程中特別有用，否則你往往得一再傳遞相同的上下文。

---

## headroom learn

還有一個 `headroom learn` 指令，可以從失敗的 Agent session 中挖掘資訊，並把修正寫回 `CLAUDE.md` 、 `AGENTS.md` 或 `GEMINI.md` 。其概念是讓你的 Agent 累積錯誤紀錄，避免重蹈覆轍。

```bash
headroom learn
```

它會解析 session 日誌、擷取失敗模式，並將結構化的學習結果附加到專案的 agent 設定檔中。

---

## 查看節省效果

使用 Headroom 一段時間後：

```bash
headroom stats
```

這會顯示累積的壓縮比、節省的 Token，以及依內容類型劃分的明細。

---

## 值得試嗎？

**如果你符合以下情況，值得一試：**

- 經常使用 AI 程式碼 Agent（Claude Code、Cursor、Codex、Aider），而且需要支付 Token 費用
- 你在建構的流程中，工具輸出與 RAG 區塊很大、而且重複性高
- 你想要跨 Agent 共用記憶，但不想自己從頭打造
- 你需要可逆壓縮——Headroom 不會丟棄原始內容

**如果你符合以下情況，建議先略過或謹慎評估：**

- 你只使用單一供應商內建的上下文管理，而且不需要更多功能
- 你在沙箱或受限環境中工作，執行本機程序會有問題
- 你的場景只是非常簡單的單輪互動，還沒有嚴重的上下文膨脹問題

---

## 快速參考

```bash
# 安裝
pip install "headroom-ai[all]"
npm install headroom-ai

# 包裝 Agent
headroom wrap claude

# 以 Proxy 執行
headroom proxy --port 8787

# 安裝為 MCP Server
headroom mcp install

# 查看節省效果
headroom stats

# 從失敗中學習
headroom learn
```

GitHub: [chopratejas/headroom](https://github.com/chopratejas/headroom)

---

原文出處： [https://dev.to/arshtechpro/headroom-cut-your-llm-token-usage-by-up-to-95-without-changing-your-answers-5g06](https://dev.to/arshtechpro/headroom-cut-your-llm-token-usage-by-up-to-95-without-changing-your-answers-5g06)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xXYYg3) [19瀏覽](https://codelove.tw/@tony/post/xXYYg3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xXYYg3)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝13 💬2 ❤️1

571

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝1 ❤️1

71

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1782697730&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxXYYg3&amp;pra=5&amp;asro=0&amp;dap=3&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1782697730752&amp;bpp=1&amp;bdt=84&amp;idt=31&amp;shv=r20260625&amp;mjsv=m202606240101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1782697603%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1782697603%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1782697603%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=8739309367755&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1605&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31099345%2C31099360&amp;oid=2&amp;pvsid=4098922558166162&amp;tmod=636781205&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1605%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=46" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260625/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>