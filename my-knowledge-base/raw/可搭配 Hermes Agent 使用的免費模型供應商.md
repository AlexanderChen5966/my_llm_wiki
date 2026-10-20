---
title: "可搭配 Hermes Agent 使用的免費模型供應商"
source: "https://codelove.tw/@tony/post/amZ72q"
author:
published:
created: 2026-07-07
description: "[Hermes Agent](https://github.com/nousresearch/hermes-agent) 由 NousResearch 開發，是一個開源 AI Agent，做到了多數框架做不到的事：它會從經驗中學習。它會在使用過程中建立技能，在不同工作階段之間精進記憶，並支援平行子代..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [07月06日](https://codelove.tw/@tony/post/amZ72q "2026-07-06 09:09:08")

[Hermes Agent](https://github.com/nousresearch/hermes-agent) 由 NousResearch 開發，是一個開源 AI Agent，做到了多數框架做不到的事：它會從經驗中學習。它會在使用過程中建立技能，在不同工作階段之間精進記憶，並支援平行子代理與內建的 cron 排程器。你可以把它跑在 5 美元的 VPS、GPU 叢集，或無伺服器基礎架構上。

精品

它之所以能讓人立刻上手，是因為它對供應商的彈性支援。Hermes 支援 200 多個模型，涵蓋持續增加中的供應商清單，而且你只要一個指令就能在它們之間切換，無需修改程式碼。這代表你可以完全先從免費方案開始，再決定是否要付費使用 API。

這篇文章會介紹哪些免費供應商可與 Hermes Agent 搭配使用，以及如何逐一開始設定與上手。

## 安裝 Hermes Agent

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
source ~/.bashrc
hermes
```

執行後會進入互動式 CLI。執行 `hermes setup` 來走完整個設定精靈，或執行 `hermes setup --portal` 透過 Nous Portal 連線（他們自家的代管服務，支援 OAuth，會把你所有的 API 金鑰整合成一個訂閱）。

## 切換供應商

安裝完成後，只要一個指令就能切換供應商：

```bash
hermes model
```

也可以在對話中直接輸入： `/model [provider:model]`

不用修改設定檔，也不用重新啟動。這就是讓免費方案探索變得可行的關鍵——你可以試一個模型，遇到速率限制後，幾秒內切到另一個。

## Hermes Agent 支援的免費供應商

### Nous Portal

[Nous Portal](https://nousresearch.com/) 是 Hermes 的原生供應商。它把模型存取、網頁搜尋、圖片生成和 TTS 都整合在同一個訂閱下。如果你想要與 Hermes 和 Nous 生態系最緊密的整合，這裡就是起點。註冊時請查看目前的免費方案。

### OpenRouter

[OpenRouter](https://openrouter.ai/) 是最彈性的免費選項。它透過單一 API 金鑰，讓你存取 200 多個模型，包括 Llama 4、DeepSeek、Gemma 和 Mistral。免費方案涵蓋 27 個以上的模型（標示為 `:free` 後綴），每日 200 次請求、每分鐘 20 次請求。

由於 Hermes 原生支援 OpenRouter，你可以立刻把它指向這些免費模型中的任何一個。如果你想比較 Hermes 在不同底層模型上的行為，而不必管理多個帳號，這是最好的選擇。

精品

### NVIDIA NIM

[NVIDIA NIM](https://build.nvidia.com/) 在註冊時會提供免費 API 點數，不需要信用卡。模型目錄包含 80 多個模型——Llama 4、Qwen、Mistral，以及 NousResearch 自家的 Hermes 3 模型——速率大約是每分鐘 40 次請求。點數不會過期。

如果你特別想在 Hermes Agent 底下使用 NousResearch 的模型，NIM 會是最佳免費選項，因為它直接託管 Hermes 3 微調版本。

### Hugging Face

[Hugging Face Inference Providers](https://huggingface.co/docs/inference-providers) 會為每個帳號提供每月免費點數，並透過一整個推論供應商網路（Groq、Together、SambaNova 等）進行路由。Hermes Agent 原生支援 Hugging Face。

這個免費方案最適合拿來試一些利基或較小的模型。免費層的冷啟動可能較慢，因此較不適合長時間的互動式工作階段，但很適合批次任務或評估用途。

### NovitaAI

[NovitaAI](https://novita.ai/) 是一個成本效益高的推論供應商，對新帳號提供相當慷慨的免費方案。Hermes 原生支援它。如果在大量 Agent 開發時，OpenRouter 和 NVIDIA NIM 的速率限制開始變成瓶頸，這很值得一試。

### Kimi / Moonshot

[Kimi](https://kimi.moonshot.cn/) （由 Moonshot AI 提供）有支援長上下文模型的免費方案。Hermes Agent 將它列為支援的供應商。若你的 Agent 任務需要處理大量文件或很長的對話歷史，而這些內容會耗盡其他免費模型的上下文視窗，這會特別有用。

## 供應商快速比較

| 供應商 | 免費方案 | 搭配 Hermes 的最佳用途 |
| --- | --- | --- |
| [**Nous Portal**](https://nousresearch.com/) | 註冊時查看 | 最緊密的 Hermes 整合 |
| [**OpenRouter**](https://openrouter.ai/) | 每日 200 次請求，27+ 個免費模型 | 模型多樣性、方便切換 |
| [**NVIDIA NIM**](https://build.nvidia.com/) | 註冊時提供點數，無期限 | 執行 NousResearch 模型 |
| [**Hugging Face**](https://huggingface.co/docs/inference-providers) | 每月點數 | 利基模型、批次任務 |
| [**NovitaAI**](https://novita.ai/) | 註冊時提供免費點數 | 遇到速率限制時的替代方案 |
| [**Kimi / Moonshot**](https://kimi.moonshot.cn/) | 免費方案 | 長上下文任務 |

## 在免費方案上執行 Hermes 的技巧

**在 Agentic 迴圈中，速率限制會疊加。** Hermes 的學習迴圈與子代理功能，可能讓每個任務產生很多模型呼叫。在 OpenRouter 免費方案的每分鐘 20 次請求下，多步驟任務很快就會碰到上限。你可以在工作階段中用 `hermes model` 切換到限制更高的供應商，或設定備援供應商。

精品

**若要特別使用 NousResearch 模型，請選 NVIDIA NIM。** 如果你想用 Hermes 設計時所對應的模型——Hermes 3 微調版本——NIM 直接託管在 `nousresearch/hermes-3-llama-3.1-70b` 。免費點數足以支援有實質意義的開發，而不會很快碰到每日請求上限。

**探索階段先用 OpenRouter。** 它把 27+ 個模型的免費方案整合在一起，是最快理解 Hermes Agent 在不同底層模型上表現的方式。不需要另外申請帳號，一個金鑰就能用，並且可用 `/model` 切換。

**遇到問題時執行 `hermes doctor` 。** 內建診斷指令會檢查你的供應商設定，並在你浪費時間除錯錯誤方向之前，先找出設定錯誤。

## 總結

Hermes Agent 是少數內建真正學習迴圈的開源 Agent 之一，而它的多供應商支援也代表你不會被綁死在單一 API 上。若要免費開始使用：安裝 Hermes，執行 `hermes setup` ，然後連接 [OpenRouter](https://openrouter.ai/) 或 [NVIDIA NIM](https://build.nvidia.com/) ——兩者都能直接使用，且不需要信用卡。當你探索哪個方案最適合你的使用情境時，就用 `hermes model` 切換模型。

---

原文出處： [https://dev.to/dalenguyen/free-model-providers-to-use-with-hermes-agent-13l9](https://dev.to/dalenguyen/free-model-providers-to-use-with-hermes-agent-13l9)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/amZ72q) [47瀏覽](https://codelove.tw/@tony/post/amZ72q)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/amZ72q)

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

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [$20 的 Cursor Pro 額度，這樣用一個月都花不完](https://codelove.tw/@tony/post/apXgLa)

🔥 [【你需要知道的一切】從頭到尾了解 Dify——從基本要素到最終實現](https://codelove.tw/@tony/post/qZWMAx)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝17 💬3 ❤️4

689

🥈

[我愛JS](https://codelove.tw/@JsLover0018)

📝2 💬5 ❤️3

137

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;abgtt=6&amp;lmt=1783385946&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32%2C44%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FamZ72q&amp;pra=5&amp;asro=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OS4wLjc4MjcuMTAzIixudWxsLDAsbnVsbCwiNjQiLFtbIkdvb2dsZSBDaHJvbWUiLCIxNDkuMC43ODI3LjEwMyJdLFsiQ2hyb21pdW0iLCIxNDkuMC43ODI3LjEwMyJdLFsiTm90KUE7QnJhbmQiLCIyNC4wLjAuMCJdXSwwXQ..&amp;dt=1783385946639&amp;bpp=1&amp;bdt=79&amp;idt=31&amp;shv=r20260706&amp;mjsv=m202606300101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1783385939%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1783385939%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=1321158250263&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1599&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=95395662%2C42533294%2C95393485%2C95395497&amp;oid=2&amp;pvsid=5558322822804980&amp;tmod=1849337775&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1599%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=42" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="728" height="90" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260706/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>

指令碼語言