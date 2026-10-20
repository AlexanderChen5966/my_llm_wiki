---
title: "為什麼大家都改用 Vite？React 最強開發工具誕生！"
source: "https://realnewbie.com/posts/why-developers-are-switching-to-vite-the-ultimate-react-dev-tool"
author:
  - "[[徐培鈞]]"
published: 2025-04-09
created: 2026-06-30
description: "在 React 剛興起的時候，Create React App（CRA）是很多前端新手的第一站。它一行指令就能跑起來，幫我們把 Babel、Webpack、ESLint、環境變數……這些麻煩事都打包處理好了。但你可能也遇過這些情況：開發時每次改檔都要等個幾秒才會更新畫面想改個 webpack 設定，結果被 eject 嚇退項目越大，啟動越慢，連裝個第三方 lib 都讓人心累這些痛點，"
tags:
  - "clippings"
status: processed
---
## 前言：CRA 已經不夠用了？

在 React 剛興起的時候，Create React App（CRA）是很多前端新手的第一站。

它一行指令就能跑起來，幫我們把 Babel、Webpack、ESLint、環境變數……這些麻煩事都打包處理好了。

但你可能也遇過這些情況：

- 開發時每次改檔都要等個幾秒才會更新畫面
- 想改個 webpack 設定，結果被 `eject` 嚇退
- 項目越大，啟動越慢，連裝個第三方 lib 都讓人心累

這些痛點，讓越來越多開發者開始尋找「更快、更輕、更可控」的選擇——這就是 Vite 登場的時刻。

---

## Vite 是什麼？為什麼這麼快？

### Vite 的設計理念：不再「一開始就打包全部」

