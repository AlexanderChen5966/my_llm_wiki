---
title: "新版 Claude Code 終端機攔截滑鼠點擊超煩！用 CLAUDE_CODE_DISABLE_MOUSE_CLICKS 環境變數把它關掉"
source: "https://israynotarray.com/ai/20260629/2995176290/"
author:
  - "[[Ray]]"
published: 2026-06-29
created: 2026-07-02
description: "用 Claude Code 開發時，想用滑鼠選取終端機的文字或點開錯誤訊息裡的連結，結果點擊都被 Claude Code 攔走？這篇紀錄 CLAUDE_CODE_DISABLE_MOUSE 與 CLAUDE_CODE_DISABLE_MOUSE_CLICKS 兩個環境變數的差別、加到 .zshrc 的做法，並附上不想改設定時的熱鍵繞過方式（iTerm2 / macOS Terminal / VS Code 各自怎麼按）。"
tags:
  - "clippings"
status: processed
---
![新版 Claude Code 終端機攔截滑鼠點擊超煩！用 CLAUDE_CODE_DISABLE_MOUSE_CLICKS 環境變數把它關掉](https://images.israynotarray.com/blog/ai/claude-code-mouse-clicks/ChatGPT%20Image%202026%E5%B9%B46%E6%9C%8830%E6%97%A5%20%E4%B8%8A%E5%8D%8809_46_00.png)

新版 Claude Code 終端機攔截滑鼠點擊超煩！用 CLAUDE\_CODE\_DISABLE\_MOUSE\_CLICKS 環境變數把它關掉

> **Note**  
> Claude Code 在終端機內預設會擷取滑鼠事件來提供點擊展開、hover、滾動加速等互動，但這也讓你沒辦法用滑鼠選取文字或點開連結。要關掉 **沒有 settings.json 欄位、沒有 CLI flag** ，只能用環境變數。最常用的是 `CLAUDE_CODE_DISABLE_MOUSE_CLICKS=1` （v2.1.195+），只關點擊/拖曳/hover、保留滾輪。

## 前言

最近 Claude Code 更新之後，開始支援滑鼠可以點終端機裡面的東西，例如…Yes、No 等，但身為開發者的我，通常會開複數專案，每次只要切換專案回來，就會不小心點到 Yes，明明我不想要執行！！！！

這體感非常的糟糕，所以就乾脆來記錄一下這一篇怎麼解決。

## 為什麼滑鼠突然會被攔走？

簡單來講呢…就是 Claude Code 在 v2.1.195 之後新增了終端機內的滑鼠互動，你可以拖曳、點擊跟懸停等行為，之前的版本是沒有的。

這功能我認為是不錯，但是問題是身為開發者的我，時常會開 N 個專案切換來切換去，又長期在 VSCode 的內建終端機裡面工作，結果每次切換專案回來，滑鼠點擊就會不小心點到 `YES` ，然後就執行了我不想要的操作，這體感非常糟糕。

而且為了避免 Claude Code 詢問我是否要執行：

```bash
rm -rf ...
```

結果我卻點到 `YES` ，然後就刪掉了我不想刪的東西的話，那這件事情就真的滿嚴重的了…

然後這個功能主要是透過環境變數 `CLAUDE_CODE_DISABLE_MOUSE_CLICKS` 來控制的，這個是「只關點擊/拖曳/hover、保留滾輪」的版本，我自己平常用這個就夠了。

```bash
CLAUDE_CODE_DISABLE_MOUSE_CLICKS=1 # 關閉滑鼠點擊/拖曳/懸停，保留滾輪
```
```bash
CLAUDE_CODE_DISABLE_MOUSE_CLICKS=0 # 開啟滑鼠點擊/拖曳/懸停
```

如果你連滾輪都不想被攔，那就改用另一個：

```bash
CLAUDE_CODE_DISABLE_MOUSE=1 # 完全關掉滑鼠攔截（連滾輪都關）
```
```bash
CLAUDE_CODE_DISABLE_MOUSE=0 # 開啟滑鼠攔截功能
```

然後目前並沒有支援 `/config` 選單或 settings.json 欄位可以關掉這個功能，所以我們只能靠環境變數來控制。

## 加到.zshrc

由於這邊我使用的是 zsh，所以我這邊只會針對 zsh 來說明，如果你使用 bash 或 fish 的話也可以參考，因為大致上大同小異。

首先直接打開 `~/.zshrc` ，在最後加一行：

```bash
# Disable Claude Code mouse click/drag capture (keep scrolling)
export CLAUDE_CODE_DISABLE_MOUSE_CLICKS=1
```

存檔後讓設定生效，二選一：

```bash
# 開新的終端機視窗
# 或在現有視窗執行：
source ~/.zshrc
```

> **Note**  
> **已經開著的 `claude` session 不會自動套用** ，要關掉重開 Claude Code 才會吃到新的環境變數。

bash 用戶就改 `~/.bashrc` 、fish 用戶在 `~/.config/fish/config.fish` 加 `set -gx CLAUDE_CODE_DISABLE_MOUSE_CLICKS 1` ，原理一樣。

## 不想動設定？三個熱鍵繞過

如果你不想改 `.zshrc` ，只是偶爾要選一下文字，每個 terminal 都有 bypass key 可以 **單次** 繞過滑鼠擷取：

| Terminal | 按鍵 |
| --- | --- |
| **iTerm2** | 按住 `Option` 再拖選 |
| **macOS Terminal** | 按住 `Fn` 再拖選 |
| **VS Code / Cursor 內建終端** | 按住 `Shift` 再拖選 |
| **其他終端** | 多半也是 `Shift` 再拖選 |

VS Code 還可以在 settings 加 `terminal.integrated.macOptionClickForcesSelection: true` ，讓 `Option + click` 直接強制改為選取游標位置，這是 VS Code 自己的設定，跟 Claude Code 無關。

## 總結

總之這次 Claude Code 把滑鼠互動加進來本來是好事，但對長期切專案、又喜歡用滑鼠選文字的人來說真的是滿煩的。

最簡單的解就是把 `CLAUDE_CODE_DISABLE_MOUSE_CLICKS=1` 塞進 `~/.zshrc` ，滾輪保留、點擊跟拖曳全部不被攔，平常就不會再不小心點到 YES 把奇怪的指令送出去了。

你的支持會直接轉換成更多技術筆記

如果我的筆記讓你少踩一個坑、節省 Debug 的時間，  
也許你可以請我喝杯咖啡，讓我繼續當個不是 Array 的 Ray ☕

 [![buymeacoffee](https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png)](https://www.buymeacoffee.com/israynotarray)| [![line](https://israynotarray.com/images/line-images.png) ](https://store.line.me/search/zh-Hant?q=israynotarray)| [![portaly](https://israynotarray.com/images/logo.b91a6dbf.webp)](https://portaly.cc/israynotarray/support)

Terminal

ray — -zsh — 80x24

Welcome to Ray Terminal v1.0.0

Type "help" to see available commands.

\------------------------------------

➜ ~