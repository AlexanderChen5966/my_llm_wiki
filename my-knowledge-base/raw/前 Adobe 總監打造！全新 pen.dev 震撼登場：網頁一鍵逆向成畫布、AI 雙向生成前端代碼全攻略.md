---
title: "前 Adobe 總監打造！全新 pen.dev 震撼登場：網頁一鍵逆向成畫布、AI 雙向生成前端代碼全攻略"
source: "https://www.aiposthub.com/pen-dev-agentic-canvas-guide-web-import-to-code/"
author:
  - "[[Philo]]"
published: 2026-09-19
created: 2026-09-22
description: "由前 Adobe XD / Flash 核心主管 Tom Krcha 創立、a16z speedrun 押注的全新 pen.dev 正式發表！主打「絕不從零開始」，支援將任何線上網站或 localhost 本地服務一鍵逆向還原為可編輯向量圖層；首創 Code on Canvas 讓畫布直接運行 JavaScript 並自動生成 UI 滑桿控制項；深度支援 Component Slots 插槽、Vibe Shaders 與深淺色 Design Tokens。更原生支援 MCP 協定，串聯 Claude Code 與 Cursor，實現 Design ↔ Code 雙向閉環。本文提供最詳盡的架構拆解、隱藏功能盤點與手把手實戰教學！"
tags:
  - "clippings"
status: "inbox"
---
---

> 「隆重介紹全新的 pen.dev——專為構建大膽創意而生、由 AI 代理驅動的設計畫布（An agentic canvas for building bold ideas）。  
>   
> 絕不從零開始（Never start from scratch）。將任何網站或 Web App 直接匯入畫布，隨心迭代，再一鍵推送回程式碼。  
>   
> 立即更新或下載體驗。」  
> —— Tom Krcha，pen.dev 創辦人兼執行長（2026 年 9 月 17 日）

---

![pen.dev 官方發表視覺：專為構建大膽軟體而生的 Agentic Canvas](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/hero_og.png)

pen.dev 正式發表：專為軟體構建而生、由 AI Agent 深度驅動的全新設計畫布，徹底打通網頁逆向、視覺迭代與程式碼雙向同步。圖片來源： pen.dev 官方網站 。

---

在 2026 年由 Cursor、Claude Code 等工具引領的「Vibe Coding」浪潮中，全球無數工程師與獨立開發者體驗到了前所未有的編程狂飆快感。然而，在享受 AI 秒生後端與邏輯代碼的同時，一個長久以來橫亙在軟體開發中的最大痛點依然存在： **視覺前端的調整極度痛苦且割裂。**

以往的前端工作流中，每當你想要修改一個按鈕的陰影、微調卡片的間距、更換色票、或是探索不同的排版風格時，你往往只能在對話框裡與 AI 反覆「猜謎式溝通」——「Padding 再稍微大一點點」、「把那個卡片標題往左挪 4px」、「按鈕改綠色但要帶有一點高級感」。一個微小的視覺調整，往往來回消耗十幾次對話還無法精準到位。

而在另一端，Figma 雖然提供了自由的視覺繪圖環境，但 Figma 畫出來的設計稿與真實程式碼之間有著一道天塹：設計師的圖層結構無法直接映射到現代 React / Tailwind 元件樹，手刻切版依然是一場耗時耗力的災難。

為了解決這個長達數十年的結構性矛盾， **前 Adobe XD 產品負責人、Adobe Flash 核心先驅 Tom Krcha 所創立、並獲得 a16z speedrun 押注投資的全新設計工具——pen.dev（前身為知名專案 Pencil.dev），於 2026 年 9 月 17 日正式迎來核彈級改版！**

pen.dev 將自己重新定義為 **「專為打造軟體而生的 Agentic Canvas」** ，其核心口號就是震撼業界的四個字： **「Never start from scratch（絕不從白紙從零開始）」** ！

