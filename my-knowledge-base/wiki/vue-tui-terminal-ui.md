---
title: Vue-tui 0.1：Vue 官方原生終端 UI 框架
date: 2026-06-30
tags: [dev/architecture, type/concept]
source: https://codelove.tw/@tony/post/3kVVOx
type: article
status: processed
asset_value: 40
---

vue-tui 是 Vue 官方原生終端 UI 框架（0.1 發布、Evan You 轉發），讓 Vue 開發者做互動式終端介面（CLI、TUI）時不必再切換成 React Ink 的 React 語法。

> 註：前端框架週邊，非本知識庫核心焦點，作為技術動態備查。CLI/終端 UI 與 AI Agent 命令列工具生態有間接關聯。

## T（核心論點）

以往做好看、可互動的終端介面基本只能用 React Ink，Vue 開發者被迫切換技術棧、成本高。vue-tui 讓 Vue3 / setup 語法 / SFC / 響應式 API 直接復用到終端開發，消除技術棧割裂。

## B（適用場景）

Node 腳手架、CLI 工具、終端面板、本地 AI 終端用戶端、維運監控介面——且團隊以 Vue 為主。

## R（主要內容）

**四大優勢**：技術棧無縫統一（原生 Vue3/SFC/響應式）、終端 HMR 熱更新、自帶元件級終端測試工具、組合式 API 管理輸入/焦點/視窗。

**版面系統**：基於 Yoga 版面引擎（與 React Ink / React Native 同源），用前端通用 Flex 版面。核心元件 Box（容器，屬性近 CSS Flex）、Text（粗體/變色/截斷）、Spacer（自適應占位）。

**終端互動能力**：鍵盤監聽、焦點管理（Tab 切換）、貼上事件（適配 AI 對話批次輸入）、視窗適配、幀動畫、無互動輸出（CI 日誌快照）。

**入坑理由**：Vue 經驗零浪費、開發體驗（熱更新/元件化/可測試）輾壓傳統終端開發、Vue 官方維護核心 API 已穩定。

## C（對自身的啟發）

- [ ] 終端 UI 框架的成熟，反映 CLI/TUI 作為 AI Agent 主要介面的趨勢
- [ ] Yoga（Flex）跨 React/Vue/Native 同源，是版面引擎抽象復用的例子

## 相關連結

- [[vite-frontend-build-tool]] — 同屬 Evan You 前端生態
