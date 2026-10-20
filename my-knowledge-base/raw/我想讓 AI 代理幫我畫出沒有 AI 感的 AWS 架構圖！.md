---
title: "我想讓 AI 代理幫我畫出沒有 AI 感的 AWS 架構圖！"
source: "https://codelove.tw/@tony/post/anbd0a"
author:
published:
created: 2026-09-29
description: "你們過去請 Claude 或 Codex（之後統稱為「AI 代理」）幫忙製作的架構圖，是不是長這樣？![AWS Diagram MCP Server 繪製的圓角 AWS 架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-..."
tags:
  - "clippings"
status: processed
---
[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [09月27日](https://codelove.tw/@tony/post/anbd0a "2026-09-27 08:03:45")

## 我想讓 AI 代理幫我畫出沒有 AI 感的 AWS 架構圖！

你們過去請 Claude 或 Codex（之後統稱為「AI 代理」）幫忙製作的架構圖，是不是長這樣？

[![AWS Diagram MCP Server 繪製的圓角 AWS 架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2Fdd66ae63-528b-4fc8-82fb-77d477df13c6.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5d318bcff73dae5788d9b9a83c6c5dac)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2Fdd66ae63-528b-4fc8-82fb-77d477df13c6.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5d318bcff73dae5788d9b9a83c6c5dac)

圖示和子網路的角看起來都圓圓的，很有 AI 味吧。「這樣還不如自己畫算了」就會這麼想。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=2278612020&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790671264&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbd0a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669078818&amp;bpp=5&amp;bdt=1165&amp;idt=5&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790671147%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790671147%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790671147%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873&amp;nras=3&amp;correlator=2201155589796&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=2599&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95403098%2C95403358%2C95390278%2C122880836%2C122881195&amp;oid=2&amp;pvsid=5989143209932568&amp;tmod=933744953&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

雖然不只程式開發，連雜務等日常工作都能交給 AI 代理了，但為什麼還是生成不出理想中那種漂亮的架構圖呢？

為了找出能做出符合期待架構圖的方法，我實際用幾種 MCP 和 Skill 做了驗證。  
結果做出了（我個人覺得）相當滿意的架構圖，所以接下來要分享的是：如何輸出業務上也能用的（我個人認為）架構圖！

[![把指示補齊後，Claude Code 繪製的 AWS 架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2Fa19bca81-ac7d-4357-9fba-a928e0dc5ec8.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2c1cfe766e4f8e5385a1db788d055e83)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2Fa19bca81-ac7d-4357-9fba-a928e0dc5ec8.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2c1cfe766e4f8e5385a1db788d055e83)

我是以 2026 年 9 月的 Claude Code 2.1.278（模型為 Claude Opus 5）進行驗證的。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=367994246&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790671323&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbd0a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669078831&amp;bpp=1&amp;bdt=1178&amp;idt=1&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790669078%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790669078%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790669078%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873%2C952x280%2C728x90%2C334x250&amp;nras=5&amp;correlator=2201155589796&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=4117&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=645&amp;eid=95403098%2C95403358%2C95390278%2C122880836%2C122881195&amp;oid=2&amp;psts=AOrYGsl9JOaZFHlsfSPaQ8qwHByVl-DXBVOKZ8rOf0i8rO7yDILymsIsf4yDzggCFuXxWEmKjwTmJp97oVsUjg8_XxAWPwAUc5MlRhqa4sCBAeJPLL6XGBOVMlj09n2N7qkpWLU%2CAOrYGsnn98F8_ZdU0zATuXlXArPIJOkCfwbEPfrDHeTpZdd4sxRap7z2JdMajl1DlNttEB2EGfQWCau6pY5yDdcRTs3O3DG-rajYSJ0q05FpAQod&amp;pvsid=5989143209932568&amp;tmod=933744953&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1933%2C30%2C1933%2C30%2C1920%2C30%2C1920%2C954%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=3&amp;uci=a!3&amp;btvi=4&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

構成圖的喜好本來就帶有一點宗教性。  
（例如：元素要放哪裡、使用者需求是從上往下看，還是從左往右看等等……）

這次是出於我個人「做出漂亮的架構圖了！想分享給大家！」的心情來做驗證並寫成文章，所以如果你有自己的喜好，也可以自行客製化使用。

## 常見的煩惱

你是否也有以下這些煩惱或不滿？我以前常常這麼覺得。

- 想讓 AI 代理生成架構圖，但生成品質不好
- 明明也用了官方的 MCP、Skill、外掛，看起來好像不錯，但總覺得還差一點
- 最後還是不知道到底該用哪個工具（MCP、Skill 等）

因此這次我用 AWS 和 [draw.io](http://draw.io/) 官方提供的 MCP 與 Skill，再加上什麼都沒設定的 Claude Code，一共 5 種模式，用同一組指示來做架構圖。  
接著觀察生成結果中讓人介意的地方，逐步把那些內容加回指示裡，看看能改善到什麼程度。

## 比較的 5 種模式

這次比較的是以下 5 種模式：

## 使用的工具 輸出物 URL

1 AWS Diagram MCP Server 1.0.23（MCP・不建議） 只有 PNG [PyPI](https://pypi.org/project/awslabs.aws-diagram-mcp-server/)  
2 `aws-architecture-diagram` Skill（Skill） `.drawio` （如果有要求也可輸出 PNG、SVG、PDF） [GitHub](https://github.com/awslabs/agent-plugins/tree/main/plugins/deploy-on-aws/skills/aws-architecture-diagram)  
3 [draw.io](http://draw.io/) 官方 MCP Tool Server（MCP） 開啟 [draw.io](http://draw.io/) 的 URL（可改寫既有 `.drawio` ） [GitHub](https://github.com/jgraph/drawio-mcp/tree/main/mcp-tool-server)  
4 [draw.io](http://draw.io/) 官方 Claude Code 外掛（Skill） `.drawio` （如果有要求也可輸出 PNG、SVG、PDF、URL） [GitHub](https://github.com/jgraph/drawio-mcp/tree/main/plugins/claude-code)  
5 Claude Code（無 Skill・MCP） 依指示而定 [文件](https://code.claude.com/docs/ja/overview)

輸出 PNG 或 SVG 需要 [draw.io](http://draw.io/) desktop。  
這次除了 1 以外，其餘模式我都要求「以 [draw.io](http://draw.io/) 的 XML 格式保存成檔案」，所以 3 和 5 也輸出了 `.drawio` 。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1318457866&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790671324&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbd0a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669078846&amp;bpp=1&amp;bdt=1192&amp;idt=1&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790669078%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790669078%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790669078%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873%2C952x280%2C728x90%2C334x250%2C952x280%2C952x280&amp;nras=7&amp;correlator=2201155589796&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=5482&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=2017&amp;eid=95403098%2C95403358%2C95390278%2C122880836%2C122881195&amp;oid=2&amp;psts=AOrYGsl9JOaZFHlsfSPaQ8qwHByVl-DXBVOKZ8rOf0i8rO7yDILymsIsf4yDzggCFuXxWEmKjwTmJp97oVsUjg8_XxAWPwAUc5MlRhqa4sCBAeJPLL6XGBOVMlj09n2N7qkpWLU%2CAOrYGsnn98F8_ZdU0zATuXlXArPIJOkCfwbEPfrDHeTpZdd4sxRap7z2JdMajl1DlNttEB2EGfQWCau6pY5yDdcRTs3O3DG-rajYSJ0q05FpAQod%2CAOrYGsk801jUbT6r6SWL0Oc0Tc1H1DuobXoWhrde2tx99FWfsjwyxJYOuzKsR6fR2aAg6u1jZN7PPrQm67IcyNIf5v6B90JD5bxHBt0eIt-ul58sv5Q&amp;pvsid=5989143209932568&amp;tmod=933744953&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1933%2C30%2C1933%2C30%2C1920%2C30%2C1920%2C954%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=5&amp;uci=a!5&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

## 應達成的目標

如果要在業務中使用，我認為需要達成以下內容：

- 同樣的指示每次都能產生相似的架構圖（可重現性）
- 用較少的 token 和時間完成（性價比）
- 只畫出指示的內容，不要出現沒要求的編號或說明文字（對指示的忠實度）
- 線條與標籤不重疊，易於閱讀（可讀性）
- 沒有圓角圖示或多餘框線，不會有 AI 感（外觀）
- 生成後可手動修改的格式（`.drawio` ）（易編輯性）

主要就是從這 6 個面向，對包含官方 MCP 與 Skill 在內的 5 種模式做了架構圖比較。

## 測試時特別注意的事項

這次的驗證是比較多個 MCP 和 Skill 的生成結果。若已有使用者自訂的 Skill、外掛、CLAUDE.md（AGENTS.md），都可能影響輸出結果。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=312393960&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790671326&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbd0a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669078854&amp;bpp=1&amp;bdt=1200&amp;idt=1&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790669078%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790669078%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790669078%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873%2C952x280%2C728x90%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=8&amp;correlator=2201155589796&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=6315&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=3049&amp;eid=95403098%2C95403358%2C95390278%2C122880836%2C122881195&amp;oid=2&amp;psts=AOrYGsl9JOaZFHlsfSPaQ8qwHByVl-DXBVOKZ8rOf0i8rO7yDILymsIsf4yDzggCFuXxWEmKjwTmJp97oVsUjg8_XxAWPwAUc5MlRhqa4sCBAeJPLL6XGBOVMlj09n2N7qkpWLU%2CAOrYGsnn98F8_ZdU0zATuXlXArPIJOkCfwbEPfrDHeTpZdd4sxRap7z2JdMajl1DlNttEB2EGfQWCau6pY5yDdcRTs3O3DG-rajYSJ0q05FpAQod%2CAOrYGsk801jUbT6r6SWL0Oc0Tc1H1DuobXoWhrde2tx99FWfsjwyxJYOuzKsR6fR2aAg6u1jZN7PPrQm67IcyNIf5v6B90JD5bxHBt0eIt-ul58sv5Q%2CAOrYGsnWuyIVuoq6-JQ2cTNB9FYWd2zGkxQyIzVY70KWOsfkuUbmb4Ax1wErFWA7EFNIEVOU565aeOu9sjv4oRu7onD6XetINV2q2M65xkTjYJjm&amp;pvsid=5989143209932568&amp;tmod=933744953&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1933%2C30%2C1933%2C30%2C1920%2C30%2C1920%2C954%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

因此我只載入專案設定，並加上只使用指定 MCP 的選項來啟動（這部分是我一邊問 Claude Code 一邊調整的）。

如果不加 `--strict-mcp-config` ， [claude.ai](http://claude.ai/) 那邊連動的 MCP（例如 AWS 文件搜尋）會殘留著。

```
# AWS Diagram MCP Server、draw.io MCP Tool Server
claude --setting-sources project --strict-mcp-config --mcp-config mcp.json

# aws-architecture-diagram Skill
claude --setting-sources project --strict-mcp-config --plugin-dir agent-plugins/plugins/deploy-on-aws

# draw.io Claude Code 外掛
claude --setting-sources project --strict-mcp-config --plugin-dir drawio-mcp/plugins/claude-code

# Claude Code（無 Skill・MCP）
claude --setting-sources project --strict-mcp-config
```

[![以 --strict-mcp-config 啟動時 /context all 的結果。只有想比較的 MCP 被載入](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2Fa5657be5-2ba7-4a23-8c1b-66eeb6cf2db9.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=25b118dbbc7e008d82c89d380cf06846)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2Fa5657be5-2ba7-4a23-8c1b-66eeb6cf2db9.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=25b118dbbc7e008d82c89d380cf06846)

可以看到只有要比較的 MCP 被載入，Claude Code 內建的 Skill 則仍保留著。

一開始我也試過 `--safe-mode` 。這個選項會把 [CLAUDE.md](http://claude.md/) 、Skill、外掛、MCP 全部停用，但這樣連這次要比較的 MCP 和外掛也都無效了，所以不能用。

## 先讓它照普通方式畫看看

一開始我用下面這段指示來請它畫架構圖。

```
以下のAWS構成の構成図を作ってください。

- CloudFront の背後に ALB を置く
- ALB のターゲットは ECS Fargate のサービスで、2つのAZに分散する
- データベースは Aurora MySQL、Writer 1台とReader 1台を2つのAZに分ける
- 静的ファイルは S3 に置き、CloudFront から配信する
- ログとメトリクスは CloudWatch に集約する
- VPC は 2AZ 構成で、各AZにパブリックサブネットとプライベートサブネットを持つ
- ALB はパブリックサブネット、ECS と Aurora はプライベートサブネットに置く
- 各AZのパブリックサブネットに NAT Gateway を置き、ECS からの外向き通信を通す
- ラベルは日本語で書く。ただし AWS のサービス名は英語のままにする
- 図は draw.io の XML 形式で、out/ 配下にファイルとして保存する
```

AWS Diagram MCP Server 只能輸出 PNG，所以最後一行我改成「圖請以 PNG 形式保存到 out/ 底下」再送出。

這個指示在 5 種模式下執行後，生成了以下架構圖。  
裡面有多餘的文字、圖示角落圓圓的等等，若直接拿去業務使用會有問題，所以我覺得還需要再微調。

### AWS Diagram MCP Server（不建議）

線條很雜，看起來很難讀。  
圖示和子網路等的角也都偏圓，AI 感非常重。

[![沒有加入外觀指示時，AWS Diagram MCP Server 繪製的架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2Fdd66ae63-528b-4fc8-82fb-77d477df13c6.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5d318bcff73dae5788d9b9a83c6c5dac)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2Fdd66ae63-528b-4fc8-82fb-77d477df13c6.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=5d318bcff73dae5788d9b9a83c6c5dac)

AWS Diagram MCP Server 已經被標示為不建議使用，而且也停止更新了。  
這次是為了比較才拿來用，但不建議之後再使用。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3034205192&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1790671410&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbd0a&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669078879&amp;bpp=1&amp;bdt=1225&amp;idt=1&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790669078%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790669078%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790669078%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;prev_fmts=0x0%2C1680x873%2C952x280%2C728x90%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=2201155589796&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=189&amp;ady=10586&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=7106&amp;eid=95403098%2C95403358%2C95390278%2C122880836%2C122881195&amp;oid=2&amp;psts=AOrYGsl9JOaZFHlsfSPaQ8qwHByVl-DXBVOKZ8rOf0i8rO7yDILymsIsf4yDzggCFuXxWEmKjwTmJp97oVsUjg8_XxAWPwAUc5MlRhqa4sCBAeJPLL6XGBOVMlj09n2N7qkpWLU%2CAOrYGsnn98F8_ZdU0zATuXlXArPIJOkCfwbEPfrDHeTpZdd4sxRap7z2JdMajl1DlNttEB2EGfQWCau6pY5yDdcRTs3O3DG-rajYSJ0q05FpAQod%2CAOrYGsk801jUbT6r6SWL0Oc0Tc1H1DuobXoWhrde2tx99FWfsjwyxJYOuzKsR6fR2aAg6u1jZN7PPrQm67IcyNIf5v6B90JD5bxHBt0eIt-ul58sv5Q%2CAOrYGsnWuyIVuoq6-JQ2cTNB9FYWd2zGkxQyIzVY70KWOsfkuUbmb4Ax1wErFWA7EFNIEVOU565aeOu9sjv4oRu7onD6XetINV2q2M65xkTjYJjm%2CAOrYGslfvrwebwuwbrfZ6bEUMxgnh-CkG1JJI860hfLVenyzvE0tUOusmpDqRsbIBbuUF2WHGeNhyIiYiIRRHqYOEKhlUYpxKb-LKyZfP7ma9eO1r20&amp;pvsid=5989143209932568&amp;tmod=933744953&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1933%2C30%2C1933%2C30%2C1920%2C30%2C1920%2C954%2C1680%2C873&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.14&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

### aws-architecture-diagram Skill

加入了指示中沒有的方框數字，以及圖片右側的資料流。  
服務圖示周圍也有方框，看起來還是很有 AI 味。

[![沒有加入外觀指示時，aws-architecture-diagram Skill 繪製的架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F4e0582e2-97db-4d4c-931a-1a56bba2bf39.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=202ab56c78917c940304dd632529f61e)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F4e0582e2-97db-4d4c-931a-1a56bba2bf39.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=202ab56c78917c940304dd632529f61e)

### draw.io MCP Tool Server

和其他架構圖相比算不錯，但線條有重疊，閱讀性稍差。  
還有一些沒要求的文字（例如「ECS / Aurora は各AZのプライベートサブネットに配置」）也被寫進去了。

[![沒有加入外觀指示時，draw.io MCP Tool Server 繪製的架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F02c11681-ed2a-47a8-a526-9c10e6f83250.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=17e8ca8f51a17887d5f0cacc2ee0e342)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F02c11681-ed2a-47a8-a526-9c10e6f83250.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=17e8ca8f51a17887d5f0cacc2ee0e342)

### draw.io Claude Code 外掛

這個也比其他幾個看起來更好一些。  
不過線條角度有點圓，文字和線重疊的地方也有一些。

[![沒有加入外觀指示時，draw.io 的 Claude Code 外掛繪製的架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F8c542d87-480d-44f0-b214-8b5383b21eeb.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=dcd41fc6eb95d770411a56aea0faa992)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F8c542d87-480d-44f0-b214-8b5383b21eeb.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=dcd41fc6eb95d770411a56aea0faa992)

### Claude Code（無 Skill・MCP）

我也讓沒有設定 MCP 或 Skill 的 Claude Code 來生成，作為比較。  
整體其實整理得蠻乾淨，但圖示顏色有些怪異之處（例如 S3 是橘色），而且服務名稱同時寫在圖示上下兩側。

[![沒有加入外觀指示時，沒有 Skill 也沒有 MCP 的 Claude Code 繪製的架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F2aa42245-c7bf-41e0-b02e-04f148af510a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fa99236dce9fd7411914b952aa856b5d)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F2aa42245-c7bf-41e0-b02e-04f148af510a.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=fa99236dce9fd7411914b952aa856b5d)

## 一步步加進去的指示

我把同樣的指示丟給 5 種模式後，會查看生成出來的架構圖，把覺得不順眼的地方加回指示中。  
追加的內容大致如下。

| 覺得不對勁的地方 | 加進去的指示 |
| --- | --- |
| 文字和線重疊 | 不要讓線和標籤重疊 |
| 為了避免重疊，圖示被改成方框 | 改用 AWS 官方圖示，不要用方框代替 |
| ECS 還是方框 | 一定要使用圖示 |
| 文字放在圖外面 | 不要把文字放在圖外 |
| 線的標籤都消失了 | 線的標籤可以保留 |
| 子網路的上下順序與顏色提示都消失了 | 可接受附加在元素上的簡短補充說明 |
| AZ 被擅自寫成 A、C | 請寫正式的 AZ 名稱 |
| 使用者位置上上下下、左右不一 | 把使用者放在圖的最上方 |
| 新舊圖示混雜 | 圖示統一使用最新設計（但沒成功，最後的指示中拿掉了） |
| Aurora 變成了執行個體圖示 | 使用服務層級的圖示 |
| 標籤位置每次都不同 | 圖示標籤固定放在圖示下方 |
| 線從圖示的奇怪位置接出去 | 線從圖示邊框的中央引出 |

## 驗證結果

最後我是用下面這段指示來請它畫架構圖。

```
以下のAWS構成の構成図を作ってください。

- CloudFront の背後に ALB を置く
- ALB のターゲットは ECS Fargate のサービスで、2つのAZに分散する
- データベースは Aurora MySQL、Writer 1台とReader 1台を2つのAZに分ける
- 静的ファイルは S3 に置き、CloudFront から配信する
- ログとメトリクスは CloudWatch に集約する
- VPC は 2AZ 構成で、各AZにパブリックサブネットとプライベートサブネットを持つ
- アベイラビリティゾーンの名前は正式名称で書く。「A」「B」のような略記にしない
- ALB はパブリックサブネット、ECS と Aurora はプライベートサブネットに置く
- 利用者は図の上端に置く
- 各AZの中は、パブリックサブネットを上、プライベートサブネットを下に置く
- 各AZのパブリックサブネットに NAT Gateway を置き、ECS からの外向き通信を通す
- AWS 公式のアイコンを必ず使う。四角や丸で代用しない。サービス単位のアイコンを使い、インスタンスなど個別リソースのアイコンは使わない
- ラベルは日本語で書く。ただし AWS のサービス名は英語のままにする
- アイコンのラベルはアイコンの下に置く
- アイコンやラベルの文字に線を重ねない。ラベルの下から線を出すのはかまわない。線どうしを重ねない。交差するのはかまわない
- 線はできるだけアイコンの辺の中央から出す
- 図の外にタイトル、凡例、説明パネルを置かない。線のラベルや、要素に付く短い補足は入れてよい
- 図は draw.io の XML 形式で、out/ 配下にファイルとして保存する
```

順帶一提，同樣的指示執行 3 次時，時間與 token 的平均如下：

| 模式 | 時間 | 輸出 token | 可重現性 |
| --- | --- | --- | --- |
| AWS Diagram MCP Server（MCP） | 5.5 分 | 23,393 | 每次差異都蠻大 |
| `aws-architecture-diagram` Skill（Skill） | 5.4 分 | 33,510 | 變動時有點多 |
| [draw.io](http://draw.io/) MCP Tool Server（MCP） | 4.1 分 | 25,439 | 幾乎一樣。NAT Gateway 圖示每次不同 |
| [draw.io](http://draw.io/) Claude Code 外掛（Skill） | 5.8 分 | 34,544 | 幾乎一樣。S3 的位置會變 |
| Claude Code（無 Skill・MCP） | 5.4 分 | 32,805 | 幾乎一樣 |

### AWS Diagram MCP Server（不建議）

線條仍然是曲線，圖示角落也還是圓的，AI 感還是沒完全消除。  
而且只能輸出 PNG，後續沒辦法再修改不滿意的地方，這點也很麻煩。

[![以最終指示讓 AWS Diagram MCP Server 繪製的架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F239b6723-aa65-4b87-b57b-84f4d728b9e6.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2f7aa128f25aec0ef0a3cbf67145cb72)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F239b6723-aa65-4b87-b57b-84f4d728b9e6.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=2f7aa128f25aec0ef0a3cbf67145cb72)

### aws-architecture-diagram Skill

框線和配置整理得很漂亮。

不過和其他模式相比，線條還是偏少，像是到 CloudWatch 的線，以及 Aurora 的複寫都沒有畫出來。  
NAT Gateway 的圖示變成 VPC 圖示也讓人有點在意（也可能是我的指示下得不好）。

[![以最終指示讓 aws-architecture-diagram Skill 繪製的架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2Fd538cf5a-4972-4ddd-89da-f042f12de941.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b3b9cfb2b49c800b8e23d884c295f348)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2Fd538cf5a-4972-4ddd-89da-f042f12de941.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=b3b9cfb2b49c800b8e23d884c295f348)

### draw.io MCP Tool Server

線條很俐落，我個人相當喜歡。  
不過這裡 NAT Gateway 的圖示也變成了 VPC 圖示，還差一點點。

[![以最終指示讓 draw.io MCP Tool Server 繪製的架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F96cda8b0-5576-49ae-8350-37218427feea.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6a18bfc667574e84b344d1cfc027b9c9)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F96cda8b0-5576-49ae-8350-37218427feea.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=6a18bfc667574e84b344d1cfc027b9c9)

### draw.io Claude Code 外掛

圖示一致，資訊量也剛剛好，我覺得不錯。

[![以最終指示讓 draw.io 的 Claude Code 外掛繪製的架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F5d2911e5-f02d-4912-9151-7834f5dbeaf1.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bfbc73d1a022f1d142dcc6ad2a86d038)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F5d2911e5-f02d-4912-9151-7834f5dbeaf1.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=bfbc73d1a022f1d142dcc6ad2a86d038)

### Claude Code（無 Skill・MCP）

明明什麼都沒加，卻沒有線條交叉，而且即使重複多次生成架構圖，差異也是最小的。

[![以最終指示讓沒有 Skill 也沒有 MCP 的 Claude Code 繪製的架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F1b825ddc-c4f3-4e6c-a6a7-63374292113d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=da1ff190d36b4e4daf35a89770dbc3c4)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F1b825ddc-c4f3-4e6c-a6a7-63374292113d.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=da1ff190d36b4e4daf35a89770dbc3c4)

## 結論

只要逐步補上指示，各種模式都比一開始好很多了！  
就我個人而言， [draw.io](http://draw.io/) MCP、draw.io 外掛、Claude Code 這三種模式都表現得不錯。

與其糾結到底該用哪個官方 MCP 或 Skill，更重要的是要有明確的指示，像是「我想要這種架構圖，請考慮這些點來畫」。

寫程式也是一樣，指示不夠時，就會冒出多餘的註解或過度實作。架構圖也是如此。

……話雖如此，每次都要寫這麼長的指示，不會很麻煩嗎？

## 補充

所以我把這次一路加進去的指示，整理成一個 Skill 了！  
如果有興趣可以試試看。

用這個 Skill 生成與本文相同的需求時，會變成這樣。

[![以本文相同需求讓自製 Skill 繪製的 AWS 架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F3d34db36-d00b-4657-916b-7f3bf7bab588.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=44bf949a58fa87e59676de0741d2b1de)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F3d34db36-d00b-4657-916b-7f3bf7bab588.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=44bf949a58fa87e59676de0741d2b1de)

也可以畫沒有 VPC 的架構，例如 Amazon Bedrock AgentCore 和 Lambda Web Adapter 的範例。

[![自製 Skill 繪製的 AgentCore 與 Lambda Web Adapter 架構圖](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F386ea134-d006-4c06-a621-4a5b77c61114.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ce1ac5fc801d3118193a6f37dd47d492)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2743663%2F386ea134-d006-4c06-a621-4a5b77c61114.png?ixlib=rb-4.1.1&auto=format&gif-q=60&q=75&s=ce1ac5fc801d3118193a6f37dd47d492)

大家在讓 AI 代理畫架構圖時，都會下什麼指示呢？如果能在留言告訴我，我會很開心！

本文中的架構圖使用的是 [AWS Architecture Icons](https://aws.amazon.com/architecture/icons/) 。

---

原文出處： [https://qiita.com/sagochiko/items/ef77b084ff2dee1f859c](https://qiita.com/sagochiko/items/ef77b084ff2dee1f859c)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/anbd0a) [23瀏覽](https://codelove.tw/@tony/post/anbd0a)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/anbd0a)

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

🔥 [話題中的 Jev 是什麼？簡要解說一種不撰寫文章、只回傳判斷結果的 AI 模型](https://codelove.tw/@tony/post/anbj0a)

🔥 [Codex 怎麼買、怎麼充值？先把這兩套計費搞清楚](https://codelove.tw/@tony/post/q44PLq)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝59 💬2

850

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

> [!dark] Dark
> [⭐️ 站長新作品｜川川電商：中小企業電商架構、設計與開發工法 ➡️ 前往看看](https://chuanchuan.tw/)

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1790669078&amp;plaf=7%3A2&amp;plat=1%3A16777216%2C3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2Fanbd0a&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1NC4wLjgwMzcuNTgiLG51bGwsMCxudWxsLCI2NCIsW1siQ2hyb21pdW0iLCIxNTQuMC44MDM3LjU4Il0sWyJHb29nbGUgQ2hyb21lIiwiMTU0LjAuODAzNy41OCJdLFsiTm90IEEoQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;dt=1790669078066&amp;bpp=17&amp;bdt=429&amp;idt=85&amp;shv=r20260928&amp;mjsv=m202609210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3Db934e6665c1b4184%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MY9HFNaAMzOc50bDWLpTMau_opL7w&amp;gpic=UID%3D000015717d63c1dd%3AT%3D1789457068%3ART%3D1790650089%3AS%3DALNI_MZ0CzPil6jV5KhaXFobHcQlbYl26Q&amp;eo_id_str=ID%3Dec30b0affb026999%3AT%3D1789457068%3ART%3D1790650089%3AS%3DAA-AfjZ033CkCoxpw7LBLBwCqnCB&amp;nras=1&amp;correlator=2201155589796&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=873&amp;scr_x=0&amp;scr_y=0&amp;eid=95403098%2C95403358%2C95390278%2C122880836%2C122881195&amp;oid=2&amp;pvsid=5989143209932568&amp;tmod=933744953&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C873&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=134" title="Advertisement" aria-label="Advertisement"></iframe>