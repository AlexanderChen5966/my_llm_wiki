---
title: Claude Motion（2026-10 Beta）：Claude 寫程式做可修改的解說動畫，不是影片生成模型
date: 2026-10-19
tags: [ai/claude, ai/workflow]
source:
  - https://www.aiposthub.com/claude-motion-editable-animation-20261009/
  - https://www.bnext.com.tw/article/92639/claude-motion-three-step-tutorial
  - https://vocus.cc/article/6ac8a933fd897800013ad182
  - https://www.bnext.com.tw/article/92620/claude-motion-slides-to-video-voiceover
  - https://www.aiposthub.com/claude-motion-hyperframes-edit-music-workflow/
  - https://www.aiposthub.com/claude-motion-runway-explainer-assets-workflow/
official_refs:
  - https://claude.com/resources/articles/dashboards-and-motion（2026-10-19 Team Lead 核對）
  - https://support.claude.com/en/articles/17454997-get-started-with-claude-motion（經媒體轉述，未核對）
  - https://support.claude.com/zh-TW/articles/17454997（經媒體轉述，未核對）
type: article
status: processed
asset_value: 51
---

Anthropic 於 2026-10-08 推出 Claude Motion（beta）。使用者在 Claude 訊息框輸入 `/motion`，用自然語言描述需求，Claude 會寫程式，把文字、圖表、形狀和圖片做成解說動畫，可以下載為 MP4。

> 📎 2026-10-19 Team Lead 核對官方公告頁（`dashboards-and-motion`）：**「Starting today」（2026-10-08）；beta，開放 Team 與 Enterprise**（公告頁未提 Free／Pro／Max）；**Enterprise 預設關閉**，由管理員在 Organization settings > Artifacts 啟用；入口為訊息框輸入 `/motion`；**no generated footage and no AI-generated people**，由 Claude 寫程式把文字、圖表、形狀、圖片做成動畫；可下載 MP4；夥伴 Adobe、Descript、HeyGen、Higgsfield、invideo、Luma AI、Runway，Canva、Captions 標示 coming soon；同頁宣布 Dashboards，並說明 Docs／Slides／Design 已脫離 beta。**解析度、長度上限、聲音、GIF、側邊欄入口，官方頁皆未載。**
>
> ⚠️ support 文件（17454997，en／zh-TW）的內容**只經 aiposthub 與數位時代李先泰轉述，未經本次核對**。
>
> 📎 2026-10-19 Team Lead 核對 support 17454997（en）：① **Team 方案預設開啟**（"on by default on Team plans"）；② Enterprise 預設關閉，由**擁有者（owner）**於 Organization settings > Artifacts 啟用，可用自訂角色限制群組——公告頁寫「admins」，兩份官方文件用詞並列；③ 入口：訊息框選 **Output → Motion**，或在 **Artifacts 分頁選 Motion 範本**（公告頁另寫輸入 `/motion`，三者並列）；④ 計入方案用量，較長或較複雜的動畫用量高於一般訊息；⑤ 動畫預設私人，點 Share 分享；⑥ 匯出為 **Export → MP4**，support 頁**未提 GIF**；⑦ 長度、解析度、聲音仍官方未載。zh-TW 版未核對。
> 🔴 來源集中：6 篇來源中 aiposthub／Philo 佔 3 篇（同站同作者，不構成交叉佐證，只算一個轉述來源；本批 aiposthub 共 7／10 篇）；Motion 的獨立來源共 4 個（aiposthub、李先泰、韋惟珊、vincent），**只有韋惟珊、vincent 兩篇實測**，兩人都沒說明自己用的是哪個方案。

## T（核心論點）

Claude Motion 是「**Claude 寫程式產生動畫**」，不是影片生成模型：畫面由程式描述，所以可以只改一個數字或一段時間，不必整支重做。它的價值不在第一版做得多快，而在**後續修改**：月報換數字、流程改一步，都能沿用原本的結構。另一項價值是 MP4 匯出，以及交接給下游影片工具繼續編輯。它也有限度：Motion 只負責「把既有內容編排成會動的說明」，真人、實拍、配音這類素材要另外準備。

## B（背景）

**適用（官方公告舉例）：** 季度報告摘要、會議中的圖表解說、新客戶的產品流程介紹——內容本來就存在，動畫只是讓觀看順序更清楚。