它不僅支援將網路上任何公開網頁、甚至是你在本機運行的 `localhost:3000` 專案一鍵逆向解析成完全可編輯的向量圖層，更首創了能直接在畫布上跑 JavaScript 的「Code on Canvas」、深度組件約束「Component Slots」、即時著色器「Vibe Shaders」，以及原生支援 MCP（Model Context Protocol）與 Cursor / Claude Code 無縫協同的雙向閉環能力。

本文將帶您深度開箱 pen.dev 的底層架構、逐一盤點官方文檔中暗藏的殺手級功能，並提供一套超實戰的「逆向網頁 ➔ AI 視覺迭代 ➔ 導出 Tailwind / React 組件」全工作流教學！

---

## 一、核心架構拆解：為什麼 pen.dev 敢說「絕不從零開始」？

傳統設計工具（如 Figma、Sketch）本質上是「點陣與靜態形狀的集合」，而 pen.dev 在架構設計之初就確立了「設計即代碼（Design as Code）」的根本定位。

![](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/image-132.png)

官方實機演示：在內建瀏覽器輸入網址，點選一鍵匯入，原本由 HTML/CSS 組成的動態網頁瞬間被逆向解析為可自由拖拽、修改樣式的向量圖層。影片來源：pen.dev 官方展示。

### 1\. 痛點終結：告別面對空白畫布的「創作焦慮」

在過去，無論是想做一個新的 Landing Page、後台儀表板（Dashboard），還是為 App 設計一套註冊流程，設計師或工程師往往要從一張空白畫布開始慢慢拉框。

pen.dev 的顛覆之處在於， **你的創作起點就是「現有的世界」** ：

- 看到 Stripe 的結帳卡片設計得極為優雅？直接抓進來。
- 覺得 Linear 的排版風格很符合專案調性？直接抓進來。
- 自己專案裡已經寫好了 80% 的本地前端頁面，但對樣式不滿意？直接從 `localhost` 抓進來！

抓進來的不再是一張死板的截圖，而是 **包含 Auto-layout（Flexbox）、字級大小、Padding、色票、邊界半徑全部拆解好的「活體圖層」** ，你可以直接讓 AI 在其基礎上進行外科手術級的修改。

---

## 二、官方文檔深度解密：不可不知的七大硬核殺手級功能

在深度爬梳 `docs.pen.dev` 的官方文檔後，我們發現 pen.dev 遠不只是一個「帶 AI 對話框的簡易版 Figma」，它在內部深度融合了許多前所未見的創新機制：

### 功能一：網頁與 Web App 逆向工程（Web to Canvas）

<video controls=""><source src="https://hctfc8iexhqk0x3o.public.blob.vercel-storage.com/marketing/import-loop-cc7797b02701.mp4" type="video/mp4"></video>

官方實機演示：在內建瀏覽器輸入網址，點選一鍵匯入，原本由 HTML/CSS 組成的動態網頁瞬間被逆向解析為可自由拖拽、修改樣式的向量圖層。影片來源：pen.dev 官方展示。

pen.dev 提供兩種極致的網頁逆向方式：

1. **內建瀏覽器（Built-in Browser）** ： 在桌面版應用頂端點擊「地球」圖示即可喚出內建瀏覽器。此時你可以輸入任何公開網址， **甚至可以直接輸入本地開發伺服器 `http://localhost:3000` 或 `http://localhost:5173`** ！
	- **整頁匯入（Full Page）** ：將整頁 DOM 樹轉換成一個巨大的頂層 Frame。
		- **單一元素選取（Select an element）** ：游標移動到網頁任何區塊，藍框高亮後點擊 Import，只把特定按鈕、導航列或卡片抓進畫布。
		- **截圖模式（Screenshot）** ：若網頁涉及敏感登入資料或包含 WebGL Canvas 遊戲，可切換為純像素截圖做為參考。
2. **專屬 Chrome 擴充套件（pen.dev Web Import）** ： 官方已在 Chrome Web Store 上架外掛。你在一般 Chrome 瀏覽器看網頁時，選中目標區塊按右鍵複製，切換回 pen.dev 按 `Ctrl/Cmd + V` ，即可無縫貼上為向量圖層！

---

