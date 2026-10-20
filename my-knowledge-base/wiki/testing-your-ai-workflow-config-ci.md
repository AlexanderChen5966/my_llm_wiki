---
title: 幫「工作流」本身寫測試：用 CI 守住 Claude Code 設定
date: 2026-07-17
tags: [ai/claude, dev/ci-cd, dev/testing]
source: https://medium.com/@n913239/幫-工作流-本身寫測試-用-ci-守住你的-claude-code-設定-e44ac4c81630
type: article
status: processed
asset_value: 72
---

## T（核心論點）

你為 AI 搭的結構（hook 把關、agent 分工、command 收斂流程）**本身也是程式碼，也會壞**——而且是**不會報錯、只會靜默失效**的那種 regression：hook 照跑、綠色小勾照亮，只是它其實什麼都沒擋。解法是把「先寫測試」的紀律**轉過來對準工具自己**：為決定性的部分寫行為測試、掛一條 9 秒跑完的 CI，讓守門員本身也一直被守著。

## B（適用場景）

**適用：** 已經有 hook／settings.json／自訂 agent 的人；把工作流設定抽成可重用種子庫要給多專案用

**🔴 不適用（本篇最重要的界線）：** **需要 LLM 在場才有意義的元件不能寫自動測試**——見下方「手動 vs 自動的分界」

## R（四個壞法與三關 CI）

### 一、四種「靜默失效」的壞法

| 壞法 | 為什麼可怕 |
|---|---|
| **exit code 手滑** | hook 靠 `exit 2` 擋下、`exit 0` 放行。把攔截區塊的 `exit 2` 改成 `exit 0` → 從「擋下」變「放行」，**功能還在、指令還跑、什麼錯都不報**，只是再也擋不住任何東西 |
| **`settings.json` 多一個逗號** | JSON 解析失敗，hook **完全不會被掛上**——不是「掛上但沒作用」，是**根本沒載入**，而你以為它一直在 |
| **grep pattern 打錯** | 少一個跳脫、多一個空白，某一類該擋的東西就悄悄漏過去 |
| **matcher 寫錯** | 把 `PreToolUse` 的 matcher 寫成 `"Bash(git commit*)"`——**它其實只比對工具名稱（`Bash`）**，指令層級的判斷要在腳本裡做；寫錯會讓 hook 要嘛不觸發、要嘛觸發時機全錯 |

作者實測第一種：把 `exit 2` 改成 `exit 0`、其他一字不動 → 三個「該擋」的 case 立刻紅（預期 exit 2 實際拿到 0），CI 直接變紅。
> **那行手滑如果沒有測試，你永遠不會發現。** 有了這組測試，那行改動連 push 都 push 不過去。

### 二、hook 為什麼好測——它是純函式

> 「讀 stdin 的 JSON → 掃 staged diff → 用 exit code 表態」——**不需要真的跑一次 `git commit`，也不需要 Claude 在場。**

測法：**在暫存 git repo 裡 stage 一個檔 → 餵它一段模擬的 hook JSON → 驗它吐出的 exit code**。

`run_case` harness 的核心（簽名：`run_case <名稱> <預期exit> <staged內容> <指令JSON>`）：
```bash
tmp="$(mktemp -d)"; git -C "$tmp" init -q
printf '%s\n' "$content" > "$tmp/file.swift"; git -C "$tmp" add file.swift
CLAUDE_PROJECT_DIR="$tmp" bash -c 'echo "$1" | "$2"' _ "$json" "$GUARD" >/dev/null 2>&1
code=$?; [ "$code" -eq "$expected" ] && echo "  ✓ $name" || echo "  ✗ $name — 預期 $expected，實際 $code"
```

**六個情境 = 三該擋 + 三該放行**（涵蓋 hook 的兩種責任）：

| 預期 | 情境 |
|---|---|
| `2` | 有 `print` ／ 有 hardcode token ／ 有 `TODO` 的 commit 應被擋 |
| `0` | 乾淨的 commit ／ 沒有 staged 檔 ／ **非 commit 指令一律放行** |

> 🔑 **最後那個 case 特別重要**：同樣一段有 `print` 的髒 code，只要指令不是 `git commit`（例如 `ls -la`）就必須放行——**不然 hook 會攔下每一個 Bash 指令，Claude 什麼都做不了。**

🔑 **一個藏著功力的細節**：
- **測試 harness 用 `set -uo pipefail`**（刻意**不加 `-e`**）——某個 case 失敗時不要立刻中斷整輪，要看到「6 個裡面掛了哪幾個」
- **hook 本體用完整的 `set -euo pipefail`**——它一旦出錯就該乾脆地失敗

