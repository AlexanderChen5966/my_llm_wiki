---
title: Claude Opus 5.5 發布：價格、跑分、API 破壞性變更與方案差異
date: 2026-09-29
tags: [ai/claude, ai/agent, type/comparison]
source: https://rar.design/posts/claude-opus-5-5-guide ; https://www.aiposthub.com/claude-opus-5-5-creative-coding-showcase-deep-dive-2026/ ; https://www.aiposthub.com/claude-opus-5-5-interactive-learning-page-guide/
type: article
status: processed
asset_value: 80
---

Anthropic 於 2026-09-22（美國時間；台灣 9/23 00:30）發布 Claude Opus 5.5，是 Claude 5.5 家族的首發模型。官方定位「大部分工作追平 Fable 5.1」，單價降到 $4/$20，一般工作量成本比 Opus 5 低約 40%、輸出快 30% 以上。本條目以 rar.design（設計師 Riven 整理官方公告、平台文件與 21 則合作夥伴回饋）為主幹，併入 aiposthub 兩篇（作者 Philo，同日發布）的成本拆解、評估方法與互動頁範本。

## T（核心論點）

Opus 5.5 是新的日常主力：官方說大部分工作追平 Fable 5.1，價格卻只有 $4/$20。**「成本降 40%」不是單價降 40%**——單價只降 20%（每 1M 輸入加 1M 輸出：$30 → $24），其餘來自完成任務用的 token 變少、cache read 降 60%。所以實際省多少要看工作量，比較時應該用「**完成一個驗收合格任務的成本**」，而不是牌價或單次回覆。

## B（適用場景）

**適用：**
- 日常 Claude Code 與 API 工作（官方建議：不確定用哪顆就先用 Opus 5.5，卡住先調 effort，還不夠才換 Fable 5.1）
- Pro 使用者：Opus 5.5 吃訂閱額度；Fable 5.1 要另開 usage credits、按 API 費率計費，等於不加錢拿到接近頂規的表現
- 跨多檔修改、長時間程式碼稽核、可驗證的原型製作（互動教學頁等創意編程）

**不適用 / 注意：**
- 🔴 整合程式裡如果**關閉 thinking、強制 tool_choice、使用 `computer_20251124`**，換 model ID 後會直接壞掉（見 R 層破壞性變更）
- 防護轉送：大多數資安任務會轉給 Opus 4.8 回答，生物與前沿模型開發題目轉給 Opus 5；日常找 bug、修 bug 不受影響
- Free 方案沒有 Opus
- 固定格式改寫、短問答，更便宜的模型可能就夠（aiposthub）

## R（操作步驟與規格）

### 1. 家族位置與規格（官方）

- 家族排序：Mythos 5.1（只開放給審核通過的組織）＞ Fable 5.1 ＞ **Opus 5.5** ＞ Sonnet 5 ＞ Haiku 4.5
- Sonnet 5.5、Haiku 5.5 官方說「未來幾週」推出，**沒有給日期**

> ⚠️ 2026-10-08 更新：Sonnet 5.5 已於 2026-09-28 發布（作者轉述官方，見 [[claude-models-real-task-eval-tokyo-trip]]），家族排序中 Sonnet 那一層應改讀為 Sonnet 5.5；Haiku 5.5 仍未推出。
>
> 📎 2026-10-08：同一篇真實任務評測中，Opus 5.5 與 Fable 5.1 四個任務的表現一直很接近，支持官方「Opus 5.5 大部分工作與 Fable 5.1 同級」的說法（單一作者、每題 1–2 次的小樣本）。

- API model ID：`claude-opus-5-5`；上下文視窗 1M token；輸入文字＋圖片、輸出文字（aiposthub 引官方模型目錄）
- 通路：Claude 網頁／桌面／手機 App、Claude Code、Claude API、Amazon Bedrock、Google Cloud、Microsoft Foundry

選型面的陣容表與更正，見 [[claude-model-selection-guide]]〈2026-09-23 陣容更新〉。

### 2. 定價（每百萬 token，美元）

| 項目 | Opus 5.5 | Opus 5 | Fable 5.1 |
|------|------|------|------|
| 輸入 | $4 | $5 | $10 |
| 輸出 | $20 | $25 | $50 |
| Cache read | $0.20 | $0.50 | ⚠️ 待核對官方定價頁 |
| 快取寫入（5 分鐘） | $5 | $6.25 | $12.50 |
| 快取寫入（1 小時） | $8 | — | — |

