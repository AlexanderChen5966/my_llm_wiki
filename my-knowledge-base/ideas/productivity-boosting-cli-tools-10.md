---
title: 開發效率提升的 10 款 CLI 工具與指令
date: 2026-07-27
tags: [dev/ci-cd]
source: https://codelove.tw/@tony/post/aYWROx（原文 https://qiita.com/NekoByte/items/efa81aaa8a61d3478568）
type: idea
status: processed
asset_value: 16
---

作者分享大幅提升個人開發效率的 10 款 CLI 工具與實用設定：

- **fzf**：互動式模糊搜尋，可接 `git checkout` 免手打分支名
- **peco**：另一款 fuzzy finder，示範用於 `gh issue list` 快速跳轉 Issue
- **ripgrep (rg)**：高速搜尋，預設排除隱藏檔與 `.git`，取代 `find`/`grep` 組合
- **tmux**：終端機多工器，分割 pane 管理編輯/測試/Git，可存成 session 隔天 `tmux attach` 還原
- **task**：Go 寫成的 Makefile 替代方案，YAML 定義任務（`Taskfile.yml`）
- **httpie**：可讀性高的 HTTP 請求工具，取代手寫 `curl` JSON 易出錯的問題
- **direnv**：依目錄自動載入 `.envrc` 環境變數，取代手動 `source .env`
- **gh CLI**：終端機內完成建立 PR、列出自己 PR、留言 Issue，減少切換瀏覽器
- **delta**：讓 `git diff` 輸出上色高亮，接在 `.gitconfig` 的 pager
- **http-server**：npm 版簡易靜態伺服器，取代 `python -m http.server`

## 相關連結

- （目前 wiki 中無直接相關的 CLI 工具清單條目，備查即可）
