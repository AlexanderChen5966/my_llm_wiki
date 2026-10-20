---
title: "最近導入的 Claude Code 個人技巧 4 選"
source: "https://codelove.tw/@tony/post/ag2jV3"
author:
published:
created: 2026-09-08
description: "[](#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB)前言---------------------------------------------這裡介紹我最近採用的 4 個 Claude Code 小技巧，其中只有 1 個是工具相關。都不是什麼大規模的機制，所..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月08日](https://codelove.tw/@tony/post/ag2jV3 "2026-09-08 08:10:48")

## 前言

這裡介紹我最近採用的 4 個 Claude Code 小技巧，其中只有 1 個是工具相關。  
都不是什麼大規模的機制，所以如果你有興趣，挑自己在意的部分參考就好。

## 本文快速圖解

[![qitta-cctips.png](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3579041%2F36a9cdf0-5e70-45e7-a22d-9cb6207a8ce7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d7b2ce1cfd5fedcb242962bf30324612)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F3579041%2F36a9cdf0-5e70-45e7-a22d-9cb6207a8ce7.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=d7b2ce1cfd5fedcb242962bf30324612)

## 1\. 自動替 Session 命名

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788837868&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2jV3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788829908639&amp;bpp=20&amp;bdt=7&amp;idt=20&amp;shv=r20260901&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788829907%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788829907%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788829907%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;prev_fmts=0x0%2C1522x969&amp;nras=3&amp;correlator=136488463383&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=110&amp;ady=1422&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400775%2C122880885&amp;oid=2&amp;pvsid=6332077392285080&amp;tmod=90341669&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

相信很多人都知道，Claude Code 的 session 可以命名。  
用 `/rename` 設定後，會以標籤的形式顯示在提示詞輸入欄右下角。

