---
title: HyperFrames（HeyGen）：用 HTML 寫影片、輸出 deterministic MP4 的開源框架，三條安裝路徑與 Studio 桌面版
date: 2026-10-19
tags: [ai/agent, ai/workflow, type/how-to]
source:
  - https://github.com/heygen-com/hyperframes
  - https://www.aiposthub.com/heygen-hyperframes-30-days-day-1-12/
  - https://www.aiposthub.com/hyperframes-studio-app-codex-claude-code-video-editor/
  - https://www.aiposthub.com/claude-code-video-skills-2026/
  - https://des13.com/news/ai/1734-claude-opus.html
official_refs:
  - https://github.com/heygen-com/hyperframes（README，2026-10-19 Team Lead 核對）
  - https://hyperframes.dev/studio（下載頁，經 aiposthub 轉述，未核對）
  - https://hyperframes.heygen.com/packages/cli（CLI 文件，經 aiposthub 轉述，未核對）
  - https://hyperframes.heygen.com/studio-app/index（Studio App 文件，經 aiposthub 轉述，未核對）
type: article
status: processed
asset_value: 64
---

> ✅ **骨幹是官方 README（Team Lead 2026-10-19 核對）**。下文凡標「✅ README」者為一手；其餘依序標「aiposthub 轉述」「作者實測」「作者觀點」。
>
> 🔴 **來源集中**：5 個來源中 aiposthub／Philo 佔 3 篇（Day 1～12、Studio、Skills 推薦），同站互為延伸閱讀，只算**一個轉述來源**，而且三篇都**沒有實測 HyperFrames**（Skills 推薦的總表自己標「未實測，有教學文章」）。**唯一的作者實測是益盛科技**（自家公司與客戶案例）。
>
> ⚠️ **更正（2026-10-19）**：本批 Agent A 原本認為三篇的安裝說法不一致；經 README 核對，三種寫法是**官方並列的三條安裝路徑，不是矛盾**。Claude Code 外掛指令也已由 README 證實，不再是單一來源。

## T（核心論點）

HyperFrames 是 HeyGen 維護的開源框架，把 HTML、CSS、媒體與可 seek 的動畫轉成 **deterministic MP4**（✅ README："Write HTML. Render video. Built for agents."）。它的定位是**讓 coding agent 改影片專案**：畫面、時間、字幕都是專案裡的檔案，所以同一個片頭、字幕樣式或場景可以留下來，下一支影片繼續改。作者的評價是，好處會隨產量變大——它適合固定量產同類影片、已經在用 coding agent 的團隊；偶爾做一支片的人，用圖形介面工具比較快（aiposthub 作者觀點）。

## B（背景）

**適用（aiposthub 作者觀點）：**
- 每週固定做功能發布、數據更新、教學短片、企業新聞的團隊：品牌文件、字幕元件、舊場景都能重用
- 已經在用 Claude Code、Cursor 或 Codex 的創作者：在原本的對話環境管理素材、改檔、輸出不同版本

**不適用／注意：**
- 只想偶爾快速做一支影片的新手：仍要處理 Node.js、FFmpeg、專案資料夾和終端機；CapCut、Canva 或 HeyGen 原本的圖形介面可能更快（作者觀點）
- 長篇訪談、複雜調色、多機剪輯、精細混音：仍是 Premiere Pro、Final Cut Pro 這類 NLE 比較擅長（作者觀點）
- 🔴 **剪口播自動刪停頓、刪 NG 不是內建功能**：官方示範影片的效果，用的是創作者自己寫的剪輯工具組（aiposthub 轉述）。這是實用的期望校正
- 研究正確性、品牌取捨、肖像、素材授權、成品 QA 仍要人負責；官方案例裡的新聞與數字只是展示，用之前要自己查證（作者提醒）

## R（關鍵細節）

### 1. 官方事實（✅ README，Team Lead 2026-10-19 核對）

