---
title: 為什麼大家都改用 Vite？現代前端開發工具原理
date: 2026-06-30
tags: [dev/architecture, type/concept]
source: https://realnewbie.com/posts/why-developers-are-switching-to-vite-the-ultimate-react-dev-tool
type: article
status: processed
asset_value: 44
---

Vite（Evan You 設計）是現代前端開發工具，解決 Webpack/CRA 在開發階段「專案越大啟動越慢、改一行等好幾秒」的痛點。核心設計：**開發階段不打包全部，只處理瀏覽器當下需要的模組**。

> 註：前端 React/Vue 工具鏈非本知識庫核心焦點（AI 工具/PKM/Flutter/Spring Boot），此條目作為工具原理備查。

## T（核心論點）

傳統 Webpack 在開發一開始就把整個專案打包合併，改一個檔也要重解析一堆沒改的檔（拖卡等）。Vite 反其道：利用瀏覽器原生 ES Modules，遇到 import 才用 HTTP 請求該模組——「需要什麼才給什麼」，啟動與熱更新都快。

## B（適用場景）

- 各類中大型前端專案、新技術堆疊（React/Vue/Svelte/Vanilla）
- 受不了 CRA 啟動慢、eject 才能改設定
- Astro、SvelteKit 等框架也以 Vite 為底層

## R（三大關鍵設計 + 兩階段架構）

**開發快的三個秘密**：
1. **原生 ESM**：每個 .js/.jsx/.ts 當獨立模組，import 時才 HTTP 載入，跳過「一開始全打包」
2. **依賴預編譯**（dependency pre-bundling，由 esbuild）：啟動時把 node_modules 第三方套件打包成快取，之後不再重編
3. **esbuild 極速轉譯**（Go 語言、多執行緒，比 Webpack 快 10–100 倍）：改 jsx/ts 幾乎即時轉成瀏覽器可執行 JS

**兩階段處理**（開發與建置分開，各用最適工具）：
| 階段 | 工具 | 任務 |
|------|------|------|
| 開發 | esbuild | 只轉譯不打包，模組即時載入 |
| 建置 | Rollup | Tree-shaking、壓縮、產出精簡 dist/ |

**一行建立 React 專案**：`npm create vite@latest my-app -- --template react` → cd → npm install → npm run dev（開發伺服器 ~300ms 啟動）。

**vs CRA**：啟動快、熱更新秒更、設定開放（直接改 vite.config.ts 不用 eject）、插件靈活；CRA 已停止主動開發。

## C（對自身的啟發）

- [ ] 「開發/建置需求不同、各用最適工具」的兩階段思路，是工程分層的通則
- [ ] 「只處理當下需要的」按需載入哲學，呼應 Agent 的 Deferred Tool Loading

## 相關連結

- [[ai-website-cloner-template]] — AI 重建 React 網站（產出 Vite/React 專案）
- [[vue-tui-terminal-ui]] — 同為 Evan You 生態的終端 UI 框架
