---
title: OpenMontage：把 AI Coding Assistant 變成完整影音製作團隊的開源系統
date: 2026-07-28
tags: [ai/agent, ai/workflow]
source: https://tools.wingzero.tw/articles/3998
type: article
status: processed
asset_value: 55
---

## T（核心論點）

多數 AI 影片工具是「輸入 Prompt 直接產生短片」的單次生成模式，**OpenMontage 走的是完全不同的路線**：把影片製作拆成完整工作流程（研究→腳本→分鏡→素材→配音→字幕→剪輯→輸出），讓 Claude Code / Cursor / Copilot / Codex 等 AI Coding Assistant 直接閱讀專案內的 Pipeline、技能文件與工具，自行決定如何逐步完成每一階段——本質是一套**給 AI 用的「影片製作作業系統」**，而非單一模型或 SaaS 平台。

## B（適用場景）

**適用：** 需要大量、可重複執行影音內容的創作者（YouTuber、教育內容製作者、品牌行銷團隊、技術教學作者）；想建立可客製化、能與現有 AI Coding Assistant 深度整合的影音自動化流程

**不適用 / 注意：** 偶爾產一支短片用一般商業服務（Runway/Pika）更直覺；此專案更適合開發者/技術團隊，設定門檻高於純消費型 AI 影片工具

⚠️ 2026-10-19：評價落差並列——aiposthub 站內實測（15 秒、79 秒直式影片）滿分 5 分給 2.5～3 分：環境設定最耗時、不同製作流程結果不穩、字幕條不能細調，「還不適合當穩定量產工具」；授權為 AGPL-3.0（修改後提供他人使用或架成網路服務須公開原始碼，aiposthub 轉述）。本條原評價（wingzero）保留，不擇一。見 [[claude-codex-video-toolchain-selection]]。

## R（架構與內容）

### Agent-first 架構，無傳統 Workflow Engine

沒有集中式的流程控制器，而是讓 AI Coding Assistant 直接讀取專案的 Pipeline 定義、Agent Skills 與工具清單，自行規劃執行順序。

### 8 種內建 Pipeline

| Pipeline | 用途 |
|---|---|
| Animated Explainer | 動畫解說影片 |
| Cinematic | 電影預告、品牌影片 |
| Documentary Montage | 紀錄片風格剪輯 |
| Talking Head | 人物口播影片 |
| Podcast Repurpose | Podcast 內容再利用 |
| Screen Demo | 軟體操作示範 |
| Localization & Dub | 翻譯與配音 |
| Clip Factory | 長影片切短影音 |

### 免費工具 + 商業 API 混合可用

即使沒有付費 API，也能靠 Piper TTS（離線語音）、Archive.org、NASA、Wikimedia Commons 等免費素材來源，搭配 Remotion、FFmpeg 完成剪輯輸出；若接上 OpenAI、Runway、Google Veo、Kling、ElevenLabs 等 API，可自動切換供應商提升品質——**整合超過 50 種影音工具與數百項 Agent Skills**。

### 🔑 Reference Video 模式

提供一支 YouTube Shorts/TikTok/Reels 作參考，OpenMontage 會分析節奏、分鏡、旁白與整體風格，**提出新的創作方向而非直接複製**——比自行寫大量 Prompt 更容易快速建立符合需求的影片企劃。

### 品質把關機制

正式輸出前會執行影片品質檢查、自動驗證、成本估算與決策紀錄，降低產出失敗或品質不佳的機率——這類設計較少出現在一般 AI 影片工具，更偏向專業製作流程思維。

## C（關鍵行動）

- [ ] 需要單次快速產一支短片 → 用 Runway/Pika 等商業服務即可，不必上手 OpenMontage
- [ ] 需要可重複執行、可客製化的影音自動化流程 → 評估 OpenMontage 的 Agent-first 架構
- [ ] 想降低成本 → 優先接免費工具（Piper TTS/Archive.org/Wikimedia），再視品質需求疊加商業 API
- [ ] 有現成短影片想模仿風格時，用 Reference Video 模式分析節奏與分鏡，而非直接複製

## 補充資料：Claude Video——同思路的輕量對照（2026-08-05 併入）

來源：`github.com/bradautomates/claude-video`（tools.wingzero.tw 介紹）。與本條目**定位互補而非重疊**，故併入對照而不另建條目：

| | **OpenMontage**（本條目）| **Claude Video** |
|---|---|---|
| 定位 | 完整影片**製作**流程（研究→腳本→分鏡→素材→配音→剪輯→輸出）| 影片**後製加工**（剪除片段、合併、裁切、加字幕、轉格式、音訊同步）|
| 核心 | Agent-first，8 種 Pipeline，整合 50+ 工具 | Claude 直接操作 **FFmpeg**，把繁多指令交給 AI 規劃 |
| 產出 | 從無到有的新影片 | 改造既有素材 |
| 適合 | 需要大量、可重複執行的內容生產 | 需要頻繁後製但不想記 FFmpeg 參數 |

**Claude Video 的共同設計哲學**：同樣採 AI Agent 工作模式而非「單一 Prompt 對應一條指令」——同時要求「剪除前後空白＋加字幕＋輸出社群尺寸」時，AI 會拆解為多步驟依序完成。且因所有操作最終由 FFmpeg 執行，**理論上 FFmpeg 能做的都能逐步擴充支援**，不受限於固定功能。

> 兩者可疊加：用 OpenMontage 生產，用 Claude Video 做後續格式與尺寸的批次調整。

## 相關連結

- [[codex-computer-use-modes]] — Codex 操作瀏覽器/電腦的四種方式；OpenMontage 是同一類「讓 AI Coding Assistant 執行專業領域工作流」思路在影音製作的延伸
- [[claude-code-markdown-single-source-multi-format]] — Markdown 單一真相來源多格式輸出；與 OpenMontage 的 Pipeline 化思維同屬「把 AI Coding Assistant 能力延伸到非程式碼產出」
- 📎 2026-10-19：[[claude-motion-beta-2026-10]] — Claude Motion：Claude 寫程式做解說動畫並匯出 MP4，下游可接 Runway 等；配音仍需在 Motion 外處理
- 📎 2026-10-19：[[claude-codex-video-toolchain-selection]] — 影片工具五類地圖，OpenMontage 歸「整套影片代理」，含 aiposthub 實測評分與 AGPL 授權提醒