| 項目 | README 內容 |
|---|---|
| 定義 | 把 HTML、CSS、媒體與可 seek 的動畫轉成 deterministic MP4；"Write HTML. Render video. Built for agents." |
| 維護者 | HeyGen（heygen-com），內部生產使用 |
| 授權 | Apache 2.0 |
| 系統需求 | Node.js 22+、FFmpeg |
| CLI 流程 | `npx hyperframes init my-video` → `npx hyperframes preview` → `npx hyperframes render` |
| Studio | 瀏覽器預覽／編輯介面，狀態 "Available, evolving"，連到 hyperframes.dev/studio；header GIF 的替代文字說 native Mac 與 Linux app 已推出；**README 未提 Windows，也沒有給 app 的安裝指令** |

- 「確定性渲染」：rar.design 寫「HyperFrames 把確定性渲染列為核心設計」，與 README 的 "deterministic MP4" 一致 ✅（rar.design 的完整脈絡見 [[claude-code-pure-code-animation-practice]]）

### 2. 安裝：三條路徑，官方並列、不互斥（✅ README）

| 路徑 | 指令 | 用途 |
|---|---|---|
| ① Claude Code 外掛 | `claude plugin marketplace add heygen-com/hyperframes`<br>`claude plugin install hyperframes@hyperframes` | 裝好後用 `/hyperframes:hyperframes` |
| ② Skills CLI | `npx skills add heygen-com/hyperframes` | 互動挑選；`--all` 裝全部 21 個 skill，`--skill <name>` 只裝一個 |
| ③ agent／非互動 | `npx hyperframes skills update` | README 建議 agent 用這條 |

- aiposthub 補充（轉述）：`npx skills` 讀的是第三方技能目錄 skills.sh 的副本，可能比 GitHub 晚幾個小時，要最新就用 `npx hyperframes skills update`——與 README「agent 用 skills update」的建議方向一致
- 益盛的實測只跑路徑 ③（作者實測）；Day 1～12 文的安裝段標「截至 2026-10-11」，是 07-18 發布後補寫的，本條一律以 README 為準

### 3. HyperFrames × Codex：三種說法並列，不擇一

| 出處 | 說法 |
|---|---|
| ✅ README | **沒有 Codex 安裝指令**，只描述 `bun run package:codex-plugin` 會產生 `dist/hyperframes-plugin.zip` |
| aiposthub 轉述 Day 1 貼文 | Skills 可以裝到 Claude Code、Cursor、Codex |
| aiposthub〈Skills 推薦〉 | 「Codex 等其他工具」用 `npx skills add heygen-com/hyperframes` |
| [[codex-plugins-12-guide]] | 把「HyperFrames by HeyGen」列為 Codex 插件 |

### 4. HyperFrames Studio（桌面版）

**一手層（✅ README）**：Studio 是瀏覽器預覽／編輯介面，"Available, evolving"；header GIF 替代文字稱 Mac 與 Linux 原生 app 已推出。

**轉述層（⚠️ aiposthub 單一來源，查詢日 2026-10-06，作者明言未實測、操作情境是說明範例）：**
- 發布：2026-10-06（台灣時間）HyperFrames 官方 X 貼文；文中稱 Studio「由開源 HyperFrames 的團隊推出」
- 下載頁：macOS、Linux（x86_64 AppImage）；Windows／Windows arm64 標示 Coming soon
- 兩段登入：App 用 HeyGen 帳號；agent 另外登入——Claude Code 用 Claude 或 Anthropic Console 帳號，Codex 用 ChatGPT 帳號
- Studio 可協助安裝 Claude Code；Codex 要自己裝；兩者都有時預設 Claude Code，可在聊天區選單或 Settings > Agent 切換
- 費用：AI 用量計入 agent 帳號的方案，也可改用 API key；框架 Apache-2.0，但模型與素材不等於免費；官方沒有每支影片的統一總價
- 第一支影片引導：Hook／Proof／Turn／Call to action 四段；輸入網站可抓字型、配色、標誌
- 編輯：Select、Draw、Comment；送出前先進 Pending video edits，**上限 10 項**；時間軸可拖動、修剪、分割、刪除，Quick edit 調速度、音量、淡入淡出；每次 agent 執行有獨立 Undo
- 匯出：MP4／WebM／MOV，24／30／60 fps；預設 1080p MP4 30fps；4K 要專案符合條件才會出現；AI 還在建專案時不能 Export；存到 Downloads
- 專案存在本機的 HyperFrames Studio 資料夾，可用網頁編輯器或 CLI 開啟；但 agent 仍要連線，**不等於完全離線**

