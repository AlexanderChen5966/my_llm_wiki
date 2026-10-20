---
title: Claude 程式動畫的需求、驗收與交付：狀態清單＋時間表、由時間算畫面、HTML／SVG／MP4 依用途選
date: 2026-10-19
tags: [ai/claude, ai/prompt-engineering]
source:
  - https://www.aiposthub.com/claude-animation-prompts-ui-morph-timing-seamless-loop/
  - https://www.aiposthub.com/claude-svg-animation-logo-loading-chart-tutorial/
  - https://www.aiposthub.com/claude-animation-html-svg-mp4-delivery-formats-guide/
  - https://www.aiposthub.com/prompt-motion-claude-opus-animation-gallery/
type: idea
status: processed
asset_value: 29
---

> ⚠️ **本條談 Claude／Opus 5.5 寫程式做動畫（HTML、SVG、CSS、Playwright 逐張渲染），不是 Claude Motion 產品**；Motion 產品見 [[claude-motion-beta-2026-10]]。
> ⚠️ **Prompt Motion（prompt-motion.com，作者 p4n）是第三方案例網站**，與 Anthropic 的 Claude Motion 無關，只是名稱相近。
>
> 🔴 **來源集中**：四篇都是 aiposthub／Philo（2026-10-02～10-09），互為延伸閱讀，**單一作者、互引不構成佐證**；四篇都**沒有附實測輸出**（提示詞、交付兩篇明言未實測），案例與時間表是作者假設。一手資料只有 MDN 文件、Prompt Motion 網站與 p4n 的 X 公告。
> 為何放 ideas：四篇 asset_value 分別為 29（提示詞）／22（SVG 教學）／22（交付格式）／10（Prompt Motion），合併後仍是單一作者的方法論，待自己實作驗證。
>
> 📎 2026-10-19：本條方法論（由時間算畫面、先做短樣本、驗收迴圈）已有 **3 個獨立作者實測**（rar.design、數位時代、益盛科技），實作版另立 wiki [[claude-code-pure-code-animation-practice]]；本條保留，作為提示詞與交付格式的方法論。他人實測已出現，但 C 層「自己做過一輪再決定」的待驗證仍有效。工具選型與 Remotion 授權另見 [[claude-codex-video-toolchain-selection]]。

## T（核心論點）

用 Claude 寫程式做動畫，提示詞要把形容詞（流暢、有質感、科技感）換成**狀態清單＋時間表＋驗收條件**；技術上，🔑 **畫面要由時間計算，不依賴前一張畫面留下的狀態**，逐張渲染才能一致、可以重現，也才能單獨檢查某一秒。交付時先問清楚用途，再選 HTML、SVG 或 MP4——**MP4 不能取代可編輯的來源**。

## B（背景）

- 適用：Logo 描邊、Loading、動態長條圖、UI 狀態變形、產品功能短片；自己控制程式來源、要能反覆修改的動畫
- 不適用：寫實人物、複雜材質、大量剪輯的影片（SVG 教學文自己也這樣說）
- 與 Claude Motion 的差別：Motion 是 Team／Enterprise 的產品功能，由 Claude 代管程式；本條是自己請 Claude 或 Claude Code 寫出可下載、可版控的來源檔。Artifact 的方案與前置開關（雲端程式執行與檔案建立）見 [[claude-opus-5-5-release-notes]] 第 8 節，此處不重寫

## R（關鍵細節）

### 1. 需求：五欄＋狀態清單＋時間表

- **五欄需求**（SVG 教學文）：主體／畫布／動作／順序／輸出。例如「單一 HTML，內含 SVG、樣式與必要程式，不連外抓素材」
- 三組入門範例（作者自擬）：Logo 描邊 2 秒＋文字淡入 0.5 秒；三點 Loading 錯開開始時間；示範資料 A=20／B=40／C=60 的動態長條圖（附文字版資料表）
- **狀態清單**（提示詞文）：先寫每個狀態的靜態描述，再寫兩個狀態之間怎麼變（位置、大小、圓角）；指定什麼要**持續存在**（主容器、按鈕中心、游標），觀眾才追得上
- **時間表**：BPM 120 → 一拍 0.5 秒，四拍一小節 2 秒；作者自擬「16 秒、八個 2 秒區段」的範例（作者觀點，假設案例）
- 提示詞流程：**先交八個狀態的靜態圖與轉場規劃，確認後才做動態**；交付時附可修改的來源＋影片，並說明需要的字型與音樂

