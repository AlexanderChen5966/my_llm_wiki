---
title: 給 agent 讀的設計畫布：pen.dev（原 Pencil）與 Paper.design
date: 2026-11-19
tags: [ai/mcp, ai/workflow, type/comparison]
source: https://rar.design/posts/pen-dev-website-import-agentic-canvas（rar.design／設計師 Riven〈Pencil 改名 pen.dev 變了什麼？現成網站匯入、六個 agent 比稿與 .pen 格式〉，2026-09-23；主幹，有對照官方文件與限制）
source_2: https://www.aiposthub.com/pen-dev-agentic-canvas-guide-web-import-to-code/（aiposthub／Philo〈前 Adobe 總監打造！全新 pen.dev 震撼登場〉，2026-09-19；功能細節，行銷語氣重）
source_3: https://www.aiposthub.com/paper-svg/（aiposthub／Joanna Chang〈Paper.design 是什麼？超酷新技術，一鍵生成 SVG 檔〉，2026-09-24；作者單次操作示範）
source_4: https://medium.com/@simon3458/open-design-frame-32bcd0bd2e1f（Medium／Simon Liu〈open-design-frame〉，2026-09-10；補充資料）
official_refs:
  - https://pen.dev
  - https://docs.pen.dev
  - https://paper.design
type: article
status: processed
asset_value: 102
---

三篇都在講同一類工具：設計畫布不再只給人看，而是做成 **agent 讀得懂、改得動、能接回程式碼** 的格式。pen.dev 走「設計檔是 repo 裡的 JSON」，Paper 走「畫布本身就是 HTML／CSS」。

> ⚠️ **來源性質**：三篇皆二手整理，本庫**未自行安裝試用、未回官方文件逐項核對**。aiposthub pen.dev 文語氣偏行銷（「100% 映射」「完全開源」等說法 rar.design 沒有背書，見下方並陳）；Paper 文是作者一次操作示範，非評測。功能、價格（目前免費）、平台支援皆為 **2026-09 下旬快照**。

## T（核心論點）

> **這類工具的差別不在「AI 畫得多漂亮」，而在設計結果能不能直接往下一步走：從現成網頁開始、存成 agent 讀得懂的檔案、改完推回程式碼。**
> 代價是團隊協作那一塊（即時共編、留言、prototype）還沒有，適合「一個人帶一群 agent 做完設計到前端」的人，不是取代 Figma 的團隊主場。

兩條路線：

| | pen.dev（原 Pencil，High Agency） | Paper.design |
|---|---|---|
| 畫布底層 | `.pen`＝JSON 物件樹（類 HTML、flexbox 排版、reusable＝元件），可進 Git | 建立在 HTML／CSS 概念上，版面本身就接近網頁結構 |
| agent 怎麼進來 | 內建 agent／MCP（Claude Code、Codex、Gemini CLI 等；連線清單仍顯示舊名 `pencil`）／CLI | MCP 讓 Codex、Claude Code、Cursor 讀選取物件、節點結構與樣式，可改文字、調 CSS、建畫板、寫入 HTML |
| 本批重點功能 | 網頁匯入成可編輯圖層、最多六個 agent 平行出稿 | 圖片＋描述 → AI 重畫成 SVG |

## B（適用場景）

**適用：**
- 一個人包辦設計和前端、手上多是**網站改版**、已經在用 Claude Code／Codex 的人（pen.dev）
- 改版案前一兩天常耗在「把線上畫面重建成設計檔」——匯入可以直接跳過這段
- 想拿同一個 brief 比較哪家模型最抓得到調性（pen.dev Side by Side）
- 需要一批風格統一的插圖、裝飾元素、Icon 方向（Paper SVG）

**不適用 / 注意：**
- 設計師主導、要跟 PM／客戶在同一份檔案來回、要 prototype 驗證流程的團隊 → Figma 還是主場（兩者可並用，`.fig` 能整份匯入 pen.dev）
- 本人工作主線是 Figma → Flutter（見 [[figma-ai-make-mcp-design-agent]]、[[claude-code-figma-mcp-write-setup]]）；pen.dev 匯出走 HTML＋Tailwind／CSS 或請 agent 產 React／Next.js／shadcn，**沒有提到 Flutter**
- Logo、正式品牌識別、要精準幾何的圖形，Paper 產的 SVG 仍要人工整理

## R（操作步驟與重點）

### 1. pen.dev：匯入 → 迭代 → 推回程式碼

