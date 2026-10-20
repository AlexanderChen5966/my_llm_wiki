---
title: Claude Code 四技巧：ai-title 撈進標籤、狀態列連結化、鎖定 rate limit 視窗
date: 2026-09-08
tags: [ai/claude, type/how-to]
source: https://codelove.tw/@tony/post/ag2jV3（原文 https://qiita.com/kuma_3838/items/00cb0b8d61ca76769c88）
type: article
status: processed
asset_value: 36
---

## T（核心論點）

> **Claude Code 的 session title 有兩套機制：`ai-title` 與 `custom-title`。**
> 標題其實早就自動產生了，**只是沒有傳到會顯示的地方**。

`ai-title` 由 Claude Code 在第一輪往返後自動生成，寫進 `~/.claude/projects/` 下該 session 的
`.jsonl`（`{"type":"ai-title","aiTitle":"…"}`），顯示於 `--resume` 列表、終端機標題、`/status`；
而提示欄右下角的**標籤只讀 `custom-title`**（由 `/rename` 或 hook 明確設定）。
把前者撈出來回傳給後者，就完成了「自動命名 session」。

另三招各自解一個小問題：狀態列把 Issue/PR 做成可點連結（OSC 8）、
用雲端 routine 每天固定時間 ping 一次以**鎖定 5 小時 rate limit 滾動視窗的起點**、
以及改用 Orca 集中多 worktree 的並行代理資訊。

## B（適用場景）

**適用：**
- 同時開很多 session、常靠 `--resume` 檢索的工作方式
- 在意 5 小時 rate limit 視窗、想在固定工時內多夾到一次重設
- 多 worktree 並行跑 agent，資訊分散在多個視窗

**不適用 / 注意：**
- ⚠️ Session 名稱與狀態列都要自己寫腳本，會花一點工夫；rate limit 設定與 Orca 則裝上就能用
- ⚠️ 只有 `UserPromptSubmit` 與 `SessionStart` 兩個 hook 事件接受 `sessionTitle`，在 `Stop` 等其他事件回傳會被**直接忽略**
- ⚠️ 已知限制：`ai-title` 是第 1 輪往返後才生成，而 hook 在送出 prompt 瞬間執行，所以名稱實際上要到送出**第 2 則**訊息才會變更

## R（操作步驟）

### 1. 用 UserPromptSubmit hook 把 ai-title 轉成 custom-title

hook 只要在標準輸出回傳這個 JSON 即可設定 session 名稱：

```json
{"hookSpecificOutput": {"hookEventName": "UserPromptSubmit", "sessionTitle": "搜尋索引的調查"}}
```

腳本本身**不負責生成標題**，只是讀取既有 ai-title 後回傳：

```bash
#!/bin/bash
input=$(cat)
session_id=$(printf '%s' "$input" | jq -r '.session_id // empty')
transcript=$(printf '%s' "$input" | jq -r '.transcript_path // empty')
[ -z "$session_id" ] && exit 0
[ -f "$transcript" ] || exit 0

# 記號檔避免同一 session 反覆改名
marker="$HOME/.claude/.session-titled/$session_id"
[ -f "$marker" ] && exit 0
mkdir -p "$(dirname "$marker")"

title=$(jq -r 'select(.type == "ai-title") | .aiTitle' "$transcript" | tail -1)
[ -z "$title" ] && exit 0

# 分支名含 #123 就加到標題前綴
cwd=$(printf '%s' "$input" | jq -r '.cwd // "."')
branch=$(git -C "$cwd" rev-parse --abbrev-ref HEAD 2>/dev/null)
number=$(printf '%s' "$branch" | grep -oE '#[0-9]+' | head -1 | tr -d '#')
[ -n "$number" ] && title="#$number $title"

touch "$marker"
jq -n --arg t "$title" \
  '{hookSpecificOutput: {hookEventName: "UserPromptSubmit", sessionTitle: $t}}'
```

註冊到 `~/.claude/settings.json` 的 `hooks.UserPromptSubmit`（`type: command`，`timeout: 10`）。

🔑 **分支名 `feature/#123_add_search` 會讓標題變成 `#123 搜尋索引的調查`**，
之後用 `--resume` 搜尋可直接用編號找。

### 2. 狀態列把 Issue / PR 連結化（OSC 8）