**Windows 並列（不擇一）**：README 未提 Windows ／ aiposthub 轉述下載頁「Coming soon」（10-06 查詢）／〈Skills 推薦〉寫「目前沒有 Windows 版」。

- Studio 也是 Claude Motion「Send to → HyperFrames」交接的目的地（單一來源轉述，見 [[claude-motion-beta-2026-10]]）

### 5. 30 Days of HyperFrames：Day 1～12（aiposthub 轉述 HeyGen X 貼文與官方文件，作者未實測）

| Day | 主題 | 重點 |
|---|---|---|
| 1 | 安裝 Skills | 讓 agent 拿到固定的製作流程（格式、時間軸表示法、完成後的檢查、輸出方式） |
| 2 | 品牌 Motion Language | `DESIGN.md`（顏色、字型、語氣）＋ `FRAME.md`（節奏、進場、轉場、動態風格） |
| 3 | PR-to-Video | 讀 PR 標題、說明、diff、commit、reviewer，做成解說動畫 |
| 4 | Music-to-Video | 先分析 BPM、重拍、段落、能量、停止點，畫面與音效共用同一份 timing map |
| 5 | Faceless Explainer | 從題目／文章／筆記做無真人解說；研究正確性仍要人工審 |
| 6 | Talking-Head-Recut | HeyGen Avatar＋lip-sync，再轉錄成逐字時間，讓圖卡對上說話內容 |
| 7 | Motion Graphics | 趨勢圖、Logo、數字卡、資訊卡、系列片頭五種 |
| 8 | HeyGen CLI 音樂與 SFX | 從終端機搜尋素材、放進時間軸（github.com/heygen-com/heygen-cli） |
| 9 | Figma Import | 匯入圖層、圖片、SVG、品牌變數、storyboard，存到本機專案，輸出時不必連 Figma（hyperframes.heygen.com/guides/figma） |
| 10 | Prompt 結構 | 六段：Route／Spec／Beats／Copy／Technique／Negatives |
| 11 | 重用舊場景 | 官方把部分發布影片的原始專案放在 github.com/heygen-com/hyperframes-launches；示範把 HeyGen × Stripe 的文字輪播改成 Tesla |
| 12 | 動態字幕 | 逐字時間＋caption component；可用官方 Catalog（hyperframes.heygen.com/catalog）或自建品牌樣式 |

- 作者建議一般創作者先看 Day 2、5、10、11；**Day 10 最值得先讀**，因為「把需求拆成尺寸、節點、文案、技法與禁止事項」對任何 AI 影片工具都適用（作者觀點）
- Day 10 六段範例（轉述）：「做一個資料夾打開的動畫」只說了物件；補上 6 秒、正方形、游標從右下移入、第二秒按兩下、標籤固定顯示 Projects、不要旁白，agent 才排得出時間軸

### 6. 四層可重用模型（aiposthub 作者觀點）

| 層級 | 對應內容 | 可以重用什麼 |
|---|---|---|
| 製作規則 | Skills、Prompt 結構 | agent 的工作流程與檢查方法 |
| 品牌規則 | DESIGN.md、FRAME.md | 顏色、字型、語氣、動態語言 |
| 影片元件 | Motion Graphics、字幕、舊場景（composition） | 片頭、資訊卡、字幕、轉場 |
| 素材與資料 | Figma、PR、音樂、SFX、逐字稿 | 設計稿、程式變更、音訊與時間資訊 |

🔑 「真正降低成本的不是第一支影片少剪幾分鐘，而是第十支影片可以沿用前九支累積的元件」（作者觀點）。

### 7. 授權與費用

- 核心框架 Apache 2.0 ✅ README
- 若工作流另外呼叫 HeyGen Avatar、API、雲端服務或付費素材，可能另外收費（aiposthub 轉述）
- Studio 的 AI 用量計入 agent 帳號方案（aiposthub 轉述，見第 4 節）