### 功能二：畫布直接跑代碼！Code on Canvas（腳本節點 </>）

這是 pen.dev 最具創見的功能。以往要在設計軟體畫複雜圖表（如圓餅圖、熱力圖、參數化幾何圖紋）往往需要手動複製幾十個圖層，而 pen.dev 直接在畫布上原生支援跑 JavaScript！

![pen.dev 官方文檔：Code on Canvas 與雙向閉環](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/docs_og.png)

pen.dev 的 Design as Code 哲學：將代碼能力內嵌於畫布，以統一的.pen 格式連通設計與工程。圖片來源： docs.pen.dev 。

- **Script Node（腳本節點）** ：在工具列點選 `</>` ，在畫布上放置一個節點，並指向外部的 `.js` 檔案。
- **自動生成 UI 控制項（Sliders / Color / Enum）** ： 在 JS 檔案頂端宣告型態註解：
```javascript
/**
 * @schema 2.18
 * @input columns: number(min=1, max=12) = 4
 * @input gap: number(min=0, max=40) = 8
 * @input barColor: color = #3B82F6
 * @input layout: enum("horizontal", "vertical") = "vertical"
 */
const cols = Math.floor(pencil.input.columns);
const cellW = (pencil.width - pencil.input.gap * (cols - 1)) / cols;
const nodes = [];
for (let c = 0; c < cols; c++) {
  nodes.push({
    type: "rectangle",
    x: c * (cellW + pencil.input.gap),
    y: 0,
    width: cellW,
    height: pencil.height,
    fill: pencil.input.barColor,
    cornerRadius: 6
  });
}
return nodes;
```

pen.dev 右側屬性面板 **會立刻根據 `@input` 自動生成滑桿、顏色吸管、下拉選單** ！拉動滑桿，畫布排版即時響應重繪。

- **Hot-Reload 與一鍵向量化** ：直接在外部 IDE 存檔，畫布立即更新；若滿意效果，點擊面板底部的 **「Convert to layers」** ，動態腳本立刻固化為標準向量圖層供後續編輯。

---

### 功能三：Component Slots——為 AI 戴上「設計拘束器」

在過去讓 AI 自由排版時，最大的災難就是「AI 容易破壞組件結構」，例如往嚴格設計規範的 Card 裡面塞入奇怪的文本框。pen.dev 借鑒了前端開發的「插槽（Slot）」概念：

- **Make Slot（空插槽）** ：製作組件時，選取一個空的子 Frame 點擊「Make Slot」，該區域在畫布上會呈現特殊的斜線網底。
- **Suggested Slot Components（推薦插槽組件）** ： 你可以為這個 Slot 設定限制或推薦。例如做一個 `Modal` 組件，Slot 推薦 `Button-Group` ；做一個 `Table` 組件，Slot 推薦 `Table-Row` 。
- **AI 安全約束** ：當你指令 AI Agent 建立頁面實例時，AI 會嚴格遵循 Slot 規範，只在指定插槽中塞入被推薦的組件， **有效守護了大型產品設計系統的一致性** 。

---

### 功能四：視覺效果天花板：Vibe Shaders 與 Mesh Gradients

![pen.dev Vibe Shaders 即時著色器渲染效果](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/preview_shader.webp)

pen.dev 的 Vibe Shaders 實時著色器渲染：支援在任意圖層上運行 GLSL 動態著色器，帶來令人驚豔的現代視覺質感。圖片來源：pen.dev 官方展示。

1. **Vibe Shaders（即時著色器）** ： 有別於傳統設計軟體死板的線性漸層，pen.dev 在填充模式中加入了 **Shader** 。你可以匯入任何 GLSL 著色器檔案（或從內建的 `Shader Gallery` 選擇），在畫布上渲染動態流動光影、噪點顆粒紋理、金屬折射等極具現代感（Vibe）的視覺效果，且 Shader 的參數均可在屬性面板直接微調。
2. **Mesh Gradients（網格漸變）** ： 支援高階向量網格漸變。你可以設定 3×3 或 4×4 的控制網格，個別拖動錨點並著色，輕易調出蘋果風格的流光漸層壁紙。
3. **客製化字型全打包** ： 支援直接拖入 `.ttf` 、`.otf` 、`.woff` 、`.woff2` 字型，字型會直接跟隨 `.pen` 檔案走，在任何電腦打開都不會發生「遺失字型跑版」的悲劇。

