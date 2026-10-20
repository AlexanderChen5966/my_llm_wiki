---
title: "把 API 金鑰交給 AI agent 沒問題嗎？對話文字、.env、環境變數、MCP、OAuth——用 Claude Code、Codex、Gemini CLI、Copilot、Cursor 實測 6 種傳遞方式"
source: "https://codelove.tw/@tony/post/qJRyNq"
author:
published:
created: 2026-10-06
description: "![AI 代理人傳遞金鑰的 6 種方式示意圖。1 貼到對話內容中：金鑰會送到 AI 公司，且也會留在你的對話紀錄中（不建議）。2 寫入檔案（如 .env）再讀取：AI 一讀到內容就會進入對話。排除設定只能阻擋 AI 的「讀取」功能（不建議）。3 設成環境變數（如 .zshrc）：AI 看不到，但 A..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [10月06日](https://codelove.tw/@tony/post/qJRyNq "2026-10-06 08:06:25")

## 把 API 金鑰交給 AI agent 沒問題嗎？對話文字、.env、環境變數、MCP、OAuth——用 Claude Code、Codex、Gemini CLI、Copilot、Cursor 實測 6 種傳遞方式

[![AI 代理人傳遞金鑰的 6 種方式示意圖。1 貼到對話內容中：金鑰會送到 AI 公司，且也會留在你的對話紀錄中（不建議）。2 寫入檔案（如 .env）再讀取：AI 一讀到內容就會進入對話。排除設定只能阻擋 AI 的「讀取」功能（不建議）。3 設成環境變數（如 .zshrc）：AI 看不到，但 AI 執行的指令一開始就會繼承到（需要縮限設定）。4 寫進 MCP 設定：寫在設定檔裡會連同檔案一起外洩。改用環境變數參照（不要直接寫入）。5 透過 OAuth 授權：不把金鑰字串交給 AI，設定期限與範圍進行授權（最安全）。6 登錄到雲端金鑰保管處：在 Claude Code 與 Codex 的雲端中，AI 不會持有真正的金鑰（視工具而定）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-methods.dfa951c3.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7dfd10aad0cb014c44d43ca8379ed78d)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-methods.dfa951c3.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=7dfd10aad0cb014c44d43ca8379ed78d)

有時候會想把需要使用 API 金鑰的工作交給 Claude Code、Codex 這類 AI 代理人處理。這時候，使用者該怎麼把金鑰交出去呢？貼在對話內容、寫進.env 讓它讀、放在.zshrc 的環境變數……不同的傳遞方式，金鑰會遭遇的情況也不同。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1791269749&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRyNq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuOTUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3Ljk1Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy45NSJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1791266543302&amp;bpp=2&amp;bdt=1621&amp;idt=2&amp;shv=r20261005&amp;mjsv=m202610010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1791266542%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1791266542%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1791266542%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873&amp;nras=3&amp;correlator=7575220285028&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=1425&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95403356%2C95405219%2C95340252%2C95340254&amp;oid=2&amp;pvsid=1914951884277642&amp;tmod=1218001112&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

如開頭圖所示，從使用者的操作來看，金鑰的傳遞方式可分成 6 種。金鑰字串越接近 AI 能看到的地方，外洩路徑就越多。最安全的是不要把金鑰字串本身交給 AI，而是使用 OAuth。

