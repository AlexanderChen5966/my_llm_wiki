---
title: 工具描述與錯誤訊息就是提示詞：平常怎麼做寫描述，碰壁怎麼辦寫錯誤
date: 2026-09-23
tags: [ai/mcp, ai/prompt-engineering, type/concept]
source: https://ithelp.ithome.com.tw/articles/10411905; https://ithelp.ithome.com.tw/articles/10412617
type: article
status: processed
asset_value: 75
---

WikiBrain 系列 Day 07（主體）＋ Day 08「衝突回應要附上目前完整內容」一節，作者為 iT 邦幫忙使用者 20184129（WikiBrain 作者）。表面上是六個 MCP 工具的 API 設計，實際上是**偽裝成 API 設計的提示詞工程**。這條原則適用於任何寫給 agent 用的介面：MCP 工具、Skill、CLI 的錯誤輸出。

> 🌱 **「工具介面設計」主題的種子條目**：未來同主題累積到 3 篇以上時，可升級為精粹條目。

## T（核心論點）

**模型看不到原始碼，只看得到工具名稱、描述和參數說明——那幾行字就是你唯一的指揮權。** 所以「平常怎麼做」寫在描述裡，「碰壁之後怎麼辦」寫在錯誤訊息裡：描述每一步都要重送給模型（寫得越細、每一步越貴），錯誤訊息只在出錯那一次才花 token。但**描述只能影響意圖，擋不住行為**——真正不能發生的事要寫成程式碼。

## B（適用場景）

**適用：**
- 設計 MCP 工具、Claude Code Skill、給 agent 呼叫的 CLI 或 API
- agent 經常撞同一種錯誤、每次都要人介入——先檢查錯誤訊息有沒有寫下一步
- 工具數量正在膨脹，需要判斷哪些話該放描述、哪些該放錯誤

**不適用 / 注意：**
- 會弄壞資料的限制不能只寫在描述裡，要在程式碼裡擋（與 [[wikibrain-llm-wiki-implementation-patterns]] 的分界原則「會弄壞資料的寫成程式碼、影響品質的寫成規則」是同一條線）
- 描述也會被平台端的 LLM 審查，寫法不當可能被標為不安全，反面案例見 [[mcp-chatgpt-connector-not-safe-fix]]

## R（操作步驟）

1. **好描述三要素：回傳哪一層的什麼、什麼時候該呼叫、一個模型熟悉的類比。**
   ```ts
   // 寫給人看的
   description: '取得指令內容'
   // 寫給模型看的
   description: '回傳 schema/ 層的 AI 編纂指令全文（等同知識庫的 CLAUDE.md）。動筆前先呼叫本工具。'
   ```
   「等同知識庫的 CLAUDE.md」是寫給模型的：它懂 CLAUDE.md 是什麼，一句話就知道該用什麼態度對待這一層。
2. **描述只講規則，碰到規則之後怎麼辦寫進錯誤訊息。** `create_note` 的描述只寫「path 須以 `raw/`、`wiki/` 或 `schema/` 開頭並以 .md 結尾；已存在則回錯誤（不覆蓋）」；撞到既有路徑時回 `CONFLICT`：「筆記已存在：{path}（請改用 update_note 並帶 if_version）」。實測 agent 讀完就自己換工具，沒有卡住，也沒有重試三次。
3. **這比要求 agent 先查存不存在更好**：先查多一次呼叫，而且查完到建立之間還是有競態空窗；讓建立失敗時把正確做法寫在錯誤裡，一步就接上。
4. **衝突回應要附上目前的完整內容（Day 08）**：只回 409，agent 唯一能做的是重讀再重試，多一次工具呼叫，中間還可能又被改。錯誤要附 `{version, content, updated_at}` 加下一步指示，例如「目前為 v9，你帶的 if_version=7。請以目前內容為基礎重新編輯後再送」，agent 就能直接合併重送、不必問人。資料層怎麼做到見 [[human-agent-shared-data-layer-optimistic-locking]]。
5. **工具數量要少**：每次呼叫都要把全部描述重送一次，工具越多每一步越貴。WikiBrain 只有六個工具。
6. **讓 agent 先讀規則的三層引導**（三層都不是強制）：
   - 讀規則的工具排**第一個**——工具清單順序會原樣送給模型，排最前面本身就是暗示；
   - 描述寫死時機（「動筆前先呼叫本工具」）；
   - 規則頁模板再重申一次流程。
   實測 agent 幾乎每次都先讀，但那是**被引導出來的行為，不是被保證的行為**。