```bash
osc8_link() {
  printf '\033]8;;%s\033\\%s\033]8;;\033\\' "$1" "$2"
}
```

- Issue 編號從分支名擷取；URL 由狀態列 JSON 的 `workspace.repo` 組出（含主機名／擁有者／repo，**不限 GitHub**）
- PR 用同一份 JSON 的 `pr.number` 與 `pr.url`；沒傳這些的環境則走 `gh pr list` 快取路徑
- 🔴 **踩坑**：OSC 8 結尾含反斜線，若顏色字串用 `printf '%b'` 傳入會與跳脫解讀衝突導致連結壞掉。
  改成把顏色用 `$'\033[33m'` 直接當位元組保存、輸出統一用 `%s` 才正常
- ⚠️ 狀態列每次顯示都會執行，同步呼叫 `gh pr list` 會明顯變慢 → 改成讀快取顯示、過期時背景更新

### 3. 用 routine 鎖定 5 小時 rate limit 視窗起點

5 小時 rate limit 是**滾動視窗，起點是你送出第一則訊息的時間**。
所以可用雲端 routine 每天固定時間自動 ping 一則（雲端執行，電腦沒開也能運作）。

作者 8 點上班故設 6 點：

```
06:00 - 11:00  ← 3h(08:00 - 11:00)
11:00 - 16:00  ← 4h(1h 休息)
16:00 - 21:00  ← 2h(16:00 - 18:00)
```

🔑 效果：拘束時間算 10 小時（8 工作 + 1 休息 + 1 加班），**沒有 routine 只會夾到 1 次重設
（起點在開始上班瞬間就固定），固定在 6 點則能夾到 2 次**。

### 4. 用 Orca 取代終端機多工器

Orca 是多 worktree 並行 AI 代理的桌面 app。作者原用 herdr（+ Ghostty），實測 herdr 上會用到的
功能 Orca 全都能做，且**內建**：檔案操作（IDE 式）、瀏覽器、Mac 睡眠控制、模型用量顯示、
GitHub 整合、行動裝置支援。herdr 靠外掛也能做到，但內建明顯方便。

最有感的是「資訊集中在同一處」——每個任務的終端機、瀏覽器、md 檔在同一畫面，不必反覆切窗找資料。

## C（關鍵行動）

- [ ] 若常用 `--resume`，套用第 1 節的 hook 並確認 `~/.claude/settings.json` 沒有和既有 UserPromptSubmit hook 衝突
- [ ] 把分支命名習慣改成含 `#<issue>`，讓 session 標題自帶編號
- [ ] 依自己的上班時間往前推 2 小時設一個 routine ping，鎖定 rate limit 視窗起點
- [ ] 若目前用 herdr，評估是否換 Orca（判準：是否需要瀏覽器與檔案操作內建）

## 原始摘要

譯自 Qiita 的四個 Claude Code 實用設定。最有價值的是揭露 session title 有 ai-title 與
custom-title 兩套機制：ai-title 在第一輪往返後自動生成並寫進 `~/.claude/projects/` 的 `.jsonl`，
但提示欄標籤只讀 custom-title，作者用 UserPromptSubmit hook 把它撈出來回傳 `sessionTitle` 補上
這條路。另三招是狀態列用 OSC 8 escape 把 Issue／PR 做成可點連結、用雲端 routine 每天固定時間
ping 一次以鎖定 5 小時 rate limit 滾動視窗起點、以及改用 Orca 桌面應用集中多 worktree 的並行代理資訊。

## 相關連結
- [[claude-code-hooks-memory]] — hook 機制總覽；🔑 本篇補上「只有 `UserPromptSubmit` 與 `SessionStart` 接受 `sessionTitle`」這個限制
- [[claude-code-billing-usage]] — 用量與計費；本篇補「5 小時滾動視窗起點可以人為鎖定」
- [[claude-code-cost-optimization-7-tips]] — 省用量技巧；本篇的 routine ping 屬視窗排程而非省 token
- [[claude-code-session-handoff]] — session 交接；本篇解決的是命名與 `--resume` 檢索
- [[claude-code-handoff-memory-strategy]] — 跨 session 記憶策略
- [[herdr-agent-terminal-multiplexer]] — 🔑 **直接對照**：Orca 正是 herdr 的桌面 app 對手，差異在內建功能
- [[git-worktree-parallel-agents]] — 多 worktree 並行代理的基礎作法
