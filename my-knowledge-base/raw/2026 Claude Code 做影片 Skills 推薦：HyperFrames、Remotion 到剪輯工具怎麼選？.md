---
title: "2026 Claude Code 做影片 Skills 推薦：HyperFrames、Remotion 到剪輯工具怎麼選？"
source: "https://www.aiposthub.com/claude-code-video-skills-2026/"
author:
  - "[[Philo]]"
published: 2026-10-11
created: 2026-10-13
description: "Claude Code 做影片該裝哪個 Skill？整理 HyperFrames、Remotion、GSAP、OpenMontage 等工具的授權、安裝指令與本站實測結果，Codex 也適用。"
tags:
  - "clippings"
status: processed
---
想用 Claude Code 做影片，最先遇到的問題常是不知道該裝哪個工具，換成 Codex 也一樣。

原因是 Claude Code 和 Codex 本身不會輸出 MP4，它們負責寫程式或操作其他軟體。

要從零做出動畫，得先有一個渲染引擎把程式碼算成影片，例如 HyperFrames 或 Remotion，再依需求加上動畫風格工具。

手上已經有拍好的素材，則要找剪輯與字幕工具，兩條路需要的東西不同。

GitHub 上的影片相關 Skill 名字相近，分工卻差很多：有的輸出影片，有的教 AI 動畫節奏，有的處理剪輯，有的包辦整套流程，還有的專門拆解別人的影片。

本站介紹過的工具可以分成這五類，下面也標出哪些做過實測、哪些只根據官方資料介紹。

## 用 Claude Code 做影片，先對照你的需求

Skill 是一個資料夾，裡面的說明檔會教 AI 怎麼完成某一類工作。

裝好之後，你只要用一般對話提出需求，AI 就會照著說明檔的步驟做。

