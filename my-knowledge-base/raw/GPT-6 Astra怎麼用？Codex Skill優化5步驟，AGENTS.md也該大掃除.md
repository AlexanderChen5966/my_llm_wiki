---
title: "GPT-6 Astra怎麼用？Codex Skill優化5步驟，AGENTS.md也該大掃除"
source: "https://www.bnext.com.tw/article/92145/gpt-6-astra-skill-cleanup-less-is-more"
author:
  - "[[李先泰]]"
published: 2026-09-07
created: 2026-09-07
description: "GPT-6 Astra 上線，Codex 成員 Provencher 建議為 skill 檔大掃除：描述縮短、結構拆開、刪食譜式流程，再定義完成。"
tags:
  - "clippings"
status: processed
---
如果你過去一年用 [Codex](https://openai.com/codex/) 、 [Claude Code](https://claude.com/claude-code) 這類 AI 編程代理工作，很可能累積了一整櫃 skill 檔和一份越寫越長的專案指令檔（Codex 是 `AGENTS.md` ，Claude Code 是 `CLAUDE.md` ）。 **[GPT-6 Astra](https://openai.com/index/gpt-6-astra/) 發表後，這些為了「管住」舊模型而堆出來的指令值得重新盤點，其中過度細化或重複的部分，很可能已經成了負擔。**

這不是外人的揣測。依 openai/codex 程式庫的更新紀錄，OpenAI 在 8 月 13 日把 Codex 內建的 skill-creator（教 Codex 怎麼寫 skill 的官方範本）從 416 行砍到 229 行，開頭第一條原則改成「假設 Codex 已經很有能力，只寫會改變它決策的資訊」。

Astra 發表隔天，Codex 團隊成員 Eric Provencher 就在 X 上發表長文〈 [Rethinking skills and prompts for GPT-6 Astra](https://x.com/pvncher/status/2095991462416490862) 〉，把該刪什麼、該怎麼改講得更具體。Provencher 是 macOS 工具 [Repo Prompt](https://repoprompt.com/) 的開發者，2026 年 5 月加入 OpenAI，負責 Codex 的開發者體驗；OpenAI 開發者體驗主管 Romain Huet 當時在 X 上公開歡迎他加入。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

一般人拿到新模型的反應是再多裝幾個 skill、再補幾條規則。Provencher 的建議正好反過來： **先做減法** 。

## Skill 裝太多，模型為什麼反而變笨？

先弄清楚 skill 是什麼。白話說，它就是一份存成 Markdown 的提示詞，有時附帶腳本，讓模型在特定任務時照著做。每個 skill 都有名稱和描述，這兩樣會常駐在模型的 context（可以理解為模型的工作記憶）裡，模型靠它們判斷什麼時候該用哪個 skill。

問題出在預算。依 OpenAI 的 Codex 文件，開工時載入的初始 skill 清單（含每個 skill 的名稱、描述與檔案路徑）最多只能占 context 的 2%；若模型的 context 大小未知，則以 8,000 個字元為上限。裝得太多，Codex 會先把描述截短，再不夠就直接省略部分 skill 並顯示警告。這個預算只管初始清單，模型一旦選中某個 skill，仍會讀完整份 `SKILL.md` 。

換句話說，一旦裝到超出預算，模型看到的就是一排被截短的殘缺說明。Provencher 說，結果就是模型更難判斷該挑哪一個；更糟的是，描述之間互相矛盾，或寫得太有「選我」的味道，會讓模型載入一堆對任務沒幫助的指令。

## 五個步驟，怎麼幫 skill 大掃除？

### 第一步：縮短描述，說清楚做什麼、何時用

Provencher 的第一條規則：描述只需簡述這個 skill 做什麼、什麼時候該用，在這個前提下越短越好。他用一個資料庫 skill 當例子：

```yaml
# 不好的寫法
建立並驗證 Postgres schema 遷移。適用於處理資料庫、查詢、資料模型或持久化操作時。

# 好的寫法
建立並驗證 Postgres schema 遷移。適用於新增、修改遷移檔，或檢視其部署狀況時。
```

差別在後半句。前者只要任務碰到資料庫、查詢、資料模型，模型就會想套用這個 skill；後者把觸發範圍收窄到「新增、修改 migration 或檢視上線」，其他資料庫任務不會被干擾。

### 第二步：把 SKILL.md 拆成路由器加附件

第二條規則叫「漸進式揭露」（progressive disclosure）。模型每讀一份 skill 都要花 context，讀進來的內容還不一定跟手上任務有關。

Provencher 建議，一個 skill 若包含多種工作流程，主檔 `SKILL.md` 就只留共同原則和「什麼情況去讀哪份附件」的路由，細節放到 references 或 scripts 資料夾。OpenAI 新版 skill-creator 也用同樣邏輯，把 skill 資訊分成三層：名稱與描述、 `SKILL.md` 本文、支援檔案，越後面的層級越晚才讀。簡單且自成一體的 skill 則不必硬拆，一份 `SKILL.md` 就夠。

```
cloud-deploy/
|-- SKILL.md          只放選擇條件：用 AWS 讀 aws.md，用 GCP 讀 gcp.md
\`-- references/
    |-- aws.md
    |-- gcp.md
    \`-- azure.md
```

### 第三步：刪掉食譜式流程

很多 skill 和 `AGENTS.md` 被寫成一步步的食譜。Provencher 指出，模型理解細微差異和模糊指令的能力已經大幅進步，過去有幫助的細節指令，換到 Astra 上反而礙事。

最典型的是「每次編輯前先讀完某幾份文件」：

```markdown
# 不好的寫法
每次編輯前，請先閱讀 architecture.md、database.md 與 deployment.md。

# 好的寫法
處理服務邊界時請參考 architecture.md，變更 schema（資料庫結構）時參考 database.md，準備部署時則參考 deployment.md。
```

改一個錯字也得先讀完整份架構文件，只是在燒 context、拖慢速度。Astra 自己會判斷該讀什麼，你只要告訴它哪份文件管哪件事。同理，舊模型要被催著跑測試、檢查成果，Astra 會自己做，沿用「一律要跑測試」的舊指令，可能導致不必要的測試。

### 第四步：把禁令改成授權，並定義完成

Provencher 特別提醒兩件事。第一，如果你曾因為舊模型擅自行動而在 `AGENTS.md` 加了強硬的「先問再做」，Astra 會把這些邊界看得很重，可能在你其實希望它繼續的地方停下來。OpenAI 發表時公布的一項評測也呼應這點：在 OpenAI 針對困難或不可能完成任務設計的特定越權評測中，未加正式部署防護的上一代模型 GPT-5.6 Sol 有 48% 的測試案例越過授權目標，Astra 則未觀察到此行為。這是特定評測的結果，不代表日常使用的越權機率。

與其寫一堆禁令，不如給它一段明確的授權，寫清楚哪個工作流程是安全的：

```markdown
本地測試使用的是拋棄式的測試資料（fixtures），且無法存取正式環境。請直接執行測試，修復因本次變更所造成的錯誤，並重新執行受影響的測試，過程中不需逐一步驟向我確認。
```

第二，Astra 比 Sol 更傾向做完第一版就回來找你檢視，即使工作還沒做完。OpenAI 的模型指南也提到，Astra 在額外資訊可能改變結果時，更常停下來問使用者。

**解法是開工前先定義完成** ：如果任務包含把程式跑起來、檢查結果、修掉錯的地方，就把這三件事寫進要求裡；如果你希望它做完第一版後繼續探索，就說清楚要探索什麼、到哪裡停。

### 第五步：叫 Astra 自己做一次盤點

Provencher 在文末建議，直接請 GPT-6 Astra 依上述原則對你的專案做一次稽核。原文沒有附提示詞，以下依他的建議整理成一則可直接貼入 Codex 的版本：

```
請盤點這個專案的 AGENTS.md 與所有 skill 檔（含使用者層級的 skill），依下列原則逐項提出修改建議，先列清單、經我確認後再動手改：
1. 每個 skill 的 description 是否簡述了「做什麼、何時該用」？冗長、重複或觸發範圍過寬的，給出縮短版本。
2. 哪些 SKILL.md 包含多種工作流程卻沒有拆成路由器加附件？列出可移到 references/ 的段落。
3. AGENTS.md 中哪些指令屬於「每次動作前先讀某些檔案」或「每次都要跑測試」？標出可刪除或改為「哪份文件管哪件事」的條目。
4. 哪些指令是禁令式的「先問再做」？建議改寫成明確授權的版本，並說明前提條件。
5. 哪些 skill 的描述彼此重疊或矛盾？
```

輸出應該有這五部分：縮短後的 description 清單、可拆分的 `SKILL.md` 與建議搬移的段落、可刪的 `AGENTS.md` 條目、改寫後的授權句、重疊的 skill 配對。缺哪一段，就在同一個對話補一句「第 N 項再展開」。

## 什麼情況不該急著刪？

Provencher 自己留了但書：repo 裡的 skill 會被同事的 agent 讀到，對方可能用 Sol、Luna 或其他模型，對 Astra 剛好的指令對它們可能不夠。OpenAI 的模型指南另外提醒，Astra 對 skill 檔內的指令更敏感，寫得模糊或互相衝突會讓它提前停工。也就是說，這次大掃除的重點是「寫準」，不只是「寫少」。

因此，這一波大掃除可以「先刪描述，再拆結構」，也就是先重新檢視過時或重複的限制，再把完成條件寫清楚。剩下的，就交給模型自己判斷。

> 延伸閱讀： [一句「幹嘛」引爆脆9.5萬讚！故宮南院小編如何把嬰兒枕、馬關條約，變民眾進館理由？](https://www.bnext.com.tw/article/92138/npm-south-threads-social-media)

資料來源： [Eric Provencher X 長文](https://x.com/pvncher/status/2095991462416490862) 、 [OpenAI Codex skills 文件](https://learn.chatgpt.com/docs/build-skills) 、 [OpenAI GPT-6 Astra 模型指南](https://developers.openai.com/api/docs/guides/latest-model) 、 [openai/codex skill-creator 更新紀錄](https://github.com/openai/codex/commit/5e32f728f1f86a967c6be057351f12505778df8f) 、 [《The Register》](https://www.theregister.com/ai-and-ml/2026/09/03/openai-throws-astra-into-the-top-tier-model-ring/5294378) 、 [Romain Huet X 貼文](https://x.com/romainhuet/status/2059684035778240979)

本文初稿為AI編撰，整理．編輯/ 李先泰