[![image](https://qiita-user-contents.imgix.net/https%3A%2F%2Fpub-a61206ae19934ac48bdb67c6c59ee370.r2.dev%2Fa4e3351a58b903ede074016a54ed4ca1.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ab825c182590391d639b91d33d59f157)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fpub-a61206ae19934ac48bdb67c6c59ee370.r2.dev%2Fa4e3351a58b903ede074016a54ed4ca1.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ab825c182590391d639b91d33d59f157)

如果同時開很多個 session，很容易搞不清楚每個到底是在談什麼。於是我開始幫它們命名，但每次都手動輸入很麻煩。  
所以我就想把這件事自動化，這就是起點。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788837868&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2jV3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788829908715&amp;bpp=10&amp;bdt=83&amp;idt=18&amp;shv=r20260901&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788829907%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788829907%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788829907%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;prev_fmts=0x0%2C1522x969%2C952x280&amp;nras=4&amp;correlator=136488463383&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=110&amp;ady=2420&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400775%2C122880885&amp;oid=2&amp;pvsid=6332077392285080&amp;tmod=90341669&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

### ai-title 和 custom-title 是不同的東西

先說一下原理。  
Claude Code 的 session title 有兩種。

種類誰建立的顯示在哪裡ai-titleClaude Code 在第一輪往返後自動生成 `--resume` 的列表、終端機標題、 `/status` 等custom-title由 `/rename` 或 hook 明確設定除了上面那些之外，還會顯示成標籤

自動生成的 ai-title 會寫入 Claude Code 儲存對話紀錄的檔案中。  
`~/.claude/projects/` 底下每個 session 都有各自的 `.jsonl` ，發言與回應旁邊也會列出標題那一行。

```
{"type":"ai-title","aiTitle":"搜尋索引的調查"}
```

不過，這段文字不會顯示在標籤上。  
標籤讀取的是 custom-title。

也就是說， **標題文字其實已經產生了，只是沒有傳到會顯示的地方** 。

### 透過 hook 把 ai-title 轉成 custom-title

`UserPromptSubmit` hook 只要在標準輸出回傳下列格式的 JSON，就能設定 session 名稱。

```
{"hookSpecificOutput": {"hookEventName": "UserPromptSubmit", "sessionTitle": "搜尋索引的調查"}}
```

能接受這個 `sessionTitle` 的只有 `UserPromptSubmit` 和 `SessionStart` 兩種。若在 `Stop` 等其他 hook 中回傳，會直接被忽略。

（補充）hook 事件列表只有粗體的兩個事件會接受 `sessionTitle` 。

事件觸發時機 **SessionStart** session 開始或恢復時Setup以 `--init-only` 啟動時、以 `-p` 執行 `--init` / `--maintenance` 時 **UserPromptSubmit** 送出提示詞之後、Claude 開始處理之前UserPromptExpansion輸入的指令展開成提示詞時PreToolUse工具執行前PermissionRequest需要判斷是否允許工具執行時PermissionDeniedauto 模式拒絕工具執行時PostToolUse工具執行成功後PostToolUseFailure工具執行失敗後PostToolBatch一批並行的工具呼叫結束後NotificationClaude Code 發出通知時MessageDisplay回應文字顯示期間SubagentStart子代理啟動時SubagentStop子代理結束時TaskCreated用 `TaskCreate` 建立任務時TaskCompleted任務被視為完成時StopClaude 結束回應時StopFailureAPI 錯誤導致該輪結束時TeammateIdle隊友代理進入 idle 之前InstructionsLoaded載入 [CLAUDE.md](http://claude.md/) 或 `.claude/rules/*.md` 時ConfigChange session 中設定檔變更時CwdChanged工作目錄變更時DirectoryAdded session 中新增工作目錄時FileChanged 監視中的檔案在磁碟上變更時WorktreeCreate建立 worktree 時WorktreeRemove刪除 worktree 時PreCompact壓縮 context 之前PostCompact壓縮 context 完成後PreModelSwitch套用模型切換之前PostModelSwitch session 的模型變更後ElicitationMCP 伺服器在執行工具時要求輸入時ElicitationResult使用者回應 MCP 的輸入要求後SessionEnd session 結束時事件未來可能會增加，最新資訊請參考官方文件。

我寫的腳本如下。  
它只是讀取 ai-title 然後回傳，並沒有負責生成標題本身。

```
#!/bin/bash
input=$(cat)
session_id=$(printf '%s' "$input" | jq -r '.session_id // empty')
transcript=$(printf '%s' "$input" | jq -r '.transcript_path // empty')
[ -z "$session_id" ] && exit 0
[ -f "$transcript" ] || exit 0

# 為了避免同一個 session 一再改名，先做記號，設定過一次就不再重複
marker="$HOME/.claude/.session-titled/$session_id"
[ -f "$marker" ] && exit 0
mkdir -p "$(dirname "$marker")"

# 抓取內建功能生成的最新 ai-title；如果還沒有，就留到下一次 prompt 再處理
title=$(jq -r 'select(.type == "ai-title") | .aiTitle' "$transcript" | tail -1)
[ -z "$title" ] && exit 0

# 如果分支名稱裡有 #123，就加到標題前面
cwd=$(printf '%s' "$input" | jq -r '.cwd // "."')
branch=$(git -C "$cwd" rev-parse --abbrev-ref HEAD 2>/dev/null)
number=$(printf '%s' "$branch" | grep -oE '#[0-9]+' | head -1 | tr -d '#')
[ -n "$number" ] && title="#$number $title"

touch "$marker"
jq -n --arg t "$title" \
  '{hookSpecificOutput: {hookEventName: "UserPromptSubmit", sessionTitle: $t}}'
```

將它註冊到 `~/.claude/settings.json` 。  
路徑請改成你自己環境中的位置。

```
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "/Users/you/.claude/hooks/auto-session-title.sh",
            "timeout": 10
          }
        ]
      }
    ]
  }
}
```

### 名稱會在送出第 2 則時才變更

ai-title 是在第 1 輪往返結束後才生成的。  
而已註冊的 hook 是在送出 prompt 的瞬間執行，所以第 1 則訊息時其實還沒有任何標題。實際上會在送出第 2 則 prompt 時，名稱才會變更。

最後幾行是把分支名稱中包含的 Issue 編號加到標題前面。如果你是在 `feature/#123_add_search` 這種分支上工作，標題就會變成 `#123 搜尋索引的調查` 。這是因為之後用 `--resume` 搜尋時，能直接用編號找會更快。

## 2\. 在狀態列顯示 Issue 和 PR

我在 X 上看到有人說狀態列可以放連結。  
既然如此，那是不是也能動態顯示 Issue 和 PR 的連結呢？我就是這樣做出這個設定的。

[![image](https://qiita-user-contents.imgix.net/https%3A%2F%2Fpub-a61206ae19934ac48bdb67c6c59ee370.r2.dev%2Fb399b17b645f3c56a0cb37d7d77add9c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b9689c9fdf66e131de711d9db5bde64c)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fpub-a61206ae19934ac48bdb67c6c59ee370.r2.dev%2Fb399b17b645f3c56a0cb37d7d77add9c.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b9689c9fdf66e131de711d9db5bde64c)

第 2 行的 `#123` 和 `PR#456` 都是連結，點擊後會開啟 GitHub 的對應頁面。

### Issue 編號從分支名稱取得

因為我有把 Issue 編號寫進分支名稱的習慣，所以就直接從那裡擷取。

```
issue_number_from_branch() {
  printf '%s' "$1" | sed -n 's/.*#\([0-9][0-9]*\).*/\1/p'
}
```

連結的 URL 是從傳到狀態列的 JSON 裡的 `workspace.repo` 組出來的。裡面已經分別包含主機名稱、擁有者與儲存庫名稱，所以不必只限定 GitHub。

PR 則使用同一份 JSON 裡的 `pr.number` 和 `pr.url` 。為了支援沒有傳這些資訊的環境，我另外做了透過 `gh pr list` 結果來快取的路徑。狀態列每次顯示都會執行，所以如果每次都同步呼叫 `gh` ，會明顯變慢。因此我改成讀取快取來顯示，過期時再於背景更新。

### 連結化使用 OSC 8

要在終端機裡把文字做成連結，要使用 OSC 8 這個跳脫序列。

```
osc8_link() {
  printf '\033]8;;%s\033\\%s\033]8;;\033\\' "$1" "$2"
}
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3034205192&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1788837902&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2jV3&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788829908889&amp;bpp=1&amp;bdt=258&amp;idt=1&amp;shv=r20260901&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788837870%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788837870%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788837870%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;prev_fmts=0x0%2C1522x969%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=136488463383&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=110&amp;ady=6572&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=2737&amp;eid=95399629%2C95400775%2C122880885&amp;oid=2&amp;psts=AOrYGslb_SA9ScDGo2ZWixVPiI7sYd98vHY8w-9UqtA6_0sOy6yROJc3EgSKqDCVU-V2xkWLFxDhuWt3_OVjQEnZOdpXPZ3ocdHrgeF024kVzIuahepXxorwMBrl5NLyJzMJCg%2CAOrYGsl6DBXoZX47QnXjvVBKNBP_43QAdJCgQygozYFOHWMK9rXSjx_y4rnzPVbKRd5hIZb6ZMTSvSGrm1AiHjlHayODtGQLYFPV35yCe_e5Ssf-3A%2CAOrYGsmkdEYriHIDWZX7DJ1VDu7ApcpzG-CKknP4rV7XCmdeLgGnmodeY3ucFX9q7zdd-QBC0NvypVeyq0J8bDzLTWcucMo5N7ZXnugQ5Fy7UIOZEA&amp;pvsid=6332077392285080&amp;tmod=90341669&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1762%2C1050%2C1522%2C969&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.16&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這裡我卡了一下。OSC 8 的結尾包含反斜線，因此如果把有顏色的字串用 `printf '%b'` 的方式傳入，跳脫解讀會互相衝突，導致連結壞掉。

後來改成把顏色用 `$'\033[33m'` 的形式直接當成位元組保存，輸出則統一用 `%s` ，問題就解決了。

## 3\. 統一速率限制的重設時間

這個統一速率限制重設時間的小技巧，是我從下面這篇文章知道的。

Claude 的速率限制是以 5 小時的滾動視窗計算， **起點是你送出第一則訊息的時間** 。也就是說，一旦你開始工作後送出第一則訊息，之後 5 小時就從那一刻開始計時。

因此有人會用 routine 功能，在每天固定時間自動送出 1 則 ping，讓起點固定下來。因為是在雲端執行，就算電腦沒開也能運作。

### 我的設定範例

我通常在 8 點前後開始上班，所以設定在 6 點。

```
06:00 - 11:00  ← 3h(08:00 - 11:00)
11:00 - 16:00  ← 4h(1h休息)
16:00 - 21:00  ← 2h(16:00 - 18:00)
```

效果很明顯。若把拘束時間算成 10 小時（8 小時工作 + 1 小時休息 + 1 小時加班），沒有 routine 的話，這段期間只會夾到 1 次重設。  
因為起點會在開始上班的瞬間就固定下來。

如果固定在 6 點，這 10 小時內就會夾到 2 次重設。  
實際上在需要在意 5 小時用量的情境中，體感上少很多。

## 4\. 把資訊集中到 Orca

這也是我在 X 上看到後試用的。  
Orca 是一款用來在多個 worktree 上並行運作 AI 代理的桌面應用程式。

在那之前，我是用同樣用途的終端機多工器 herdr，並在 Ghostty 上啟動來使用。  
實際換過之後，發現我在 herdr 上會用到的功能，Orca 全都能做。

而且它還內建了以下功能：

- 檔案操作（像是顯示在檔案總管、寫入、刪除這類 IDE 式功能）
- 瀏覽器
- Mac 睡眠控制
- AI 模型目前的使用量顯示
- GitHub 整合
- 支援行動裝置

herdr 其實也能透過外掛或搭配其他工具做到同樣的事。  
不過因為是內建功能，所以 Orca 明顯方便很多。

最有感的地方，是資訊可以集中在同一處。  
不用再反覆切換視窗去重新找資料，能夠把每個任務的資訊（終端機、瀏覽器、md 檔等）整理在同一個畫面上，感覺腦中的負擔也變輕了。

## 最後

Session 名稱和狀態列都需要自己做，所以會稍微花工夫；但速率限制設定和 Orca 則是裝上去就能用。可以先從有興趣的部分試試看。

## 參考資料

## 株式会社シンシア

在 株式会社シンシア，我們招募沒有實務經驗的工程師以及學生工程師實習生，一起工作。  
※ 想了解シンシア的工作方式，請看這裡

我們公司每年會收到超過 100 位沒有實務經驗者的應徵，並進行技術面試。  
如果這篇文章對你有一點幫助，也非常歡迎你去看看 wantedly 的故事頁面，我們會很開心！

---

原文出處： [https://qiita.com/kuma\_3838/items/00cb0b8d61ca76769c88](https://qiita.com/kuma_3838/items/00cb0b8d61ca76769c88)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/ag2jV3) [3瀏覽](https://codelove.tw/@tony/post/ag2jV3)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/ag2jV3)

---

> [!secondary] Secondary
> 文字內容提供幾種功能：  
> 1) --- 會變成分隔線（上一行必須是空白）  
> 2) # 會變成一級標題  
> 3) ## 會變成二級標題  
> 4) ### 會變成三級標題  
> 5) \*\*粗體文字\*\*會顯示 **粗體文字**  
> 6) \`\`\`當第一行與最後一行會顯示程式碼  
> 7) 請搜尋 Markdown 語法，了解各種格式

[小編精選 - 技術文章翻譯](https://codelove.tw/@tony)

---

精選技術文章翻譯，幫助開發者持續吸收新知。

熱門文章

---

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝25

363

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

3

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1788829907&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fag2jV3&amp;pra=5&amp;aiof=11&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1Mi4wLjc5NzcuODMiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTIuMC43OTc3LjgzIl0sWyJOb3Q_QV9CcmFuZCIsIjI0LjAuMC4wIl0sWyJHb29nbGUgQ2hyb21lIiwiMTUyLjAuNzk3Ny44MyJdXSwwXQ..&amp;dt=1788829907248&amp;bpp=14&amp;bdt=505&amp;idt=86&amp;shv=r20260901&amp;mjsv=m202609030101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D306081604bb90e5a%3AT%3D1788764546%3ART%3D1788829907%3AS%3DALNI_Marr1MZFbycjoptvI7Z_3F8wDutVg&amp;gpic=UID%3D0000156849de063c%3AT%3D1788764546%3ART%3D1788829907%3AS%3DALNI_Ma0nlZ7WOPxuzqYwieWJpvFP5rcyw&amp;eo_id_str=ID%3Ddf15463d9a8f87a4%3AT%3D1788764546%3ART%3D1788829907%3AS%3DAA-AfjYxIuf9qW52Y5YZhHAiFzSR&amp;nras=1&amp;correlator=136488463383&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1522&amp;bih=969&amp;scr_x=0&amp;scr_y=0&amp;eid=95399629%2C95400775%2C122880885&amp;oid=2&amp;pvsid=6332077392285080&amp;tmod=90341669&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1522%2C969&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=152" title="Advertisement" aria-label="Advertisement"></iframe>