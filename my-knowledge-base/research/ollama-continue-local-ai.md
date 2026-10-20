---
title: Ollama × Continue.dev：本地端 AI 開發環境設定
date: 2026-05-28
tags: [ai/agent, dev/architecture]
source: https://www.cc.ntu.edu.tw/chinese/epaper/home/20260320_007606.html
type: article
status: processed
asset_value: 18
---

## 核心論點

Ollama（本地 LLM）+ Continue.dev（VSCode 擴充）= 免費、離線、資安合規的 AI 輔助開發環境，適合不允許使用雲端 AI 的企業或公家機關。

## 設定步驟

1. 安裝 Ollama，在本地啟動 LLM（API：localhost:11434）
2. VSCode 安裝 Continue 擴充模組
3. Continue 設定連接本地 Ollama API
4. 選擇模型（越大越好但需考慮硬體）→ Connect → 自動下載

## 適用場景

- 企業內部禁用雲端 AI（商業機密保護）
- 政府/公家單位（合規、可控、安全）
- 想完全免費使用 AI 輔助開發

## 相關連結

- [[dual-tool-strategy]]
