---
title: 讓 Claude 用 gh 顧 CI：CLI 是 AI 的原生介面 + 三層放手防護
date: 2026-07-24
tags: [ai/claude, dev/ci-cd, ai/workflow]
source: https://medium.com/@n913239/讓-claude-用-gh-幫你顧-ci-從看紅綠-讀失敗-log-到開-pr-ee5c19a9101a
type: article
status: processed
asset_value: 70
---

## T（核心論點）

CI 掛好了，但「去看它紅還是綠、紅了點進去讀 log」還是你在做——這一步也能交給 Claude，**關鍵不是叫它開瀏覽器截圖，而是讓它驅動 `gh`**。核心道理：**CLI 是 AI 的原生介面**——你的工具愈是純文字、可組合、把狀態講清楚，AI 就愈能替你接手；只給它一張截圖，它就只能跟你一起瞇著眼猜。

## B（適用場景）

**適用：** 已有 GitHub Actions CI 想把「監看 + 診斷」交出去；想建「改一行 → hook 把關 → CI 守 hook → Claude 替你看 CI」的完整鏈

**不適用 / 注意：**
- 前提是 `gh auth login` 已綁好帳號
- 🔴 **自動化不等於全自動**——「合併／刪除／對外」一定要留給自己（見 Part 6 三層防護）

## R（五個應用 + 一個真實的坑）

### 一、為什麼是 `gh` 而不是開瀏覽器

| 特性 | 為什麼對 AI 重要 |
|---|---|
| **純文字** | 一行就講完「紅還綠、幾秒、誰推的」，不必「看圖」猜狀態 |
| **結構化** | 支援 `--json`，能吐欄位讓 Claude **程式化判斷**（`conclusion == "failure"`），而非靠肉眼辨識綠勾 |
| **可組合** | 能接 `grep`、接 `jq`、塞進迴圈輪詢。瀏覽器截圖慢、脆、還要對座標，**GitHub 一改版就崩** |

### 二、監控一次 CI run

```bash
gh run list -L 1
# → completed  success  Add CI + hook tests  CI  main  push  28765628543  8s

# 結構化取值，Claude 拿去寫 if 判斷不會看錯
gh run list -L 1 --json status,conclusion -q '.[0].conclusion'   # → success

gh run view <id>       # 列出每一關（✓ test in 5s）
gh run watch           # 阻塞等到跑完

# 或讓 Claude 自己輪詢（「可組合」的實例）
until gh run list -L 1 --json status -q '.[0].status' | grep -q completed; do sleep 5; done
```

### 三、🔑 紅了讓它自己讀 log

傳統做法：開 GitHub → Actions → 點進那次 run → 展開失敗 job → 往下捲找紅字（**六層點擊**）。

```bash
gh run view <id> --log-failed    # 只抓「失敗那一關」的 log，不是整包
```

> **`--log-failed` 對 AI 特別重要**：整包 log 動輒上千行，全灌進上下文**又貴又吵**；只給失敗那段等於先幫它濾掉雜訊——**省 token，也更準**。

例：`shellcheck` 那關紅了 → 吐出 `scripts/precommit-guard.sh:23:24: warning: Quote this to prevent word splitting. [SC2086]` → Claude 讀到 `SC2086` + 行號就知道第 23 行變數沒加引號。**從「CI 紅了」到「知道哪行、怎麼修」，一條指令的距離。**

### 四、🔴 一個真實的坑：hook 測試的假 token 被 GitHub 擋下

作者用 gh 建公開 repo 推設定，`git push` 被打槍：
```
remote: - Push cannot contain secrets — Stripe API Key detected
 ! [remote rejected] main -> main (push declined due to repository rule violations)
```

原因：hook 測試裡有個**假的** `sk_live_` 開頭 token（用來測「該擋 hardcode 密鑰」那個 case），**剛好長得像真的 Stripe 金鑰**，被 GitHub secret scanning 擋下整個 push。

Claude 讀了 `remote:` 訊息，判斷「**這不是程式壞了，是 secret scanning 誤把測試用的假 key 當真的**」，只換掉那一個字串：
```diff
- let apiKey = "sk_live_0123456789…"            // 像真的 → 被 GitHub 擋
+ let apiKey = "placeholder-not-a-real-secret"  // 明顯的假值 → GitHub 放行
```

> 🔑 **關鍵洞察**：GitHub secret scanning 在意的是「**像不像**密鑰的樣式」，hook guard 在意的是「**有沒有** `apiKey = "…"` 這種寫法」。換個明顯的假值，**前者放行、後者照樣攔（測試仍是 exit 2）——兩邊都對。**
>
> 這就是把 CI／git 互動交給「會讀文字」的 AI 的價值：**錯誤訊息它讀得懂，能自己分辨「真的壞了」還是「被規則擋了」，然後對症下藥**——而不是把紅字原封不動丟回來要你自己 google。