- Batch API 半價；Fast mode（Claude Code 與 Claude Platform）最高 2.5 倍速，$8 / $40
- 官方註明 cache read 占代理任務與寫程式工作的大部分成本，所以這一格降 60% 影響最大
- ⚠️ Fable 5.1 cache read：來源表格列的數字不符合「約為輸入價一成」的慣例、又比 Opus 5 還便宜，疑似誤植，本庫不收錄該數字，待核對官方定價頁
- ⚠️ Sonnet 5 價格：9/23 官方平台文件列 $2/$10，但 [[claude-model-selection-guide]] 7/16 版記的是「$3/$15，8 月底前優惠 $2/$10」。兩種可能並列、不下結論：(a) 優惠延長；(b) 已永久降價。待核對官方定價頁

### 3. 成本與跑分怎麼讀

**「40%」的拆法**（aiposthub 試算）：單價合計 $30 → $24 只降 20%；其餘降幅來自 token 用量變少與快取降價。實際帳單取決於上下文長度、快取命中率、工具呼叫與重試次數。

**API 預設 effort 由 high 改為 medium。** 多家測試者說 medium 就夠用（Factory：第一個願意預設開 medium 的模型，表現追平 Opus 5 開 high）；但同一個檔位它會比 Opus 5 想得更多，開到 xhigh／max 時更明顯。

**官方跑分**（⚠️ 廠商自評、尚未第三方驗證；Opus 5.5 以 max effort 測，Terminal-Bench 4.0 用 xhigh，開著正式防護測）：

| Benchmark | Opus 5.5 | Fable 5.1 | Opus 5 | GPT-6 Astra |
|------|------|------|------|------|
| Terminal-Bench 4.0 | 66.4% | 55.8% | 52.3% | 57.9% |
| CursorBench 4.0 | 57.8% | 51.8% | 46.6% | — |
| GDPval-AA v2.1（Elo） | 1846 | 1735 | 1708 | 1542 |
| AutomationBench | 40.0% | 31.4% | 26.9% | **41.4%** |
| Humanity's Last Exam | 67.7% | 65.6% | 63.6% | 57.2% |
| OSWorld 2.0（partial） | 81.8% | 80.7% | 74.0% | — |
| Terminal-Bench-Science | 58.7% | 52.6% | 29.0% | **64.6%** |

- FrontierCode 預設檔位 54.6%，官方稱超過 Astra 最高檔、每任務成本約五分之一
- 三個讀法：(1) 官方自己說「到這個能力等級，跑分差距越來越不能代表實際差距」，Opus 5.5 與 Fable 5.1 實際差距比分數小；(2) Astra 在 AutomationBench、Terminal-Bench-Science 領先（AutomationBench 由 Zapier 自跑、沒開備援模型）；(3) 防護轉送可能讓分數偏低
- aiposthub 轉述官方註腳：各模型的推理強度、執行環境與成績來源不完全一致，不宜把單一分數讀成全面勝負

**評估方法**（aiposthub 作者建議）：長程工程任務拆成「看懂原有程式／做出變更／通過測試／留下可審查的差異」四個環節分別觀察；Ryan Sael 的鏡頭焦點互動頁（Claude 官方轉發）屬於**單一挑選案例**，不能證明重現率與可維護性。

### 4. API 破壞性變更（官方平台文件，換 model ID 前必看）

1. thinking 不能設為關閉（adaptive thinking 永遠開著，改用 effort 控制深度）
2. 不支援強制呼叫工具：`tool_choice` 設 `any` 或指定工具會回錯誤
3. thinking 區塊綁定模型與對話
   - 📎 2026-10-19：preserved thinking 會核對推理區塊的生成模型與先前內容，系統指示／工具／前文變動可能使區塊失效（官方說影響「部分新帳號」的前文處理），見〈補充資料：防蒸餾研究與開發者影響〉
4. Claude API 與 Google Cloud 不再接受 `computer_20251124`

另有一項不報錯的行為變更：工具呼叫之間的進度文字移進 thinking 區塊，**預設為空**；若介面會即時顯示這段文字，要另外設定才拿得到。

### 5. 防護與安全（官方）

- 第一個帶 Fable 5.1 等級防護的 Opus（資安、生物、防蒸餾），轉送規則見 B 層
  - 📎 2026-10-19：防蒸餾研究（2026-10-12）後續——平台觀察到的推理資料擷取量降約 90–99%（🔴 公司自報、研究原文未取得）、自 2026-09-24 起部分被防護攔截的請求仍會計費，詳見〈補充資料：防蒸餾研究與開發者影響〉
- 約 2,000 個情境的自動行為稽核，偏差行為幾乎都比近期 Claude 模型好；越界嘗試比 Opus 5／Mythos 5.1 少約 85%（皆低嚴重度、自行回報）；官方也承認模型常懷疑自己正在被測試，讓評估更難
- 帶歐盟 AI 法案浮水印（見 [[claude-text-watermark]]）
- 財報分析捏造測試：Opus 5.5 18 份有 16 份通過，Fable 5.1 與 Opus 5 一份都沒過
- 寫作改成重點先講、少術語、照使用者給的寫作規則走；官方稱「更容易跟上、也更容易檢查」同時是安全上的好處

