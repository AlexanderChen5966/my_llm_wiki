---
title: tmux 入門速查：Session / Window / Pane 三層與常用指令
date: 2026-08-14
tags: [dev/architecture]
source: https://vocus.cc/article/69cb6b60fd897800016c0eb5
type: reference
status: processed
asset_value: 28
---

輕量速查筆記。**通用工具教學、網路隨手可查，未達 wiki 門檻**，但因 [[herdr-agent-terminal-multiplexer]] 的定位是「為 AI agent 場景重做的 tmux」且沿用同一組快捷鍵（`Ctrl+B` 前置鍵），保留此篇作為背景參照。

## 核心概念：三層階層

1. **Session（對話）** — 最高層級，通常一個專案一個獨立 Session
2. **Window（視窗）** — 類似瀏覽器分頁，一個 Session 可含多個 Window
3. **Pane（面板）** — Window 內切割出的區塊，每個都是獨立的指令列介面

## 為什麼要用

SSH 連線到伺服器時，網路不穩導致連線中斷、程式被迫停止。tmux 讓程式在背景持續運行，並可在同一畫面切多個區塊（一邊寫程式、一邊看日誌、一邊監控資源）。

## 安裝

```bash
brew install tmux                 # macOS
sudo apt install tmux             # Ubuntu / Debian / Mint
sudo dnf install tmux             # CentOS / RHEL / Fedora
tmux -V                           # 驗證（大寫 V）
```

Windows 建議在 **WSL** 環境下使用。

## 常用 Session 指令（在進入 tmux 前輸入）

```bash
tmux                  # 開新 Session
tmux new -s <名稱>     # 建立具名 Session
tmux ls               # 列出所有 Session
tmux attach           # 返回最近使用的 Session
tmux attach -t <名稱>  # 返回指定 Session
```

## 相關連結

- [[herdr-agent-terminal-multiplexer]] — 懂 agent 狀態的終端多工器：含 tmux 全部多工功能，另加 agent 狀態追蹤與 Socket API；⚠️ **在 herdr pane 裡再開 tmux 會導致狀態偵測失效**
- [[loop-engineering-playbook-cli-tools]] — 迴圈該住在哪個 runtime