### 2. 可重現：由時間算畫面、無縫循環

- 案例：Prompt Motion 收錄的 @twoclipping〈Shape morphing through UI states〉，案例頁標示使用 HTML + Playwright、模型 Opus 5.5；原作者要求**由時間計算畫面**，以利逐張渲染一致（媒體轉述案例頁；作品與提示詞版權屬原作者，本條不轉貼全文）
- 想像成查時間表：給定秒數，就知道主形體、文字、游標應該在哪裡，不必從第一秒重播
- 🔑 **無縫循環要接上狀態，也要接上速度**：首尾位置相同還不夠，結尾快速向右、開頭卻停住，仍會出現頓點；連續播放數輪、特別看接縫前後；音訊尾音也可能形成接縫
- 與 [[research/pixel2motion-agent-skill-plus-scripts]] 互補：那邊的 **Final Frame Contract**（結束時必須回到已驗收的靜態 SVG）與「固定毫秒取樣排成 motion strip」，正好是「由時間算畫面」的驗收手段

### 3. 三輪驗收（作者觀點）

| 輪次 | 看什麼 | 常見問題 → 修改方向 |
|---|---|---|
| ① 資訊（靜態） | 主要文案、層級、順序 | 看完不知道完成什麼 → 減少次要狀態 |
| ② 動作（正常速度） | 主體好不好追、文字讀不讀得完 | 主形體跳位 → 指定前後位置與轉場 |
| ③ 交付 | 尺寸、長度、音訊、循環、來源可重開 | 循環時閃一下 → 比較接縫的狀態、速度、音訊 |

- 修改一次只指一個對象與一個結果（「把圓點上下移動距離減半，其他保持原樣」），附時間點、預期與實際畫面
- 可及性：`prefers-reduced-motion` 要由製作方自己實作，不會因為用了某個模型就自動包含（MDN 一手＋作者觀點）；描邊動畫用 `stroke-dashoffset` 搭配虛線長度（MDN 一手）
- 「提示詞＝這次要做什麼；Skill＝這類工作怎麼完成」：常做同類短片時，把字型、尺寸、狀態規劃與驗收方法整理成 Skill

### 4. 交付：先問三題，再選格式

先回答：**在哪裡看？要不要互動？誰會修改？**

| 格式 | 適合 | 交付與驗收重點 |
|---|---|---|
| HTML | 網站互動、教學工具 | 交代依賴（字型、圖片、程式庫、外部資料）；放到乾淨位置重開確認 |
| SVG | 可縮放圖形、線條動畫、Logo、圖表 | 嵌入方式會影響腳本與互動；文字保留還是轉路徑，要確認字型 |
| MP4 | 社群、簡報、固定時間播放 | **另附製作來源**；MP4 是容器，能不能播看內部編碼（MDN）；確認比例、聲音、播放裝置 |
| GIF | 短小、無聲循環 | 色彩與檔案大小受限；**不跟隨 reduced-motion**，重要資訊另給靜態版（作者觀點） |

- HTML 轉 MP4 要錄製，**互動不會保留**
- 瀏覽器可能限制有聲自動播放（MDN）——與 [[claude-motion-beta-2026-10]] 中 vincent 單檔 HTML 要先按「開啟聲音」是同一件事
- 🔑 **「可編輯」可能指三件事：提示詞可改、程式來源可改、能輸出專案檔**，三者不能互相替代；交接時請接收者實際做一項常見修改（例如標題 4 字改 8 字），確認來源真的接得住
- ⚠️ SVG 教學文說「設計畫布可 Export 獨立 HTML」，屬媒體轉述、未附出處；文中插圖是 ImageGen 生成的示意圖，不是官方截圖

### 5. 參考案例庫：Prompt Motion（第三方網站）

