---
title: text-to-lottie：用 Claude Code/Codex 生成 Lottie 動畫的 Skill
date: 2026-06-17
tags: [ai/agent]
source: https://github.com/diffusionstudio/lottie
type: idea
status: processed
asset_value: 42
---

text-to-lottie 是 diffusionstudio 的開源 Skill，讓 Claude Code/Codex 等 coding agent 從自然語言描述直接生成可上線的 Lottie JSON 動畫檔，並附帶本地預覽 player。

安裝：`npx skills add diffusionstudio/lottie`

關鍵 prompt 技巧：提供 SVG 素材（比純文字描述效果好很多）、使用動態設計術語（ease-in/ease-out）、描述鏡頭運動（push/pan/zoom）、明確指定 FPS 和總幀數。

輸出格式：標準 Lottie JSON，可直接用於 Web（lottie-web）/React Native（lottie-react-native 或 React Native Skia）/iOS Swift（Lottie SDK）/Android Kotlin（LottieAnimationView）/Flutter（lottie 套件）。

暫時記錄關注，若 Flutter 專案需要動畫功能時可評估導入。

## 補充（來源：https://muki.tw/lottie-skill/ ，2026-08-14）

🔑 **關鍵新資訊：這其實是「兩個 Skill 互補」而不是一個。** 原本本條目只記了產出端。

| Skill | 角色 | 職責 |
|---|---|---|
| **motion-design-skill** | 教科書 ＋ 導演 | 不產出檔案，而是先把動態設計原則餵給 agent：**時間拿捏**（快／慢／停頓）、**節奏**（循環／漸進／強弱對比）、**編排**（元素怎麼出場與互動）、**情緒張力**（緊張／輕鬆／期待／高潮）|
| **text-to-lottie** | 工程師 ＋ 剪接師 | 把想法做成可用的 Lottie JSON（內建播放器可即時預覽微調）|

> **motion-design-skill 幫你想得更專業，text-to-lottie 幫你做出成品。**

**實驗一（SVG 小動畫）**：從 heroicons 抓 SVG ＋ AI 生成素材 → 配文字說明丟給 text-to-lottie。屬技術驗證：文字 + SVG + 合適的 Skill 就能做出堪用動畫。

**實驗二（五子棋對弈）**：作者只下了一句很口語的指令——「俯視棋盤、上下各兩隻手下五子棋、一來一往、約 8 秒、**不要一下就分出勝負，來回還是要有一點難度**」。**沒有指定座標與關鍵幀時間點**，講的是節奏與觀眾感受；motion-design-skill 扮演導演決定哪裡快、哪裡慢、何處停頓把情緒拉滿，再轉交 text-to-lottie 實作。

🔑 **作者的方法論總結（本篇最值得留的一句）：偏好「用粗淺的日常對話，搭配複雜完善的 Skill」——不是每個人都懂 After Effects 或 Figma Motion，但很多人對心中的畫面有清楚的感受與想像。把專業知識與工程產出都收進 Skill，使用者就能只專心說出自己要什麼，而不是被工具牽著走。**

這條「一句話想法 → 可上線動畫資產」的工作流，與 [[skill-decision-what-to-automate]] 的「省的是重複判斷」是同一個主張的創作端版本。

## 相關連結

- [[lottie-detools-article]] — 第三來源，補適用邊界（適合單場景／向量圖示／動態圖形／資料視覺化；不適合多鏡頭、複雜剪輯、音訊同步）
- [[skill-decision-what-to-automate]] — 什麼該做成 Skill
- [[open-design-taste-skill-integration]] — 品味類 Skill 的另一例
- 📎 2026-10-19：[[ideas/claude-code-animation-prompt-qa-delivery]] — 用 Claude 寫 HTML／SVG 程式動畫的需求、驗收與交付（另一種產出格式）
- 📎 2026-10-19：[[claude-codex-video-toolchain-selection]] — Remotion 接 Lottie（`@remotion/lottie`），部分 expression 不保證逐幀一致，正式輸出前要檢查閃爍
