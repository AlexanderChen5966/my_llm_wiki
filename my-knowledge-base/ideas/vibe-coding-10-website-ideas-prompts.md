---
title: Vibe Coding 10 種互動網站靈感與 prompt 寫法：收集參考圖、列共同特徵、一句話寫流程、一次只改一件事
date: 2026-10-16
tags: [ai/prompt-engineering, ai/workflow]
source: https://www.aiposthub.com/vibe-coding-website-ideas/
type: idea
status: processed
asset_value: 12
---

aiposthub（Joanna Chang，2026-10-14）整理 10 種可用 ChatGPT／Claude 做的創意互動網站，每種附設計思路與可複製的 prompt。價值在於 prompt 的寫法，而非個別案例；未見實測結果以外的驗證。

## 10 種主題

復古自拍亭（getUserMedia 連拍四格黑白底片）、熱感應（游標當熱源）、音檔可視化（點陣聲紋）、白板便利貼（拖曳＋畫筆）、賽博監控（追蹤框＋REC）、萬花筒（鏡像切片）、電子郵票（POST//FUTURE）、動態文字（SWARM／GLITCH／GRAVITY）、像素房間（VIBE HOUSE）、重力感應（Matter.js）。

## 可遷移的做法

1. 收集幾張喜歡的參考圖，列出**共同特徵**清單，當作 prompt 素材
2. 把使用流程寫成**一句話**（進入首頁 → 拉開布簾 → 倒數 → 閃光 → 出現底片）
3. 第一版後**一次只改一件事**：先調顏色／疏密，再調動畫手感，最後才加音效、鏡頭等功能
4. 通用 prompt 條件：單一 HTML 檔、手機可用、拒絕權限時顯示提示、支援 `prefers-reduced-motion`、標題保持可讀
5. 描述視覺風格、操作流程、動畫與素材，比「做個很酷的網站」更容易得到預期成果

## 技術小提醒

- `getUserMedia` 只能在 localhost 或 HTTPS 運作，雙擊 HTML 通常沒反應，可用 `npx serve`
- Canvas 讀本機圖片會被視為跨來源而變黑；用本機伺服器或轉 base64
- 熱場在約 200–300 欄的低解析網格上計算再放大，逐像素計算在手機會卡
- 音檔分析用 Web Audio `AnalyserNode`（fftSize 2048）；重力碰撞用 Matter.js

## 備註

- 原文第 08 篇（動態文字）圖說誤貼像素公寓文字，屬原文瑕疵
- 方法論層面的 vibe coding 討論見 [[vibe-vs-agentic-coding-maturity]]；本篇只是創意題材與 prompt 清單，無衝突
- 與 [[karpathy-four-output-formats-asd-ste100]] 的「用 HTML 做互動頁」是同類做法
