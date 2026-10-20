---
title: "如何為 Claude Code・Codex 建立超高速且超安全的沙盒環境"
source: "https://codelove.tw/@tony/post/3kV7zx"
author:
published:
created: 2026-07-13
description: "こんにちは！我是哥倫比亞大學博士課程、研究 AI 與資安的 Koukyosyumei。那麼，我想大多數讀者都是使用 Claude Code 或 Codex 來開發的吧；大家對於「AI agent 在執行 shell 指令或網路存取時，是如何確認安全性」這件事，理解到什麼程度呢？Agent 雖..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月13日](https://codelove.tw/@tony/post/3kV7zx "2026-07-13 08:15:10")

## 如何為 Claude Code・Codex 建立超高速且超安全的沙盒環境

こんにちは！我是哥倫比亞大學博士課程、研究 AI 與資安的 Koukyosyumei。

那麼，我想大多數讀者都是使用 Claude Code 或 Codex 來開發的吧；大家對於「AI agent 在執行 shell 指令或網路存取時，是如何確認安全性」這件事，理解到什麼程度呢？

Agent 雖然能自動完成各種工作，但也可能不小心刪除檔案、把機密資訊傳送到外部，或執行會大量消耗 CPU 資源的低效率程式。

當然，Claude Code 和 Codex 都內建了用來安全執行 agent 的 sandbox 機制。此外，像是 [h5i](https://github.com/h5i-dev/h5i/tree/main) 和 [docker-agent](https://github.com/docker/docker-agent) 這類更高性能、面向 agent 的 sandbox 也逐漸受到關注。

本文將解說 Claude Code 與 Codex 內建 sandbox 功能的運作方式，並進一步說明更高性能的 sandbox——h5i 的簡單使用方法。

## Claude Code 的沙盒

Claude Code 可以將 OS 層級的沙盒功能（Bubblewrap、socat、seccomp 等等）套用到 Bash Tool 上。至於 Read、Write、WebFetch 等其他工具，則透過適時向使用者確認的 permission model 來確保安全性。

這些沙盒設定可以在 `settings.json` 的 [sandbox block](https://code.claude.com/docs/en/sandboxing) 中決定，並能針對各個 Bash Tool 設定允許的指令、排除的指令、是否可存取網路、憑證的處理方式等。

```
{
  "sandbox": {
    "enabled": true,
    "network": {
      "tlsTerminate": {},
      "allowedDomains": ["*.github.com", "registry.npmjs.org"]
    },
    "credentials": {
      "envVars": [
        { "name": "GH_TOKEN", "mode": "mask", "injectHosts": ["api.github.com"] },
        { "name": "NPM_TOKEN", "mode": "mask" }
      ]
    }
  }
}
```

另外，也可以在 permissions block 中，針對各工具靜態定義 allow / ask / deny 規則。

```
{
  "permissions": {
    "allow": [
      "Bash(npm run *)",
      "Bash(git commit *)",
      "Bash(git * main)",
      "Bash(* --version)",
      "Bash(* --help *)"
    ],
    "deny": [
      "Bash(git push *)"
    ]
  }
}
```

## Codex 的沙盒

在 Codex 中，會結合兩種原則：決定「允許什麼」的 sandbox policy，以及決定「要向誰詢問」的 approval policy，並使用 OS 層級的沙盒來執行所有指令。

sandbox policy 提供以下三種模式：

- read-only：可以讀取整個磁碟，但不能寫入，也不能存取網路
- workspace-write：可以對目前目錄、/tmp 等位置進行寫入
- danger-full-access：允許更大範圍的存取模式。

approval policy 例如有以下三種選項：

- on-request：預設模式，當模型判斷有需要時，會向使用者請求核准。
- never：一律拒絕需要提升權限的請求。
- granular：可對特定請求進行拒絕等更細緻的設定。

## h5i

h5i 是一套可用更高自由度設定更廣範圍的沙盒工具。Claude Code 和 Codex 基本上是專注於判斷個別指令是否可執行，而 h5i 則是分離工作環境本身。

具體來說，透過 `h5i env` 指令，可以建立一個在 OS 層級沙盒中隔離的專用 Git worktree。除了 Claude Code 和 Codex 所具備的系統呼叫層級限制與資料夾存取限制之外，還能設定記憶體、程序數量等資源限制。此外，也能彈性設定以網域為單位的存取允許與否，以及憑證在不同環境之間的分離等。

**安裝**

```
curl -fsSL https://raw.githubusercontent.com/h5i-dev/h5i/main/install.sh | sh
```

**初始化**

```
h5i init
h5i hook setup --write --wrap-bash
```

**建立沙盒環境**

```
h5i env create claude-env --profile agent-claude
h5i env shell claude-env
box$ claude --dangerously-skip-permissions
box$ exit

h5i env diff claude-env      # 檢查環境內的變更
h5i env propose claude-env   # 提出環境內的變更
h5i env apply claude-env     # 將提議的變更套用回原始分支
```

`h5i env` 的預設模式 supervised 模式，不使用像 docker 這類較重的容器，而是透過結合系統呼叫監控與 namespace 設定來實現輕量級的沙盒環境，因此只要約 0.2～3 秒就能進入沙盒內的 shell。

在這個環境中，即使 agent 因為提示注入攻擊、幻覺或其他原因而誤作動，也能避免對原始環境造成破壞性變更，或防止機密資訊外洩等事故。此外，h5i 以「可稽核的工作區」為標語，環境內執行過的指令、其結果、使用者提供的提示等都會被完整記錄，之後也可以加以驗證。

## 參考

- [https://github.com/anthropics/claude-code](https://github.com/anthropics/claude-code)
- [https://github.com/openai/codex](https://github.com/openai/codex)
- [https://github.com/h5i-dev/h5i](https://github.com/h5i-dev/h5i)
- [https://zenn.dev/koukyosyumei/articles/d6538033062ad5](https://zenn.dev/koukyosyumei/articles/d6538033062ad5)

---

原文出處： [https://qiita.com/Koukyosyumei/items/3bd4696237ea64c38971](https://qiita.com/Koukyosyumei/items/3bd4696237ea64c38971)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/3kV7zx) [5瀏覽](https://codelove.tw/@tony/post/3kV7zx)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/3kV7zx)

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

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Anthropic 為何限制中國大陸使用 Claude？](https://codelove.tw/@tony/post/qO2P8a)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝17 💬3 ❤️5

735

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝3 💬6 ❤️4

165

🥉

[流引色群@jptd847utpp](https://codelove.tw/@sp9863)

1

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783904812&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F3kV7zx&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783904812888&amp;bpp=1&amp;bdt=144&amp;idt=34&amp;shv=r20260709&amp;mjsv=m202607070101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783589372%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783589372%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783589372%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6910944867498&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1676&amp;bih=1003&amp;scr_x=0&amp;scr_y=0&amp;eid=42532523%2C42533293%2C95394427%2C95395869&amp;oid=2&amp;pvsid=7268457042686023&amp;tmod=865723311&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest%26page%3D1&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1676%2C1003&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=61" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260709/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>