### 三、GitHub Actions 三關（一關對應一種壞法）

```yaml
- run: jq empty .claude/settings.json              # ① 守「多一個逗號就整個沒載入」
- run: shellcheck scripts/precommit-guard.sh ...   # ② 抓 shell 老問題
- run: bash tests/test-precommit-guard.sh          # ③ 六個行為測試
```

- **`shellcheck` 最典型抓什麼**：沒加引號的變數——`$added` 遇到含空白或 `*` 的內容會被 **word-split 或展開成檔名**，行為就跑掉。這種錯在本機常「剛好沒踩到」，**上了別人的機器才爆**
- **用 Ubuntu runner，完全不碰 Xcode**——守的是「工具本身」不是 iOS App，與沉重的 App build **解耦**，所以能在最便宜最快的環境跑
- 實測 **9 秒**、三關全過、公開 repo 免費

> **守工具的成本，本來就該遠低於守產品。便宜到這種程度，你就沒有藉口不掛。**

### 四、🔴 手動測試 vs 自動測試的分界（本篇核心）

| 類型 | 元件 | 為什麼 |
|---|---|---|
| **決定性 → 交給 CI 持續守** | `precommit-guard.sh`、`settings.json` | 同樣的輸入永遠得到同樣的輸出 → **能寫成斷言** |
| **需要 LLM 在場 → 交給人當下驗** | `test-writer`、`code-reviewer`、`/precommit`、`/pr-description` | 產出的**措辭、切入點每次都會變**——你沒辦法對「code-reviewer 有沒有抓到 retain cycle」寫一個 `assertEqual` |

**人工測法舉例**：故意寫一個 closure 捕獲 `self` 沒加 `[weak self]`，叫 `code-reviewer` 審，看它抓不抓得到、標不標成 Critical。

> **決定性的部分交給 CI 持續守，需要判斷的部分交給人當下驗。**
> 把兩者搞混——想幫 agent 寫單元測試、或反過來以為 hook「手動測過一次就永遠沒事」——**只會兩頭落空**。

## C（關鍵行動）

- [ ] 為現有 hook 寫行為測試：**暫存 repo + 模擬 JSON + 驗 exit code**，不需要真跑 git commit
- [ ] 測試務必包含「**不該管的一律別擋**」那個 case（否則 hook 可能攔下所有 Bash 指令）
- [ ] CI 加三關：`jq empty` 驗 JSON、`shellcheck` 靜態檢查、跑行為測試
- [ ] **用 Ubuntu runner**——守工具不需要 macOS/Xcode，成本應遠低於守產品
- [ ] 測試 harness 用 `set -uo pipefail`（不加 `-e`）以看到全部失敗項；hook 本體用完整 `set -euo pipefail`
- [ ] 盤點自己的元件哪些是決定性、哪些需 LLM 在場——**別想為 agent 寫單元測試**

## 原始摘要

MikeWang Claude Code 系列連載第 13 篇（Medium，2026-07-17）。承接前篇〈從 Demo 到 Production〉把一天的工作流（Plan Mode 先規劃、`test-writer` 先寫測試、唯讀 `code-reviewer` 審查、`/precommit` 把關、pre-commit hook 自動掃 staged diff）抽成可重用種子庫後，作者意識到「**這個 hook 是一段 shell script，它自己也是程式碼——程式碼就會壞**」。文末預告下一篇：Claude Code 能直接驅動 `gh`（`gh run list` 看紅綠、`gh run view` 抓失敗 log），連「去看 CI 結果」都能交出去。

## 相關連結

- [[claude-code-hooks-memory]] — **同系列且直接相關**：本篇測的正是那篇的 hook 機制；`PreToolUse` exit 2 才阻擋、matcher 只比對工具名稱這兩個細節，本篇證明**寫錯會靜默失效**
- [[obligations-as-code-tedium-is-stability]] — 「verifier 必須可判定」；本篇的「決定性 vs 需 LLM 在場」正是**判定可判定性的界線**
- [[claude-code-git-workflow]] — 同系列；本篇的 pre-commit hook 是該篇 commit 流程的自動化把關
- [[boris-cherny-prompt-to-verification]] — 「投資驗證」與本篇「守門員也要被守」同源；但注意評測集會折舊，**決定性測試比 LLM eval 長壽**
- [[ai-code-review-safeguards]] — AI code review 護欄
- [[claude-md-optimization-techniques]] — 「屢次違反的規則轉成 Hook」；本篇補上「轉成 Hook 之後要怎麼確保它沒壞」
