---
title: "Claude Code 2026 新命令大盤點 — 除了 /claude-api 你還該認識這 12 個"
source: "https://hackmd.io/@BASHCAT/SJ7kShMJGx"
author:
published:
created: 2026-05-14
description: "五週三十個版本、十個新 slash commands、兩個被砍掉的舊命令。一篇文搞懂 Claude Code 2026 上半年命令系統的完整變化、Skill 統一架構、Plugin marketplace 爆發，以及哪些命令真正改變工作流。"
tags:
  - "clippings"
status: processed
---
五週、三十個版本、十個全新 slash commands。我在準備寫這篇文章前用 `/release-notes` 拉了一下 changelog，從 v2.1.83 到 v2.1.139 — 也就是 2026 年 3 月底到 5 月中 — Anthropic 在 Claude Code 上的更新節奏密集到，光是手動讀 release notes 就要花一個下午。靠杯，這還只是看，沒實際試。

你最後一次打 `/help` 把整份命令清單看完是什麼版本？老實說我自己也很久沒看。直到上週試用 `/goal` 把一個重構任務丟著跑了兩小時，回來看終端機螢幕的時候才意識到 — Claude Code 已經不再是「Claude 的 CLI」，它變成一個 **會自己呼叫雲端、會自己跑 PR、會自己 review 的工作流引擎** 。

這篇文章拆解 Claude Code 2026 上半年新增的全部命令，重點放在「哪幾個真的會改變你的工作流」。我會跳過 `/claude-api` （這個太熱門了大家都寫過），把鎂光燈打在那些被低估、但真的好用的新東西。

## 五週、三十個版本：先看一下這份瘋狂的更新清單