### 6. 方案差異（官方方案頁）

| 方案 | Opus 5.5 | Fable 5.1 |
|------|------|------|
| Free | — | — |
| Pro | 包含在方案額度 | 需開 usage credits，按 API 費率計費 |
| Max 5x／20x | 包含在方案額度 | 最多用到每週額度的 50% |

Pro／Max／Team 的五小時上限同步調高，並送一次可自選時間使用的額度重置，細節見 [[claude-code-billing-usage]]。

### 7. 合作夥伴回饋（⚠️ 官方轉述的個案自述，非受控實驗）

- 20 萬行 codebase 稽核加修復不到 3 小時；同一件事 Opus 5 跑超過 20 小時、用 2.5 倍 token
- Quantium：同一個複雜程式任務 38 次指令／4 天 → 11 次／3 小時
- Deloitte 程式碼審查：Opus 5.5 最低檔抓到 72% 已知 bug，Opus 5 開 high 為 56%

### 8. 互動教學頁範本（aiposthub）

官方〈GPS, explained by Claude Opus 5.5〉影片是依公開 GPS 星曆做的**模擬**，不是接收即時衛星訊號；影片是官方錄製的，Opus 5.5 本身輸出只有文字、不能產生影片。Artifact 在 Free／Pro／Max／Team／Enterprise 都能建，但受管理員設定與「雲端程式執行和檔案建立」開關影響。

需求範本（可直接貼給 Claude，方括號換成自己的主題）：

```text
請製作一個繁體中文互動教學頁，對象是［讀者年齡或背景］，主題是［概念］。
讀者操作［一個滑桿或按鈕］時，畫面必須即時顯示［可觀察的變化］，並用一句白話說明原因。
請先列出你使用的假設與資料來源，再製作頁面。
完成後檢查手機寬度、鍵盤操作、文字對比，以及操作到最小值和最大值時是否出錯。
若某個數值只是示意，請直接在頁面標示「示意」。
```

四項驗收：① 手機上能操作嗎 ② 只用鍵盤能完成主要互動嗎 ③ 控制項調到邊界時數值與畫面仍合理嗎 ④ 每個科學／財務數值有來源或標為示意嗎。

📎 2026-10-19：用 Claude 寫程式做動畫的需求、驗收、交付見 [[ideas/claude-code-animation-prompt-qa-delivery]]。⚠️ 方案不要混：Artifact 為 Free～Enterprise，Claude Motion 只開 Team／Enterprise（[[claude-motion-beta-2026-10]]）。

## C（關鍵行動）

- [ ] 檢查自己的 API 整合：有沒有關閉 thinking、強制 `tool_choice`、使用 `computer_20251124`，改完再換 model ID
- [ ] 依賴預設 effort 的 API 呼叫，注意已從 high 改成 medium
- [ ] 用自己最近的真實任務重新量「每個合格任務的成本」，不要直接套 40%
- [ ] 原本硬開 Fable 5.1 的任務，拿一份真實工作在 Opus 5.5 重跑比較
- [ ] 核對官方定價頁：Fable 5.1 cache read、Sonnet 5 $2/$10 是否為永久價

## 補充資料：防蒸餾研究與開發者影響（2026-10-19 併入）

來源：https://www.aiposthub.com/anthropic-distillation-defenses-thinking-integration/ （Philo，2026-10-13）；官方連結僅有 preserved thinking 文件 https://platform.claude.com/docs/en/build-with-claude/preserved-thinking

> ⚠️ 官方連結未經本次核對（2026-10-19）。🔴 **研究本文連結原文未附**，以下研究數字皆經 aiposthub 轉述、研究原文未取得；成效數字為**公司自報**。

- **發布**：Anthropic 於 2026-10-12 公布防蒸餾研究，針對未經授權、違反平台規則的大規模推理資料擷取
- **成效（🔴 公司自報）**：自家平台觀察到的推理資料擷取量下降約 **90%–99%**；被擷取的推理 token 約為先前的 **1/10–1/100**
  - ⚠️ 指標是「平台上的**擷取行為**」，**不是外部模型能力**；不能讀成「其他模型變弱九成」
- **官方自己承認的限制**：防護效果能維持多久不確定；可能誤判正常請求
- **防護方向**（三項）：控制推理資料流動、偵測擷取請求、提高重複嘗試的成本
- **計費**：**自 2026-09-24 起，部分被防護攔截的請求仍會計費**——原文寫「部分」、有指定範圍，但作者未查明條件，**不可擴大成「每種錯誤都收費」**
- **preserved thinking**：影響「部分新帳號」的前文處理方式（適用範圍原文模糊，未查明）；服務會核對推理區塊的**生成模型與先前內容**，系統指示、工具與前文變動都可能讓區塊失效