**不適用／注意：**
- 🔴 需要人物演出、實拍場景、產品情境影像：官方明言不生成影片畫面、不生成 AI 人物
- 🔴 方案不能從 Dashboards 推得：兩者在同一頁宣布，但開放方案不同（見下表）
- Beta：規格與方案範圍可能變動

### 方案對照（同一頁公告，三者不要互推）

| 功能 | 開放方案 | 出處 |
|---|---|---|
| **Claude Motion** | **Team、Enterprise**（beta） | 官方公告頁（Team Lead 2026-10-19 核對）；公告頁未提 Free／Pro／Max |
| Claude Dashboards | Pro、Max、Team、Enterprise（beta） | [[claude-dashboards-beta-2026-10]] |
| 一般 Artifact | Free～Enterprise（受管理員設定與雲端程式執行開關影響） | [[claude-opus-5-5-release-notes]] 第 8 節 |

| 開關 | 說法 | 等級 |
|---|---|---|
| Enterprise 預設關閉，管理員在 Organization settings > Artifacts 啟用 | 官方公告頁 | 官方（Team Lead 2026-10-19 核對） |
| ⚠️ Team 預設開啟 | aiposthub（截至 10-09）、李先泰（截至 10-14）轉述 support 文件 | 經轉述，未核對；**公告頁沒有寫** |
| ⚠️ 可用自訂角色限制群組 | aiposthub 轉述 support 文件 | 經轉述，未核對 |

## R（關鍵細節）

### 官方事實（公告頁，Team Lead 2026-10-19 核對）

- 2026-10-08 起提供（「Starting today」），beta
- 入口：訊息框輸入 `/motion`
- 作法：Claude 寫程式，把文字、圖表、形狀、圖片做成動畫；**no generated footage and no AI-generated people**
- 輸出：可下載 MP4
- 下游夥伴：Adobe、Descript、HeyGen、Higgsfield、invideo、Luma AI、Runway；Canva、Captions 為 coming soon（🔴 預告不等於今天可用）
- 官方示範：用自行車資料建看板，再把內容轉成動畫（aiposthub 明言是官方展示，不是作者實測）

### 官方未載（不要從其他來源補成規格）

| 項目 | 狀態 |
|---|---|
| 解析度、fps | **官方未載**。唯一出現的 1920×1080／30fps 是 vincent 用 Claude Code 後製的結果，**不是 Motion 規格**（見下方延伸工作流） |
| 長度上限 | **官方未載**（李先泰文的「30 秒」只是編輯自擬提示詞範例） |
| 聲音／配樂 | **官方未載**；兩位實測者都說會自動配樂與音效（見實測觀察） |
| GIF 匯出 | **官方未載**；vincent 實測說可以（見衝突並列） |
| 側邊欄入口 | **官方未載**；李先泰、vincent 寫「成品／Artifacts → Motion」 |

### support 文件細節（⚠️ 經 aiposthub、李先泰轉述，未核對）

- 用量計入方案使用限制；較長或較複雜的動畫比一般訊息耗更多額度，沒有固定的單支費用（aiposthub）
- 作品預設私人（aiposthub）
- 可在編輯器或對話中修改單一細節、保留其他內容（aiposthub）；李先泰的寫法是「右側預覽可直接改文字；要改動畫就用留言工具圈選元素，寫一句話送出」，例如「減慢第二個場景」「將 Q3 數字變更為 18%」
- 提示建議：說明**誰會看、在哪裡播、大約多長**，並附上自己的報告、資料或圖片（兩篇都這樣轉述）；李先泰文中的提示詞範本是編輯自擬，**非官方**

### 媒體轉述與官方／彼此不一致處（並列，不擇一）

