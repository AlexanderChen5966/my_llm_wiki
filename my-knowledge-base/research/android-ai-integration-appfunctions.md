---
title: Android AI 整合：AppFunctions、A2UI、ADK
date: 2026-06-01
tags: [ai/mcp, ai/agent]
source: https://codelove.tw/@tony/post/xNQodq
type: note
status: processed
asset_value: 36
---

Google 於 2026 年在 Android 官方層面推出多項 AI 時代支援，核心是讓 Android App 成為 AI Agent 的工具服務提供者，而非只是使用者直接操作的界面。AppFunctions 被官方定位為「Android MCP」，A2UI 讓 AI Agent 動態生成 Jetpack Compose UI，ADK 則提供官方多 Agent 編排框架。

## T（核心論點）

2026 年 Android 平台的 AI 整合分四個方向：

1. **AppFunctions（Android MCP）**：App 把部分能力（資料/動作）以「自我描述函式」的方式暴露給 AI Agent（如 Gemini）。使用者只需自然語言指令，不需打開目標 App。例：「從圖庫找我貓的照片」→ Gemini 觸發圖庫 App 的 AppFunction 直接返回結果
2. **Gemma 4 / Gemini Nano 4 端側推理**：透過 Prefix Caching（儲存重用提示中的重複部分中間狀態）縮短裝置端推理時間。支援結構化 JSON，適合簡單本地任務
3. **A2UI Jetpack Compose Renderer**：AI Agent 可動態產生 Compose UI——模型輸出的不是 Dart/HTML 程式碼，而是結構化 UI 描述，Compose Renderer 將其渲染為真實界面。高德的 AGenUI 是同類開源框架
4. **ADK for Android**：Google 首個官方面向 Android 的 Agent Development Kit。支援裝置端+雲端混合多 Agent 工作流，主打「混合編排」——雲端模型為主 orchestrator，特定任務卸載至裝置端子 Agent

**背景**：XML 佈局已宣布不再更新，Compose 成為唯一官方 UI 路線；JetBrains 的 Koog 支援 KMP 場景（不同於 Google ADK）

## B（適用場景）

**適用：**
- Android 原生開發者評估如何讓自己的 App 接入 AI Agent 生態（AppFunctions）
- 需要了解 Flutter vs Android 原生 AI 支援差距的開發者
- 研究跨平台 A2UI 協議的架構師

**不適用 / 注意：**
- AppFunctions 目前仍在推進中，生產就緒程度需持續追蹤
- 端側 AI（Gemini Nano）目前適合「簡單本地任務」，複雜推理仍需雲端模型
- 仍缺少類似 Dart MCP 的 Android MCP 用於 AI 除錯

## R（操作步驟）

1. 評估自己 App 哪些能力值得暴露為 AppFunction（CRUD 操作、媒體存取、資料查詢）
2. 透過 AIcore 開發者預覽版測試 Gemini Nano 4 的端側能力
3. 如需動態 UI 生成，評估 A2UI Compose Renderer 的整合成本
4. 追蹤 ADK for Android 的穩定版發布時間

## C（關鍵行動）

- [ ] 閱讀 AppFunctions 官方文件，評估三隆 App 是否有適合暴露的功能
- [ ] 關注 Flutter GenUI/A2UI 是否推出跨平台版本
- [ ] 追蹤 ADK for Android 正式版發布（目前為 preview）

## 原始摘要

介紹 Google 在 Android 官方層面針對 AI 時代推出的支援，包括 AppFunctions（定位為 Android MCP，讓 App 為 AI 提供 MCP 呼叫服務）等新機制。整理 Android 平台如何與 AI Agent 生態整合的官方路線。

## 相關連結

- [[flutter-google-io-2026]] — Flutter 3.44/Dart 3.12 AI 整合（同期 Google IO 2026）
- [[google-adk-2-graph-workflow]] — Google ADK 2.0 圖工作流架構
- [[agent-skill-design-patterns]] — Google 5 大 Skill 設計模式
