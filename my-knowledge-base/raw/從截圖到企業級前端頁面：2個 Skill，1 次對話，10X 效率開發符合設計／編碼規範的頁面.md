---
title: "從截圖到企業級前端頁面：2個 Skill，1 次對話，10X 效率開發符合設計／編碼規範的頁面"
source: "https://codelove.tw/@tony/post/xdbdka"
author:
published:
created: 2026-06-08
description: "從截圖到企業級前端頁面：2個 Skill，1次對話，10X 效率開發符合設計／編碼規範的頁面  ===============================================    > **《AI 前端效率提升實踐》往期精彩推薦**> > - [《🔥🔥🔥AI Agent 從..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [06月05日](https://codelove.tw/@tony/post/xdbdka "2026-06-05 16:03:50")

## 從截圖到企業級前端頁面：2個 Skill，1 次對話，10X 效率開發符合設計／編碼規範的頁面

## 從截圖到企業級前端頁面：2個 Skill，1次對話，10X 效率開發符合設計／編碼規範的頁面

```
> **《AI 前端效率提升實踐》往期精彩推薦**
```

> - [《🔥🔥🔥AI Agent 從入門到實踐：從底層原理到如何從 0 手搓一個 AI 智能體》](https://juejin.cn/post/7624357532246507561)

週五下午五點，Leader 丟來十幾張設計圖：「下週一上線，快速處理一下。」

我熟練地截圖、餵給 AI，三秒出碼。打開預覽，血壓直接拉滿——滿屏藍紫漸層，按鈕像根螢光棒，版面全憑想像力。

「能不能用 DevUI 元件庫來實現？」我壓著火又試了一次。

AI 很聽話，轉頭給我生造了一個 `d-input-box` ，繼續塗著它那標誌性的 AI 味配色……

盯著那堆散發著「賽博廉價感」的 **AI 味頁面** ，我陷入了沉思：

「怎麼讓 AI 真正照著圖片樣式來生成？能不能讓它學會用現有元件庫，而不是憑空造輪子？」

這兩個問題不解決，再強的模型也只是個高級塗鴉工具。於是我索性自己動手，做了兩個專門解決這些問題的 Skill。

下面就是這套「截圖 → 企業級頁面」的完整拆解。

**讀完這篇文章，你將收穫**

- 一個從 **截圖→企業級頁面** 的自動化實踐方案；
- 一個讓 AI **不再瞎猜版面** 的圖片識別增強 Skill 設計思路；
- 一個 **DevUI 元件庫 Skill** 的設計與實現；
- 一次對話便能生成完整前端頁面及其功能互動的 **企業級端到端自動化實踐**

現在我給你看到的是一張我隨意畫的潦草手繪圖， **沒有添加任何額外提示詞** ，直接觸發 Skill 生成的頁面：

[![1.png](https://i.imgur.com/0ro42Tn.jpeg)](https://i.imgur.com/0ro42Tn.jpeg)

這背後的核心，就是今天要分享的 2 個自研 Agent Skill：

1. **image-analyze-improvement** —— 把截圖、原型圖甚至手繪圖，轉成一種結構化的「UI 描述語言」；
2. **vue-devui-practices** —— 生成符合 DevUI 規範和最佳實踐的企業級程式碼。

## 一、AI 生成前端頁面的三種方式

在講具體方案之前，先聊聊目前用 AI 生成前端頁面，到底有哪幾條路可以走。搞清楚全貌，才能理解後面為什麼要做 Skill。

### 1.1 基於自然語言的提示詞驅動生成

這是目前門檻最低、最直觀的方式。你只需要用自然語言（中文或英文）清楚地描述你想要的頁面結構、樣式和功能，AI 就能自動生成對應的 HTML、CSS 和 JavaScript 程式碼。

> "頁面頂部是一個導覽列，左側 Logo，右側有使用者頭像和下拉選單；中間區域左右分欄，左邊 240px 側邊欄包含可摺疊選單，右邊是主內容區，有一個搜尋欄和表格……"

聽起來沒問題？但實際用起來有幾個硬傷：

- **提示詞極長** ：一個中等複雜度的後台頁面，光描述版面和元件就得 300-500 字，比直接寫程式碼還累；
- **資訊容易遺漏** ：間距、對齊、層級關係……寫提示詞時漏掉一個，AI 就自由發揮，出來的頁面歪七扭八；
- **還原度不足** ：設計風格固化，AI 味的藍紫漸層色，版面容易自由發揮

**結論** ：手寫描述適合於現有頁面的微調，但要從 0 還原設計稿，或許「自己寫程式碼都比寫提示詞快」不是段子。

### 1.2 Design-to-Code 方案：從設計原始檔生成程式碼

既然手寫描述太慢，那能不能直接從設計稿中提取結構化資訊來生成程式碼？這就是業界主流的 **圖像生成前端程式碼** 方案之一——不從截圖「猜」，不用手寫描述，而是直接解析設計工具的原始檔。

**核心流程** ：

[![2.png](https://i.imgur.com/8bPT46s.jpeg)](https://i.imgur.com/8bPT46s.jpeg)

**代表工具** ：Pixso D2C、Semi Design、Appy Pie、Figma 官方外掛、Figma-Context-MCP、imgCook 等

**優勢** ：

- **像素級精準還原** ：由於直接讀取原始檔資料，它能完美保留設計稿的尺寸、間距、色彩、排版和字體規範，視覺還原度極高（通常可達 95% 以上）。
- **設計與開發高度一致** ：能夠最大程度減少設計師與前端開發之間的溝通成本和視覺走查誤差，生成的程式碼結構清晰，更易於後續維護。
- **程式碼品質高，符合工程規範** ：能夠智慧辨識 List、Grid 等版面方案，生成語義化的 HTML 標籤和符合 BEM 等命名規範的 CSS。支援輸出 Vue、React、微信小程式等多種標準框架程式碼。

**局限** ：

- **強依賴設計工具** 和原始檔格式，截圖、手繪圖、互動示意圖等無法使用；
- **使用門檻高** ，需要對接 Figma 平台、MCP 工具等，需要進行開發賦能
- 生成的樣式程式碼存在大量的絕對定位版面， **難以達到生產可用**
- **客製化投入大** ：DSL 到程式碼的映射規則需要針對元件庫客製化，前期投入大；
- **對設計稿規範度有要求** ，圖層混亂的設計稿解析效果打折，設計師為了方便會從已有圖層複製過來進行疊加修改。

### 1.3 像素級視覺方案：從圖片直接生成程式碼

另一種思路：不依賴設計原始檔，讓 AI 直接「看圖寫碼」。核心是 **視覺－語言多模態神經網路** ，打通圖像像素、視覺語義、程式碼文本三大模態，實現 UI 截圖、設計圖、圖表等圖像到可執行程式碼（HTML/CSS/React/Vue/ 圖表程式碼等）的端到端轉換。

**核心原理** ：

[![3.png](https://i.imgur.com/Pk08zBm.jpeg)](https://i.imgur.com/Pk08zBm.jpeg)

**代表工具** ： [v0.dev](http://v0.dev/) 、Screenshot-to-code、AIUI.me 等

**優勢** ：

- 門檻極低，無需配置、不用懂設計工具
- 專為快速 `UI` 原型設計而打造，適合快速原型驗證、頁面初稿。
- 支援手繪稿 / 截圖轉程式碼，無需專業設計稿

**局限** ：

- **平台綁定，技術框架單一** ，框架 React/Next.js 為主，樣式預設使用 Tailwind CSS；
- **程式碼複用性與工程化困難** ，對企業級元件庫（DevUI、Ant Design 等）無感知，這會導致程式碼庫中出現大量重複且難以維護的冗餘程式碼。
- **還原精度有限，細節易失真** ，很難做到像素級還原。對標高保真設計稿時，圖示、特殊動效、客製化主題、複雜排版容易走樣，仍需人工微調。

### 1.4 圖像識別增強 + DevUI 元件庫的 Skills 方案

那麼，有沒有一種方案，既能像 **像素級方案** 一樣「丟圖即出碼」，又能像 **結構化方案** 一樣做到「程式碼風格可控、元件庫適配」？

這正是本文要分享的方案： **圖像識別增強 + DevUI 元件庫的 Skills 方案** ——用 Skill 把「看圖」和「寫規範程式碼」兩件事串聯起來。本質上，它在像素級視覺方案的基礎上，透過 Skills 注入結構化分析能力和元件庫知識，彌補 **平台適配、元件適配、框架適配** 的短板。

[![4.png](https://i.imgur.com/FoeDTaF.jpeg)](https://i.imgur.com/FoeDTaF.jpeg)

**優勢** ：

- 使用 **門檻低** ，日常使用方便，有截圖即可生成
- 平台適配：支援 **現有 IDE／Agent** 圖片上傳能力的直接適配
- **元件庫與框架適配，客製化門檻低** ：適配門檻低，工程化規則、元件庫實踐可隨時更新和補充，可替換 vue-devui、ng-devui、ant-design 等不同元件庫
- 支援 `手繪稿` 、 `原型圖` 、 `互動示意圖` 等多種輸入，輸入靈活，支援意圖識別
- 元件庫自帶設計規範（間距、顏色、圓角、陰影等），AI 只需呼叫 `<d-button>` 而非重複寫 `<button class="px-4 py-2 rounded...">` 。生成結果 **風格統一** ，不會出現一個頁面裡兩種按鈕樣式。
- 元件庫經過生產驗證（可及性、鍵盤導覽、焦點管理、響應式斷點），AI 直接繼承這些優點，避免自己造輪子出 bug。

**局限** ：

- 不追求 100% 像素對齊，優先 **元件化、可維護、可擴充**

本質上，這仍屬於像素級視覺方案，我們只是做了一系列更符合我們企業內部生產直接可用的實踐優化。

## 二、圖片識別增強 Skill：讓 AI 讀懂圖片

### 2.1 為什麼需要它

現在 AI 的現狀是，只要你能說清楚，它就能做出來。

可問題就在「說清楚」這一步——它本身就是一個苦力活。明明截圖裡已經把結構畫得明明白白，為什麼還得再寫三百字的提示詞去描述一遍？

雖然大多數 IDE 和 Agent 已經支援上傳圖片並分析，但分析結果往往不盡如人意：資訊頻繁遺失，生成的頁面更是時好時壞、品質堪憂。

**使用者** ：

> 「請描述這張圖片的 UI。」

**AI 回覆** ：

> 「這張圖是一個商品管理頁面，頂部有搜尋框，中間有卡片列表，底部有分頁。」 —— **太模糊，開發者獲取的資訊嚴重不足，生成的程式碼自然容易偏離設計稿** 。

這個 skill，便是讓 Agent 內建的圖片分析能力更上一層樓。

### 2.2 這個 Skill 做了什麼

它的任務很純粹：接收一張頁面截圖，使用 Agent 內建工具 `AnalyzeImage` 分析圖片的結構、版面、內容和樣式，並根據內容理解業務分析互動邏輯，輸出一份 **結構化的 UI 描述語言** （UIDL，UI Description Language）。

### 2.3 適用場景

- 基於圖片首次生成頁面時
- 用於分析圖片的版面、樣式、內容，並推理其互動需求

### 2.4 如何使用

你只需要在上傳圖片的同時，在提示詞中啟用這個 Skill。例如：

> `/image-analyze-improvement` 分析圖片

### 2.5 設計思路

圖像識別增強 Skill 本質上是一個 **圖片→結構化文件** 的轉換管線：以四維分析模型為引擎，以量化描述規範為約束，以雙形式（Markdown + JSON）輸出為產物，最終產出可直接落地為 UI 程式碼的分析文件。

[![5.png](https://i.imgur.com/1RkRziR.jpeg)](https://i.imgur.com/1RkRziR.jpeg)

**四維分析模型** ：對介面／元件從 Position、Content、Interaction、Style 四維進行確定性描述，確保輸出可直接落地為 UI 程式碼。

維度含義說明核心問題Position定義版面結構、層級與相對位置元件在哪裡展示？Content明確元件類型、文字與資料內容展示哪些資訊？Interaction定義互動行為與狀態變更邏輯內容如何動態變化？Style約束視覺表現、尺寸與顏色規範呈現何種視覺效果？ **描述規範體系** ：要求結構清晰 + 數值量化 + 狀態完整，全覆蓋版面、元件、內容、樣式、互動五大維度，所有描述具備唯一性。

- **版面描述** ：整體→局部，上→下，左→右.
- **位置描述** ：使用精準方位： `上方` 、 `下方` 、左側、右側、 `居中` 、左上角、右下角，明確模組與模組、模組與父容器的相對位置，禁用「旁邊、附近、周圍」等模糊性表達
- **層級關係表述**: 父子關係：使用「 `包含` 、 `位於…內部` 、隸屬於、嵌套於」描述層級。兄弟關係：使用「並列、相鄰、 `左側/右側` 、 `上方/下方` 」描述同級位置
- **尺寸描述** ：具體數值（如 16px）、比例\`、占滿寬度、佔據剩餘空間等，禁用「大、小、寬、窄」。
- **顏色描述** ： 使用 `十六進位` ，可使用語義化命名：主題藍、警告橙、成功綠
- **文字完整性**: 圖片中所有可見 `文字必須完整` 擷取並描述

這個 Skill 的核心是一套精心設計的系統提示詞。我在這裡分享它的核心框架：

```
css 體驗 AI 程式碼助手 程式碼解讀複製代碼---

   # 圖片分析增強技能

   ## 工作流程

   1. **接收圖片分析請求** → 呼叫 \`AnalyzeImage\` 分析圖片結構、版面、內容、樣式
   2. **執行結構化分析** → 四維確定性描述（Position / Content / Interaction / Style）
   3. **輸出結果** → Markdown + JSON 統一輸出
   4. **保存結果** → \`image-analysis-{pageName}.md\`

   ## 分析四維模型

   | 維度 | 含義 | 核心問題 |
   |------|------|----------|
   | Position | 版面結構、層級、相對位置 | 元件在何處展示？ |
   | Content | 元件類型、文字、資料內容 | 展示哪些資訊？ |
   | Interaction | 互動行為、狀態變更邏輯 | 內容如何動態變化？ |
   | Style | 視覺表現、尺寸、顏色規範 | 呈現何種視覺效果？ |

   ## 描述規範

   - **順序**：整體→局部，上→下，左→右
   - **層級**：父子（包含／嵌套於）+ 兄弟（並列／相鄰／左側／右側）

   ## 輸出模板

   \`\`\`markdown
   ## 頁面整體結構分析
   * **頁面類型**：[...]
   * **版面結構**：[...]
   * **視覺風格**：[...]

   ---

   ## 版面結構

   **整體結構**：[區域劃分，從上到下、從左到右、從整體到局部]

   **[區域名稱]**：位於[位置]，包含[元件]，[排列方式]。
   [逐層描述，體現父子與兄弟關係]

   ---

   ## JSON 結構化資料

   \`\`\`json
   {
     "layout": {
       "order": "vertical | horizontal",
       "sections": [{
         "name": "[區域名稱]",
         "order": "[排列方向]",
         "type": "[DevUI元件]",
         "spacing": { "between": 16, "marginTop": 16 },
         "align": "left | center | right",
         "fixed": "right | left",
         "children": [{
           "name": "[元件名稱]",
           "type": "[元件類型]",
           "text": "[顯示文字]",
           "icon": "[圖示]",
           "states": [],
           "interactions": {},
           "purpose": "[功能說明]"
         }]
       }]
     }
   }
```

---

## 互動邏輯

### 1\. \[互動場景\]

- \[互動行為描述\]
```
有了這個提示詞後，我把它配置為一個 Agent Skill，使用時**只上傳截圖觸發技能**，它就能輸出一份標準的結構化 UI 描述文件。

這裡僅是介紹 Skill 的基本結構和設計思路，完整版 Skill 會包含更多詳細內容和規則，後面會開源同步出來。

### 2.6 實踐案例：設計稿截圖 → UIDL

下面就是一次真實使用：我直接上傳截圖，使用 Skill 分析圖片，生成結構化 UI 描述

![6.png](https://i.imgur.com/wORbiNk.jpeg)

你會發現，這份文件已經精確描述了版面結構、模組位置、元件功能（tabs、搜尋框、按鈕、連結）、互動邏輯、頁面上的文案以及對應的 JSON 結構化 UIDL。

---

三、DevUI 元件 Skill：DESIGNS.md + 元件庫最佳實踐
-------------------------------------

> [DevUI](https://link.juejin.cn?target=https%3A%2F%2Fdevui.design%2Fhome) 是面向企業中後台產品的開源前端解決方案，其設計價值觀基於「高效、開放、可信、樂趣」四種自然與人文相結合的理念，旨在為設計師、前端開發者提供標準的設計體系，並滿足各類落地場景，是一款企業級開箱即用的產品。

### 3.1 為什麼需要它

由於大模型訓練資料的原因，AI 總是偏向於藍紫漸層色、大圓角、玻璃態和陰影浮層的頁面，設計風格固化，**AI 味明顯**。

- ​**缺少設計約束**​：在未提供設計規範的情況下，模型會取「最大公約數」風格，即最常見的模板。
- ​**不了解元件庫**​：通用模型不知道團隊用的是 Ant Design、Element Plus 還是 DevUI，更不知道主題色、圓角 token、間距系統。

與其讓 AI 重複造輪子、瞎猜版面，現有的經過檢驗的企業級元件庫及其設計規範，用得更讓人放心。DevUI 元件 Skill 便是讓 AI 能夠了解現有元件及其用法，生成符合 DevUI 元件規範的頁面。

### 3.2 這個 Skill 做了什麼

拿到 UIDL 之後，DevUI Skill 負責將它**落地為符合 DevUI 元件庫規範的業務程式碼**，包括：

![7.png](https://i.imgur.com/s6Vfa8T.jpeg)

1. **識別元件需求**：根據使用者需求識別需要使用的 DevUI 元件，讓 AI 知道 DevUI 有哪些元件（Button, Table, Form, Modal...）
2. ​**讀取設計系統 Design Token**​：把 \`DESIGNS.md\` 中的顏色、圓角、間距、字體規範直接餵給 AI，禁止它自由發揮。
3. ​**讀取元件文件**​：讀取需要使用的元件的 API / Demo 文件
4. **根據元件文件和最佳實踐生成頁面**

### 3.3 適用場景

- 需要使用 DevUI 元件時
- 取得 DevUI 元件相關 API 文件、Demo 用法
- 生成符合 DevUI 設計規範的前端頁面

### 3.4 如何使用

在提示詞中啟用 DevUI Skill：

> \`/vue-devui-practices\` 使用 DevUI 元件生成一個表單頁面

### 3.5 設計思路

**Skill 目錄結構**
```

txt 体验AI代码助手 代码解读复制代码vue-devui-practices/  
│ [SKILL.md](http://skill.md/)  
└───references/  
│ [DESIGNS.md](http://designs.md/)  
│ Form-表單.md  
│ Icon-圖標.md  
│ Input-輸入框.md  
│...  
└───demo/  
│ [table-virtualScroll.md](http://table-virtualscroll.md/)  
└───...

```
我為它準備了另一套系統提示詞，核心邏輯是將 UIDL 中的抽象描述與 DevUI 元件能力做映射。部分提示詞如下：
```

markdown 体验AI代码助手 代码解读复制代码## Overview

{技能的整體定位與核心能力概述，1-2句話說明技能做什麼、怎麼做}

## Quick Start

### 1\. 識別元件需求

{根據使用者需求，對應到技能支援的元件／能力清單，按分類列舉}

### 2\. 取得元件文件

{使用 Read 工具按需讀取 references 目錄中的文件，路徑規則： `references/{元件名}.md` }

**重要** ：若 references 中無對應文件，需參考 `references/DESIGN.md` 設計規範自行實現。

### 3\. 取得設計規範與變數

{讀取設計規範文件，取得樣式變數、設計約束等}

### 4\. 根據元件文件和最佳實踐生成頁面

{文件內容輸出清單，如：元件介紹、用法範例、Props/Events/Slots/Methods/型別定義、最佳實踐}

## Component Usage

### 基本規範

**元件引入** ：{元件註冊方式與使用前綴}

## 核心編碼原則

...

## Resources

### references/

**元件文件** ：每個文件包含：

- {文件內容項1}
- {文件內容項2}
- ...

**其他文件** ：

- `references/DESIGNS.md` ：{設計規範說明}
- `references/Install.md` ：{安裝配置說明}

## Best Practices

- {最佳實踐1}
- ...

### 3.6 實際案例：UIDL → DevUI 程式碼展示

我把表單截圖生成的 UIDL 交給 DevUI 元件庫技能，生成頁面效果如下：

[![8.png](https://i.imgur.com/Jo2Paq9.jpeg)](https://i.imgur.com/Jo2Paq9.jpeg)

可以看到，頁面整體版面基本一致，元件使用正確，僅部分細節版面和間距等需要調整優化。

再看下生成的程式碼效果：

[![9.png](https://i.imgur.com/88I9uzY.jpeg)](https://i.imgur.com/88I9uzY.jpeg)

可以看到，無論是頁面效果還是程式碼規範度完全符合 DevUI 規範，表單使用了 DevUI 的 `d-form` 和 `d-input` ，按鈕是 `d-button` primary 區塊級。所有命名、API、樣式變數全都是 DevUI 生態下最標準的寫法。

## 四、組合使用：端到端企業級前端開發實踐

前面只是分步使用 Skill，我們也可以將多個 Skill 串聯，你將能透過一次對話、一張截圖，快速生成前端頁面：

1. 用 **圖像識別增強 Skill** 解析設計稿截圖，得到結構化版面和元件清單。
2. 將版面描述 + 元件清單交給 **DevUI Skill** ，生成符合規範的前端頁面程式碼。
3. 此時，再搭配 **介面呼叫生成 Skill** 使用，一鍵生成介面呼叫程式碼，完成從 UI 頁面到介面呼叫的功能互動！

> 介面呼叫生成 Skill 詳細介紹： [《介面即程式碼：一個 Skill 輕鬆搞定型別定義、介面呼叫、Mock 與除錯》](https://juejin.cn/post/7646587364768858146)

**實踐案例** ：ip 白名單存取控制頁面

把三個 Skill 串起來，整個流程無比順暢，一次對話便可完成需求開發：

1. Step1：在 Agent 中，我 **上傳一張截圖** ，然後直接觸發 `圖像識別 Skill` ，分析圖片並使用 `devui skill` 生成頁面；
2. Step2：輸入後端提供的介面設計：直接觸發 `介面呼叫生成 skill` ，一鍵生成介面定義、呼叫、mock、配置等
3. Step3：按需求完成從 UI 頁面到介面呼叫的功能互動

[![10.png](https://i.imgur.com/pada2sr.jpeg)](https://i.imgur.com/pada2sr.jpeg)

```
体验AI代码助手 代码解读复制代码可以看到，表格資料的查詢、搜尋、分頁功能全都正常可用，並且自帶 mock 機制，後端介面未就緒也不阻塞開發，僅部分元件配置／間距細節需要微調：
```

[![11.png](https://i.imgur.com/6Otk56b.jpeg)](https://i.imgur.com/6Otk56b.jpeg)

## 五、總結

如果你也面臨：前端頁面生成困難，效果不佳，元件不會用，功能實現繁瑣

不妨試試這套組合：

- **圖像識別增強 Skill** —— 讓 AI 真正看懂版面、樣式、互動，把模糊的視覺輸入變成嚴謹的資料結構；
- **DevUI 元件 Skill** —— 讓 AI 遵循設計規範，告別隨機風格，使用已有的標準化元件。
- **[介面呼叫生成 skill](https://juejin.cn/post/7646587364768858146)** —— 讓 AI 遵循編碼規範生成功能介面邏輯

三者結合，就實現了 **從截圖到企業級程式碼的端到端自動化** ，直觀、精準、可複用。對於有固定元件庫和設計規範的團隊來說，這個思路幾乎可以消滅一大半的頁面搭建工作。

如果你也想動手試試，可以參考上面的 Skill 提示詞，替換成你們自己專案規範的元件庫（Element Plus、Ant Design、Naive UI、 [MateChat](https://link.juejin.cn/?target=https%3A%2F%2Fmatechat.gitcode.com%2Fuse-guide%2Fskills.html) 一樣適用），讓你的 Agent 會用元件，遵循設計規範、遵循編碼規範。

## 📣 加入我們

MateChat 正在快速發展，我們歡迎更多開發者加入：

- 💬 DevUI 微信小助手：devui-official（新增時請備註 MateChat）
- 🌟 程式碼倉庫位址： [gitcode.com/DevCloudFE/…](https://link.juejin.cn/?target=https%3A%2F%2Fgitcode.com%2FDevCloudFE%2FMateChat)

[![](https://i.imgur.com/V2sRxrF.jpeg)](https://i.imgur.com/V2sRxrF.jpeg) ---

如果這篇文章對你有幫助，歡迎按讚、收藏，有疑問直接在留言區聊～

> 感謝貢獻者 **[@liuguolin](https://link.juejin.cn/?target=https%3A%2F%2Fgitcode.com%2Fliuguolin)** 提供的優質好文！

---

原文出處： [https://juejin.cn/post/7646396172870008847](https://juejin.cn/post/7646396172870008847)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xdbdka) [22瀏覽](https://codelove.tw/@tony/post/xdbdka)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xdbdka)

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

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

🔥 [我如何使用 Claude Code Skills 實現股票篩選自動化 \[Python × yfinance × Vibe Coding\]](https://codelove.tw/@tony/post/q5nlWx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝6 💬2

337

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1780889251&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A134250504%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fxdbdka&amp;pra=5&amp;asro=0&amp;itsi=10&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMjE2IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4yMTYiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjIxNiJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1780889251278&amp;bpp=1&amp;bdt=79&amp;idt=47&amp;shv=r20260604&amp;mjsv=m202606020101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1780888825%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1780888825%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1780888825%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5997127794393&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=3&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=18086&amp;eid=95390667%2C95392292&amp;oid=2&amp;pvsid=3697554348359679&amp;tmod=473573085&amp;uas=0&amp;nvt=7&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D2&amp;fc=1920&amp;brdim=1920%2C68%2C1920%2C68%2C1920%2C30%2C1920%2C985%2C1680%2C938&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=1.14&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=55" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260604/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=4&amp;uci=a!4&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>