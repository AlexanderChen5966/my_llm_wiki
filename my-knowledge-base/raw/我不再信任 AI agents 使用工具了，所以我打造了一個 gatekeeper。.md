---
title: "我不再信任 AI agents 使用工具了，所以我打造了一個 gatekeeper。"
source: "https://codelove.tw/@tony/post/31oZ8q"
author:
published:
created: 2026-08-17
description: "> [github.com/deghosal-2026/agent-tooltrust](https://github.com/deghosal-2026/agent-tooltrust) · `pip install agent-tooltrust` · [實地測試報告](https://gith..."
tags:
  - "clippings"
status: "processed"
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [08月15日](https://codelove.tw/@tony/post/31oZ8q "2026-08-15 08:05:51")

## 我不再信任 AI agents 使用工具了，所以我打造了一個 gatekeeper。

> [github.com/deghosal-2026/agent-tooltrust](https://github.com/deghosal-2026/agent-tooltrust) · `pip install agent-tooltrust` · [實地測試報告](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/field-test/FIELD_TEST_REPORT.md) · [設計決策](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/design/design-decisions.md)

## 快速備註 - 我從一開始就把實地測試放在心上

我前面三個專案教了我同一件事。模擬代理會說謊。單元測試會通過。Demo 看起來很乾淨。然後真正的代理一跑起來，一切都壞掉了。

在我的 [eval harness](https://dev.to/debashish_ghosal/i-built-an-agent-eval-harness-real-agents-broke-the-clean-version-of-the-story-53dj) 上，我承認了這件事：實地測試「是臨時、很晚才加進來的，因為我開始擔心單元測試和模擬代理掩蓋了真正的整合問題。」在我的可觀測性工具上，我寫的是：「我以為是偵測器的問題。我錯了。」

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1718674035&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786936818&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F31oZ8q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932858174&amp;bpp=4&amp;bdt=1469&amp;idt=4&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936692%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936692%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936692%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908&amp;nras=3&amp;correlator=6432990981627&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1021&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95398731%2C95399506%2C122880367%2C122880369%2C42533294&amp;oid=2&amp;pvsid=7060619402679603&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=2&amp;uci=a!2&amp;btvi=1&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

同樣的教訓。三次。但教訓只有在你改變下一步怎麼做時才有意義。

所以這次我反著來。零模擬代理。涵蓋 10 個框架、共 83 個真實代理。一個涵蓋式設計，把原本 12 天的測試矩陣壓縮成一個下午。還有一個發布門檻：在真實代理證明政策有效之前，不准發版。

結果成功了。2,490 項測試全綠。83/83 個代理通過。PyPI 已發布。Repo 已公開。而那 7 個失敗案例，教會了我一些我無法用其他方式學到的東西。

## 白名單的問題

大家都在競相替 AI 代理提供更多工具。幾乎沒有人在打造決定這些工具何時該觸發的權限系統。

目前，代理權限只有二元：允許或拒絕。那是可達性，不是授權。同一個工具在測試環境無害，在正式環境卻很危險。同一個讀取動作，在公開文件上沒問題，在客戶資料上卻很敏感。CI 沙箱裡的 `delete` 和正式環境裡的 `delete` 不是同一件事。

大約 18% 的 MCP 伺服器部署實作了某種存取範圍控管。80% 的組織承認代理曾經做出超出預期範圍的操作。OWASP 把代理工具濫用列為一級風險。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3764087313&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786936818&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F31oZ8q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932858187&amp;bpp=1&amp;bdt=1481&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936692%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936692%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936692%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280&amp;nras=4&amp;correlator=6432990981627&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=1849&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95398731%2C95399506%2C122880367%2C122880369%2C42533294&amp;oid=2&amp;pvsid=7060619402679603&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=3&amp;uci=a!3&amp;btvi=2&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

給代理工具很容易。難的是決定它應該被允許做什麼、在哪裡做、以及要在什麼保護機制下做。我在碰引擎程式碼之前，先寫了 [PRD](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/design/PRD.md) 和 [架構規格](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/architecture/architecture-v0.1.0.md) ——一方面是為了讓自己誠實面對問題，一方面是因為我已經從錯誤中學到，跳過設計只會導致你把錯的東西發佈出去。

## 我做了什麼

Agent ToolTrust 是一個情境式風險與權限引擎。在代理的工具呼叫執行之前，引擎會跑一個五階段流程——正規化、評分、決策、解釋、稽核——然後回傳四種決策之一：允許、稽核、升級處理或拒絕。

```python
from agent_tooltrust.engine.engine import Engine
from agent_tooltrust.policy.models import default_policy
from agent_tooltrust.adapters.raw import RawAdapter

engine = Engine(default_policy("balanced"))
adapter = RawAdapter(engine)

@adapter.guard(
    tool_name="deploy_service",
    action="deploy",
    environment="production",
    data_class="restricted",
)
def deploy_service(service: str) -> str:
    return f"deployed {service}"

# 代理呼叫工具。引擎先評估。
# 在正式環境中部署受限資料 → 升級處理
deploy_service("payment-api")
# ToolTrustDecisionError: escalate — "在正式環境中對受限資料執行寫入動作（deploy）
# 需要核准..."
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=244472045&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786936818&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F31oZ8q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932858210&amp;bpp=1&amp;bdt=1504&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936692%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936692%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936692%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280&amp;nras=5&amp;correlator=6432990981627&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=2974&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95398731%2C95399506%2C122880367%2C122880369%2C42533294&amp;oid=2&amp;pvsid=7060619402679603&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=0&amp;ifi=4&amp;uci=a!4&amp;btvi=3&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

裝飾器就是整合點。代理呼叫工具。引擎攔截、評估，然後放行、稽核、升級給人處理，或直接拒絕。代理永遠看不到政策。LLM 也不知道規則存在。

引擎是決定性的。LLM 負責提案，政策負責裁決。再怎麼調提示詞，都不能推翻拒絕——因為引擎是在模型外部，而不是藏在提示詞裡。

不是兩種，而是四種決策。 `allow` 和 `deny` 很直觀。 `audit` 表示「允許，但全部記錄——這是對敏感資料的讀取。」 `escalate` 表示「先停下來，找人核准。」二元的允許/拒絕會逼你在過度授權的代理和審核疲勞之間二選一。四種狀態提供了中間地帶。

每個決策都會附帶解釋——包含原因程式碼、人類可讀句子，以及顯示是哪個維度驅動這次判定的因素拆解。可選的 LLM 文案預設關閉。LLM 無法改變決策。

每個決策都會被稽核——JSONL、SQLite 或 Postgres，包含政策版本、時間戳記與 session ID。

預設就附帶三種安全姿態——strict、balanced、permissive——所以沒有人要從空白檔案開始。也提供給人類用的 YAML 政策後端，以及給已經有 Rego 政策的團隊使用的 OPA/Rego 後端。還有 shadow mode，所以你可以先部署、觀察原本會被拒絕的內容、調整，然後再正式強制執行——而且不用改代理程式碼。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=186534417&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786937026&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F31oZ8q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932858218&amp;bpp=1&amp;bdt=1516&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786936692%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786936692%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786936692%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x272%2C334x250&amp;nras=7&amp;correlator=6432990981627&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=3818&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=213&amp;eid=95397679%2C95398731%2C95399506%2C122880367%2C122880369%2C42533294&amp;oid=2&amp;psts=AOrYGsmKV3n1FsOnZtN7KcDVICFYjz6vVsUMbAUH_XjgWmiBjrw5v-mEs1hEoJm6UxtSsgRtE7pqPRJ0GWPuwOl5lauf6WuJdkxVjBMzXyFEpxrbOpl_UpxrZstJmokyw8WZ%2CAOrYGsnfvLtOqtVIUAIbD8fUyLjKN4phKkBUHkMsPu8xmNgtWtYUZmWo_cfpvy8G0uYJuuz-fPmZcPdDj4tTBvIdOVJu01CODipoR87KyxnITsj0%2CAOrYGsmf0yeVEMM5xd4dtSJm-Jb0ziBrVxSbT2DURvYrvpZHbVxNO4DI4nhzeZPLMXPPNlDJpVHzbTV3fd1WAc6qFyzbYbf-ZwRFN4aTwjG6Cng%2CAOrYGsl7lw2MiWit-qUcD6--0iZit6Rw8DWfwI--2T2byn4m-2o1CuGcwVp5KfLmNzZhE2gBkDYSx4dfdQwT-KxnWybWpQaQWOyG7v35s5bk9zyE%2CAOrYGsmDj9-Fk5cUQkaW8RgC6l-cFzwKkukZpfIAxLpOhZUT_-rMTLwOBQ7oIY8NwXXWHF162qymEHTozU-gmuIxUsKTYWY7Joo8JqcKPl9Aay1t&amp;pvsid=7060619402679603&amp;tmod=144435645&amp;uas=3&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=5&amp;uci=a!5&amp;btvi=6&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

全程 fail-closed。未知工具 → 拒絕。輸入格式錯誤 → 拒絕。引擎當掉 → 拒絕。替代方案是 fail-open，也就是攻擊者只要讓引擎崩潰，就能無限制存取工具。這是 [設計決策 DD-14](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/design/design-decisions.md) ——在第一行程式碼之前就先寫下來，而不是在差點出事之後才補的。

這就是架構。但架構最簡單。真正的問題是：當真實代理真的拿它來用時，它還有效嗎？

## 這次，我把學到的東西用上了

在前幾個專案裡，實地測試是我跳過、最後又後悔的那一步。在 EvalForge 上，我很晚才加上它，結果發現通過率只有 9%——不是因為工具不好，而是因為模擬代理把所有整合問題都藏起來了。在 AgentObservatory 上，我學到的是「出問題的是整合，不是裁判。」

這次，我在寫任何 adapter 程式碼之前，就把它寫進規格裡了。 [DD-11](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/design/design-decisions.md) ：「任何發版前都必須通過實地測試。測試使用真實代理，不用模擬。」DD-12：「跨主要平台使用 8 到 10 個真實代理。」

我做得比這兩條都更多。不是 8 到 10 個代理，而是涵蓋 10 個框架、共 83 個真實代理。而且 [實地測試計畫](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/field-test/field-test-plan.md) 從第一天就放進了 WBS。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1038984369&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786937216&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F31oZ8q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932858229&amp;bpp=1&amp;bdt=1531&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786937026%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786937026%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786937026%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x272%2C334x250%2C952x280&amp;nras=8&amp;correlator=6432990981627&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=4631&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=1018&amp;eid=95397679%2C95398731%2C95399506%2C122880367%2C122880369%2C42533294&amp;oid=2&amp;psts=AOrYGsmKV3n1FsOnZtN7KcDVICFYjz6vVsUMbAUH_XjgWmiBjrw5v-mEs1hEoJm6UxtSsgRtE7pqPRJ0GWPuwOl5lauf6WuJdkxVjBMzXyFEpxrbOpl_UpxrZstJmokyw8WZ%2CAOrYGsnfvLtOqtVIUAIbD8fUyLjKN4phKkBUHkMsPu8xmNgtWtYUZmWo_cfpvy8G0uYJuuz-fPmZcPdDj4tTBvIdOVJu01CODipoR87KyxnITsj0%2CAOrYGsmf0yeVEMM5xd4dtSJm-Jb0ziBrVxSbT2DURvYrvpZHbVxNO4DI4nhzeZPLMXPPNlDJpVHzbTV3fd1WAc6qFyzbYbf-ZwRFN4aTwjG6Cng%2CAOrYGsl7lw2MiWit-qUcD6--0iZit6Rw8DWfwI--2T2byn4m-2o1CuGcwVp5KfLmNzZhE2gBkDYSx4dfdQwT-KxnWybWpQaQWOyG7v35s5bk9zyE%2CAOrYGsmDj9-Fk5cUQkaW8RgC6l-cFzwKkukZpfIAxLpOhZUT_-rMTLwOBQ7oIY8NwXXWHF162qymEHTozU-gmuIxUsKTYWY7Joo8JqcKPl9Aay1t%2CAOrYGsn5ZJ8zOv1HmSlRc5Pm1CthiqW--R5FQrI3Fp_sl7IVTpJ_rnN-u97HfIP0d17BZOod5zEcQ0kqkcIMW3PqLoxk9cPd9dl5E3D3Z99TRmAYHw&amp;pvsid=7060619402679603&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=6&amp;uci=a!6&amp;btvi=7&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這就是「學到教訓」和「把教訓用上」的差別。

## 建立 Adapter 是探索性的過程

我想讓它能在真實的代理生態系統中運作，而不只是某個我剛好熟悉的框架。所以我為 10 個框架做了 adapter：

**LangGraph、PydanticAI、CrewAI、OpenAI Agents SDK、Google ADK、AutoGen/AG2、LlamaIndex、smolagents、SWE-bench（自我測試）、ToolTrust MCP** （自我測試）。

每個 adapter 都遵守同一個合約——擷取 `CallContext` ，傳給 `Engine.evaluate()` ，再把決策回傳：

```python
@dataclass(frozen=True)
class CallContext:
    tool_name: str
    action: str
    environment: str
    data_class: str
    agent_id: str
    session_id: str | None = None
    arguments: dict[str, Any] | None = None
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=1621299728&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786937220&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F31oZ8q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932858237&amp;bpp=1&amp;bdt=1539&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786937026%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786937026%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786937026%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x272%2C334x250%2C952x280%2C952x280&amp;nras=9&amp;correlator=6432990981627&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=5522&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=1898&amp;eid=95397679%2C95398731%2C95399506%2C122880367%2C122880369%2C42533294&amp;oid=2&amp;psts=AOrYGsmKV3n1FsOnZtN7KcDVICFYjz6vVsUMbAUH_XjgWmiBjrw5v-mEs1hEoJm6UxtSsgRtE7pqPRJ0GWPuwOl5lauf6WuJdkxVjBMzXyFEpxrbOpl_UpxrZstJmokyw8WZ%2CAOrYGsnfvLtOqtVIUAIbD8fUyLjKN4phKkBUHkMsPu8xmNgtWtYUZmWo_cfpvy8G0uYJuuz-fPmZcPdDj4tTBvIdOVJu01CODipoR87KyxnITsj0%2CAOrYGsmf0yeVEMM5xd4dtSJm-Jb0ziBrVxSbT2DURvYrvpZHbVxNO4DI4nhzeZPLMXPPNlDJpVHzbTV3fd1WAc6qFyzbYbf-ZwRFN4aTwjG6Cng%2CAOrYGsl7lw2MiWit-qUcD6--0iZit6Rw8DWfwI--2T2byn4m-2o1CuGcwVp5KfLmNzZhE2gBkDYSx4dfdQwT-KxnWybWpQaQWOyG7v35s5bk9zyE%2CAOrYGsmDj9-Fk5cUQkaW8RgC6l-cFzwKkukZpfIAxLpOhZUT_-rMTLwOBQ7oIY8NwXXWHF162qymEHTozU-gmuIxUsKTYWY7Joo8JqcKPl9Aay1t%2CAOrYGsn5ZJ8zOv1HmSlRc5Pm1CthiqW--R5FQrI3Fp_sl7IVTpJ_rnN-u97HfIP0d17BZOod5zEcQ0kqkcIMW3PqLoxk9cPd9dl5E3D3Z99TRmAYHw%2CAOrYGskJaGoFK0px7NbOCN58o0iiC_W_PScCnIwPizFmSqh_DeVzjAuJ5KCDqFgzXxuGGAo2S5xiegXX3e7uRYa2HWR4gRyRgL02PkBL80_LHJk&amp;pvsid=7060619402679603&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=7&amp;uci=a!7&amp;btvi=8&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

這個合約很乾淨。但要走到那一步並不容易。

每個框架對工具如何註冊、如何呼叫，以及錯誤如何拋出，都有自己的想法。我會先寫 adapter，拿去跑真實代理，看它在某個框架特有的地方失敗，修掉，再重來一次。每個失敗都讓我更了解那個框架實際怎麼運作——不是文件怎麼寫，而是當真實代理在驅動它時，它實際上怎麼表現。完整的各框架接線筆記在 [實地測試報告的 §5](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/field-test/FIELD_TEST_REPORT.md) ——共 12 個獨立發現。

LangGraph 的 `ToolTrustToolNode` 繼承自 `ToolNode` ，並覆寫 `_run_one()` 。但在 langgraph v1.x 裡，這個 node 不能直接呼叫——所以我改成在工具進入 graph 之前先包裝它：

```python
# LangGraph — 先包裝工具，再交給 graph
adapter = RawAdapter(engine)
guarded_tool = adapter.guard(
    tool_name="query_logs",
    action="read",
    environment="staging",
    data_class="internal",
)(query_logs_fn)

# 接著把 guarded_tool 交給 create_react_agent(llm, tools=[guarded_tool])
```

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3970593978&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786937221&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F31oZ8q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932858245&amp;bpp=1&amp;bdt=1547&amp;idt=1&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786937026%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786937026%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786937026%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x272%2C334x250%2C952x280%2C952x280%2C952x280&amp;nras=10&amp;correlator=6432990981627&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=6402&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=2806&amp;eid=95397679%2C95398731%2C95399506%2C122880367%2C122880369%2C42533294&amp;oid=2&amp;psts=AOrYGsmKV3n1FsOnZtN7KcDVICFYjz6vVsUMbAUH_XjgWmiBjrw5v-mEs1hEoJm6UxtSsgRtE7pqPRJ0GWPuwOl5lauf6WuJdkxVjBMzXyFEpxrbOpl_UpxrZstJmokyw8WZ%2CAOrYGsnfvLtOqtVIUAIbD8fUyLjKN4phKkBUHkMsPu8xmNgtWtYUZmWo_cfpvy8G0uYJuuz-fPmZcPdDj4tTBvIdOVJu01CODipoR87KyxnITsj0%2CAOrYGsmf0yeVEMM5xd4dtSJm-Jb0ziBrVxSbT2DURvYrvpZHbVxNO4DI4nhzeZPLMXPPNlDJpVHzbTV3fd1WAc6qFyzbYbf-ZwRFN4aTwjG6Cng%2CAOrYGsl7lw2MiWit-qUcD6--0iZit6Rw8DWfwI--2T2byn4m-2o1CuGcwVp5KfLmNzZhE2gBkDYSx4dfdQwT-KxnWybWpQaQWOyG7v35s5bk9zyE%2CAOrYGsmDj9-Fk5cUQkaW8RgC6l-cFzwKkukZpfIAxLpOhZUT_-rMTLwOBQ7oIY8NwXXWHF162qymEHTozU-gmuIxUsKTYWY7Joo8JqcKPl9Aay1t%2CAOrYGsn5ZJ8zOv1HmSlRc5Pm1CthiqW--R5FQrI3Fp_sl7IVTpJ_rnN-u97HfIP0d17BZOod5zEcQ0kqkcIMW3PqLoxk9cPd9dl5E3D3Z99TRmAYHw%2CAOrYGskJaGoFK0px7NbOCN58o0iiC_W_PScCnIwPizFmSqh_DeVzjAuJ5KCDqFgzXxuGGAo2S5xiegXX3e7uRYa2HWR4gRyRgL02PkBL80_LHJk%2CAOrYGsnYJgYUe-GxM8wW4P-rNKAPW5JeXY3jBbOoYa_lQ120kNqArn1r06Qc-reIGh-iF96khkhoNatoujp8Nz5IgGD0Iy-1rLiAMl2qpwaD6TAgIg&amp;pvsid=7060619402679603&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=8&amp;uci=a!8&amp;btvi=9&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

Google ADK 的 LLM registry 只認 Gemini。若要用本機模型，你要傳入 `LiteLlm(model=f"openai/{MODEL}", api_base=ENDPOINT)` 。而 `InMemorySessionService.create_session()` 是一個 coroutine——你得 `await` 它，不能同步呼叫。文件沒提到這件事。執行時環境會教你。

LlamaIndex 舊版的 `ReActAgent` 沒有 `.query()` 或 `.chat()` 。你需要用 `llama_index.core.agent.workflow` 裡的 workflow agent。而且執行是由 `async for event in handler.stream_events()` 來驅動——單獨 `await handler` 不會產生任何東西。 `async for` 才是讓代理往前跑的關鍵。沒有它，代理會默默什麼都不做。我花了一個小時才搞懂這個。

AutoGen 需要把 agent ID 裡的連字號清掉（ `ag-01` → `ag_01` ）。本地 Qwen 模型會以文字回答，除非你明確告訴它：「你必須精確呼叫名為 `scn_<id>` 的工具。不要跳過工具呼叫。」

smolagents 要求每個 `@tool` 都要有完整 docstring，並且每個參數都要寫說明——不然它會丟出 `DocstringParsingException` 。CrewAI 需要 `litellm` 當 fallback。OpenAI Agents SDK 需要 `function_tool(..., strict_mode=False)` 才能修掉一個 pydantic 衝突。

這些問題在模擬代理上完全看不到。只有在你跑來自真實 repo 的真實程式碼時，它們才會浮現。而我修掉的每一個問題，都讓 adapter 變得更強。

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" width="952" height="280" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?gdpr=0&amp;client=ca-pub-1009112352200321&amp;output=html&amp;h=280&amp;num_ads=1&amp;adk=661613056&amp;adf=3355444817&amp;abgtt=6&amp;w=952&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1786937221&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=3933869671&amp;ad_type=text_image&amp;format=952x280&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F31oZ8q&amp;fwr=0&amp;pra=3&amp;rh=200&amp;rw=952&amp;rpe=1&amp;resp_fmts=3&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;fa=27&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932858251&amp;bpp=1&amp;bdt=1553&amp;idt=0&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786937026%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786937026%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786937026%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;prev_fmts=0x0%2C1634x908%2C952x280%2C952x280%2C952x280%2C1005x272%2C334x250%2C952x280%2C952x280%2C952x280%2C952x280&amp;nras=11&amp;correlator=6432990981627&amp;frm=20&amp;pv=1&amp;u_tz=480&amp;u_his=2&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=166&amp;ady=7266&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=3697&amp;eid=95397679%2C95398731%2C95399506%2C122880367%2C122880369%2C42533294&amp;oid=2&amp;psts=AOrYGsmKV3n1FsOnZtN7KcDVICFYjz6vVsUMbAUH_XjgWmiBjrw5v-mEs1hEoJm6UxtSsgRtE7pqPRJ0GWPuwOl5lauf6WuJdkxVjBMzXyFEpxrbOpl_UpxrZstJmokyw8WZ%2CAOrYGsnfvLtOqtVIUAIbD8fUyLjKN4phKkBUHkMsPu8xmNgtWtYUZmWo_cfpvy8G0uYJuuz-fPmZcPdDj4tTBvIdOVJu01CODipoR87KyxnITsj0%2CAOrYGsmf0yeVEMM5xd4dtSJm-Jb0ziBrVxSbT2DURvYrvpZHbVxNO4DI4nhzeZPLMXPPNlDJpVHzbTV3fd1WAc6qFyzbYbf-ZwRFN4aTwjG6Cng%2CAOrYGsl7lw2MiWit-qUcD6--0iZit6Rw8DWfwI--2T2byn4m-2o1CuGcwVp5KfLmNzZhE2gBkDYSx4dfdQwT-KxnWybWpQaQWOyG7v35s5bk9zyE%2CAOrYGsmDj9-Fk5cUQkaW8RgC6l-cFzwKkukZpfIAxLpOhZUT_-rMTLwOBQ7oIY8NwXXWHF162qymEHTozU-gmuIxUsKTYWY7Joo8JqcKPl9Aay1t%2CAOrYGsn5ZJ8zOv1HmSlRc5Pm1CthiqW--R5FQrI3Fp_sl7IVTpJ_rnN-u97HfIP0d17BZOod5zEcQ0kqkcIMW3PqLoxk9cPd9dl5E3D3Z99TRmAYHw%2CAOrYGskJaGoFK0px7NbOCN58o0iiC_W_PScCnIwPizFmSqh_DeVzjAuJ5KCDqFgzXxuGGAo2S5xiegXX3e7uRYa2HWR4gRyRgL02PkBL80_LHJk%2CAOrYGsnYJgYUe-GxM8wW4P-rNKAPW5JeXY3jBbOoYa_lQ120kNqArn1r06Qc-reIGh-iF96khkhoNatoujp8Nz5IgGD0Iy-1rLiAMl2qpwaD6TAgIg%2CAOrYGsn1IYmg9yrbbkMwe9j3_1MFWLLeVX_ioN9LB0UN7cQ9x3x2k5TlhCgm0oy_geWjpFwLS6MGb9uQtIpC4TW7Zya24Kv7jTDBc0ZmIdbsFQAv2oE&amp;pvsid=7060619402679603&amp;tmod=144435645&amp;uas=1&amp;nvt=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1408&amp;brdim=1938%2C31%2C1938%2C31%2C1920%2C30%2C1874%2C989%2C1634%2C908&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=128&amp;bc=31&amp;bz=1.15&amp;ifi=9&amp;uci=a!9&amp;btvi=10&amp;fsb=1&amp;dtd=M" title="Advertisement" aria-label="Advertisement"></iframe>

最後，10 個框架都完成了建置、記錄了決策，並把真實代理送進引擎跑了一輪。10 個框架，攔截點都被證明有效，而不是停留在理論上。

## 83 個真實代理、30 個情境、零模擬

我從 GitHub 找來 83 個真實代理。不是玩具範例——而是真實 repo，有真實相依性、真實封裝方式、以及對如何呼叫 LLM 的真實偏好。

我寫了 30 個情境：20 個決策情境，涵蓋 5 類代理（ci-bot、engineer、general、analyst、sensitive）的所有四種決策類型；再加上 10 個對抗情境——prompt injection、Unicode 混淆、重放攻擊、空白工具名稱、格式錯誤輸入、繞過授權嘗試。完整矩陣在 [實地測試報告](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/field-test/FIELD_TEST_REPORT.md) 裡——每個代理、每個情境、每個預期與實際決策都列出來了。

算式是：83 個代理 × 30 個情境 = 2,490 次執行。每次執行都會呼叫本地 LLM——透過 OMLX 在 Apple Silicon 上跑 `Qwen3.5-4B-4bit` 。每次呼叫要 30 到 80 秒。大約是 10 個 worker 跑 2.7 小時。

但 2,490 只是理論最低值。實務上你還得除錯。adapter 會壞。代理無法匯入。LLM 會用文字回答而不是呼叫工具。你修正、重跑、再修正。實際的 LLM 呼叫次數是理論值的 4 到 5 倍——超過 10,000 次呼叫到一個本地 4B 模型。

這就是零模擬代理的代價。我願意再付一次。

模擬代理不需要 LLM。它們不會花 80 秒。它們不會帶著 ABI 不相容的 C extension。它們不會在模組層級硬寫 API key。它們也不會在 import 的時候往 `/root` 寫東西。

真實代理會做這些事。而每一個這樣的失敗，都是如果我用了模擬代理就會被帶去上線的 bug。

## 把 12 天壓成 1 天

接下來我開始停止硬幹。讓 2,490 次、透過本地 4B 模型跑出的測試，去重新證明 deterministic tests 已經涵蓋的東西，根本沒有意義。引擎正確性早就驗證過了——2,490 個斷言，零 LLM 呼叫，100% 綠燈。引擎與框架無關。 `Engine.evaluate()` 不在乎呼叫者是 LangGraph 還是 CrewAI。把每一格都重跑一遍是重複浪費。

實地測試真正的任務是驗證 adapter——每個框架是否能在真實代理迴圈中正確呈現 allow、audit、escalate、deny？這是涵蓋問題，不是笛卡兒積問題。

所以我把它拆成兩個計畫。

**Plan A — 每個代理一個情境（83 次）。** 每個代理剛好對應一個情境。分配結果涵蓋全部 30 個情境、10 個框架、5 類代理。結果： **83/83，100%。**

**Plan B — 每個框架的決策類型驗證（123 次）。** 每個框架都用一個 tier-1 代理跑完全部 4 種決策類型，再加上對抗情境。結果： **116/123，94%。**

合計： **206 次，而不是 2,490 次。覆蓋範圍相同——30/30 個情境、83/83 個代理、10/10 個框架、5/5 個類別。約縮減 12 倍。** 完整的覆蓋理由在 [實地測試報告的 §8](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/field-test/FIELD_TEST_REPORT.md) 。

這是我最自豪的部分。不是引擎——那很直接。是涵蓋式設計。是理解到，昂貴的 LLM 呼叫應該用在只有真實代理才能證明的部分，而不是拿來重新證明 deterministic tests 已經覆蓋的內容。

完整的交叉乘積本來會花大約 12 天。涵蓋式設計只花了一個下午。同樣的信心。以前的專案裡，我不是跳過實地測試，就是硬幹到時間不夠。這次，我做了最佳化。

## 那 7 個失敗教會了我什麼

Plan B 裡那 7 個失敗案例，是實地測試最有價值的部分。不是因為它們把東西弄壞了，而是因為它們揭露了模擬代理永遠抓不到的東西。

這 7 個案例都有同一個型態： `not-available` 。guard 根本沒有觸發，因為 LLM 沒有呼叫工具。本地 Qwen 模型在同時給 5 個工具時，有時會直接用文字回答，而不是呼叫受保護的工具。引擎甚至沒機會做出決策。

模擬代理永遠會呼叫工具。真實的 4B 模型有時不會。

**永遠不要把 `not-available` 解讀成政策失敗。** 這代表 LLM 沒有呼叫工具。這和 `unexpected-decision` 不一樣——後者是 guard 有執行，而引擎做出了錯誤判定。只有後者才是真正的回歸問題。

在 Plan B 中，所有真正執行到的工具呼叫都產生了正確的決策。那 7 個失敗指向的是 LLM，而不是引擎。

這對 CI 很重要。如果你把 `not-available` 當成失敗，因為模型非決定性，你的門檻就會很不穩定。如果你只在 `unexpected-decision` 時失敗，你的門檻就會既嚴格又穩定。 [實地測試報告](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/field-test/FIELD_TEST_REPORT.md) 建議提交一份 golden `not-available` 允許清單，讓 CI 只在真正回歸時失敗，而不是因為 LLM 當天狀況不好。

這是用模擬代理學不到的。它需要 83 個真實代理。

## 重新規劃迴圈

我最滿意的結果是：deny → replan → allow 的安全迴圈。

當代理嘗試 `drop_database` 時，引擎會拒絕。好的代理不會就此停下——它會重新規劃。它會選擇另一個無害工具。引擎允許。兩次呼叫都會被稽核。

```python
@adapter.guard(
    tool_name="drop_database",
    action="delete",
    environment="production",
    data_class="restricted",
)
def drop_database(db: str) -> str:
    return f"dropped {db}"

@adapter.guard(
    tool_name="query_audit_log",
    action="read",
    environment="production",
    data_class="internal",
)
def query_audit_log(query: str) -> str:
    return f"audit rows for {query}"

# 代理嘗試 drop_database → 引擎拒絕（正式環境中的刪除）
# 代理重新規劃 → 呼叫 query_audit_log → 引擎允許（正式環境中的讀取）
# 兩次決策都被稽核。代理被導向，而不是被阻擋。
```

已在全部 8 個 LLM 框架上測試。 **8/8 live，8/8 scripted。** 每個框架都拒絕了破壞性呼叫，重新規劃成無害的讀取，並獲得允許。完整的重新規劃結果在 [實地測試報告的 §2.5](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/field-test/FIELD_TEST_REPORT.md) 。

代理不是被擋下來，而是被導向。每一步都在稽核軌跡上。這就是我在 v0.2 要延伸的模式：升級處理的往返流程，由人類核准或拒絕，然後代理繼續。

## 我學到了什麼

在前幾個專案裡，我學到實地測試應該要事先規劃，而不是臨時拼湊。這次我學到更深的一層： **它應該被最佳化，而不是硬幹。**

涵蓋式設計——Plan A + Plan B——就是這個學習的實踐。206 次，而不是 2,490 次。涵蓋範圍相同。昂貴資源只花在只有真實代理能證明的地方。這個演進過程：跳過它 → 很晚才加上 → 從一開始就規劃 → 做最佳化。四個專案，四個步驟。

**零模擬代理是正確選擇。** 整合成本是真實的——8 個相容性 wrapper、3 個 pyproject 修正、1 個隔離處理的 C extension、12 個框架怪癖 [都記錄在報告裡](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/field-test/FIELD_TEST_REPORT.md) 。但每個修正都抓到了一個模擬代理會藏起來的真 bug。模擬的成本在上線前是看不見的。真實代理的成本在第一次執行時就看得見。

**`not-available` 不是政策失敗，而是 LLM 可靠性訊號。** 把它和 `unexpected-decision` 區分開來，就是 flaky CI 門檻和穩定門檻的差別。

**全程 fail-closed 是不可妥協的。** [DD-14](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/design/design-decisions.md) ，在第一行程式碼之前就已寫下。

**重新規劃迴圈可大規模運作。** 8/8 個框架。代理不是被阻擋，而是被導向。

**10 個框架是 v0.1 的正確數量。** 足以證明 adapter 合約可泛化。又不會多到讓整合把引擎淹沒。需要 LLM 呼叫的 8 個都通過了。2 個自我測試框架在 CI 中以 deterministic 方式運作。

## 你可以拿來用什麼

**想做帶工具的代理？** `pip install agent-tooltrust` ，執行 `tooltrust init --posture balanced` ，把你的工具加上裝飾器。四狀態決策、含解釋與稽核軌跡。沒有基礎設施需求。 [快速上手](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/reference/quickstart.md) 有完整流程。

```bash
pip install agent-tooltrust
tooltrust init --posture balanced
```

**你已經有 OPA/Rego 政策？** 雙後端可以沿用。相同輸入，相同輸出。不需要重寫。 [API 參考](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/reference/api.md) 兩者都涵蓋。

**想先觀察再強制執行？** shadow mode（ `dry_run=True` ）會記錄每個決策但不阻擋。先部署、再觀察、再調整、最後強制。

**你在用 LangGraph、PydanticAI、CrewAI、OpenAI Agents SDK、Google ADK、AutoGen、LlamaIndex 或 smolagents？** 已經有用真實代理測過的 adapter。 [整合指南](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/real-agent-integration/README.md) 裡有各框架的接線方式。

## 你可以如何擴充

**自訂風險函式** — 用 `@tooltrust.risk_function` 註冊自己的函式，接進加權總和，不用動引擎。

**社群政策包** — 把某個工具生態系（GitHub admin、AWS cost ops、Notion writes）對應到這套分類法。 `tooltrust pack validate` 、 `tooltrust pack add` 。

**新的稽核 sink** — `AuditSink` 介面可插拔。Splunk、Datadog，或你用的任何 SIEM 都可以。

**新的框架 adapter** — `BaseAdapter` 只有三個方法。擷取 context、傳給引擎、顯示決策。可能 50 行左右。這個模式已經在 10 個框架上被證明有效。

**自訂安全姿態預設值** — 內建的三種 preset 都是 YAML 檔。你可以 fork 一份、調整閾值，然後變成你們組織的預設值。

如果你想看內部實作， [架構文件](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/architecture/architecture-v0.1.0.md) 和 [14 個設計決策](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/design/design-decisions.md) 都在 repo 裡。

## 已交付內容

Repo: [github.com/deghosal-2026/agent-tooltrust](https://github.com/deghosal-2026/agent-tooltrust)  
PyPI: `pip install agent-tooltrust` （v0.1.1）

2,490 項 deterministic tests。Ruff 0。Mypy strict 0。Docker 通過。SWE-bench verified。OWASP 5/10。OpenSSF Silver。10 個框架、83 個真實代理、30 個情境、零模擬。83/83 的 Plan A。116/123 的 Plan B。8/8 的重新規劃迴圈。分支保護。Repo 公開。

這是 v0.1.0。引擎已交付且已驗證。平台——升級處理往返、政策包、規則組合、HTTP `/authorize` 、重放偵測、子代理委派——是 v0.2.0 的 32 個開放議題。 [WBS](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/wbs/README.md) 有完整追蹤。 [實地測試報告](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/field-test/FIELD_TEST_REPORT.md) 也誠實地說明了哪些已被證明、哪些還沒有。

## 問題

- 當你的代理嘗試呼叫不該呼叫的工具時，會發生什麼事？你的系統知道「在測試環境的讀取」和「在正式環境的寫入」之間的差異嗎？還是你只是用了白名單，然後祈禱？
- 如果你曾經跨多個框架做過實地測試，最先壞掉的是政策、adapter，還是 LLM？模擬代理是否掩蓋了後來才浮現的問題？
- 有人遇過 `not-available` 問題嗎——LLM 沒有呼叫工具，結果你分不出是政策失敗還是模型問題？你在 CI 裡怎麼處理？
- 四狀態（allow/audit/escalate/deny）是正確的粒度，還是相較於二元過度設計？我最不確定的是 `audit` 這個狀態。
- 對 OPA/Rego 使用者來說——雙後端（YAML + Rego）有意義嗎，還是你寧可只用 Rego？
- 涵蓋式設計把矩陣縮小了 12 倍。還有人把組合測試應用到基於 LLM 的代理測試嗎？我沒在其他地方看過這種模式——它是新穎的，還是只是文件不足？

這個 [repo](https://github.com/deghosal-2026/agent-tooltrust) 裡有完整的 [PRD](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/design/PRD.md) 、 [架構](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/architecture/architecture-v0.1.0.md) 、 [設計決策](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/design/design-decisions.md) 、 [實地測試計畫](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/field-test/field-test-plan.md) 和 [實地測試報告](https://github.com/deghosal-2026/agent-tooltrust/blob/main/docs/field-test/FIELD_TEST_REPORT.md) 。歡迎 star、fork、把它弄壞。我寧可你現在把它弄壞，也不要等到你把它送上正式環境之後才壞。

---

原文出處： [https://dev.to/debashish\_ghosal/i-stopped-trusting-ai-agents-with-tools-so-i-built-a-gatekeeper-26fb](https://dev.to/debashish_ghosal/i-stopped-trusting-ai-agents-with-tools-so-i-built-a-gatekeeper-26fb)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/31oZ8q) [20瀏覽](https://codelove.tw/@tony/post/31oZ8q)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/31oZ8q)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [WorkBuddy 從入門到精通：一句話讓 AI 幫你搞定所有繁瑣工作](https://codelove.tw/@tony/post/xdb40a)

🔥 [Google Gemini CLI 教學：如何安裝與使用（附圖片）](https://codelove.tw/@tony/post/qvpDJ3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [關於 x86、x64、amd64、arm64 的簡單說明](https://codelove.tw/@howtomakeaturn/post/2aneP3)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝9 💬4 ❤️2

257

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

7

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1786932857&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2F31oZ8q&amp;pra=5&amp;aiof=13&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE1MS4wLjc5MjIuMTM4IixudWxsLDAsbnVsbCwiNjQiLFtbIk5vdD1BP0JyYW5kIiwiOTkuMC4wLjAiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNTEuMC43OTIyLjEzOCJdLFsiQ2hyb21pdW0iLCIxNTEuMC43OTIyLjEzOCJdXSwwXQ..&amp;dt=1786932856887&amp;bpp=2&amp;bdt=94&amp;idt=117&amp;shv=r20260812&amp;mjsv=m202608110101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1786932726%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1786932726%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=6432990981627&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1634&amp;bih=908&amp;scr_x=0&amp;scr_y=0&amp;eid=95397679%2C95398731%2C95399506%2C122880367%2C122880369%2C42533294&amp;oid=2&amp;pvsid=7060619402679603&amp;tmod=144435645&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1634%2C908&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=181" title="Advertisement" aria-label="Advertisement"></iframe>