延伸閱讀： [Claude Skills 是什麼？免費就能用的 AI 客製化功能完整教學](https://www.aiposthub.com/claude-skills-complete-guide/)

下表依影片類型，列出第一個該看的工具類別。

| 你想做的影片 | 先看哪一類 | 代表工具 |
| --- | --- | --- |
| 產品宣傳短片 | 動畫風格 Skill 加渲染引擎 | Cinetic 搭配 Remotion 或 HyperFrames |
| 知識型動畫、資訊圖卡 | 渲染引擎 | Remotion、HyperFrames |
| 口播影片初剪、加字幕 | 剪輯與字幕工具 | HyperFrames 剪輯流程、ChatCut、Palmier Pro |
| 網站或產品頁的互動小動畫 | 動畫風格 Skill | GSAP Skills、Hairline、SVG 動畫 |
| 從腳本到成片一次跑完 | 整套影片代理 | OpenMontage、Higgsfield |
| 拆解爆紅短影音的結構 | 影片分析 Skill | hook-lab |

表格只列代表工具，每個工具的授權、費用與本站是否實測，整理在下一張總表。

## 工具總表：授權、費用與本站實測狀態

這張表的資料查核日期是 2026 年 10 月 11 日。

本站對 HyperFrames、ChatCut 等工具寫過教學，但教學不等於評測，這張表只把本站作者自己安裝並產出成品的算作實測。

| 工具 | 類型 | 授權與費用 | 本站實測 |
| --- | --- | --- | --- |
| HyperFrames | 渲染引擎 | Apache-2.0 開源，沒有按次渲染費 | 未實測，有教學文章 |
| Remotion | 渲染引擎 | 個人與 3 人以下公司免費，4 人以上營利公司需購買授權 | 有，Codex 搭配 Remotion |
| Cinetic | 動畫風格 Skill | MIT 開源，用 Remotion 輸出時仍適用 Remotion 授權 | 未實測，有介紹文章 |
| GSAP Skills | 動畫風格 Skill | GSAP 可免費商用，Skills 為 MIT | 有，網站動畫，非影片 |
| Hairline | 動畫風格 Skill | MIT 開源 | 未實測，有介紹文章 |
| ChatCut | 剪輯與字幕 | AI 功能使用點數計費 | 未實測，有教學文章 |
| Palmier Pro | 剪輯與字幕 | GPL-3.0 開源，AI 生成另計點數 | 未實測，有教學文章 |
| OpenMontage | 整套影片代理 | AGPL-3.0 開源，模型與素材服務另計 | 有，滿分 5 分給 2.5 到 3 分 |
| Higgsfield MCP | 整套影片代理 | 需訂閱多個服務 | 未實測，有介紹文章 |
| hook-lab | 影片分析 Skill | MIT 開源 | 有，6 款中唯一能直接用 |

本站實測過的四項是 Codex 搭配 Remotion、GSAP Skills、OpenMontage 與 hook-lab，各自的結論寫在對應章節。

安裝任何一個之前，建議先看文末的「安裝前的四個檢查」，裡面整理了授權、權限與系統需求。

## 渲染引擎：把 AI 寫的畫面變成 MP4

渲染引擎負責最後一步，把程式寫出的畫面逐格算成影片檔。

沒有這一層，AI 寫得再多動畫程式碼，也只能在瀏覽器裡預覽。

目前最常見的兩個選擇是 HyperFrames 和 Remotion。

### HyperFrames：用網頁語法寫畫面

HyperFrames 是 HeyGen 開源的影片框架，讓 AI 用 HTML 與 CSS 排版畫面，再輸出成 MP4。

它採用 Apache-2.0 授權，可以商用，也沒有按次渲染的費用。

電腦需要先裝好兩樣東西：Node.js 22 以上版本，這是執行 JavaScript 的環境，另一樣是影片轉檔工具 FFmpeg。

下面的指令要貼在電腦的終端機執行，不是貼進 Claude Code 的對話框。

Claude Code 使用者可以安裝官方外掛：

```
claude plugin marketplace add heygen-com/hyperframes
claude plugin install hyperframes@hyperframes
```

Codex 等其他工具則用通用安裝指令，選單出現時勾選 Core Skills 即可：

```
npx skills add heygen-com/hyperframes
```

這個指令讀的是第三方技能目錄 skills.sh 的副本，內容可能比 GitHub 最新版晚幾個小時。

想拿到最新版，可以改用 `npx hyperframes skills update` 。

延伸閱讀： [HyperFrames x Codex 快速上手：安裝、工作流、案例分享](https://www.aiposthub.com/hyperframes-codex-quickstart-guide/)

延伸閱讀： [HeyGen HyperFrames 是什麼？Day 1～12 教學與 AI 影片工作流](https://www.aiposthub.com/heygen-hyperframes-30-days-day-1-12/)

HeyGen 也推出桌面版 HyperFrames Studio，可以在畫面上圈選要改的地方，再交給 Claude Code 或 Codex 修改，目前支援 Mac 與 Linux，Windows 版尚未推出。

延伸閱讀： [HyperFrames Studio 是什麼？用 Codex、Claude Code 做影片，圈選畫面就能改](https://www.aiposthub.com/hyperframes-studio-app-codex-claude-code-video-editor/)

### Remotion：用 React 寫影片

Remotion 是用 React 寫影片的框架，React 是常見的網頁前端開發工具。

個人、非營利組織與 3 人以下的營利公司可以免費商用 Remotion。

4 人以上的營利公司則需要購買公司授權，價格以官網為準。

官方也提供 Agent Skills，涵蓋建立專案、字幕、輸出與互動等主題：

```
npx skills add remotion-dev/skills
```

本站實測時，用 Codex 搭配 Remotion 做出 AI Agent 運作流程的知識動畫，也輸出了透明背景版本。

實測發現，沒有特別要求的話，成品容易變成一張張文字卡，所以想要圖示或流程動畫，提示詞裡要直接寫明。

延伸閱讀： [Codex + Remotion 教學：3分鐘教你使用 AI 製作動畫](https://www.aiposthub.com/codex-remotion-ai-animation/)

### 兩者怎麼選？

如果你會一點 HTML 與 CSS，或希望 AI 像改網頁一樣修改畫面，先試 HyperFrames。

如果團隊本來就用 React，或已經有 Remotion 專案，就選 Remotion，公司超過 3 人又要商用時，授權成本也要列入比較。

兩邊都不熟的新手，可以照本站 Codex 搭配 Remotion 的實測文章做，裡面有完整提示詞與踩雷紀錄。

接著看動畫風格類 Skill，它們影響的是畫面的節奏與版面。

## 動畫與視覺風格 Skill：決定畫面長什麼樣子

這類工具本身不輸出影片，它們教 AI 掌握節奏、運鏡與版面。

Cinetic 要搭配上一節的渲染引擎才能得到 MP4，GSAP Skills 與 Hairline 做的則是網頁上的互動動畫，直接在瀏覽器播放。

### Cinetic：產品宣傳短片

Cinetic 把產品短片拆成企劃、視覺、節奏、聲音與檢查幾個階段，讓 AI 照步驟製作。

它預設用 Remotion 輸出，也可以改用 HyperFrames。

Cinetic 本身是 MIT 授權，但用 Remotion 輸出時，仍要遵守 Remotion 的授權規定。

```
npx skills add Leonxlnx/cinetic
```

它需要 Node.js 22、Python 3.11、FFmpeg 與 Chromium 瀏覽器核心，安裝步驟比其他 Skill 多。

這個專案在 2026 年 9 月 28 日才建立，功能與安裝方式可能還會改動，本站尚未實測。

延伸閱讀： [Cinetic 是什麼？讓 Claude Code 製作產品宣傳動畫的 Skill](https://www.aiposthub.com/cinetic-claude-code-product-animation-skill-guide/)

### GSAP Skills：網站與產品頁動畫

GSAP 是常見的網頁動畫函式庫，被 Webflow 收購後改為免費，包含商業用途。

主要限制是不能拿它做和 Webflow 競爭的無程式碼動畫編輯器，完整條款以官方授權頁為準。

官方 Skills 教 AI 正確使用時間軸、捲動觸發與各種外掛：

```
npx skills add https://github.com/greensock/gsap-skills
```

本站實測用它做網站動畫，發現只裝 Skills 做出的網站仍然有模板感。

要搭配設計參考，並寫明不要的風格，成品才會有明顯差異。

延伸閱讀： [【AI工具地圖】做網站總是像 AI 模板？我用 GSAP Skills做出高質感互動網站](https://www.aiposthub.com/claude-gsap-skills/)

### Hairline：會跟著滑鼠動的線條插圖

Hairline 提供等角線條風格的插圖，會跟著滑鼠移動產生反應。

它附帶的 Skill 可以依照你的描述設計新圖形，輸出成單一 HTML 檔：

```
npx skills add lucasmarkes/hairline
```

裝好後輸入 `/hairline-create` 加上你的點子即可。

依官方範例，它的風格集中在單一的線條插圖，人物與長篇敘事不是它的重點。

延伸閱讀： [Hairline 提供 AI 設計技能，把描述轉成可互動線條動畫](https://www.aiposthub.com/hairline-ai-agent-skill-isometric-line-animation/)

### 不裝 Skill 也能先試：SVG 與提示詞

如果只想做 Logo 描邊、Loading 或動態圖表，直接請 Claude 產生 SVG 動畫就夠了。

延伸閱讀： [Claude 動畫教學：用 SVG 製作 Logo、Loading 與動態圖表](https://www.aiposthub.com/claude-svg-animation-logo-loading-chart-tutorial/)

想讓動畫更精準，可以把需求寫成畫面狀態、時間表與驗收條件，比只寫「流暢、高級」有效。

延伸閱讀： [Claude 動畫提示詞怎麼寫？拆解 UI 變形、節奏與無縫循環](https://www.aiposthub.com/claude-animation-prompts-ui-morph-timing-seamless-loop/)

Prompt Motion 收集了大量 Claude 動畫案例，影片旁邊就附上提示詞與 Skill，適合拿來拆解學習。

延伸閱讀： [Prompt Motion 收集 Claude 動畫案例：影片旁邊就能看提示詞與 Skill](https://www.aiposthub.com/prompt-motion-claude-opus-animation-gallery/)

上面這些工具都在從零做出畫面。如果手上已經有拍好的素材，需要的是另一類工具。

## 剪輯與字幕：處理已經拍好的素材

口播、訪談與 Podcast 的工作重點，是剪掉多餘片段、加上字幕。

這類工具多半先產生逐字稿，再讓 AI 根據文字決定剪輯點。

### Claude Code 搭配 HyperFrames

這個做法讓 Claude Code 指揮 HyperFrames 與 FFmpeg，完成初剪、字幕、圖卡與輸出。

自動刪除停頓與 NG 片段不是內建功能，官方示範影片裡的效果，用的是創作者自己寫的剪輯工具組。

延伸閱讀： [Claude Code 剪影片教學：搭配 HyperFrames 自動加字幕、動畫與輸出 MP4](https://www.aiposthub.com/claude-code-hyperframes-video-editing-guide/)

### ChatCut 與 Palmier Pro

ChatCut 可以安裝成 Codex 外掛，用對話操作多軌時間軸，完成口播初剪、繁體中文字幕與配樂。

它目前主要根據逐字稿判斷內容，畫面分析功能還在開發中。

Palmier Pro 是 Mac 剪輯軟體，Claude Code 與 Codex 可以透過 MCP 讀取和修改它的時間軸。

MCP 是讓 AI 連接外部軟體的標準介面，裝好後 AI 就能直接操作那個軟體。

Palmier Pro 只支援 Apple 晶片的 Mac，系統要 macOS 26 以上。

延伸閱讀： [ChatCut with Codex 教學：從 Plugin 安裝到 AI 剪輯、字幕與匯出](https://www.aiposthub.com/codex-chatcut-ai-video-editing-guide/)

延伸閱讀： [Palmier Pro 教學：用 Claude、Codex 生成與剪輯 AI 影片](https://www.aiposthub.com/palmier-pro-ai-video-editor-tutorial/)

🎓

若想用 Claude 桌面版的 Code 分頁，跟著範例完成一支口播影片，可看《Claude AI 口播剪片全攻略》。課程從剪空白、刪重拍到字幕與配樂，示範如何先確認剪輯方案，再逐段驗收。

最後會把流程保存成自己的剪片 Skill，讓下一支影片沿用已確認的做法。

課程預告：2026 年 10 月 30 日上線。 [查看 Skill Shot 14｜Claude AI 口播剪片全攻略課程內容與報名資訊](https://academy.aiposthub.com/p/skill-shot-14-claude-ai-video-editing?ref=aiposthub.com)

### Podcast 粗剪

Podcast 可以先產生帶時間碼的逐字稿，再請 Claude Code 標出贅字、重錄與離題段落。

這份清單仍要人工確認，降噪與混音也得交給其他工具。

延伸閱讀： [AI Podcast 剪輯教學：Claude Code 如何找贅字、重錄與離題片段？](https://www.aiposthub.com/claude-code-ai-podcast-editing-tutorial/)

🎓

音檔整理好後，若想接著做成 YouTube 長片，可看《從 Podcast 音檔到 YouTube 上架》。課程會用 Claude Code 將逐字稿、照片與音檔組成影片。

同一份內容也會延伸成標題、說明欄與標籤，並更新 Google 試算表中的製作進度。

課程預告：2026 年 11 月 3 日上線。 [查看 Skill Shot 15｜從 Podcast 音檔到 YouTube 上架課程內容與報名資訊](https://academy.aiposthub.com/p/skill-shot-15-podcast-youtube-workflow?ref=aiposthub.com)

如果不想自己串接逐字稿、剪輯與字幕這些步驟，下一類工具想把整個流程一次包辦。

## 整套影片代理：從腳本到成片

整套影片代理會自己研究主題、寫腳本、規劃分鏡、生成素材，最後剪成影片。

這類工具涵蓋的步驟最多，設定也比較繁瑣，本站實測過的 OpenMontage 就是如此。

### OpenMontage

OpenMontage 是開源的影片製作系統，可以搭配 Claude Code、Codex 等工具，再用 Remotion 或 HyperFrames 輸出。

它採用 AGPL-3.0 授權，這種授權要求你修改程式並提供給他人使用時，也要公開修改後的原始碼，包含架成網路服務的情況。

本站實測做出一支 15 秒與一支 79 秒的直式影片，滿分 5 分給了 2.5 到 3 分。

最花時間的是環境設定，不同製作流程的結果也不穩定，字幕條無法細調。

適合想研究影片代理怎麼運作的人，還不適合當成穩定的量產工具。

![OpenMontage 製作看板，顯示研究、提案、腳本與分鏡階段已核准，素材與剪輯階段尚未執行](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/claude-code-video-skills-openmontage-board.png)

本站實測 OpenMontage 時的製作看板，研究、腳本與分鏡已核准，素材與剪輯階段還沒開始。 圖片來源： AI郵報實測：OpenMontage 。

延伸閱讀： [AI Agent 影片工具 OpenMontage 實測：真的能接手整套影片製作流程嗎？](https://www.aiposthub.com/openmontage-ai-video-agent-framework-aorus-master-16-gen2-review/)

### Higgsfield MCP 搭配 After Effects

Higgsfield 讓 Claude 透過 MCP 串接影像生成、Blender 與 After Effects，最後交付可以繼續編輯的專案檔。

它需要同時訂閱多個服務，總成本要先算清楚，本站尚未實測。

延伸閱讀： [Higgsfield × Claude Opus 5.5 如何做 Motion Design？從 MCP 到 After Effects 的可編輯工作流](https://www.aiposthub.com/higgsfield-claude-opus-5-5-motion-design-mcp-after-effects/)

## 開工前先拆解：爆紅影片分析

做影片之前，也可以先讓 AI 拆解別人的爆紅短影音，看它的開頭怎麼抓人、段落怎麼安排。

本站實測了 6 款影片拆解 Skill，只有 hook-lab 能直接使用，其他幾款卡在 YouTube 與 TikTok 的防爬蟲機制。

有些工具的替代做法會讀取 Chrome 的登入資料，使用前要確認自己能接受這個風險。

延伸閱讀： [如何一句話拆解「爆紅影片」並複用？6 款 Claude Code Skill 實測評比與使用教學](https://www.aiposthub.com/ai-video-teardown/)

選好工具之後，安裝前還有幾件事值得花幾分鐘確認。

## 安裝前的四個檢查

第一，看授權。

Remotion 對 4 人以上的營利公司收費，OpenMontage 的 AGPL 會影響你能不能把它包成服務，GSAP 則不能拿去做競爭的動畫編輯器。

第二，看權限與隱私。

Claude Code 官方文件說明，Skill 可以預先允許 AI 使用某些工具，也能在載入時執行指令，所以安裝前先打開它的 SKILL.md 看一遍。

外掛還可能附帶 hooks 與 MCP 伺服器，hooks 是在特定動作時自動執行的指令，安裝前要確認它們會做什麼。

`npx skills` 指令預設會傳送匿名使用統計，不想傳送可以設定 `DISABLE_TELEMETRY=1` 。

第三，看系統需求。

HyperFrames 需要 Node.js 22 與 FFmpeg，Cinetic 還要 Python 3.11 與 Chromium，Palmier Pro 限 Apple 晶片的 Mac，HyperFrames Studio 目前沒有 Windows 版。

第四，看成熟度。

Cinetic 在 2026 年 9 月 28 日建立，Hairline 在 2026 年 10 月 1 日建立，兩者都很新，功能和安裝方式可能改動。

## 常見問題

### Claude Code 和 Codex 可以直接做影片嗎？

不能直接輸出 MP4。它們負責寫程式或操作軟體，需要搭配 HyperFrames、Remotion 這類渲染引擎，或 ChatCut、Palmier Pro 這類剪輯工具，才會產生影片檔。

### Claude Code 和 Codex 能用同一個 Skill 嗎？

多數可以。用 `npx skills add` 安裝時，工具會依你使用的 AI 放到對應資料夾。少數 Skill 只提供 Claude Code 外掛版本，安裝前看一下專案說明。

### 不會寫程式也能用 Claude Code 做影片嗎？

可以從對話開始，但仍要能在終端機貼上安裝指令，並看懂錯誤訊息。想少碰指令，可以先試 HyperFrames Studio 這類有圖形介面的桌面版，或在 Codex 裡用 ChatCut 外掛。

### 用這些 Skill 做影片要付費嗎？

多數 Skill 本身免費，但 Claude Code 或 Codex 需要訂閱方案，或依使用量付費。素材生成服務另計，Remotion 也有公司授權規定。

### Skill 和 MCP 差在哪？

Skill 是教 AI 怎麼做事的說明檔，MCP 則是讓 AI 連接外部軟體的介面。剪輯軟體通常用 MCP 讓 AI 操作，動畫風格則多用 Skill 提供做法。

## 結語

要從零做動畫，先選一個渲染引擎，再加風格 Skill。已經有拍好的素材，就直接看剪輯與字幕工具，想先研究別人的影片則用 hook-lab。

如果只能先試一個，想照現成教學做出知識動畫，就從本站實測過的 Codex 搭配 Remotion 開始。想讓 AI 像改網頁一樣修改畫面，就選 HyperFrames。

一次只裝一兩個，出問題時也比較好排查。本頁會隨著新工具與本站實測持續更新。

延伸閱讀： [Codex + Remotion 教學：3分鐘教你使用 AI 製作動畫](https://www.aiposthub.com/codex-remotion-ai-animation/)