本文將從 [Claude Code](https://renkeimap.jp/tool/claude-code) 、 [Codex](https://renkeimap.jp/tool/codex) 、Gemini CLI、 [GitHub Copilot](https://renkeimap.jp/tool/github-copilot) 、 [Cursor](https://renkeimap.jp/tool/cursor) 這 5 種工具的官方文件出發，回答不同傳遞方式會發生什麼事，以及該如何設定才能保護金鑰。引用的原文則逐行收錄在調查頁面 [把金鑰交給 AI 代理人沒問題嗎——6 種傳遞方式](https://renkeimap.jp/guide/decide/ai-agent-keys) 中。

---

## 1\. 先統一用語

## 用語意義金鑰系統用來確認「是不是允許使用的對象」的秘密字串。包含密碼、API 金鑰、存取權杖等。本文統稱為「金鑰」API 金鑰金鑰的一種。程式在呼叫其他系統的 API 時，為了表明自己是誰而送出的字串。持有 API 金鑰的人可以做出和擁有者相同的操作AI 代理人接收指示後，自己讀取檔案並執行命令來完成工作的 AI殼層／命令電腦上用文字下指令的畫面（shell），以及在其中輸入的指令（command）.env存放 API 金鑰、密碼等秘密設定的檔案環境變數執行程式時從外部傳入的設定值。使用者會寫在.zshrc 等檔案中MCP將 AI 與外部服務或工具連接起來的規範OAuth以設定期限與範圍的方式授權，而不是使用固定金鑰的機制2. 貼在對話內容中會怎樣

[![示意圖說明貼到對話內容中的金鑰會流向何處。會送到 AI 公司的伺服器（Cursor 在關閉 Privacy Mode 後會保存提示詞，且可能用於訓練）。也會留在本機的對話紀錄中（Claude Code 與 Codex 會將對話保存在本機，且 Claude Code 是以未加密的明文保存）。也會同步到 GitHub 帳號（Copilot CLI 的初始設定）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-paste.7b883ca2.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=421193c8e97f5d303c9d1720d13d2d45)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-paste.7b883ca2.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=421193c8e97f5d303c9d1720d13d2d45)

貼到對話內容中的金鑰，會被送到 AI 公司，並且也會留在你的對話紀錄中。Copilot CLI 在初始設定下，會把對話同步到 GitHub 帳號。Google 也在使用條款中明訂，免費版 Gemini API 不要傳送敏感資訊。使用者最好的做法，就是不要把金鑰貼進對話內容。

> 📎 各工具的原文可見 [把金鑰交給 AI 代理人沒問題嗎 的「貼在對話內容中」](https://renkeimap.jp/guide/decide/ai-agent-keys#paste) 。

## 3\. 寫入.env 再讀取會怎樣

[![說明在設定中拒絕讀取 .env 時，會擋到哪裡的圖，以 Claude Code 畫面示意分成 3 種情況。①透過 AI 工具的「讀取檔案」功能開啟（Read(.env)）會被擋住。②用命令直接點名檔名來讀（Bash(cat .env)）則視工具而定。Claude Code 會擋，但 Cursor 寫明無法在代理人終端機中阻擋。③不明講檔名的讀取（grep -r）或由腳本自行開檔（python）不會被擋住，仍然可能讀到 .env 內容。要阻擋這類情況必須使用沙箱](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-bypass.ae40d150.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=280aefe92b45983f860365f615b89fd9)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-bypass.ae40d150.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=280aefe92b45983f860365f615b89fd9)

如果 AI 讀取了.env，.env 的內容就會進入對話，等同於把金鑰貼到對話內容中。Gemini CLI 會自動載入.env。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1791269751&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRyNq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuOTUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3Ljk1Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy45NSJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1791266543309&amp;bpp=1&amp;bdt=1628&amp;idt=0&amp;shv=r20261005&amp;mjsv=m202610010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1791266542%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1791266542%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1791266542%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250&amp;nras=7&amp;correlator=7575220285028&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=4100&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=635&amp;eid=95396000%2C95403356%2C95405219%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsn7KEv9E6y2q9Qzpo_mWdtBi_sqKtlC3lxtBMMrwVBgs0lL5V5kDvyNIus3mXR5_l1sYvV3QbaWGRE4IjB-f9GKlQRv01u0ZQrEuEi1hhPd0zkpdDX_wL1WS9yQ0T_A4f0%2CAOrYGsm1qIJwOOOHJC8mndn0gB9isIk714IiDSHi96Si4qx1QtVS_bL_HxVRkCE7Oyusn64XfPvDvz69u2aDflEnMC2a32rt2cypeWH9f1-qhoEuBw&amp;pvsid=1914951884277642&amp;tmod=1218001112&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C954%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=5&amp;uci=a!5&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

不讓 AI 讀取的「排除設定」，只能對 AI 工具內建的「讀取檔案」功能生效。Claude Code 也會阻擋像 cat 這樣直接點名檔案的指令。另一方面，排除設定無法阻止不明講檔名的讀取指令，或是由腳本自行開啟檔案的情況。

> 📎 排除設定的寫法與無法發揮作用的地方，整理在 [把金鑰交給 AI 代理人沒問題嗎 的「調查細節」](https://renkeimap.jp/guide/decide/ai-agent-keys#research) 中，並依工具分別列出。

## 4\. 設成環境變數後會怎樣

[![示意圖說明環境變數金鑰的流向，共 4 層。1 寫在 .zshrc 等處的環境變數，會在每次開啟終端機時載入，並傳給由此啟動的程式。2 AI 代理人本身不會直接看到值。3 AI 執行的指令會從一開始就繼承到。Codex 在初始設定下會全部傳遞，且 Codex、Copilot CLI、Claude Code 都提供縮限可傳遞變數的設定。4 若讀取到的文字中藏有指令，也可能把整個環境變數送到外部（Gemini CLI 的實證）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-env.1d1ef969.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ec47c8595c60255ec7ba20104ea55a43)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-env.1d1ef969.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ec47c8595c60255ec7ba20104ea55a43)

使用者若在.zshrc 等處 export 金鑰，除非 AI 把它顯示出來，否則不會進入對話。不過，AI 執行的指令一開始就會繼承該環境變數。Codex 在初始設定下會把所有環境變數都傳給指令。也曾有實證顯示，AI 讀到的文字中若藏有指令，可能會把整個環境變數外送（Gemini CLI）。

縮限要傳給指令的變數之設定，Claude Code、Copilot CLI、Codex 都有提供。但無論哪一種工具，都必須由使用者自行啟用該設定。

> 📎 原文可見 [把金鑰交給 AI 代理人沒問題嗎 的「設成環境變數」](https://renkeimap.jp/guide/decide/ai-agent-keys#env) 。

## 5\. MCP 的設定檔要如何寫

[![公開 MCP 伺服器的金鑰取得方式長條圖。期限較長的固定金鑰（API 金鑰或個人存取權杖）占 53%，OAuth（有期限與範圍限制的授權）占 8.5%（Astrix Security）](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-mcp-auth.7e90e5d3.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=15b6bf79f7e222a303866bfaec2305b3)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-mcp-auth.7e90e5d3.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=15b6bf79f7e222a303866bfaec2305b3)

如果把金鑰直接寫進 MCP 設定檔，一旦設定檔外洩，金鑰也會一併外洩。各家都建議不要直接寫入金鑰，而是用環境變數參照。Astrix Security 的調查顯示，公開的 MCP 伺服器中有 53% 使用期限較長的固定金鑰，而使用 OAuth 的伺服器只有 8.5%。

在新增 MCP 伺服器之前，建議先確認的事項，已整理在 [新增 MCP 伺服器前該確認什麼](https://renkeimap.jp/guide/decide/mcp-before-adding) 中。

> 📎 原文可見 [把金鑰交給 AI 代理人沒問題嗎 的「寫進 MCP 設定」](https://renkeimap.jp/guide/decide/ai-agent-keys#mcp) 。

## 6\. 最安全的是透過 OAuth 授權

[![上下對比圖說明：把金鑰字串交給 AI 代理人，與透過 OAuth 授權的差異。若交出金鑰字串，金鑰會進入對話並留在紀錄中，外洩後要撤銷前都能被使用。若透過 OAuth 授權，則可設定期限與範圍，且金鑰字串不會交給 AI，因此即使授權外洩，損害也較小](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-oauth.b0224eee.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=664c17a91f87988af7648ebc7ae553e5)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-oauth.b0224eee.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=664c17a91f87988af7648ebc7ae553e5)

透過 [OAuth](https://renkeimap.jp/guide/learn/oauth) 授權時，使用者不會把金鑰字串交給 AI。使用者是設定期限與範圍來授權，因此即使授權外洩，造成的損害也會比較小。這 5 種工具都能透過 OAuth 連接外部 MCP 伺服器（GitHub Copilot 的雲端代理人尚未支援）。

對於在雲端運作的代理人，也有把金鑰註冊到雲端金鑰保管處的方式。在 Claude Code 與 Codex 的雲端中，AI 不會持有真正的金鑰。

> 📎 原文可見 [把金鑰交給 AI 代理人沒問題嗎 的「透過 OAuth 授權」](https://renkeimap.jp/guide/decide/ai-agent-keys#oauth) 。

## 7\. 各工具的金鑰保護設定

[![並列各工具金鑰保護設定的圖。Claude Code：拒絕讀取的設定可用於拒絕 Read(./.env) 的規則，也會阻擋 cat 等指令；環境變數預設會傳遞，縮限設定需自行啟用；持有金鑰的 MCP 伺服器可放在自己的範圍內；支援 OAuth。Codex：可寫下拒絕讀取 .env 的規則；環境變數預設全部傳遞，可用 shell_environment_policy 縮限；MCP 金鑰可用環境變數名稱指定；可用 codex mcp login 支援 OAuth。Gemini CLI：可建立 .geminiignore，但 .env 預設會自動讀入；有隱藏設定但預設值說明互相矛盾；建議不要直接寫入金鑰；支援 OAuth。GitHub Copilot：Content exclusion 不適用於 CLI 與代理人模式；可用 --secret-env-vars 隱藏；可用 $VAR 參照；OAuth 支援 CLI，但雲端代理人不支援。Cursor：.cursorignore 預設會排除 .env，但終端機不在範圍內；找不到縮限環境變數的設定；建議使用環境變數；支援 OAuth](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-tools.9e111b60.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=f3ab7b743980f6d3193fe0685ea1b44c)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-tools.9e111b60.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=f3ab7b743980f6d3193fe0685ea1b44c)

把「不讓它讀取的設定」「縮限環境變數的設定」「MCP 金鑰的寫法」「OAuth」四者並列來看，每種工具都會有「一開始什麼都沒做」的地方。即使只使用其中一種工具，也建議至少確認一次。各工具的訓練利用與費用等資訊，已整理在 [Claude Code](https://renkeimap.jp/tool/claude-code) 、 [Codex](https://renkeimap.jp/tool/codex) 、 [GitHub Copilot](https://renkeimap.jp/tool/github-copilot) 、 [Cursor](https://renkeimap.jp/tool/cursor) 的產品頁面中。

> 📎 各列的原文可見 [把金鑰交給 AI 代理人沒問題嗎 的「調查細節」](https://renkeimap.jp/guide/decide/ai-agent-keys#research) 。

## 8\. 要交出來就用這個順序

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3034205192&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1791269758&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRyNq&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuOTUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3Ljk1Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy45NSJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1791266543316&amp;bpp=1&amp;bdt=1635&amp;idt=1&amp;shv=r20261005&amp;mjsv=m202610010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1791266542%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1791266542%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1791266542%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873%2C952x280%2C952x280%2C952x280%2C1005x124%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=7575220285028&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=7907&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=4441&amp;eid=95396000%2C95403356%2C95405219%2C95340252%2C95340254&amp;oid=2&amp;psts=AOrYGsn7KEv9E6y2q9Qzpo_mWdtBi_sqKtlC3lxtBMMrwVBgs0lL5V5kDvyNIus3mXR5_l1sYvV3QbaWGRE4IjB-f9GKlQRv01u0ZQrEuEi1hhPd0zkpdDX_wL1WS9yQ0T_A4f0%2CAOrYGsm1qIJwOOOHJC8mndn0gB9isIk714IiDSHi96Si4qx1QtVS_bL_HxVRkCE7Oyusn64XfPvDvz69u2aDflEnMC2a32rt2cypeWH9f1-qhoEuBw%2CAOrYGskmPWkdKTbdGo7P_PI5QRvCztu9XLHfGuWW56i8mMZfjmiLs1xxUTFMsiYN7JniOb3QZJr55cXWyLAhVXGIwnQJwodiswahLknVlSy4egwxIQ&amp;pvsid=1914951884277642&amp;tmod=1218001112&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1920%2C30%2C1920%2C30%2C1920%2C30%2C1920%2C954%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

[![AI 代理人傳遞金鑰時的 5 個步驟示意圖。①不要把金鑰放進對話內容，②建立權限縮小的金鑰，③若能選擇具期限的授權（OAuth）就優先使用，④透過排除設定與沙箱縮小可讀取範圍（若要連 AI 執行的指令開啟時也一併阻擋，需使用沙箱），⑤一旦外洩就立刻撤銷](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-steps.82ea02bf.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=96e1b6ec7e9e73e6de132b9d96202c28)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fimg.renkeimap.jp%2Ff%2Fai-agent-keys-steps.82ea02bf.svg?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=96e1b6ec7e9e73e6de132b9d96202c28)

使用者第一件事，就是不要把金鑰貼進對話內容。接著，建立權限縮小過的金鑰。排除設定只是防護之一，不要只依賴排除設定。

金鑰放置位置的想法可參考 [什麼是密鑰管理](https://renkeimap.jp/guide/term/secret-management) ，金鑰的期限與撤銷方式可參考 [API 權杖何時過期、如何撤銷](https://renkeimap.jp/guide/learn/token-lifetime) 。金鑰從哪裡外洩，可參考 [API 金鑰會從哪裡外洩](https://renkeimap.jp/guide/learn/api-key-leak) ；外洩金鑰由誰來阻止，則可參考 [什麼是密鑰掃描](https://renkeimap.jp/guide/learn/secret-scanning) 。

## 一手資料

出處內容 [Claude Code Docs: Data usage](https://code.claude.com/docs/en/data-usage) 對話會送到哪裡，以及本機記錄 [Claude Code Docs: Configure permissions](https://code.claude.com/docs/en/permissions) 拒絕讀取的規則，以及無效之處 [Claude Code Docs: Cloud environments](https://code.claude.com/docs/en/cloud-environments) 雲端中 AI 不持有真正的金鑰 [OpenAI Codex Docs: Advanced configuration](https://learn.chatgpt.com/docs/config-file/config-advanced) 傳給指令的環境變數設定 [OpenAI Codex Docs: MCP](https://learn.chatgpt.com/docs/extend/mcp?surface=cli) MCP 金鑰與 OAuth [Gemini CLI Docs: Configuration](https://geminicli.com/docs/reference/configuration/) 自動載入.env [Gemini CLI Docs: MCP servers](https://geminicli.com/docs/tools/mcp-server/) 不要直接寫金鑰・OAuth [Gemini API 追加利用條款](https://ai.google.dev/gemini-api/terms) 免費版不要傳送敏感資訊 [GitHub Docs: Copilot 的 session data](https://docs.github.com/en/copilot/concepts/security-governance-and-network-settings/session-data) 把對話同步到 GitHub 帳號 [GitHub Docs: Copilot CLI command reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference) 隱藏環境變數・OAuth [Cursor Docs: Ignore file](https://cursor.com/docs/reference/ignore-file).cursorignore 與在終端機中無法阻擋的情況 [Cursor Docs: MCP](https://cursor.com/docs/mcp) 金鑰以環境變數方式處理・OAuth [Astrix Security: State of MCP Server Security 2025](https://astrix.security/learn/blog/state-of-mcp-server-security-2025/) MCP 伺服器的金鑰取得方式 [Tracebit: Gemini CLI 的實證](https://tracebit.com/blog/code-exec-deception-gemini-ai-cli-hijack) 環境變數被送到外部引用的原文與出處，都已在調查頁面逐行公開。想確認的人請參閱 [把金鑰交給 AI 代理人沒問題嗎——6 種傳遞方式](https://renkeimap.jp/guide/decide/ai-agent-keys) 。

**讀者問卷進行中**  
如果你希望這個主題再深入一點，請幫本文按「讚」。我們會依照獲得「讚」最多的主題優先追加調查，結果會在新文章中通知你（追蹤後即可收到通知）。

**【可轉載】本文轉載說明**

本文的文字與圖表皆可自由轉載。圖片請 **勿修改** 直接使用。轉載時請在出處標註並連結到本文完整版 [《把金鑰交給 AI 代理人沒問題嗎》](https://renkeimap.jp/guide/decide/ai-agent-keys) （ [renkeimap.jp](http://renkeimap.jp/) ）。無需事前聯絡。

※ 作者曾任職於日立系 IT 廠商、照護軟體廠商、大學醫院 IT 部門，現已獨立，並在協助中小企業 IT／DX 導入的同時，根據一手資料調查業務系統之間的「連結」。文中的「編輯部」是指作者所屬的 [IT 連結地圖編輯部](https://renkeimap.jp/about) 。若發現錯誤，請至 [更正窗口](https://renkeimap.jp/request) （免費、無須帳號）提出。更正紀錄也會公開。

---

原文出處： [https://qiita.com/songchong/items/873b4f14d26296176cfd](https://qiita.com/songchong/items/873b4f14d26296176cfd)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/qJRyNq) [6瀏覽](https://codelove.tw/@tony/post/qJRyNq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/qJRyNq)

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

🔥 [Jev是什麼？理解它與傳統型 LLM 與 AI 代理人的差異](https://codelove.tw/@tony/post/3DRvQq)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝54 💬2

732

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商事務所：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1791266542&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FqJRyNq&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuOTUiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3Ljk1Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy45NSJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1791266541979&amp;bpp=4&amp;bdt=340&amp;idt=216&amp;shv=r20261005&amp;mjsv=m202610010101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1791172489%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1791172489%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1791172489%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=7575220285028&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95396000%2C95403356%2C95405219%2C95340252%2C95340254&amp;oid=2&amp;pvsid=1914951884277642&amp;tmod=1218001112&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C873&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=254" title="Advertisement" aria-label="Advertisement"></iframe>