---
title: "Genex 桌面版登場：用 Claude Code、Codex 或本地模型製作網頁遊戲"
source: "https://www.aiposthub.com/genex-ai-game-builder-desktop/"
author:
  - "[[Philo]]"
published: 2026-10-06
created: 2026-10-07
description: "Genex 推出 MIT 授權桌面遊戲開發工具，可接 Claude Code、Codex 或本地模型，產出 Three.js 網頁遊戲。本文整理支援系統、模型與素材成本，以及實驗性自我修改功能的限制。"
tags:
  - "clippings"
status: "inbox"
---
想做一款小遊戲，除了生成程式，還要處理畫面、素材、執行錯誤與檔案管理。Genex 新推出的桌面版，把這些工作放進同一個介面，讓代理在本機專案中建立與修改遊戲。

它目前聚焦能在瀏覽器執行的遊戲。對想快速驗證玩法的創作者，這比直接宣稱「一句話做出任何商業大作」更貼近可用的起點。

![Genex 桌面版 AI 遊戲開發流程展示（官方影片封面）](https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/images/2026/10/x20261006-04-genex-0.jpg)

Genex 桌面版 AI 遊戲開發流程展示（官方影片封面） 圖片來源： Genex 官方貼文 。

## Genex 目前製作的是 Three.js 網頁遊戲

依 [Genex 桌面版官方頁面](https://genex.games/desktop?ref=aiposthub.com) ，專案使用 Three.js 與一般網頁技術，成果以本地檔案儲存，可匯出靜態內容。Three.js 是用來在瀏覽器呈現 3D 畫面的程式工具，適合建立可以分享網址或自行部署的遊戲。

本地檔案的優點是容易檢視與交接。創作者可以繼續修改程式，或讓其他開發工具接手，但仍需測試不同瀏覽器、載入速度與操作方式。官方把 Unity、Unreal 與原生應用列為後續方向，這些不應當作現在已支援的輸出格式。

## 訂閱帳號與本地模型怎麼選

Genex 可使用 Claude Code、Codex 的既有訂閱，或透過 Ollama、Bonsai 等方式使用本地模型。本地模型是把推論工作放在自己的裝置執行，能否順暢，取決於硬體與所選模型能力。

| 使用方式 | 主要考量 |
| --- | --- |
| Claude Code、Codex | 使用既有帳號與其用量限制 |
| 本地模型 | 自行負擔硬體、速度與模型設定 |
| 額外素材生成 | 可能透過 Genex 外掛使用付費服務 |

桌面工具本身採 MIT 授權，不代表模型、素材與部署都沒有成本。即使不用另設 API，也就是程式串接服務的介面，原本訂閱的額度仍會消耗。

<video src="https://storage.ghost.io/c/5e/61/5e6189f0-c5e0-49a0-97b4-5eddd3f43c77/content/media/2026/10/x20261006-04-genex.mp4" width="1280" height="720" controls=""></video>

Genex 桌面版 AI 遊戲開發流程展示。此為原始示範，功能與實際效果依官方說明為準。 影片來源： Genex 官方貼文 。

## macOS、Linux 可下載，Windows 仍在預告

官方頁面提供 macOS 與 Linux 桌面版本，Windows 標示為後續推出。安裝前先確認系統，是避免跟著社群影片操作卻找不到相同入口的第一步。

Genex 也展示讓代理調整自身工作框架的實驗功能，預設須主動開啟。它搭配版本快照、評估與回復機制，但不代表每次修改都安全或每個遊戲都能啟動。新手先固定工具設定，把精力放在玩法與錯誤回報，通常比較容易知道問題出在哪裡。

## Genex 常見問題

### Genex 可以免費無限生成遊戲嗎？

MIT 授權適用於軟體本身。模型用量、硬體與部分素材服務有各自成本，不能把開源直接理解成全部免費。

### 做出的遊戲可以繼續由人修改嗎？

成果儲存在本地專案，可檢視與編輯。分享之前，仍應檢查素材權利、載入資源與遊戲的實際運作。

延伸閱讀： [Codex 做的遊戲能放進掌機：ModRetro 合作讓 AI 程式有了具體試玩入口](https://www.aiposthub.com/modretro-codex-chromatic-playable-games/)

## 結語：先用一個小玩法驗證流程

我會從一個場景、少量互動的原型開始，觀察代理能否修正可重現的錯誤。當專案仍小、檔案仍看得懂，Genex 的整合介面比較容易幫創作者建立可接手的成果。