Vite（唸作 /vit/，像是 “vite-ality” 的 vite）是由 [Vue.js](https://vuejs.org/) 的作者 Evan You 所設計的一套「 **現代化前端開發工具** 」，目標是解決傳統打包工具在開發階段的痛點，像是：

- 專案越大， **啟動越慢**
- 每改一行程式碼，就要等上好幾秒才會反映在畫面上（Hot Module Replacement 效能差）

這些問題，在過去使用 Webpack 為基礎的 CRA（Create React App）非常常見。

### 舊的做法：一開始就把整個專案打包一次

以 Webpack 為代表的傳統工具，會在 **開發一開始** 就：

1. 把整個專案所有的 JS、CSS、圖片模組全數讀進來
2. 一次性「打包合併」成瀏覽器可執行的檔案
3. 用本地開發伺服器啟動，並在你改檔時重編打包

這麼做有一個根本問題： **就算你今天只要修改 App.jsx，Webpack 也要重新解析一大堆沒改的檔案** ，這就是為什麼開發起來會覺得「拖、卡、等很久」。

---

### Vite 的做法：只處理你用到的部分

Vite 的核心想法是：「 **不要一次處理全部，只處理你眼前需要的。** 」

Vite 能做到「啟動快」、「熱更新快」的秘密，來自它在開發階段的三大關鍵設計：

#### 🧩 1. 善用瀏覽器原生的 ES Modules（ESM）支援

過去我們寫前端時，模組系統（像是 `import React from 'react'` ）其實是由 **Webpack 幫你打包整理的** ，瀏覽器原本是看不懂這些語法的。

但現在的瀏覽器（Chrome、Edge、Firefox） **已經原生支援 ESM（ES Modules）** ，可以像後端那樣用 `import/export` ，並「一個一個地」透過 HTTP 載入模組。

👉 **Vite 就利用這點，跳過了「一開始全部打包」的流程** ：

- 每個 `.js/.jsx/.ts` 檔案都當成「獨立模組」
- 只有當瀏覽器打開畫面、遇到 `import` 指令時，才會用 HTTP 向開發伺服器請求那個模組
- **需要什麼才給什麼，不會一次打包整個專案**

> 🧠 比喻一下：Webpack 像是煮一桌滿漢全席，即使你只想吃一碗麵；
> 
> 而 Vite 是點餐制，客戶（瀏覽器）叫什麼，廚房（開發伺服器）才現做給你。

#### 🔄 2. 依賴預編譯（dependency pre-bundling）

你可能會想：「如果每個模組都用 HTTP 載，那我引入 React、Lodash、axios 是不是也要一個一個請求？」

這樣其實會很慢，所以 Vite 在啟動時會做一個重要動作：

> 🔧 **先幫你把第三方套件（ `node_modules` 裡的東西）一次預編譯好**

這個動作叫做 **dependency pre-bundling** ，由 `esbuild` 來完成。舉個例子：

- 你專案用到 `react` 、 `react-dom` 、 `react-router-dom`
- Vite 啟動時會用 esbuild 把它們打包成一份快取檔
- 之後開發過程中都不需要再重編這些套件，直接讀取快取就好， **節省大量等待時間**

> 💡 為什麼只預編譯「套件」而不是全部程式？
> 
> 因為這些套件內容幾乎不會在開發時變動，預處理一次就能穩定服役，超有效率。

#### ⚡ 3. esbuild 的極速轉譯（用 Go 語言打造）

Vite 的超高速度背後功臣之一，就是 [esbuild](https://esbuild.github.io/) ——一個用 **Go 語言寫成的打包工具** 。

相比於 Webpack：

| 項目 | Webpack | esbuild |
| --- | --- | --- |
| 編寫語言 | JavaScript | Go |
| 執行方式 | 單執行緒 | 多執行緒 |
| 速度表現 | 普通 | 快上 10～100 倍 |
| 處理檔案類型 | JSX、TS、CSS 等 | 同上，但速度更快 |

在開發階段，每次你改 `.jsx` 或 `.ts` 檔案，esbuild 都能 **幾乎即時地把它轉成瀏覽器能執行的 JavaScript** 。這就是為什麼 Vite 的畫面更新快到像「沒在等待」一樣。

**這三個加起來，就是 Vite 開發體驗的核心優勢**

| 功能 | 原因 |
| --- | --- |
| 快速啟動 | 不打包所有模組，只載入瀏覽器當下需要的 |
| 模組即時更新（Hot Reload） | 檔案變動只處理單一模組，無需整包重編 |
| 套件載入快又穩 | 第三方依賴預先用 esbuild 編譯過一次，快取處理 |
| 編譯速度快得驚人 | esbuild 使用 Go + 多執行緒處理，效率遠勝 JS 工具鏈 |

---

### Vite 的技術架構：兩階段處理，讓開發快又穩

Vite 的設計核心在於 **把「開發階段」和「建置階段」完全分開處理** 。

針對不同目的使用最合適的工具，既能確保開發體驗極致流暢，又能在上線時產出最小、最快、最乾淨的 bundle。

| 階段 | 使用工具 | 主要任務 | 處理方式說明 |
| --- | --- | --- | --- |
| **開發階段** | `esbuild` | 將 JSX / TS 等語法快速轉成瀏覽器可讀的格式 | 不進行打包，只做轉譯，讓模組能即時載入 |
| **建置階段** | `Rollup` | 將所有模組整理打包，壓縮、優化上線資源 | 執行 Tree-shaking、代碼壓縮等優化動作 |

### 開發階段：只做「轉換」，不做「打包」

在開發階段， **速度是第一要務** 。你需要的是：

- 修改 `.jsx` 檔後能 **立即看到畫面變化**
- 瀏覽器能夠快速取得你所撰寫的模組
- 開發伺服器能「即時」幫你轉換模組內容，而不是等待打包整包再重新載入

Vite 的解法是：

- 使用 [`esbuild`](https://esbuild.github.io/) 將 TypeScript、JSX、SCSS 等轉譯成瀏覽器可理解的原生 JavaScript/CSS
- 每一個模組都是「獨立存在的檔案」，使用 HTTP 請求逐一載入（符合 ES Modules 機制）
- 不打包、不合併，讓開發速度快得像按下即現 ✨

### 建置階段：正式「打包」，優化上線資源

開發完成後，你需要的是一個：

- 檔案數量少（減少 HTTP 請求）
- 檔案大小小（降低載入延遲）
- 沒有沒用到的程式碼（透過 Tree-shaking 移除）
- 壓縮過的 JS/CSS/圖片檔案

這時 Vite 會切換到「建置模式」，改用 [Rollup](https://rollupjs.org/) ：

- 將整個專案的依賴、模組、靜態資源 **全部收斂打包**
- 用 Rollup 強大的 Tree-shaking 移除未使用的程式碼
- 壓縮、合併、產出最終部署用的 `dist/` 資料夾

### 延伸補充：為什麼 Vite 要這樣「分兩段跑」？

這就是 Vite 聰明的地方： **它知道「開發」和「上線」有完全不同的需求** ，所以選擇最適合每個階段的工具和策略。

| 比喻類比 | 說明 |
| --- | --- |
| 📓 開發就像是寫筆記 | 快速輸入、即時看到內容變化，格式美不美先放一邊，重點是速度要夠快 |
| 📘 建置就像是出書 | 要整理格式、潤稿排版、挑版面、上光面，最終製作出一本印刷級的精緻成品 |

### 小節：兩段式處理才是現代開發的關鍵

這樣的架構讓 Vite 同時做到：

- 開發 **秒開、秒改、秒回饋** ，不讓你浪費時間等打包
- 上線時又能產出 **高效能的精簡檔案** ，提升整體載入效能

這不僅改寫了前端開發體驗，更成為現代前端工具的新標準。

無論你寫的是 React、Vue、Svelte 還是 Vanilla JS，Vite 都是開發體驗最棒的選擇之一。

---

### 一個指令就開跑！建立 React 專案超簡單

在過去，我們可能需要花上十幾二十分鐘手動設定 Webpack、Babel、開發伺服器等環境，才能跑起一個 React 專案。

但現在有了 Vite，只要一個指令，就能讓你 **秒速建立好一個現代化 React 開發環境** ，連專案結構都幫你準備好了！

### 步驟一：用 Vite 建立新專案

打開終端機（Terminal），執行以下指令：

```
npm create vite@latest my-app -- --template react
```
```
npm create vite@latest my-app -- --template react
```

#### 🧐 這行指令在做什麼？

- `npm create vite@latest` ：啟動 Vite 官方的專案建立工具（使用最新版）
- `my-app` ：你要建立的資料夾名稱（可自行更換）
- `--template react` ：指定使用「React 模板」，內建 JSX、React 設定檔與必要依賴

執行後，它會出現一些提示（例如選擇框架、模板），照著操作即可，很快就會看到：

```
✔ Scaffolding project in ./my-app...
✔ Done. Now run:

  cd my-app
  npm install
  npm run dev
```
```
✔ Scaffolding project in ./my-app...
✔ Done. Now run:

  cd my-app
  npm install
  npm run dev
```

### 步驟二：進入資料夾並安裝依賴

照著提示輸入以下指令：

```
cd my-app       # 進入新建立的專案資料夾
npm install     # 安裝所有依賴（React, Vite, Babel 等等）
```
```
cd my-app       # 進入新建立的專案資料夾
npm install     # 安裝所有依賴（React, Vite, Babel 等等）
```

這個階段會幫你安裝好開發需要的所有 npm 套件。大概幾十秒到一分鐘內完成（依網速與套件版本而異）。

### 步驟三：啟動開發伺服器

安裝完畢後，輸入：

```
npm run dev
```
```
npm run dev
```

馬上就會啟動 Vite 的開發伺服器，你會看到類似這樣的訊息：

```
VITE vX.X.X  ready in 300ms

➜  Local:   http://localhost:5173/
```
```
VITE vX.X.X  ready in 300ms

➜  Local:   http://localhost:5173/
```

👉 打開瀏覽器進入 `http://localhost:5173/` ，你就能看到 React 預設畫面成功跑起來！

### 為什麼這麼快？

- 沒有繁雜的 Webpack 設定
- 使用 esbuild 快速轉檔，開發伺服器幾乎是秒啟動
- 支援 **模組熱更新（HMR）** ：改程式不需手動刷新頁面，畫面自動更新

### 建立成功後的專案結構簡介

```
my-app/
├── index.html             # 專案入口 HTML
├── package.json           # 專案描述與依賴管理
├── vite.config.js         # Vite 的設定檔（可選）
├── src/
│   ├── main.jsx           # 程式進入點（ReactDOM.render）
│   └── App.jsx            # 主要元件（Hello Vite + React）
└── node_modules/          # 安裝好的第三方套件（自動產生）
```
```
my-app/
├── index.html             # 專案入口 HTML
├── package.json           # 專案描述與依賴管理
├── vite.config.js         # Vite 的設定檔（可選）
├── src/
│   ├── main.jsx           # 程式進入點（ReactDOM.render）
│   └── App.jsx            # 主要元件（Hello Vite + React）
└── node_modules/          # 安裝好的第三方套件（自動產生）
```

整個架構 **乾淨、清楚、沒有多餘 boilerplate** ，讓你可以專心寫程式，而不是處理工具設定。

---

## Vite 的貼心功能一次到位

| 功能 | CRA | Vite |
| --- | --- | --- |
| JSX 支援 | ✅ | ✅ |
| TypeScript | ✅ | ✅ |
| SCSS / CSS Modules | ✅ | ✅ |
| 環境變數 `.env` | ✅ | ✅ |
| 圖片 / 靜態資源 | ✅ | ✅ |
| 開發伺服器快 | ❌ | ✅ |
| 即時模組熱更新（HMR） | ⚠️ 慢 | ✅ |
| 插件系統擴充 | 普通 | 非常靈活（像是 vite-plugin-mock） |

Vite 幾乎內建了 CRA 常用的所有功能，還更快、更簡潔、更自由。

### 🧾 CRA vs Vite 總比較：誰適合你？

| 比較項目 | CRA | Vite |
| --- | --- | --- |
| 啟動速度 | 較慢（Webpack 全打包） | 超快（模組即時載入） |
| 編輯熱更新速度 | 可能卡頓 | 秒更新 |
| 自訂彈性 | 要 eject 才能改設定 | 設定開放，直接調整 vite.config.ts |
| 社群與資源 | 穩定但已停止主動開發 | 活躍成長中，插件多 |
| 適合的專案類型 | 初學者簡單專案 | 各類中大型專案、新技術堆疊 |

---

## 結語：為什麼大家都選 Vite？

Vite 就像是你期待已久的開發神隊友：

- 不囉嗦，幾秒就開跑
- 不難搞，設定好懂好改
- 不吝嗇，內建支援超齊全

不論你是 React 新手，還是 CRA 的老用戶，只要你開始用 Vite，一定會有「早知道就用這個」的感覺。

這也是為什麼越來越多前端框架（甚至包括 Astro、SvelteKit）都選擇把 Vite 當作底層工具。