### 8. 作者實測：益盛科技四支品牌動畫（林小鈴／des13，2026-09-29）

> 作者實測，**自家公司與客戶案例**（益盛科技＝作者公司，iChurch＝客戶）。工具：Claude Code＋Opus 5.5＋Node.js＋FFmpeg，執行一次 `npx hyperframes skills update`（即 README 建議的 agent 路徑 ③）。製作時間＝貼上提示詞到最後一次模型回應，含過程中的來回對話，**不含人工審片**；token 是從對話紀錄把每次模型回應的用量加總。

| 影片 | 規格 | 提示詞 | 製作時間 | 輸出 token | 總計 token |
|---|---|---|---|---|---|
| iChurch 40 秒橫式 Showreel | 1920×1080 | **一段英文**，無分鏡、無素材 | 25 分 | 7.5 萬 | 1,296 萬 |
| iChurch 15 秒直式短片 | 1080×1920、30fps | **詳細中文分鏡**＋四張 8-bit 角色圖＋網站截圖 | 23 分＋修改 4 分 | 8.3 萬 | 1,068 萬（修改 176 萬） |
| 益盛 40 秒 Showreel | 1920×1080・60fps | 沿用 iChurch 40 秒的英文提示詞，只換網址 | 39 分 | 6 萬 | 943 萬 |
| 益盛作品集 Showreel | 1920×1080・60fps | 中文提示詞（研究官網作品案例，做約 40 秒 showreel） | 41 分＋修改 12 分 | 4.5 萬 | 1,142 萬（修改 394 萬） |

- iChurch 40 秒的英文提示詞只有兩句：Research 網址＋"Make a dynamic 40-second motion graphics video … like it's your showreel. Go all out."；Opus 5.5 自己截了 8 個頁面，用 4,957 間教會的真實座標拼出台灣地圖（作者實測）
- iChurch 15 秒：Opus 5.5 把截圖拆成可單獨移動的元件，用 HTML＋GSAP 寫動畫、Python 合成原創配樂；第一版角色擋到網址，一句修改指令後 4 分鐘出新版（作者實測）
- 益盛 40 秒：背景音樂取自 Mixkit 免費授權曲庫，並附授權紀錄（作者實測）
- 益盛作品集：第一版網頁捲動太快，修改指令「不要往下捲動」，12 分鐘後出新版（作者實測）
- 🔑 **總 token 大多是快取讀取**：模型每做一步，都把前面的對話再讀一次；真正寫出的程式與回覆是「輸出 token」那欄（作者說明）。本條計算：四支的輸出 token 只佔總計約 0.4%～0.8%。要比較成本，請用 [[claude-opus-5-5-release-notes]] 的「完成一個驗收合格任務的成本」來量，不要拿總 token 直接比
- 兩種提示詞寫法（作者建議）：想讓成果照你的規劃走，就把分鏡寫細；想看 Opus 5.5 自己怎麼拍，就只給一句話。兩種寫法的整理見 [[claude-code-pure-code-animation-practice]]

> ⚠️ **原文編號自相矛盾**：原文小節編號是「1. iChurch 40 秒（英文一段提示詞）」「2. iChurch 15 秒（詳細分鏡）」，但 token 表的列序是 15 秒在前、40 秒在後。正文另外三處——第 3 支「使用**第二支**的同一段英文提示詞」、結尾「把**第二支**那段英文提示詞換上你的網址」「就像**第一支**一樣把分鏡寫細；……就像**第二支**一樣只給一句話」——用的編號與小節編號相反（剛好符合表格列序）。**本條依各節實際內容判讀：一段英文提示詞＝iChurch 40 秒，詳細分鏡＝iChurch 15 秒**，不沿用原文的「第一支／第二支」稱呼。

- 原文另附「Opus 5.5 首週爆紅 12 個用例排名」，沒有任何貼文連結或作者名，觀看數無法查核，**本條不收**

### 9. 社群案例（原作者自報，經 rar.design 轉述）

