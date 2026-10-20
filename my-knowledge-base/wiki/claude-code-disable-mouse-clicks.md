---
title: 關掉 Claude Code 終端機滑鼠攔截：CLAUDE_CODE_DISABLE_MOUSE_CLICKS
date: 2026-07-03
tags: [ai/claude, type/how-to]
source: https://israynotarray.com/ai/20260629/2995176290/
type: article
status: processed
asset_value: 46
---

Claude Code v2.1.195 後在終端機內預設攔截滑鼠事件（點擊展開/hover/滾動加速），導致沒法用滑鼠選文字或點連結，切換專案回來還容易誤點 `YES` 執行不想要的操作。解法：用環境變數 `CLAUDE_CODE_DISABLE_MOUSE_CLICKS=1` 關掉（**無 settings.json 欄位、無 CLI flag**）。

## T（核心論點）

新版滑鼠互動對長期切多專案、慣用滑鼠選文字的人是干擾（誤點 YES 可能送出 `rm -rf` 確認）。唯一關閉途徑是環境變數：`CLAUDE_CODE_DISABLE_MOUSE_CLICKS=1` 只關點擊/拖曳/hover、**保留滾輪**（推薦）；`CLAUDE_CODE_DISABLE_MOUSE=1` 連滾輪都關。

## B（適用場景）

- 同時開多專案、常在 VSCode 內建終端切換而誤點 YES
- 想用滑鼠選終端文字或點錯誤訊息裡的連結

## R（做法）

**加到 shell 設定**（zsh 例）：`~/.zshrc` 末尾加 `export CLAUDE_CODE_DISABLE_MOUSE_CLICKS=1` → `source ~/.zshrc` 或開新視窗。bash 改 `~/.bashrc`、fish 用 `set -gx ...`。
> 已開著的 claude session 不會自動套用，要關掉重開才吃到。

**不想改設定？單次熱鍵繞過**（bypass key，臨時選文字）：
| Terminal | 按鍵 |
|----------|------|
| iTerm2 | 按住 `Option` 拖選 |
| macOS Terminal | 按住 `Fn` 拖選 |
| VS Code / Cursor 內建終端 | 按住 `Shift` 拖選 |
| 其他終端 | 多半也是 `Shift` 拖選 |

VS Code 另可設 `terminal.integrated.macOptionClickForcesSelection: true`（VS Code 自身設定，與 Claude Code 無關）。

## C（對自身的啟發）

- [ ] 直接可用：把 `CLAUDE_CODE_DISABLE_MOUSE_CLICKS=1` 加進自己的 .zshrc，避免誤點 YES 送出危險指令
- [ ] 這類「無 settings 欄位、只能環境變數」的隱藏開關，值得記錄備查

## 相關連結

- [[claude-code-file-management]] — Claude Code 設定檔管理
- [[claude-code-glossary]] — Claude Code 名詞速查
- [[claude-code-10-recommended-commands]] — 常用指令與客製化