- **匯入**：桌面版上方地球圖示開內建瀏覽器，輸入網址（含 `localhost`）→ 整頁 Import，或切選取模式只抓某個區塊；也可用 Chrome 擴充「pen.dev web import」複製後貼上；只要參考圖就截圖
- **匯入三個坑**：不帶 JavaScript 互動（選單、動畫要自己重接）；字型與特效要對照原頁檢查；登入後頁面先確認沒有私人資料
- **授權**：官方說法是匯入靈感或**你自己擁有的網站**；別人的版面、圖片、文案改一改當成作品交出去有侵權風險
- **迭代**：平行 agent 一次 1～6 個；Split Work（分做不同頁）／Side by Side（同題、每個 agent 可選不同模型）；Let it cook 在第一版後再出 2～6 個變體。**每多一個 agent、多一輪變體就多一份模型請求**，額度燒得快
- **吃 SKILL.md**：輸入框打 `/` 叫內建設計技能，也能加自己的 SKILL.md 資料夾（已寫成 Skills 的品牌規範可直接搬）
- **推回程式碼**：屬性面板 Code 區選 HTML＋Tailwind 或 HTML＋CSS 複製／匯出；React 等框架請 agent 依專案產生；反向也行——專案現有元件請 agent 畫回畫布，CSS 變數與 Variables 可互相同步
- **CLI**：`npm install -g @pen.dev/cli`（需 Node.js 22.19+），`pen --out design.pen --prompt "..."` 不開視窗生成設計
- **模型接法**：官方列 15 種連線；ChatGPT Plus／Pro、Claude Pro／Max 帳號可直接登入，其餘走 API key、GitHub Copilot、Cursor 或自訂端點

### 2. pen.dev 其他畫布功能（aiposthub 文，未核對）

- **Code on Canvas**：放 `</>` 腳本節點指向 `.js`，檔頭 `@input` 註解自動長出滑桿／顏色／下拉；外部存檔即時重繪，滿意後 Convert to layers 固化成向量圖層
- **Component Slots**：元件內標空插槽並設「推薦元件」，agent 建實例時只往插槽塞推薦的元件——**用結構約束 agent，守住設計系統一致性**
- Vibe Shaders（GLSL 填色）、Mesh Gradients、字型跟著 `.pen` 檔走

### 3. Paper：圖片＋描述 → SVG

- 不是 Auto Trace 描線，而是**照你的視覺規則重畫**；提示詞拆三件事：**要處理什麼 → 想要什麼風格 → 有哪些視覺限制**
  - 範例：`remove background and redraw as an engraving illustration. thick dark blue lines on white back, light hatching for shading, no fills, minimal number of strokes`
- **統一素材風格**：固定一組提示詞（深藍線條、白底、少量排線、不填色），讓來源不同的照片／3D 圖／插圖都重畫成同一套
- SVG 可再交給 CSS／JS 做動畫，但**要看路徑結構**——細節全併成大量 Path 就很難控制單一部位
- 上線前檢查：斷線、細節失真、多餘路徑、縮到實際尺寸還看不看得清

### 4. 現在還做不到的（rar.design 對照官方文件，2026-09-23）

| 項目 | 現況 |
|---|---|
| 團隊協作 | 文件無即時共編與留言章節；協作靠 Git 與分享連結 |
| 原型 | 文件無 prototype 說明；靠 Slides 簡報模式或匯出 HTML |
| Figma 匯入 | 複製貼上 Figma 圖層時圖片貼不過來，要整份 `.fig` 匯入 |
| 格式 | 官方**保留 `.pen` 格式破壞性變更的權利**；轉成元件庫（`.lib.pen`）**不能復原**，正式專案先開副本 |
| 費用 | 軟體目前免費，未來可能收費（模型選單已出現 pen.dev Pro）；AI 用量另計 |

### 5. 並陳：說法不一致處（不裁決）

- **格式開放程度**：aiposthub 寫「`.pen` 完全開源」；rar.design 只說「規格完整公開」，且官方保留破壞性變更權利——**公開規格 ≠ 開源、≠ 穩定**
- **程式碼還原度**：aiposthub 稱「100% 映射」；rar.design 引設計師 Roger Wong：設計系統成熟的團隊，**真正的依據是程式碼裡的元件，JSON 只是它的一份描述**
- **路線之爭**：Figma 是把 agent 請進 Figma（`use_figma`、Figma Agent）；pen.dev 是把設計檔做成 agent 本來就讀得懂的格式、放進 agent 已在工作的 repo。創辦人 Tom Krcha 押的使用者是「一個人帶一群 agent」，受訪節目標題甚至是「多人協作已死」——這是創辦人立場，不是市場結論

## C（關鍵行動）

- [ ] 想試 pen.dev：拿**自己的**網站（或 localhost 專案）匯入一次，量改版第一天省多少時間；正式專案先開副本
- [ ] 用 Side by Side 拿同一個 brief 比 2～3 家模型，記下額度消耗
- [ ] 比較與現行 Figma MCP 流程的差距前，先確認 Flutter 端怎麼接（目前文件只見 HTML／Tailwind／React）
- [ ] Paper 產 SVG：固定一組「處理／風格／限制」提示詞，產出後縮到實際尺寸檢查路徑

