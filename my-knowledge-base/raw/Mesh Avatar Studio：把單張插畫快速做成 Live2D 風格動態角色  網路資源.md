---
title: "Mesh Avatar Studio：把單張插畫快速做成 Live2D 風格動態角色 | 網路資源"
source: "https://tools.wingzero.tw/articles/4087"
author:
  - "[[赤紅翼零]]"
published: 2026-10-05
created: 2026-10-06
description: "Mesh Avatar Studio 是開源 AI 動態角色製作工具，可透過 Claude Code、Codex 將單張人物插畫轉成 Live2D 風格 2D Avatar。支援眨眼、Lip Sync、轉頭、呼吸與頭髮動態，並提供即時預覽與控制點微調，適合 VTuber 與虛擬角色創作者快速製作動態頭像。"
tags:
  - "clippings"
status: "inbox"
---
Mesh Avatar Studio 是一個協助使用者將單張人物插畫製作成 Live2D 風格動態角色的開源工具。它結合 Claude Code 或 Codex 等程式代理人，以及在本機執行的編輯器：代理人會依照插畫建立角色骨架、圖層與控制點，使用者再透過即時預覽調整細節，讓角色能眨眼、說話、轉動頭部、呼吸，並呈現頭髮擺動等動態效果。對想讓靜態立繪快速動起來的創作者來說，這套流程能協助處理角色切圖與初始設定，再由使用者自行修整。

製作自己的角色時，使用者需要在專案資料夾中透過 Claude Code 或 Codex，提供插畫路徑並請代理人依照專案的 Agent Guide 建立 Avatar。專案建議使用正面、頭肩構圖、透明背景的 PNG，這類素材較容易辨識人物輪廓與各個身體部位。生成的角色專案會保存在 `projects/` 資料夾，接著可用本機編輯器開啟並檢查。

編輯器將角色部位與即時預覽放在同一個工作區。使用者可以選取左側的部位，再拖曳圖像上的控制點；右側預覽會立即呈現調整結果。Pose test 可檢查頭部轉動、傾斜與閉眼效果，Lip sync 則能測試不同母音嘴型。眼睛、手部輪廓或頭髮遮罩範圍若需要修正，也能調整對應區域並重新建立圖層，讓角色在動作時減少錯位或變形。

Mesh Avatar Studio 的主要功能包括：

- **單張插畫轉成動態角色：** 以一張人物圖像為基礎，建立可眨眼、說話、轉頭和呼吸的 2D 網格 Avatar。
- **AI 輔助角色建置：** 由 Claude Code 或 Codex 按照指引準備角色骨架、圖層和控制點。
- **即時預覽與手動微調：** 拖曳控制點，觀察臉部、頭髮與身體部位在動態預覽中的變化。
- **姿勢與口型測試：** 使用 Pose test 與 Lip sync 檢查動作、閉眼和母音嘴型。
- **繪製表情變體：** 可另外準備閉眼圖與不同嘴型，使眨眼和說話效果更自然。

開始使用需安裝 Node.js 22.17 以上版本，再執行 `npm install` 和 `npm run dev` ，於瀏覽器開啟本機編輯器。GitHub 專案附有 Miko 範例角色，可先熟悉操作；若要製作自己的角色，則需準備符合條件的插畫並使用支援的程式代理人。Codex 可透過內建圖像生成功能協助繪製眼睛與嘴型變體；Claude Code 則會準備遮罩和提示內容，後續需交由其他圖像生成工具處理。

整體來看，Mesh Avatar Studio 適合 VTuber、虛擬角色創作者，以及希望把靜態人物立繪轉為 Live2D 風格角色的使用者。它把 AI 建立初始結構、本機預覽與手動微調串成一套流程，可用來加快動態頭像或角色原型的製作。專案程式碼採 MIT 授權；範例角色 Miko 則有獨立的使用規範。官方目前描述的是自有編輯器中的 2D 網格 Avatar 製作流程，未標示能輸出 Live2D Cubism 專案檔，若目標是接續使用 Cubism 編輯器，建議先確認所需的匯出格式。

<iframe frameborder="0" src="https://7cde9425b3949eadd7ca35925a186993.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe><iframe frameborder="0" src="https://7cde9425b3949eadd7ca35925a186993.safeframe.googlesyndication.com/safeframe/1-0-45/html/container.html" title="第三方廣告內容" width="336" height="280" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allow="private-state-token-redemption" aria-label="廣告"></iframe>