- p4n 於 2026-10-06（台灣時間）在 X 公告：從 X 貼文整理了 **226 個** Opus 5.5 動態設計案例並標註創作者（數字是公告當時的）；網站有 Prompt／Skill／Popular 篩選
- 用法：**從成品回推需求**，而不是照抄提示詞；記錄重現環境（模型、工具、字型、比例、輸出要求），中文內容要重新排版
- 🔑 **看得到提示詞和影片，不代表可以商用**，授權要另外向創作者確認

## C（連結與應用）

- [ ] 下次做 Logo／Loading 小動畫時，用「五欄＋狀態清單」寫需求，先要靜態關鍵畫面再做動態
- [ ] 要輸出 MP4 的程式動畫，要求「由時間算畫面」，並用 Pixel2Motion 式的固定毫秒取樣做驗收
- [ ] 交付包：播放檔＋可修改來源＋素材授權說明，並用一項常見修改測試來源
- [ ] 待驗證：本條方法論全部來自單一作者、未實測，自己做過一輪後再決定要不要升 wiki 或寫成 Skill（可與 [[ideas/lottie-animation-skill]] 的 motion-design-skill 一起評估）

## 原始摘要

- **aiposthub〈Claude 動畫提示詞怎麼寫？拆解 UI 變形、節奏與無縫循環〉（Philo，2026-10-08，主幹）**：以 @twoclipping 案例為起點，談狀態清單、時間表、由時間算畫面、無縫循環、三輪驗收；與 Claude Motion 產品無關（只是剛好同一天發布）。未實測；文末導流未收。
- **aiposthub〈Claude 動畫教學：SVG Logo、Loading、圖表與輸出〉（Philo，2026-10-02）**：Motion 上線前的入門文；五欄需求、三組提示詞、修改句型、HTML／MP4／GIF 選擇。academy Flash Lesson 09（SCENE 提示法）課程推廣未收。
- **aiposthub〈Claude 動畫做完怎麼交付？HTML、SVG 與 MP4 的用途和檢查重點〉（Philo，2026-10-09）**：三問選格式、「可編輯」三義、MDN 一手引用；插圖用的是 Prompt Motion 畫面，文章並非專講 Claude Motion。academy HyperFrames 課程推廣未收。
- **aiposthub〈Prompt Motion 收集 Claude 動畫案例：影片旁邊就能看提示詞與 Skill〉（Philo，2026-10-07）**：介紹第三方案例站與使用方式、授權提醒。未附實測。

## 相關連結

- [[claude-motion-beta-2026-10]] — Anthropic 的 Claude Motion 產品（Team／Enterprise beta），與本條的自寫程式動畫是兩條路
- [[research/pixel2motion-agent-skill-plus-scripts]] — 🔑 強互補：Final Frame Contract、固定毫秒取樣做 Motion QA
- [[ideas/lottie-animation-skill]] — 另一種動畫產出格式（Lottie JSON）與「motion-design-skill 當導演」的分工
- [[frontend-agent-skills-30-picks]] — `remotion-best-practices`、`fixing-motion-performance` 等程式影片／動效 Skill
- [[html-as-ai-output-interface]] — 單一 HTML 當交付物的讀者端價值（本條交付段補上「HTML 要交代依賴、MP4 不能取代來源」）
- [[claude-opus-5-5-release-notes]] — Opus 5.5 互動教學頁範本與 Artifact 方案、前置開關
- 📎 2026-10-19：[[claude-code-pure-code-animation-practice]] — 本條的實作版（3 個獨立實測：縮圖總表＋膠卷自查、骨架＋子代理分工審查、另找 AI 驗收）；[[hyperframes-heygen-html-to-video]] — HyperFrames 專篇；[[claude-codex-video-toolchain-selection]] — 渲染引擎與剪輯工具選型
- 📎 2026-10-19：[[opus-5-5-video-case-libraries]] — Opus 5.5 影片案例庫（yihui-dev 475 組提示詞收藏 vs athemeroy 研究快照）；拿案例庫提示詞改寫時，先確認提示詞是否完整、製作路徑是否真的是程式繪製
