---
title: Agent 時代的 Prompt 寫法：Context / Request / Output Format / Constraints / Checkpoint
date: 2026-07-09
tags: [ai/prompt-engineering, type/how-to]
source: https://tools.wingzero.tw/article/sn/3970
type: article
status: processed
asset_value: 58
---

疑似 Anthropic 內部用來最佳化 Fable 5 的 Prompt 結構，只有五個部分。核心轉向：模型越強，Prompt 重點從「教模型怎麼思考」變成「把任務目標、邊界與交付標準講清楚」。

## T（核心論點）

Prompt 不再只是「指令文字」，而是一份簡化版的**任務規格書**。早期模型能力不穩定時，大量文字控制模型思考步驟確實有幫助；但模型越強，越不需要用文字去控制它，真正重要的是有沒有把任務本身講清楚。

## B（適用場景）

寫給 Fable 5 / Claude / Codex 等能力強的模型執行的複雜任務／多步驟工作流程；判斷 Agent 何時該停下來問你、何時該自主完成。

## R（五個部分）

| 部分 | 內容 |
|------|------|
| **Context** | 任務發生的情境、目前狀態、相關背景 |
| **Request** | 明確描述要模型做什麼，不是模糊方向 |
| **Output Format** | 輸出形式：條列、表格、JSON、文章、程式碼 |
| **Constraints** | 哪些事不能假設、不能越界、不能改動、必須遵守什麼條件 |
| **Checkpoint** | 什麼情況下必須暫停詢問，而非小問題就反覆詢問 |

**Checkpoint 是最容易被忽略的關鍵**：常見兩種失敗——模型太常停下來問（推進慢）、模型完全不問直接執行高風險操作（改壞檔案/刪錯資料/超出範圍）。**只有三種情況該暫停**：
1. 涉及不可逆操作（刪除資料、覆蓋檔案、送出正式訊息、部署正式環境）
2. 任務範圍發生變化（原本修錯誤變成要重構整個架構）
3. 需要使用者提供必要資訊（資料來源、帳號、API Key、品牌語氣、目標受眾）

除此之外模型應盡量主動完成任務，最後整理結果回報。

## C（關鍵行動）

- [ ] 寫複雜任務 Prompt 前，逐項檢查五部分是否都清楚
- [ ] Checkpoint 只設在三種不可逆/範圍變化/缺資訊情境，其餘讓模型主動推進
- [ ] Prompt 不需要寫得長或花俏，只需要精準

## 補充資料：OpenAI Academy 六欄位對照（2026-09-23 併入）

來源：〈ChatGPT 指令大全：99 個實用提示詞範本〉（https://www.aiposthub.com/chatgpt-99-prompts-guide/，2026-09-21），轉述 OpenAI Academy〈Power Prompting〉（2026-07-07 影片）的六欄位：角色／任務／背景／輸出／限制／檢查。表面上跟本條目的五欄幾乎同構，但對照後有兩處真正的差異：

| 差異 | 本條目五欄 | OpenAI 六欄位 |
|---|---|---|
| 多一欄 | 無 | **角色**：以什麼專業角度協助 |
| 「檢查」≠「Checkpoint」 | **Checkpoint＝執行中**何時暫停詢問（不可逆操作、範圍變化、缺資訊） | **檢查＝交付前**自查：完成後要檢查什麼、不確定處要標示 |

兩者互補，不能互相替代：Checkpoint 管過程中的越界，檢查管交付物的品質。另有兩點可重用：① 範本的共通防呆是每條都內建「資料不足時怎麼辦」與「不要做什麼」（例：#15 比較兩份文件「列出衝突，但不自行判定哪份正確」；#79 除錯計畫「未取得證據前不大幅重寫」）；② 建庫只挑每週重複三次以上的工作做成模板，修改時每輪只改一個變因（呼應 [[prompt-maintenance-sop-anthropic]]）。文中「/summarize」這類快速標籤是作者自訂的名稱，不是官方指令。

其他常見結構型框架（2026-09-29 補一行）：CO-STAR（Context／Objective／Style／Tone／Audience／Response）、RCTCO（Role／Context／Task／Constraints／Output）、RACE（Role／Action／Context／Expectation）、RISEN（Role／Instructions／Steps／End goal／Narrowing）、RTF（Role／Task／Format）都是同一組要素的不同切法，依任務複雜度選；作者另提「Skill 是骨架、提示詞是肌肉」（固定的角色語氣格式沉澱為 Skill，每案變動的留在提示詞），與 [[claude-skills-asset-building]] 同構（來源：數位時代〈行銷人AI提示詞怎麼寫？5大Prompt框架〉，2026-09-28，<https://www.bnext.com.tw/article/92388/better-question-better-answer-ai-prompt-frameworks>，作者觀點）。

## 相關連結

- [[synthesis-prompt-engineering-evolution]] — Prompt Engineering 演進精粹（三次認知躍遷：指令細節→結果契約→評估驅動）
- [[production-agent-architecture]] — 生產級 Agent 的 Guardrails / HITL（Checkpoint 概念的工程化對照）
- [[claude-fable-5-usage-guide]] — Fable 5 官方使用指南
- [[agent-os-workstation-division]] — 同期概念：確認邊界（工位分工版的 Checkpoint）
- [[ai-architecture-diagram-tool-vs-instruction-comparison]] — 🔑 本條原則的**控制變因實證**：同一指示 × 5 種工具（MCP／Skill／外掛／裸 Claude Code），指示明確度的影響大於工具選擇（2026-09-29 新增）
