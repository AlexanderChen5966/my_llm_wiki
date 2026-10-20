---
title: "Claude Code Remotion 教學：用 Codex 做動畫影片"
source: "https://rain.tips/2026/08/25/claude-code-remotion-codex-animation-workflow/#google_vignette"
author:
  - "[[Rain Chu]]"
published: 2026-08-25
created: 2026-09-30
description: "完整學會 Claude Code 與 Codex 的 Remotion 動畫工作流，包含官方 Skill、提示詞、逐幀校正、GSAP、D3、Lottie、Three.js、Blender 與透明背景輸出。"
tags:
  - "clippings"
status: processed
---
用 Claude Code 或 Codex 產生 Remotion 專案並不難，真正困難的是讓 [動畫](#) 從「可以播放」進步到「看起來像專業 [設計](#) 」，只下一句提示詞，AI 通常能把元件放進時間軸，卻很難同時掌握分鏡、節奏、視覺層級、轉場與品牌質感。

開發工具

比較可靠的方法，是先把 Remotion 的 Agent Skill 安裝到開發代理，接著把需求拆成分鏡、時間、版面與動態規則，先完成短片段，再逐幀檢查並反覆修正。本文會從安裝開始，整理一套 Claude Code 與 Codex 都能使用的流程，也會說明 GSAP、D3.js、Lottie、Three.js、Canvas 與 Blender 各自適合處理什麼工作。

## Remotion 是什麼

[Remotion](https://www.remotion.dev/) 是一套以 React 撰寫影片的框架。畫面被拆成 Composition，每個 Composition 會定義尺寸、幀率與總幀數，元件再依目前幀數計算位置、透明度、縮放與其他狀態。最後可以在 Studio 預覽，也能透過命令列輸出影片。

這種逐幀且可重現的設計很適合 AI 代理。代理可以讀取程式碼、修改元件、調整時間點，再重新輸出指定幀進行比對。它也適合大量產生不同文案、數據或尺寸的影片，而不是每次都回到剪輯軟體手動修改。

## 為什麼一句提示詞通常做不好

空白專案只提供了一堆可以組合的零件，沒有現成的鏡頭語言、元件規則與品質標準，當需求只有「做一支有質感的 SaaS 動畫」，代理還要自行猜測品牌顏色、字級、轉場、每一幕的長度，以及什麼才算有質感，結果自然容易變成普通投影片。

- 沒有分鏡，所以所有資訊同時出現
- 沒有明確時間點，所以動畫速度忽快忽慢
- 沒有動態規則，所以每個元素使用不同節奏
- 沒有參考幀，所以代理只能猜測版面與層級
- 沒有驗收步驟，所以第一版直接被當成完成品

Agent Skill 的價值，就是先把 Remotion 的正確用法、常見陷阱與輸出流程交給代理，這和 [建立 Claude Code 自訂 Skill](https://rain.tips/2026/06/18/%E7%94%A8-skill-creator-%E5%BB%BA%E7%AB%8B-skill%EF%BC%9Aclaude-code-%E8%87%AA%E8%A8%82%E6%8A%80%E8%83%BD%E5%AE%8C%E6%95%B4%E6%95%99%E5%AD%B8/) 的概念相同，先提供可重複使用的方法，再讓代理處理當次任務。

繪圖與動畫軟體

## Claude Code 與 Codex 都能使用 Remotion Skill

Remotion 官方的 [Agent Skills 文件](https://www.remotion.dev/docs/ai/skills) 已列出 Claude Code 與 Codex 等開發代理。先確認 [電腦](#) 已安裝 Node.js，再建立空白專案並加入 Skill。

```js
npx create-video@latest --yes --blank my-video
cd my-video
npm i
npx remotion skills add
npm run dev
```

如果希望把官方 Skill 安裝成全域能力，可以執行下面的指令。

```js
npx -y skills@latest add remotion-dev/skills -g -y
```

官方能力不只包含基本規範，也有建立作品、字幕、地圖、SaaS 產品 [動畫](#) 、互動播放器、媒體處理、版本升級與輸出等分工

這表示 Codex 並不是只能修改既有 Remotion 程式，也能從規格開始建立專案並完成輸出。

## 可直接使用的繁體中文提示詞

提示詞不要只描述主題，也要同時定義格式、時間軸、視覺系統、動態語言與驗收方式．

下面這份範本可以直接貼給 Claude Code 或 Codex，再替換中括號內的內容。

免費軟體與共用軟體

```js
請先讀取目前可用的 Remotion Skill 與專案規範，再建立一個可直接輸出的 Remotion Composition。

影片主題
[要傳達的產品或故事]

輸出格式
1920 x 1080
30 fps
總長 20 秒
Composition ID 使用 ProductLaunch

視覺方向
使用深灰、米白、青色與珊瑚紅
畫面安靜、精準、有產品發表會質感
禁止套用通用科技藍紫漸層
文字必須保留安全邊界並在手機縮圖中可讀

分鏡
0 到 3 秒顯示品牌名稱與一句核心價值
3 到 8 秒展示產品介面與主要操作
8 到 14 秒用三個步驟說明工作流程
14 到 18 秒顯示成果數據
18 到 20 秒收尾並顯示行動文字

動態規則
所有動畫都要由 useCurrentFrame、interpolate 或 spring 驅動
每次只讓一個主要資訊成為視覺焦點
入場使用淡入、位移與輕微縮放
不要使用無法由幀數重現的 CSS transition
不要使用 Math.random、目前日期或即時網路狀態

工作方式
先建立 5 秒的第一幕作為品質樣本
輸出第 0、30、60、90、120 與 149 幀供我檢查
確認沒有文字溢出、遮擋、跳動與空白畫面後，再完成全部分鏡
最後列出 Composition ID、預覽方式與輸出指令
```

這個範本刻意要求先做 5 秒樣本。若第一幕的字體、間距與動態節奏不對，先修正視覺系統會比做完 20 秒後重來省下更多時間。

商務處理流程

若想深入理解 Codex 產生動態內容的提示架構，可以延伸閱讀 [Codex 做動態圖表和短影片的工作流程](https://rain.tips/2026/07/11/codex-dynamic-charts-ai-video-workflow/) 。

## 模仿參考動畫時要先拆解畫面

把整段參考素材轉成大量截圖，再一次交給代理，通常只能得到顏色和版面大致相近的結果。，集畫面缺少元素對應、時間關係與動態方向，代理不容易判斷一個物件是移動、被遮罩，還是切換成另一個元件。

1. 每 0.5 到 1 秒擷取一張代表幀
2. 標記每個鏡頭的開始時間與結束時間
3. 列出背景、主體、文字、裝飾與遮罩等圖層
4. 描述物件從哪裡進入、移動到哪裡、使用何種緩動
5. 先重建最具代表性的 3 到 5 秒
6. 輸出相同時間點的幀並排比較

軌跡 [動畫](#) 也可以用一張乾淨的路徑草圖輔助。把路徑、起點、終點與移動方向畫清楚，再要求代理將路徑轉成 SVG 或座標 [資料](#) ，通常比只用文字描述「繞一圈再飛出去」更準確。參考作品應作為鏡頭與節奏研究，不要直接複製商標、字體、插圖或其他受保護素材。

## Remotion 與各種動畫工具怎麼選

| 工具 | 擅長內容 | 適合情境 |
| --- | --- | --- |
| Remotion 原生能力 | React 元件、分鏡、字幕、影片組合 | 產品介紹、教學短片、批次影片 |
| [GSAP](https://gsap.com/) | 時間軸、緩動、複雜介面動態 | SaaS 首頁、產品發表、投影片式轉場 |
| [D3.js](https://d3js.org/) | 資料轉換、SVG、客製圖表 | 統計變化、架構圖、地圖與路徑 |
| [Lottie](https://www.remotion.dev/docs/lottie) | JSON 向量動畫 | 圖示、付款流程、節慶與微動畫 |
| [Three.js](https://www.remotion.dev/docs/three) | 即時 3D 場景與模型 | 低多邊形場景、設備與空間示意 |
| [Fabric.js](https://fabricjs.com/) 與 [p5.js](https://p5js.org/) | Canvas 繪圖與自訂視覺 | 白板、手繪、幾何圖形與實驗動畫 |
| [Blender](https://www.blender.org/) | 建模、材質、燈光與高品質 3D | 產品模型、車輛、角色與複雜鏡頭 |

### Remotion 加 GSAP

GSAP 的 Timeline 很適合管理多段 Tween，也方便安排重疊、標籤與緩動。它能讓產品頁面以投影片般的節奏逐幕展開。不過 Remotion 最終仍要對每一幀得到固定結果，因此整合時應由目前幀數控制 GSAP 的進度，避免依賴瀏覽器實際經過的時間。

機器學習與人工智慧

### Remotion 加 D3.js

D3.js 是自由度很高的資料視覺化工具，關於人口統計變化、系統架構、交通路線與歷史路徑都很適合先由 D3 計算比例尺、座標與 SVG，再由 Remotion 控制每一段資料何時出現，資料來源應保存在專案內或在輸出前完成下載，避免輸出時受到外部 API 波動影響。

### Remotion 加 Lottie

Lottie 適合圖示、付款成功、條碼掃描與簡短節慶動畫，官方套件需要安裝 `@remotion/lottie` 與 `lottie-web` 。部分 Lottie expression 無法保證逐幀一致，正式輸出前應特別檢查是否閃爍。

```js
npm i @remotion/lottie lottie-web
```

### Remotion 加 Three.js

Three.js 適合在 React Three Fiber 中載入 3D 模型，再用 Remotion 的目前幀數控制相機、模型與燈光，官方提供 `@remotion/three` 進行整合，低多邊形產品展示、工廠流程或地圖空間化很合適，複雜建模與擬真材質則應交給 Blender。

```js
npm i three @react-three/fiber @remotion/three @types/three
```

## Blender 與 MCP 的正確分工

Blender 可以完成建模、材質、燈光、相機與骨架 [動畫](#) ，透過合適的 MCP 連接器，Claude Code 或 Codex 可以呼叫 Blender 執行部分操作，但這不代表代理已具備專業 3D 美術判斷，最有效率的分工，是由 Blender 建立複雜 3D 資產與鏡頭，再輸出透明影片或圖片序列，最後交給 Remotion 疊加文字、圖表、字幕與音訊。

設計

MCP 連接器屬於第三方整合時，要先檢查原始碼、權限範圍與維護狀態。不要讓來源不明的工具取得整台 [電腦](#) 或敏感專案的存取權。

## 如何輸出透明字卡與動畫覆蓋層

透明背景不能只把 CSS 背景設成透明，輸出格式也必須保留 Alpha 通道，若要放進 Final Cut Pro、Premiere 或 DaVinci Resolve，Remotion 官方建議使用支援 Alpha 的 ProRes 4444 或 4444 XQ，影格格式選 PNG，像素格式使用 `yuva444p10le` 。

```js
npx remotion render ProductLaunch out/overlay.mov --image-format=png --pixel-format=yuva444p10le --codec=prores --prores-profile=4444
```

網頁若需要透明影片，可以輸出 VP8 或 VP9 的 WebM，搭配 PNG 影格與 `yuva420p` ，瀏覽器支援並不一致，正式網站最好同時準備不透明的備用版本。

## 從需求到成品的完整工作流程

1. **寫清楚目的** 　先決定受眾、平台、時長與希望觀眾記住的唯一重點
2. **拆分分鏡** 　每幕只安排一個主要訊息，標出開始與結束時間
3. **定義視覺系統** 　固定顏色、字體、間距、圓角、陰影與安全邊界
4. **安裝 Skill** 　讓代理先讀取 Remotion 的官方規範與相關能力
5. **先做短樣本** 　用 3 到 5 秒確認 [設計](#) 方向，不要直接生成整支作品
6. **檢查代表幀** 　至少檢查開頭、中段、轉場前後與結尾
7. **加入專用工具** 　依需求選 GSAP、D3、Lottie、Three.js 或 Blender
8. **加入音訊** 　最後才對齊旁白、音效與背景音樂
9. **輸出與複查** 　完整播放成品，也要檢查文字是否溢出與畫面是否閃爍

預覽完成後，可以先列出 Composition，再輸出指定作品。

開發工具

```js
npx remotion compositions
npx remotion render ProductLaunch out/video.mp4
```

Remotion 與 [用 HTML 寫影片的 HyperFrames](https://rain.tips/2026/06/14/heygen-hyperframes-%E4%BB%8B%E7%B4%B9%EF%BC%9A%E7%94%A8-html-%E5%AF%AB%E5%BD%B1%E7%89%87%EF%BC%8C%E8%AE%93-ai-agent-%E8%87%AA%E5%8B%95%E5%89%AA%E8%BC%AF/) 都把畫面轉成代理可修改的程式碼。Remotion 更接近 React 生態與逐幀影片管線，HyperFrames 則適合直接以 HTML 組合動態內容。選擇重點不是哪一套比較新，而是哪一套更符合現有技術與交付格式。

## 一定要用 Remotion 嗎

不一定。若只是把現成網頁 [動畫](#) 輸出成一次性的短片，可以用 GSAP 或 D3 建立網頁，透過 Playwright 等瀏覽器工具逐幀截圖，再交給 FFmpeg 合成，這條路徑簡單直接，也能保留原本的前端互動程式。

Remotion 的優勢在於 Composition、時間軸、媒體同步、參數化、Studio 預覽與標準輸出流程，當作品包含多個分鏡、字幕、音訊、不同尺寸，或需要批次套用 [資料](#) 時，這些能力會明顯降低維護成本，單次實驗可以選瀏覽器截圖加 FFmpeg，長期內容管線則更適合 Remotion。

## 常見問題

### Codex 可以取代 Claude Code 完成這套流程嗎

可以，Remotion 官方 Agent Skills 文件已把 Codex 列為適用代理，差異主要來自代理讀取到的 Skill、專案上下文、提示詞完整度與驗收流程，而不是只能使用某一個品牌的開發工具。

### 為什麼參考圖很多，結果還是不像

大量截圖不等於完整動態規格，需要補上每個圖層的名稱、時間區間、位移方向、遮罩關係與緩動方式，先重建一個短鏡頭，再逐幀對照，會比一次交付整段素材更有效。

電視與影片設備

### 什麼工具最適合動態圖表

D3.js 適合需要客製比例尺、座標、路徑與 SVG 的資料視覺化，一般長條圖或折線圖不必為了技術感增加過多特效，先確保數據正確、標籤可讀與變化有清楚的時間順序。

### 透明影片輸出後為什麼變成黑底

通常是編碼器、像素格式或影格格式沒有保留 Alpha，剪輯軟體可用 ProRes 4444 搭配 PNG 與 `yuva444p10le` ，網頁則可測試 VP8 或 VP9 WebM 搭配 `yuva420p` 。

## 結論

Claude Code、Codex 與 Remotion 的組合確實能做出高品質動畫，但品質不是來自一句神奇提示詞。真正有效的做法，是先讓代理掌握 Skill，再把需求寫成可驗收的分鏡與逐幀規則，最後依內容選擇 GSAP、D3、Lottie、Three.js、Canvas 或 Blender。

把第一版當成可以修改的 [動畫](#) 原型，而不是最終成品，先完成短樣本、檢查代表幀、修正視覺系統，再擴充完整時間軸，AI 才會從快速生成工具變成穩定的動態 [設計](#) 工作夥伴。