| 項目 | 官方公告頁 | 媒體／實測說法 |
|---|---|---|
| 匯出格式 | MP4 | vincent：「分享 → 匯出」可下載影片，**也能下載 GIF**（附截圖與 GIF 檔）；韋惟珊：正文寫 Export MP4，但一張插圖的說明文字寫「可以匯出成 GIF 或 MP4」 |
| 匯出按鈕名稱 | 未載 | vincent「分享 → 匯出」；韋惟珊「Export」；李先泰「匯出」選 MP4；HyperFrames 文「Send to」 |
| 入口 | `/motion` | 李先泰：左側「成品」（Artifacts）選 Motion，或輸入 `/motion`（附截圖）；vincent：側邊欄「成品（原 Artifact）」→ Motion（附截圖） |
| Team 預設 | 未寫 | aiposthub、李先泰轉述 support：Team 預設開啟 |
| Enterprise 啟用者 | 管理員，Organization settings > Artifacts | aiposthub、李先泰：組織擁有者；韋惟珊：管理者 |
| 聲音 | 未提 | vincent、韋惟珊實測：自動配背景音樂與音效；HyperFrames 文把「加音樂與音效」寫成交接到 HyperFrames Studio 之後的工作；韋惟珊：**不能直接放入自己的錄音** |
| 人物 | no AI-generated people | 李先泰：「用程式畫出示意人物」，再搭配使用者提供的圖片或外部生成的影片素材（程式繪製的示意圖形和生成真人是不同的東西，兩句並列，不替讀者判斷） |
| 實測者方案 | 只開 Team／Enterprise | vincent、韋惟珊都沒說明自己用哪個方案 |

## 實測觀察（vincent／vocus，2026-10-09）

> 單一作者、單次實測，附操作截圖。⚠️ 文中混用「Claude Code」「Claude Codex」描述 Motion 的操作介面，產品歸屬描述不清，本條不沿用這些稱呼。

- 素材：大多由圖片組成的投影片，轉成 PDF 上傳（vincent）
- 入口：側邊欄「成品（原 Artifact）」→ Motion（vincent）
- 提示詞要求「單一 HTML 檔案製作成 Artifact，自動播放」；Motion 先用對話詢問**風格、配色、長度**，作者選 Other 填 300 秒（vincent）
- 成品是動畫網頁，有**背景音樂，每段動畫也有音效**；可切換 Style 配色（vincent）
- 「分享 → 匯出」可選格式下載影片，**也能下載 GIF**（vincent，附 GIF 檔）
- 作者另外請 Claude 提供單一 HTML 檔：約 97 KB、23 個場景、約 5 分鐘，含配樂與播放器，離線可播；瀏覽器不允許網頁自動出聲，要先按「開啟聲音」（vincent 引 Claude 回應）。⚠️ 同一段回應寫明「這個檔案用的是我另外寫的簡易合成器，音色和網頁版比較起來會稍微簡單一些」，還順手修了一處跑版——這份 HTML 是**請 Claude 另外打包的版本，不等於 Motion 原生的匯出格式**
- 作者觀點：官方沒有明說內部用哪個動畫框架，可用 Remotion／CSS Animation／JavaScript 理解（vincent）

## 實測觀察（韋惟珊／數位時代「一天一AI」，2026-10-14）

> 單一作者實測，描述操作流程與提示詞；作者沒說明使用的方案。

- 6 頁簡報約 10 分鐘做成動畫（韋惟珊）
- Motion 會**提問釐清需求**，並**自動搭配襯樂和音效**（韋惟珊）
- 🔑 **Motion 不能直接放入自己的錄音**：Claude 建議先調好畫面時間、按 Export 匯出 MP4，再把影片和錄音一起上傳到 Claude 對話合成；合成時 Claude 把太小聲的人聲調大，並在講話時壓低配樂（韋惟珊）。🔴 合成是在 Claude 對話中完成，**不是 Motion 內建功能**
- 上傳照稿唸的錄音後，Claude 抓出各句起訖時間、移除部分空白，再修改動畫，讓字卡動態對準唸到的字（韋惟珊）
- 作者經驗法則（作者觀點）：旁白稿以「每秒 3 個中文字」估算，每幕旁白最多佔該幕 80% 的時間；簡報轉動畫的提示詞要寫明觀眾、60–90 秒、保留原配色與插圖、每個畫面不超過 20 字

## 下游交接（夥伴方公告，經轉述）

