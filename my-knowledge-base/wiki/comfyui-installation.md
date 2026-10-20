---
title: AI 室內設計工作流（二）：ComfyUI 安裝指南
date: 2026-07-24
tags: [ai/workflow, type/how-to]
source: https://tw-blog.reroom.ai/ai-comfyui-installation
type: article
status: processed
asset_value: 24
---

ReRoom AI 室內設計系列 **Part 2/4**，ComfyUI 桌面版逐步安裝教學（Windows / Mac）。核心：確認顯卡達標 → 裝 Git → 下載官方桌面版安裝程式 → 首次啟動下載缺少的 SD1.5 大模型。

## T（核心論點）

現在 ComfyUI 有桌面版一鍵安裝程式，最簡單、最推薦新手使用——依作業系統下載、跟著流程點下一步即可，環境會自動設定好。首次啟動會缺模型（Missing Models），下載預設的 SD1.5 大模型才能跑最基本的文生圖。

## B（適用場景）

**適用：** 準備在本機安裝 ComfyUI 的新手。
**硬體：** GPU 是最關鍵零組件——Nvidia 30 系中高階以上、VRAM 越高越好；Windows 需 Nvidia 顯卡，Mac 需 Apple Silicon（M 系列）。

## R（操作步驟）

1. **裝 Git**：Windows 到 git-scm.com/downloads/win 一路下一步；Mac 先裝 Homebrew（`/bin/bash -c "$(curl -fsSL .../install.sh)"`，依提示加入 PATH），再 `brew install git`。
2. **下載 ComfyUI 桌面版**：到官方 GitHub（comfyanonymous/ComfyUI#installing）Get Started 選 **Desktop Application** → 連到 comfy.org/download，依 OS 下載。（另有 Portable 版與手動安裝，步驟較多不推薦新手。）
3. **安裝**：開啟 .dmg（Mac 拖到應用程式）→ Get Started → 選 GPU → 選安裝路徑 → 是否曾裝過（新手 Next 跳過）→ 自動更新/使用資訊可關 → 安裝程式自動設定環境。
4. **下載大模型**：首次開啟會提示 Missing Models，先下載 ComfyUI 預設的 **SD1.5** 模型（非強制，之後可換自己要的模型）。
5. **遇問題**：查 ComfyUI GitHub Repo 的 **Issues 分頁**。

## C（關鍵行動）

- [ ] 安裝前先確認 GPU 達標（Windows: Nvidia / Mac: Apple Silicon）
- [ ] 用桌面版一鍵安裝，別一開始就挑 Portable/手動版
- [ ] 首次啟動記得下載 SD1.5 才能生第一張圖，接著進 Part 3

## 原始摘要

作者 Wayne Huang（ReRoom AI），2025-02-27。以 M4 MacBook Pro 示範，Windows 程序基本相同。涵蓋硬體需求、Git 安裝（含 Homebrew）、桌面版下載安裝、SD1.5 模型下載、Issues 求助。

## 相關連結

- [[comfyui-interior-design-workflow]] — 系列 Part 1：總覽（硬體需求詳解）
- [[comfyui-interior-design-process]] — 系列 Part 3：節點與工作流實作（下一篇）
- [[comfyui-getting-started]] — ComfyUI 入門整合條目（另含 Windows+Krita 安裝、8188 端口等通用安裝細節）