### 開發者影響（檢查清單）

- [ ] **重試邏輯**：被攔截的請求可能仍計費 → 遇到同一拒絕不要無限重送；暫時性錯誤、格式錯誤、被拒絕三者分開處理
- [ ] **改系統指示／工具定義後**，舊的 thinking 區塊可能失效 → 多輪對話整合要走完整流程測（含前文回傳），不只測第一輪
- [ ] 程式若為了省 token 重新整理／壓縮前文，確認沒有改動必須原樣保留的推理區塊（依官方相容做法，不要自行猜格式）

### 作者觀點（aiposthub／Philo）

- 蒸餾本身是技術方法，合法與否看資料來源、授權與服務條件（合法蒸餾的技術面見 [[agent-trajectory-distillation-harness-zero-perplexity]]）
- 企業對其他公司的指控，需要與獨立證據分開判斷；公開防護方法只能說明平台做了什麼

## 原始摘要

- **rar.design（2026-09-23，主幹）**：整理官方公告、平台文件與 21 則合作夥伴回饋，拆解 Opus 5.5 的家族定位、40% 成本下降的組成、跑分表與三個讀法、寫作升級、防護與 4 項 API 破壞性變更、方案差異與額度調整。文末訂閱推廣與作者延伸用法未收。
- **aiposthub〈創意編程亮眼，成本與評測該怎麼看？〉（2026-09-27）**：以 Ryan Sael 互動頁為例提醒創意展示不能證明重現率；拆解 40% = 單價 20% ＋ token 與快取；建議以合格任務成本比較、長程任務分四環節觀察。
- **aiposthub〈能做互動教學頁嗎？〉（2026-09-27）**：解析官方 GPS 影片的邊界，補 1M 上下文與輸入輸出模態，提供互動教學頁需求範本與四項驗收。

## 相關連結

- [[claude-model-selection-guide]] — 選型入口；〈2026-09-23 陣容更新〉段落以本條目為來源
- [[claude-opus-5-prompting-guide]] — Opus 5 提示詞指南；Opus 5.5 起 thinking 完全不能關，是其「別關 thinking、改調 effort」的延伸
- [[claude-opus-4-8-release-notes]] — 前代發布紀錄（同命名慣例）；Opus 4.8 現為資安題轉送目標
- [[claude-fable-5-usage-guide]] — Fable 系列用法；Opus 5.5 追平 Fable 5.1 後，Fable 的出場時機變少
- [[claude-code-model-and-thinking-depth]] — Claude Code 模型與思考深度設定；對應預設 effort 改為 medium
- [[claude-code-billing-usage]] — 五小時上限調高、額度重置、優雅收尾
- [[claude-text-watermark]] — Opus 5.5 帶歐盟 AI 法案浮水印
- [[llm-cost-optimization-strategies]] — cache read 降價對長任務成本的影響
- [[ai-collaboration-five-step-acceptance-criteria]] — 「驗收合格任務的成本」與互動頁四項驗收的通用框架
- [[claude-design-tool]] — 設計師用 Claude 做 UI／互動原型
- [[claude-models-real-task-eval-tokyo-trip]] — 2026-10 真實任務評測：Opus 5.5 與 Fable 5.1 表現接近；記載 Sonnet 5.5 於 9/28 發布
- 📎 2026-10-19：[[agent-trajectory-distillation-harness-zero-perplexity]] — 合法蒸餾的技術面，對照本條防蒸餾補充段
- 📎 2026-10-19：[[ideas/claude-code-animation-prompt-qa-delivery]] — Opus 5.5 程式動畫：狀態清單＋時間表、三輪驗收、HTML／SVG／MP4 交付；[[claude-motion-beta-2026-10]] — Claude Motion 產品（Team／Enterprise）
- 📎 2026-10-19：[[claude-code-pure-code-animation-practice]] — Opus 5.5 首週動畫案例與實作工作法（由時間算畫面、看樣片迴圈、子代理分工）；[[hyperframes-heygen-html-to-video]] — 益盛四支片的 token 實數（總計大多是快取讀取，輸出 token 只佔一小部分），可對照本條「驗收合格任務成本」
- 📎 2026-10-19：[[opus-5-5-video-case-libraries]] — 社群案例規模參考（athemeroy 凍結快照 1,401 檔／1,119 yes+likely；yihui-dev 475 組提示詞）；🔴 README 自己說這些數字**不代表 Opus 實際用量**，分類器（Gemini 3.8 Flash）不證明模型呼叫，與本條「單一挑選案例不能證明重現率」一致