- **HyperFrames**：aiposthub 轉述 HyperFrames 的公告與展示影片：在 Claude Motion 選 **Send to → HyperFrames**，影片會在 HyperFrames Studio 開啟，可以繼續編輯、加入音樂與音效。⚠️ **單一來源轉述**，文內沒附公告連結，按鈕名稱也未核對；作者明言沒跑過整套流程。HyperFrames 是 **HeyGen 的產品**（官方夥伴清單寫的是 HeyGen；「HyperFrames by HeyGen」見 [[codex-plugins-12-guide]]，此文本身未提 HeyGen）。⚠️ 這條交接路徑和開源 HTML 影片工作流（[[codex-plugins-12-guide]] 的 Codex 插件、[[open-design-claude-design-comparison]] 的 HTML → MP4）**安裝方式不同**，不要混為一談
  - 📎 2026-10-19：Send to 的目的地 HyperFrames Studio 詳見 [[hyperframes-heygen-html-to-video]]（README，Team Lead 核對：Studio "Available, evolving"、已推出 Mac／Linux app；Windows README 未提，aiposthub 轉述下載頁「Coming soon」，兩說並列）
- **Runway**：在官方夥伴清單內。aiposthub 轉述 Runway 公告：可以把 Motion 動畫匯出到 Runway，加入生成影像、用描述修改片段、組成更長的內容。⚠️ 交接按鈕與格式都沒說明；作者明言沒有實測
- 🔑 **Higgsfield 示範要讀對**：李先泰指出，Higgsfield 示範影片中的實拍素材、卡通角色和配音，是在 Claude Code 接上 Higgsfield MCP、再加自訂 skill 產生的；**Motion 只負責可編輯的動態圖像**。夥伴示範不等於 Motion 本身的能力

## 延伸工作流：Claude Code 後製配音與字幕（vincent；🔴 非 Motion 匯出規格）

vincent 拿到 Motion 動畫後，另外請 Claude Code 依 HTML 內容產生旁白、逐字稿、字幕，並合成 MP4：

- 成品：MP4 6 分 31 秒、1920×1080、30fps、約 45 MB、H.264（yuv420p）＋ AAC；SRT 135 段；逐字稿依 23 個場景分段
- 配音：edge-tts 台灣華語預設女聲 HsiaoChen；字幕每段最多約 18 個中文字，不燒錄進畫面
- 🔴 **這些數字都是 Claude Code + edge-tts 的後製結果，不是 Motion 的匯出規格**；官方沒有載明任何解析度、fps 或長度上限

與韋惟珊的作法對照：兩人都在 Motion **之外**處理配音（vincent 用 TTS，韋惟珊用自己的錄音），可見目前這段工作要在 Motion 外完成。

## 作者觀點

**aiposthub／Philo（上線文、HyperFrames 文、Runway 文，皆未實測）**
- 評估重點是修改成本，不是首版速度：記錄第一版、修改、驗收各花多少時間，再決定要不要擴大使用
- 「能帶到後續工具」≠ 保留文字、圖層和程式結構；交接前先確認對方收到的是影片成品，還是可編輯的內容。🔑 「可編輯」可能指三件事：提示詞可改、程式來源可改、能輸出專案檔，三者不能互相替代（同站〈交付格式〉文，見 [[ideas/claude-code-animation-prompt-qa-delivery]]）
- 數字驗收：分母、期間、單位要同時出現；字幕和畫面文字分開檢查（大螢幕看得清楚的字，手機上可能太小）
- 交接順序：先把內容定稿，再調節奏與聲音；不要假設字型、圖片、可編輯內容在交接後會完整保留
- 分工原則：數字、價格、介面這類資訊留在 Motion 動畫裡，氛圍才交給生成素材；**生成畫面不應承載產品數字**

**李先泰／數位時代（🔴 文末自註「本文初稿為 AI 編撰」）**
- 形容風格很難，給參考最快：附幾張喜歡的動畫截圖重跑（轉述 Paul J Lipsky 影片示範，未驗證）
- 對齊自己的影片：先讓 Gemini 產生帶時間碼的 SRT，再連同截圖交給 Motion（轉述 YouTube 示範，未驗證）
- 「會動的簡報不難做，更難的是一開始就說清楚要給誰看」

## C（連結與應用）

