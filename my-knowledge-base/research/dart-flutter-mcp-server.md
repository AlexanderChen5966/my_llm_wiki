---
title: Dart and Flutter MCP Server：讓 AI 更聰明開發 Flutter App
date: 2026-05-28
tags: [dev/flutter, ai/mcp]
source: https://medium.com/彼得潘的-flutter-app-開發問題解答集/提升-ai-flutter-app-開發能力的-dart-and-flutter-mcp-server-305720171089
type: article
status: processed
asset_value: 48
---

## 核心論點

Google 推出的 Dart and Flutter MCP Server 讓 AI（Claude、GitHub Copilot 等）在開發 Flutter App 時能直接存取 Flutter/Dart 工具鏈能力：修正 runtime error、從 pub.dev 搜尋並安裝套件、存取 Flutter 文件。

## 主要功能

- **修正 runtime error**：AI 可直接讀取 Flutter 的 error log 並提出修正
- **pub.dev 套件搜尋**：AI 主動在 pub.dev 找適合的套件並安裝
- **Flutter 文件存取**：AI 可查詢官方 API 文件，減少幻覺

## 安裝方式（VS Code + GitHub Copilot）

1. 開啟 VS Code Settings (UI)
2. 搜尋 `MCP servers`
3. 加入 Dart and Flutter MCP Server 設定
4. 重啟 VS Code，Copilot 即可使用

## 適用場景

- Flutter 開發時遇到 runtime error 不知道怎麼查
- 需要找 pub.dev 套件但不確定哪個最合適
- 想讓 AI 生成更符合 Flutter 慣例的程式碼

## 相關連結

- [[flutter-google-io-2026]] — Flutter 3.44 官方 AI 功能（含 flutter/skills）
- [[gsuite-mcp]] — 另一個實用 MCP Server 範例