### 五、一路收尾到 PR

```bash
gh pr create --title "..." --body "<Claude 產的 PR 描述>"
gh pr checks    # 列出這個 PR 底下每個檢查的紅綠
```
一條龍：寫 code → 跑測試 → commit（hook 把關）→ 產 PR 描述 → 開 PR → 看 checks。**merge 那一步留給你自己按。**

### 六、🔴 三層放手防護（本篇最有價值）

| 可以放手（唯讀，錯了頂多看錯）| 要留一手（動遠端、不可逆）|
|---|---|
| `gh run list` / `run view` / `run watch` / `pr view` / `pr diff` | `gh pr merge`、`gh repo delete`、`git push --force` |

這條線**不必只靠自律去守**，有三件事疊起來：

**① 憑證從頭到尾沒離開你的機器**
Claude 跑 `gh run list` 時啟動的是你電腦上的子行程；gh 自己去讀本地憑證（系統 keychain 或它的設定檔）完成認證——**憑證沒進到模型的上下文、也沒被送上雲端**，模型看得到的只有 `gh` 這行指令和它吐出的文字。
> 這跟把 PAT 直接貼進 prompt、或塞進某個 agent 設定**截然不同**：那才是把鑰匙交到會被傳輸、被記錄的地方。
> ⚠️ **唯一前提：別叫它跑 `gh auth token`、`echo $GH_TOKEN`**——那等於自己把鑰匙念出來。

**② 最小權限**
給 Claude 用的不必是你能開能刪的萬能鑰匙——用 **fine-grained token** 只開真正需要的（讀 Actions、讀寫指定 repo 的 PR），`delete_repo`、`admin` 一律不給。
> 作者那次 `gh repo delete` 擋得下來，正是因為 **token 本來就沒那個 scope——這不是運氣，是設計**。

**③ Claude Code 自己的 `permissions`**
token 有、但你不想讓它自己來的（push、merge），把 `Bash(gh pr merge:*)`、`Bash(git push:*)` 放進 `settings.json` 的 **`ask` 清單**，每次要動遠端都得先問過你。

> **三件事合起來：憑證不出你的機器、token 只給最小權限、危險操作一律要你點頭。**
> **自動化不等於全自動**——把「讀／看／診斷」交出去，把「合併／刪除／對外」留給自己。

## C（關鍵行動）

- [ ] 用 `gh` 取代「開瀏覽器看 CI」，並優先用 `--json` + `-q` 取結構化欄位
- [ ] 紅燈一律先跑 `gh run view <id> --log-failed`（**只抓失敗那關**，省 token 又更準）
- [ ] 建 fine-grained token：只開讀 Actions + 指定 repo 的 PR 讀寫，**不給 `delete_repo` / `admin`**
- [ ] 把 `Bash(gh pr merge:*)`、`Bash(git push:*)` 放進 `settings.json` 的 `ask` 清單
- [ ] **絕不叫 AI 跑 `gh auth token` / `echo $GH_TOKEN`**
- [ ] 寫 hook 測試用的假密鑰時，**用明顯的 placeholder**（如 `placeholder-not-a-real-secret`）避免觸發 GitHub secret scanning

## 原始摘要

MikeWang Claude Code 系列連載第 14 篇（Medium，2026-07-24），承接前篇〈幫工作流本身寫測試〉。整條守護鏈：**你改一行 → hook 把關 → CI 守 hook → Claude 用 gh 替你看 CI、紅了讀 log、順手把 PR 開好**。作者強調這不是紙上談兵——前篇為驗證那套 CI，整個「建 repo → push → 盯 CI 跑綠」流程都是讓 Claude 用 gh 一路做完的（並真的踩到 Part 4 那個坑）。

## 相關連結

- [[testing-your-ai-workflow-config-ci]] — **直接前篇**：那篇建立守護（hook 測試 + CI 三關），本篇把「跟守護互動」也自動化
- [[api-cli-skills-agent-architecture]] — **「CLI 是 AI 的原生介面」的完整論證**（MCP vs CLI 對照、Steinberger「every MCP would be better as a CLI」）；本篇是該論點的實戰案例
- [[claude-code-hooks-memory]] — 同系列；本篇的 `ask` 清單與 hook 是同一套 permissions 機制
- [[claude-code-git-workflow]] — 同系列；本篇的 `gh pr create` 是該篇 PR 描述的收尾
- [[claude-code-auto-mode-permissions]] — Claude Code 權限機制；本篇的三層防護是其實務應用
- [[claude-code-cost-optimization-7-tips]] — `--log-failed` 只抓失敗段落，與該篇「PreToolUse hook 過濾冗長輸出」是同一省 token 思路