7. **規則工具的回傳內容先列待辦、再接規則**：`get_instructions` 一開頭是「待編纂的來源（N）」清單，然後才是做法；使用者只說「幫我整理一下」，agent 就找得到該做的事。必須做的收尾動作（如在 `wiki/log.md` 追加紀錄）跟著待辦清單再重複一次，比寫在規則頁第七行更難被忽略。
8. **分工**：介面與錯誤訊息歸開發者維護（改了所有人跟著改），規則頁歸使用者維護（改完不必等發版），雙方改動互不干擾。

### 第三方實證（Xcode MCP）

[[xcode-mcp-hidden-state-and-authorization]] 的實測是本原則的獨立佐證：schema 與實際行為不符四次，其中三次靠「告訴你該呼叫哪支工具、附上可用值」的錯誤訊息一輪就修好；反例是 `RunSomeTests` 只回 "The data couldn't be read because it is missing."，agent 只能自己抓 schema 逐欄比對。該作者的結論與本篇同向：**錯誤訊息是 API 的一部分，而且比 schema 更重要**；明確的錯誤便宜，安靜的錯誤昂貴。

### 與既有條目的差異

| 既有條目 | 相近說法 | 差異 |
|---|---|---|
| [[claude-5-context-engineering-rules]] | 怎麼用工具集中寫在 tool description | 該篇談的是**精簡**系統提示詞；本篇談描述與錯誤訊息**怎麼分工** |
| [[harness-engineering]] Level 2 | linter 錯誤訊息包含修復指示 | 那是 sensor 端；本篇是同一原則在**工具端**的版本 |
| [[agent-error-recovery-taxonomy]] | 錯誤分類與 recovery | 該篇談錯誤發生後 agent 怎麼恢復；本篇談錯誤訊息怎麼寫，agent **才有辦法**恢復 |
| [[rag-as-agent-tool]] | 好 Tool 的清單 | 本篇補上「描述 vs 錯誤」的 token 成本不對稱 |

## C（關鍵行動）

- [ ] 檢查本庫 Skills 的 SKILL.md：哪些「碰壁怎麼辦」的說明其實可以移到腳本的錯誤輸出，只在出錯時出現
- [ ] 自建 MCP 工具的每個錯誤訊息都補上「下一步該呼叫哪支工具／帶什麼參數」
- [ ] 衝突、重複類錯誤一律附上當前狀態，不只回狀態碼
- [ ] 真正不能發生的事確認有程式碼守門，不靠描述

## 原始摘要

Day 07：六個工具的名稱、描述與錯誤訊息是唯一能指揮模型的地方；描述講平常怎麼做，錯誤訊息講碰壁怎麼辦；`get_instructions` 用排序、描述、規則頁三層引導 agent 先讀規則；回傳內容先列待辦再列規則。Day 08 與 Day 07 講同一個 CONFLICT 例子，本條目只寫一次，另取 Day 08 的「衝突時附上完整內容」。

## 相關連結

- [[wikibrain-llm-wiki-implementation-patterns]] — 同系列 Day03+04；「描述擋不住行為，要寫成程式碼」是該條分界原則的工具端版本
- [[wikibrain-compounding-knowledge-base-demo]] — 同系列 Day05
- [[mcp-server-stateless-streamable-http-oauth]] — 同系列 Day06：這六個工具在哪裡註冊、zod `.describe()` 與 `title`
- [[human-agent-shared-data-layer-optimistic-locking]] — 同系列 Day08：`if_version` 與 ConflictError 的資料層實作
- [[xcode-mcp-hidden-state-and-authorization]] — 第三方實證與反例
- [[claude-5-context-engineering-rules]]、[[harness-engineering]]、[[agent-error-recovery-taxonomy]]、[[rag-as-agent-tool]] — 相近說法，差異見上表
- [[mcp-chatgpt-connector-not-safe-fix]] — 反面案例：工具描述也會被平台 LLM 審查