- AI 實測創作者 Vox 的〈small print〉用 HyperFrames，整支片寫在一個 `index.html`，配樂由 Opus 5.5 用 Python 合成；配樂合成後還回頭逐秒檢查渲染結果再修一輪。脈絡見 [[claude-code-pure-code-animation-practice]]
- rar.design 的 brief 範本：要輸出影片就在 Claude Code 跑，最後加一句「用 HyperFrames 渲染成 MP4」，第一次用前先照官方說明裝好 skills（作者觀點）

## C（連結與應用）

- [ ] 想試的話，先選 README 的路徑 ③（`npx hyperframes skills update`）或 Claude Code 外掛 ①，兩者不需要都裝；先確認 Node.js 22+ 與 FFmpeg
- [ ] 第一支用 `init → preview → render` 做一支 10～15 秒短片，記錄製作時間、修改次數、輸出 token（照益盛的記法），再決定要不要建 `DESIGN.md`／`FRAME.md`
- [ ] 寫提示詞先套 Day 10 六段（Route／Spec／Beats／Copy／Technique／Negatives）
- [ ] 量產時把字幕、片頭做成 component／composition，下支片直接改，不要每次重來
- [ ] 不要期待它自動剪口播（刪停頓、刪 NG 不是內建）
- [ ] 待追蹤：Codex 端正式安裝方式（README 目前只有打包 zip 的描述）；Studio Windows 版；Studio 的 Pending edits 上限與 4K 條件（目前只有單一轉述）

## 原始摘要

- **GitHub README（heygen-com/hyperframes，Team Lead 2026-10-19 核對）**：定義、Apache 2.0、Node.js 22+＋FFmpeg、三條安裝路徑、CLI、Studio "Available, evolving"、Codex 只有打包描述。
- **aiposthub〈HeyGen HyperFrames 是什麼？Day 1～12 教學與 AI 影片工作流〉（Philo，2026-07-18 發布，安裝段標「截至 10-11」）**：Day 1～12 主題、四層可重用模型、適用對象、FAQ。全文轉述 HeyGen X 貼文與官方文件，作者未實測。站內延伸閱讀未收。
- **aiposthub〈HyperFrames Studio 是什麼？…〉（Philo，2026-10-06）**：Studio 平台、登入、費用、編輯工具、匯出條件。作者明言未實測；作者自擬的課程短片提示詞未收；文末 AI 郵報學院課程導流未收。
- **aiposthub〈2026 Claude Code 做影片 Skills 推薦〉（Philo，2026-10-11）的 HyperFrames 段**：外掛指令、skills.sh 副本延遲、Studio 無 Windows、剪口播非內建。其餘工具見 [[claude-codex-video-toolchain-selection]]；Skill Shot 14／15 課程推廣未收。
- **益盛科技〈用 Claude Opus 5.5 做出四支品牌動畫…〉（林小鈴，2026-09-29）**：四支片的提示詞、時間、token（作者實測）。Threads 串文瀏覽數（作者自報、與方法無關）、「12 個爆紅用例排名」（無出處）、LINE 諮詢／AI SEO／網頁設計／電商服務導流，皆未收。

## 相關連結

- [[claude-codex-video-toolchain-selection]] — 工具選型總表：HyperFrames vs Remotion、風格 Skill、剪輯工具、安裝前四檢查
- [[claude-code-pure-code-animation-practice]] — 實作工作法：由時間算畫面、讓模型看樣片、先分鏡再分工；渲染層用框架或自寫截圖＋FFmpeg 都有實測
- [[claude-motion-beta-2026-10]] — Claude Motion 的「Send to → HyperFrames」交接，目的地就是本條的 Studio
- [[codex-plugins-12-guide]] — 「HyperFrames by HeyGen」列為 Codex 插件（與 README 的 Codex 描述並列）
- [[open-design-claude-design-comparison]] — Open Design 內建 HyperFrames HTML → MP4 輸出
- [[claude-opus-5-5-release-notes]] — 「完成一個驗收合格任務的成本」；對照益盛的 token 實數
- [[ideas/claude-code-animation-prompt-qa-delivery]] — 程式動畫的需求、驗收與交付格式（HTML／SVG／MP4）
