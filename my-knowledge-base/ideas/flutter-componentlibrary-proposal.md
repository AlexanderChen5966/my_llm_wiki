---
title: Flutter ComponentLibrary 提議：統一元件註冊協定的構想
date: 2026-06-16
tags: [dev/flutter]
source: https://codelove.tw/@tony/post/qBkmPx
type: idea
status: processed
asset_value: 40
---

Flutter 生態目前在 GenUI（AI 生成介面）相關方向上出現多套不相容方案：Stac、Widgetbook、OpenUI 各自定義自己的元件描述格式，彼此無法互通。此提議構想一個官方層級的 `ComponentLibrary`/`Component` 抽象類別，作為跨工具的統一元件註冊協定，讓不同 AI 生成介面工具能共享同一套元件描述標準，而不必各自重造輪子。

核心問題仍是開放性的：誰來維護這個註冊協定的版本相容性？現有工具（Stac 等）是否有動機遷移到新標準？目前屬於社群討論階段的提案，尚未有官方 RFC 或具體實作時程，先記錄關注，待提案進入正式 RFC 階段再評估是否需要展開成完整 wiki 條目。

相關：[[flutter-patrol-e2e-testing]]