---

### 功能五：Design ↔ Code 雙向同步（Push Back to Code）

這是將 pen.dev 提升為「工程師必備武器」的關鍵：

1. **一鍵導出 HTML + Tailwind CSS** ： 選取畫布上的任何元素，在右側屬性面板切換至 `Code` 區塊，即可一鍵複製語義乾淨、沒有多餘 inline-style 的純粹 Tailwind CSS 代碼。
2. **AI 代碼生成（Cmd/Ctrl + K）** ： 選中畫布區塊，喚醒 AI Composer，你可以下達精準的工程指令：
	- *「將選取的 Pricing Card 生成為使用 TypeScript 與 Shadcn UI 的 Next.js 組件。」*
		- *「將這個 Form 改用 React Hook Form 與 Zod 驗證架構導出。」*
		- *「把所有圖標改用 Lucide Icons 庫導出。」*
3. **Design Tokens 雙向同步** ： 支援讀取專案中的 `globals.css` ，自動將 CSS 變數（如 `--primary: #2563EB` ）映射為 pen.dev 畫布變數；在畫布調整完色彩與間距後，亦可讓 AI 一鍵將變數寫回 `globals.css` ！
4. **雙主題（Light / Dark）雙軌變數** ： 變數面板原生支援 Light 與 Dark 兩列欄位，同一組 Token 可設定兩套數值，點擊一下即可在畫布上預覽亮暗色模式切換。

---

### 功能六：極客生態：無頭 CLI、免費 ChatGPT Plus 與 MCP 聯動

![pen.dev CLI 終端命令列操作展示](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/preview_terminal.webp)

pen.dev Headless CLI：支援在純終端環境下以自然語言驅動設計生成與批次導出，完美契合 CI/CD 自動化。圖片來源：pen.dev 官方展示。

1. **ChatGPT Plus / Pro 免費串接（ `pen codex-login` ）** ： 你不需要自備昂貴的 OpenAI API Key！透過 `pen codex-login` ，支援瀏覽器或 Device Code 授權，直接調用你既有的 ChatGPT Plus / Pro 訂閱額度來驅動畫布設計。
2. **原生 MCP（Model Context Protocol）支援** ： pen.dev 本身就是一個 MCP Server。在 `Settings ⚙️ → MCP` 打開開關後，外部的 **Claude Code CLI、Cursor、Antigravity 2.0、Codex** 等 Coding Agent 可以直接透過 MCP 工具（如 `batch_design`, `get_editor_state`, `set_variables`, `export_nodes` ）在背景直接讀取與修改 `.pen` 畫布！
3. **無頭命令列（ `@pen.dev/cli` ）** ：
```bash
npm install -g @pen.dev/cli
# 在終端直接用自然語言建立設計
pen --out dashboard.pen --prompt "建立一個現代感暗黑模式數據儀表板"
# 一鍵批次匯出圖片
pen --in dashboard.pen --export dashboard.png
# 啟動互動式 Shell
pen interactive -o dashboard.pen
```
4. **Slides 演講簡報模式（直接替代 Keynote）** ： 文檔中還藏了一個彩蛋功能——畫布上的每一個頂層 Frame 都可被視為一張投影片（Slide）。在左側欄切換至 `Slides` 視圖，拖曳排序後按下 `Cmd/Ctrl + Enter` ，畫布瞬間化身全螢幕簡報播放器，並支援一鍵匯出為多頁 PDF！

---

## 三、手把手實戰教學：三分鐘體驗「逆向 ➔ 改造 ➔ 轉代碼」工作流

接下來，我們將以一個真實的使用場景，帶大家走一遍最完整的實操工作流程：

### 步驟 1：安裝與環境準備