先給點數字撐場面。根據 [Claude Code 官方 changelog](https://code.claude.com/docs/en/changelog) ，配合 [Angelo Lima 4 月整理的 Cheatsheet](https://angelo-lima.fr/en/claude-code-cheatsheet-april-2026-update/) 補完到 5 月版本資訊，2026 年 3 月底到 5 月中這段期間 Anthropic 大約推了 **30 個版本** ，平均每 1.2 天一個 release。新增的 slash commands 至少有：

| 版本 | 月份 | 新命令 |
| --- | --- | --- |
| 2.1.101 | 4 月初 | `/team-onboarding` |
| 2.1.108 | 4 月中 | `/recap` 、 `/undo` （alias） |
| 2.1.110 | 4 月中 | `/tui` 、 `/focus` |
| 2.1.111 | 4 月底 | `/ultrareview` 、 `/fewer-permission-prompts` 、 `/effort` 滑塊 |
| 2.1.118 | 4 月底 | `/usage` （合併 `/stats` + `/cost` ） |
| 2.1.139 | 5 月中 | `/goal` 、 `/scroll-speed` 、Agent 視圖 |
| 4 月（版本不詳） | \- | `/autofix-pr` 、 `/ultraplan` 、 `/powerup` |

順便兩個被砍掉的： `/vim` 跟 `/pr-comments` 。後面會講。

這個節奏不是純堆功能。仔細看新命令的取向就會發現一條主軸 — **雲端、並行、長任務** 。雲端代表 `/ultraplan` `/ultrareview` `/autofix-pr` ，並行代表 `/batch` 跟 Agent 視圖，長任務代表 `/goal` 。Claude Code 從一個會話式 CLI，正在快速往「 **會自己跑工作流的 agent 平台** 」進化。

## 雲端工作流三劍客：本地不夠你用、就丟雲端

如果你只記得 2026 上半年新增的三個命令，就記這三個 — `/ultraplan` 、 `/ultrareview` 、 `/autofix-pr` 。它們共同的特徵： **把工作從你的終端機甩到雲端，跑完再把結果送回來** 。

### /ultraplan：在瀏覽器審視草稿，再決定要不要執行

你大概有過這種經驗：丟給 Claude 一個複雜需求，它在你的終端機裡規劃了 200 行 markdown，你滑了三屏發現方向錯了，整個 context 已經被吃掉一半。 `/ultraplan` 把這個流程拆開 — 草稿在雲端跑，你在瀏覽器看完整版（甚至可以邊喝水邊滑手機），確認沒問題再決定送回終端執行還是讓它在雲端跑完。

寫硬體韌體的人會特別有感。我那種「先幫我規劃一個 BLE service migration」的任務，以前在本地會吃掉 30% context，現在直接 `/ultraplan` 丟雲端，本地只接「同意」或「不同意」兩個訊號。爽。

### /ultrareview：多 agent 並行 review，找到 bug 還會自己驗證

這個是我覺得 2026 上半年最被低估的功能。

根據 [Claude Code Ultrareview 官方文件](https://code.claude.com/docs/en/ultrareview) 和 [abZ Global 的試用心得](https://abz.global/web-development-blog/anthropic-just-launched-ultrareview-a-multi-agent-code-reviewer-for-claude-code) ， `/ultrareview` 的工作流是：

1. 把當前 branch 或 PR 丟到雲端 sandbox
2. 派出 **一隊 specialist agents** 並行 review，分別檢查 correctness、security、architecture、tests、performance、style 六個維度
3. 每個發現的 bug **再跑一次驗證** ，確認是真 bug 才寫進報告
4. 合併成一份單一報告交回給你

關鍵在第 3 步。多 agent review 最大的問題從來不是「找不到 bug」，而是「找太多假 bug」。 `/ultrareview` 加了 verification 步驟，等於先讓 agent 自己反證一遍，結果可信度直接拉滿。

```bash
# 兩種用法
/ultrareview              # review 當前 branch vs default branch
/ultrareview 1234         # review 指定 PR 編號
```

價格部分要小心。 [MindStudio 整理的 ultrareview 計費規則](https://www.mindstudio.ai/blog/claude-code-ultra-review-5-things-to-know-before-running) 指出 Pro/Max 用戶有 **3 次免費** （一次性，不會每月重置），用完之後每次 review 大約 **$5 ~ $20 美金** ，看 diff 大小。算下來不便宜，但比起雇一個 senior 花兩小時手動 review，這個價錢還算合理。

### /autofix-pr：CI 爆掉、reviewer 留言，它自己處理

這個算是 GitHub Actions 整合的進階版。在 PR 還沒 merge 之前，啟動一個雲端會話「 **盯著這個 PR** 」— CI 跑掉它就修，reviewer 留 comment 它就回應並改 code。

```bash
# 預設修所有 CI failure 和 review comments
/autofix-pr

# 限縮範圍
/autofix-pr only fix lint and type errors
```

需要 `gh` CLI 已登入。對於那種「下班前 PR 開好、希望睡一覺起來 CI 已綠」的場景超實用。但要小心，如果你的 CI 包含 deploy 或 production 變更，autofix 可能會幫倒忙。我自己只在純測試型 CI 上用。

## /goal：設一個終點線，然後走開

`/goal` 是 2026/5/12 隨 v2.1.139 推出的新命令， [ExplainX 把它形容為「2026 最被低估的 AI 功能」](https://explainx.ai/blog/claude-code-goal-command-long-running-agents-2026) 。

概念很簡單： **設一個完成條件，Claude 自動跨多輪持續工作直到達成** 。

```bash
/goal 所有單元測試通過且 coverage > 80%
/goal PR #1234 通過所有 CI 檢查並獲得 LGTM
/goal 修完 src/components/ 底下所有 ESLint 警告
```

每一輪結束後，內建的 evaluator agent 會檢查條件是否達成，回傳一段「為什麼還沒達成」或「達成了」的說明，顯示在 status view 跟 transcript 上。你可以開著一個 dashboard 看它跑了幾輪、花了多少 token、距離終點還多遠。

但 — 我要先打個預防針。 [Joe Njenga 在 Medium 上的試用心得](https://medium.com/@joe.njenga/i-tested-new-claude-code-goal-command-it-turned-into-a-self-driving-coding-agent-8ed1ac87e3d1) 一開頭就翻車：他第一次跑 `/goal` 不到一小時就停下來問他「要不要批准這個動作」。問題不在 `/goal` 本身，而在 **trust mode 沒開** 。Claude 預設遇到敏感 tool call 就會停下來確認，這個確認彈窗一出現，goal 的 loop 就斷了。

> 結論：要用 `/goal` 跑無人值守工作， **必須** 搭配 auto mode（或至少把該允許的 permission 都先設好）。否則你那個「過夜實習生」會在第 5 分鐘就坐在原地發呆。

實戰範例 — 我自己拿這個跑了一個重構：把 KCA10 韌體的 BLE service 從舊版 API 遷移到新版。設定的 goal 是「 `west build` 全綠且 unit test 全過」。設好以後切到 auto mode，去吃飯，回來工作已經做完 70%。最後那 30% 它卡在一個自己生不出來的硬體 mock，主動把 transcript 留好讓我接手。這就是 `/goal` 該長的樣子。

## Bundled Skills：除了 /claude-api，還有 5 個你可能不知道

Claude Code 內建了一組 **Bundled Skills** — 它們長得像 slash commands，但底層是 Skill 機制（也就是 prompt-based，Claude 可以自動觸發）。 `/claude-api` 是其中最有名的，但其實還有 5 個同等級的：

| 命令 | 我會這樣形容 | 什麼時候用 |
| --- | --- | --- |
| `/batch <instruction>` | 平行宇宙裡的你，一起改 30 個檔案 | 大規模 migration（如 Solid → React） |
| `/debug [description]` | 開 debug log，順便讓 Claude 自己看 log 找問題 | 詭異 bug 抓不到 |
| `/loop [interval] [prompt]` | Cron job 的 AI 版 | 監測 CI、定期文件更新 |
| `/simplify [focus]` | 三個 reviewer 同時上身，挑 code smell | 寫完一輪想做 cleanup |
| `/fewer-permission-prompts` | 自動掃 transcript 找出常用 read-only command，加進 allowlist | 已經被權限彈窗煩到爆肝 |

`/batch` 特別值得講一下。 [官方 commands 總覽頁](https://code.claude.com/docs/en/commands) 描述它會把你的指令自動拆解成 **多個獨立工作單元** （實測通常落在 5 到 30 之間，依任務規模而定），每個單元在自己的 git worktree 裡用一個 background subagent 跑，做完開 PR。我曾用 `/batch migrate src/ from JavaScript to TypeScript` 一次性處理過 12 個檔案，每個檔案開一個 PR，整個過程我只負責 review 和 merge。神扯。

`/simplify` 也很狠 — 它會並行派出 **3 個 review agent** ，aggregate 他們的發現之後自動修 code。我寫完一段比較髒的邏輯，跑 `/simplify focus on memory efficiency` ，回來通常會看到 3 ~ 5 個合理的精簡建議直接被應用。

## 命令系統大統一：.claude/commands/ 退場、Skills 上位

2026 年 Claude Code 做了一個重要的架構決策 — **把 commands 和 skills 合併** 。

過去這兩個系統並行存在：

- `.claude/commands/<name>.md` → 觸發 `/<name>` 命令
- `.claude/skills/<name>/SKILL.md` → 同樣觸發 `/<name>` ，但多了自動觸發、附加檔案、frontmatter 控制

根據 [YingTu 的分析](https://yingtu.ai/en/blog/claude-code-skills-vs-slash-commands) ，Anthropic 沒有強制遷移，而是把 Skills 升格為 **主要抽象** ，slash command 變成 Skill 的其中一個能力。寫成 Skill 的好處是：

```yaml
---
name: my-deploy-check
description: When to use this skill (Claude 用這個決定自動觸發)
disable-model-invocation: false   # true 則禁止 Claude 自動觸發
user-invocable: true              # false 則隱藏於 / 選單
allowed-tools: Read, Grep, Bash   # 限制可用工具
model: claude-sonnet-4-6           # 強制使用特定模型
context: fork                      # 在 isolated subagent 跑
---

# My Deploy Check
<skill 內容>
```

最強的兩個欄位是 `allowed-tools` （ **精細權限控制** ）和 `context: fork` （ **整個 skill 跑在 isolated subagent** ，不污染主對話 context）。寫一個常用的 deploy check skill 設成 `context: fork` ，你的主對話 context 就不會被 1000 行 lint output 吃掉。

老的 `.claude/commands/*.md` **還能用** ，但 [Heyuan 的命令大全](https://www.heyuan110.com/posts/ai/2026-03-05-claude-code-slash-commands/) 建議所有新東西都寫 Skill。如果你的專案還在用 legacy commands，趁早遷移會省事。

## Plugin Marketplace 爆發：神器與地雷各半

2026 上半年最戲劇性的生態變化 — Plugin marketplace 從幾個官方目錄擴張到 **至少四個主流市集** ：

| 平台 | 規模 / 特色 | 我的評語 |
| --- | --- | --- |
| [Anthropic 官方](https://github.com/anthropics/claude-plugins-official) | 預先設定、嚴選 | 安全第一，但少 |
| [tonsofskills.com（GitHub）](https://github.com/jeremylongshore/claude-code-plugins-plus-skills) | 425 plugins / 2,810 skills / 200 agents | 量大、品質參差 |
| [SkillsMP](https://skillsmp.com/) | 1.2M+ skills、按職業分類 | 按角色/職業分類瀏覽，類似 job board UI |

「Plugin」是「bundle」— 一個 plugin 可以包含 skills、MCP servers、commands、hooks、agents，一鍵安裝。理論很美。實際上 2,810 個 skill 裡面有多少是真的可用、有多少是 SEO 農場產品，需要自己挑。

我自己的選 plugin 原則：

1. **先看官方目錄** ，能解決就解決
2. 找 GitHub 上 star > 200 的
3. 看 [SKILL.md](http://skill.md/) 內容是不是清晰、frontmatter 有沒有亂寫
4. **絕對避免 require remote MCP server 的 plugin** — 等於把你的程式碼權限交給第三方

夭壽，這幾條原則套下來真的 2,810 個剩下不到 30 個。但這 30 個就是好東西。

## 已退場：/vim 跟 /pr-comments 的小哀悼

兩個曾經很實用的命令在 2026 上半年被砍掉：

- **`/vim`** （ [官方 commands 文件](https://code.claude.com/docs/en/commands) 標示 v2.1.92 移除）：改用 `/config` → Editor mode。我猜是因為 vim 切換邏輯太多人吵，乾脆收進 config 裡讓你一次設好
- **`/pr-comments`** （官方文件標示 v2.1.91 移除）：直接跟 Claude 說「幫我看 PR #1234 的 comments」就好，Claude 自己會 call `gh pr view` 。命令層被薄薄一層自然語言取代

兩個快捷鍵行為也改了：

- **Ctrl+O** 從「focus 切換」變「詳細日誌切換」（focus 改用 `/focus` 命令）
- **Ctrl+U** 從「刪除至行首」變「清空整個輸入緩衝區」

如果你的 muscle memory 還停在 v2.0，現在按 Ctrl+U 會發現整段話消失，那個瞬間真的會傻眼。

## 哪些命令值得馬上學？我的精選清單

寫到這裡你大概已經被資訊轟炸到頭暈。我替你篩過：

| 學習優先級 | 命令 | 為什麼 |
| --- | --- | --- |
| **必學** | `/goal` | 長任務 game changer，但要搭配 auto mode |
| **必學** | `/ultrareview` | merge 前最後一道防線，有免費額度先試 |
| **必學** | `/batch` | 一次處理 N 個檔案，並行 + 自動 PR |
| 強推 | `/simplify` | 寫完髒 code 後的清理利器 |
| 強推 | `/team-onboarding` | 新同事入職 + 你自己的工作流複盤 |
| 強推 | `/usage` | 取代 `/stats` `/cost` ，看花了多少錢 |
| 看情況 | `/ultraplan` | 大型重構規劃適用 |
| 看情況 | `/autofix-pr` | 純測試型 CI 才安全 |
| 看情況 | `/fewer-permission-prompts` | 權限彈窗煩到爆肝再用 |

不要看 changelog 看一次就以為自己學會。每個命令至少實際跑一次帶有真實 context 的任務，你才會知道哪個合自己的 workflow。我自己花了大概兩個禮拜把這些命令輪流跑過一輪，最後只把 `/goal` `/batch` `/simplify` 跟 `/team-onboarding` 留在每週固定使用的清單。其他的就是 — **知道存在，需要時想得起來** 就夠。

## 結語：Claude Code 已經不是 CLI 了

攤開命令的演進方向 — 本地保留即時互動，重活丟雲端跑（ `/ultraplan` `/ultrareview` `/autofix-pr` ），長任務交給 agent 自走（ `/goal` ），常用流程封裝成 skill 並分發。整個系統正在從「Claude 的命令列介面」變成「Anthropic 的 agent 平台前端」。

接下來會怎麼走？以下純屬個人推測，目前 **沒有任何官方訊號** — 我猜本地、雲端、web、mobile 的分工會繼續細化， `/goal` 跟 `/batch` 如果能組合，等於高層目標可以自動拆解成並行 unit、每個 unit 自己跑、自己 review、自己 merge。

回到現實。先把這 12 個新命令搞熟，你會發現 `/help` 再也不只是新手才打的東西。

---