## 原始摘要

- **rar.design〈Pencil 改名 pen.dev 變了什麼？〉（2026-09-23）**：pen.dev＝Pencil（High Agency；2025-09 IDE 內 MCP 畫布亮相 → 2026-01 正式上線 → 02 破 10 萬用戶、SWARM Mode → 07 改名、接各家模型 → 09-17 新版：網站匯入、Chrome 擴充、畫布效能）；網站匯入三步與三個坑；15 種模型連線、六個 agent、Let it cook、SKILL.md；`.pen` JSON、MCP、CLI；Figma 對照表；限制清單；FAQ（平台：macOS／Windows x64／Linux，Windows ARM 即將推出；Cursor、VS Code、Antigravity、Windsurf 擴充）
- **aiposthub〈全新 pen.dev 震撼登場〉（2026-09-19）**：Tom Krcha（前 Adobe XD 產品負責人）、a16z speedrun 投資；七大功能（Web to Canvas、Code on Canvas、Component Slots、Vibe Shaders／Mesh Gradients、雙向同步、CLI 與 MCP）；三分鐘逆向 → 改造 → 轉代碼教學；pen.dev vs Figma vs v0／Lovable vs Cursor／Claude Code 對照表
- **aiposthub〈Paper.design 是什麼？〉（2026-09-24）**：Paper 畫布建立在 HTML／CSS、MCP 讓 agent 讀寫；新功能「圖片＋提示詞 → 重畫 SVG」與 Auto Trace 的差別；提示詞三段拆法；統一素材風格；SVG 接 JS 動畫（小幽靈範例）；上線前檢查

## 補充資料

**2026-11-19 補充（來源：Medium／Simon Liu〈[ 開源專案 ] open-design-frame：透過 Coding Agent 直接生成設計稿，無縫整合畫布、檢視器與 DESIGN.md〉，https://medium.com/@simon3458/open-design-frame-32bcd0bd2e1f ，2026-09-10）**：作者自家開源專案介紹，未實測。

- **第三條路線：設計稿直接就是 React 元件**。open-design-frame 在 `frames/` 寫元件＋指定尺寸就成為畫布上的一張設計稿（`pnpm add -D @open-design-frame/core`、`open-design-frame dev`）；作者論點：過去把 agent 擅長的（寫元件）和做不到的（讀設計檔）綁在一起，這裡改用 agent 本來就會寫的語言
- **尺寸是每張 frame 的屬性**，同一檔可並存 1440 桌機版與 390 手機版，避免「agent 直接寫網頁」沒有固定尺寸的問題
- 檢視器改的是原始碼：顯示瀏覽器實際解析值，Save 才寫回；🔑 樣式來自共用常數時**明確拒絕**改（避免連帶改掉其他十幾處）
- 原型連結寫在 frame 裡跟著 git 走；留言變成 JSX 註解、可進 PR；主題＝tokens（`--odf-*`）＋ `DESIGN.md`（規格表由 token 算出，不會承諾做不到的事）
- 內建 MCP（25 個工具、無狀態 Streamable HTTP）；`write_frame` 帶上次讀到的 revision，**磁碟已變就回 409 而不覆蓋**——人與 agent 同改一檔是常態
- 與本條 pen.dev 對照：pen.dev 是 JSON 描述再轉碼，這裡設計稿本身就是會出貨的程式碼（作者說法），也因此綁 React

## 相關連結

- [[figma-ai-make-mcp-design-agent]] — Figma 那條路（Design Agent／Make／MCP／Specs）；本條是「設計檔放進 repo」的對照路線
- [[claude-code-figma-mcp-write-setup]] — Claude Code 寫入 Figma；與 pen.dev 的 MCP 讀寫 `.pen` 是同一需求的兩種落點
- [[claude-design-tool]] — Anthropic 自家的文字描述→設計稿工具，另一個不開 Figma 的選項
- [[ai-ui-acceptance-unslop-and-design-reference]] — 平行 agent 比稿之前，先把參考與驗收拆成可檢查項目
- [[ai-design-skills-anti-template]] — 比幾個資訊安排真的不同的方向再定案；pen.dev Side by Side／Let it cook 是同一做法的工具化
- [[design-md-for-ai-ui]] — 把視覺規格寫成檔案給 agent；`.pen` 是把整份設計稿都寫成檔案
- [[arc-ui-mcp-claude-code-component-workflow]] — 先查現成元件再安裝；與 Component Slots「只准塞推薦元件」同一種約束思路