1. 前往 [pen.dev/downloads](https://pen.dev/downloads?ref=aiposthub.com) 下載桌面版應用（支援 macOS Apple Silicon/Intel、Windows、Linux），或在 Cursor / VS Code 擴充套件搜尋 `pen.dev` 安裝。
2. 前往 Chrome Web Store 安裝專用擴充套件： [pen.dev Web Import](https://chromewebstore.google.com/detail/pendev-web-import/ongmlniblkfdmdihcjlbeejbcbceiooi?ref=aiposthub.com) 。
3. 打開 pen.dev，在 `Settings (⚙️) → Agents` 中連接你的 AI 帳號（可綁定 Claude API、OpenAI API，或透過 `pen codex-login` 綁定 ChatGPT 訂閱）。
![pen.dev 畫布工作流展示](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/09/tweet_thumb.webp)

pen.dev 的極簡雙欄操作介面：左側圖層與組件、中間 WebGL 極速畫布、右側屬性與 AI 代理面板。圖片來源：Tom Krcha 官方推文截圖。

### 步驟 2：逆向現有網頁（Web to Canvas）

假設我們很喜歡某個國外熱門 SaaS 的定價卡片（Pricing Card）：

1. 點擊 pen.dev 頂部工具列的「地球」圖示，開啟內建瀏覽器。
2. 輸入該網站的網址。
3. 點擊「Select an element」，將滑鼠游標懸浮在該定價卡片上，直到外框呈現選取狀態。
4. 點擊「Import」按鈕。
5. 瞬息之間，原本由複雜 HTML/CSS 構成的網頁卡片，已經化為畫布上一個個整齊清晰的向量 Frame、Text、Button 圖層！

### 步驟 3：AI Agent 深度視覺迭代（Targeted Prompting）

點選剛剛匯入的卡片圖層，右側的 AI Composer 會自動將該圖層加入對話 Context。

我們可以在對話框中輸入以下提示詞：

> **「請針對目前選取的 Pricing Card 進行視覺升級：** 整體風格改為 Cyberpunk Dark Mode，背景改為深灰色 #0F172A，加入 1px 的紫藍漸變邊框。將『Upgrade Now』按鈕改為寬度滿版（Fill Container）、高度 48px，背景套用霓虹粉色到紫色的線性漸變。將文字內容改為台灣繁體中文，定價標題改為『專業團隊版』，價格改為『NT$ 980 / 月』。在特色列表的每個項目左側加入綠色打勾圖標（使用 Feather 圖標庫）。」

點擊發送，AI Agent 會開始呼叫底層繪圖工具，直接在畫布上精準修改圖層屬性，數秒之內完成全新設計！

### 步驟 4：Push Back to Code（一鍵產出 React / Tailwind 代碼）

滿意畫布上的視覺成果後，我們要把成果放回專案代碼庫：

1. 選中修改完成的 Pricing Card。
2. **方法 A（快速切版）** ：在右側屬性面板切換至 `Code` 分頁，點擊 `Copy HTML` ，立刻獲得一串乾淨無贅詞的 `HTML + Tailwind CSS` 代碼，直接貼到專案即可使用。
3. AI 將直接產出結構嚴謹、包含型別定義與響應式斷點的前端代碼！

**方法 B（現代前端組件化）** ：按下 `Cmd/Ctrl + K` 呼叫 AI：

> *「請將這個卡片導出為 Next.js 15 App Router 可用的 React Server Component，使用 TypeScript，定義完整 Props 介面，並將所有顏色提取為 Tailwind 類別。」*

---

## 四、市場競爭格局：pen.dev vs Figma vs v0 vs Cursor 深度橫評

為了讓大家更清楚 pen.dev 的獨特定位，我們將其與目前主流的設計與代碼生成工具進行全維度對比：

| 評比維度 | pen.dev | Figma | v0.dev / Lovable | Cursor / Claude Code |
| --- | --- | --- | --- | --- |
| **核心本質** | Agentic Canvas（設計即代碼） | 專業向量 UI/UX 設計工具 | AI 網頁生成工具（Prompt to UI） | AI 編程環境（Code Editor） |
| **網頁逆向能力** | **極強** （內建瀏覽器/擴充套件，一鍵還原為向量圖層） | 需依賴昂貴且效果不佳的第三方外掛 | 弱（僅能靠文字描述或參考圖猜測） | 無法逆向視覺為圖層 |
| **視覺微調自由度** | **極高** （隨意拖拉、改座標、改邊距、改色票） | **極高** （純手工調整） | 低（只能透過對話框反覆碰運氣） | 極低（純代碼修改，盲改 CSS） |
| **程式碼還原度** | **100% 映射** （底層為 Flexbox 與 JSON 物件樹） | 差（需人工二次切版重寫） | 高（直接生成 JSX） | 高（純代碼產出） |
| **自定義代碼注入** | **支援** （Code on Canvas 跑 JS 動態渲染圖層） | 不支援 | 支援（但無法在畫布參數化微調） | 本身就是代碼 |
| **外部 Agent 整合** | **支援原生 MCP** （Claude Code / Cursor 可直控） | 僅有封閉 API，無原生 Agent MCP | 封閉平台 | 本身為 Agent Host |
| **檔案格式開放度** | **完全開源** （`.pen` 純 JSON 格式） | 封閉專有二進制格式 | 專有雲端平台 | 開放代碼檔案 |

---

## 五、結語：軟體工程第三時代——當「畫布」成為「IDE」的一部分

在傳統軟體工程的第一時代（手工切版時代），設計師在 Sketch/Figma 繪製靜態圖稿，交付給前端工程師將其重寫為 HTML/CSS；在第二時代（低代碼與 AI 對話時代），我們學會了用 v0 或 Cursor 用文字描述生成頁面，卻始終被「摸不到、調不準」的視覺盲盒所困擾。

**而 pen.dev 所代表的，正是軟體工程的第三時代——「Agentic Canvas（代理畫布時代）」。**

在第三時代中：

1. **設計不再從零開始** ：世界的任何角落都是你的設計素材庫，一鍵即可逆向解構。
2. **設計與代碼不再有邊界** ：畫布本身就是代碼的即時投影（Code on Canvas），代碼也是畫布的語義結晶（Design as Code）。
3. **人類專注於審美與決策，AI 負責繁瑣的重繪與代碼縫合** 。

無論你是想擺脫 Figma 手刻切版地獄的前端工程師、渴望快速做出高保真原型並驗證產品的獨立開發者（Indie Hacker），還是想探索 Vibe Coding 終極形態的技術探索者， **pen.dev 絕對是 2026 年下半年最不容錯過的生產力核彈工具** 。

立即下載體驗，開啟你的下一個大膽構想吧！

---

### 相關資源匯整

- **pen.dev 官方網站** ： [https://pen.dev](https://pen.dev/?ref=aiposthub.com)
- **官方文檔手冊** ： [https://docs.pen.dev](https://docs.pen.dev/?ref=aiposthub.com)
- **官方下載中心** ： [https://pen.dev/downloads](https://pen.dev/downloads?ref=aiposthub.com) （支援 macOS / Win / Linux）
- **Chrome 擴充套件** ： [pen.dev Web Import (Chrome Web Store)](https://chromewebstore.google.com/detail/pendev-web-import/ongmlniblkfdmdihcjlbeejbcbceiooi?ref=aiposthub.com)
- **VS Code / Cursor 插件** ：在 Marketplace 搜尋 `pen.dev` （識別碼： `highagency.pencildev` ）
- **CLI 工具安裝** ： `npm install -g @pen.dev/cli`
- **官方 Discord 社群** ： [https://discord.gg/Azsk8cnnVp](https://discord.gg/Azsk8cnnVp?ref=aiposthub.com)
- **創辦人 Tom Krcha X** ： [@tomkrcha](https://x.com/tomkrcha?ref=aiposthub.com) / 官方帳號 [@pendev](https://x.com/pendev?ref=aiposthub.com)