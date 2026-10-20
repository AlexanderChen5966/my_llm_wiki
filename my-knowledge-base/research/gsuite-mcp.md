---
title: gsuite-mcp：完整 Google Workspace MCP Server
date: 2026-05-28
tags: [ai/mcp, ai/workflow]
source: https://github.com/aliwatters/gsuite-mcp
type: reference
status: processed
asset_value: 18
---

## 核心論點

aliwatters/gsuite-mcp 是用 Go 寫的 Google Workspace 完整 MCP Server，涵蓋 Gmail、Calendar、Drive、Docs、Tasks、Sheets、Slides、Forms、Contacts、Meet、Chat，支援多帳號，10 分鐘內可安裝完成。

## 特色

- **單一 Go binary**：不需要 Node.js 或 Python 依賴，安裝簡單
- **完整 Workspace 覆蓋**：12 種 Google 服務一次接入
- **多帳號支援**：可同時管理個人帳號與工作帳號
- **單行安裝**：`./install.sh` 後設定 OAuth 即完成

## 安裝

```bash
git clone https://github.com/aliwatters/gsuite-mcp.git
cd gsuite-mcp
./install.sh
# 認證後加入 claude mcp 設定
```

## 相關連結

- [[google-workspace-notebooklm-gemini-admin-workflow]]
- [[twinkle-hub-taiwan-mcp]]