- [ ] 試用前先確認組織方案（只有 Team／Enterprise）；Enterprise 要請管理員到 Organization settings > Artifacts 開啟
- [ ] 第一支選「每月都要改數字」的短圖表解說，記錄修改與驗收的時間，不要只看首版速度
- [ ] 提示詞寫清楚誰看、在哪播、多長，附上自己的資料
- [ ] 需要自己的配音：沿用韋惟珊的作法（匯出 MP4 → 在 Claude 對話中合成），或 vincent 的 Claude Code + TTS 後製；兩者都在 Motion 之外
- [ ] 交付時同時保留 Motion 作品（可改的來源）和 MP4，MP4 不能取代可編輯來源
- [ ] 待追蹤：support 17454997 原文（Team 預設開啟、用量、預設私人）；GIF 是否為正式匯出格式；Canva／Captions 正式支援時間；beta 結束後的方案範圍

## 原始摘要

- **aiposthub〈Claude Motion 上線：把報告變成動畫，真正省下的是修改時間嗎？〉（Philo，2026-10-09，主幹）**：依官方公告與 support 文件整理方案、用量、預設私人、MP4 與夥伴清單；主張價值在修改成本。明言未實測。文末 aiposthub academy 課程推廣（Flash Lesson 01、09）與站內延伸閱讀未收。
- **數位時代〈Claude Motion是什麼？AI動畫生成教學、3步驟操作與方案限制〉（李先泰，2026-10-15）**：🔴 **初稿為 AI 編撰**。轉述官方公告與中文 support 文件，含三步驟操作、方案表（截至 10-14）、Higgsfield 示範澄清；未見作者實測。
- **vocus〈Claude Motion 實作：從 PDF 簡報到 AI 動畫影片…〉（vincent，2026-10-09）**：實測 PDF → Motion 動畫（配樂、音效、GIF 匯出），再用 Claude Code + edge-tts 後製 1080p MP4 與 SRT。
- **數位時代〈Claude Motion教學：簡報變解說動畫，加入自己的配音、調整畫面秒數〉（韋惟珊，2026-10-14）**：實測 6 頁簡報 → 動畫，旁白稿秒數、依錄音調整動畫、匯出後在 Claude 對話合成人聲。「一天一AI PRO」× 新商業學校課程推廣，以及剪藏尾段的 AWS 贊助內容、熱門文章，皆未收。
- **aiposthub〈Claude Motion 接 HyperFrames 教學…〉（Philo，2026-10-10）**：Send to → HyperFrames 交接（單一來源轉述），以及後製節奏、聲音規格建議；未實測。
- **aiposthub〈Claude Motion 接 Runway 教學…〉（Philo，2026-10-10）**：Runway 合作（轉述），以及「資訊留在動畫、氛圍交給生成素材」的分工；Runway Image-to-Video 指南細節與本條關係弱，未收；未實測。

## 相關連結

- [[claude-dashboards-beta-2026-10]] — 同一頁公告的另一半；🔴 方案不同（Dashboards 含 Pro／Max，Motion 只有 Team／Enterprise）
- [[claude-design-tool]] — 同頁宣布 Design 已脫離 beta；設計畫布（原型、Export HTML）和 Motion（解說動畫、MP4）分工不同
- [[claude-opus-5-5-release-notes]] — Artifact 方案（Free～Enterprise）與互動教學頁；Motion 的方案範圍窄得多
- [[ideas/claude-code-animation-prompt-qa-delivery]] — 另一條路：自己請 Claude／Claude Code 寫程式做動畫（需求、驗收、交付），不經 Motion 產品
- [[open-design-claude-design-comparison]] — HyperFrames HTML → MP4 的開源路線（與 Motion 的 Send to 交接不同）
- [[codex-plugins-12-guide]] — HyperFrames by HeyGen、Remotion：Codex 端的程式影片插件
- [[openmontage-ai-video-agent-system]] — 用 coding agent 跑完整影片製作管線（Remotion／FFmpeg）的對照
- [[html-as-ai-output-interface]] — 單一 HTML 封裝動畫與播放器（vincent 的 97 KB 單檔 HTML 是同一種形態）
- 📎 2026-10-19：[[hyperframes-heygen-html-to-video]] — HyperFrames 專篇（Studio 即 Send to 目的地）；[[claude-code-pure-code-animation-practice]] — 自寫程式動畫的實作工作法（產品路線的對照）；韋惟珊（本條實測作者之一）另一篇 24 秒宣傳片實測也收在該條（不同主題）
