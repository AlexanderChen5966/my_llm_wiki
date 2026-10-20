---
title: "手把手教你寫一個 AI Skill，讓 AI 真正學會你的工作流程"
source: "https://codelove.tw/@tony/post/xNQDEq"
author:
published:
created: 2026-05-27
description: "手把手教你寫一個 AI Skill，讓 AI 真正學會你的工作流程  ===================================    `Skill` 可以理解成 AI 的「專業能力模組」。它不是臨時 prompt，也不一定要連網呼叫外部工具。更準確地說，它是一個資料夾，把某類..."
tags:
  - "clippings"
status: processed
---
> [!primary] Primary
> 🔴 **訂閱 YT 頻道！** 站長每週分享心得！ ➡️ [YT 頻道](https://www.youtube.com/@codelove_tw) ➡️ [LINE 社群](https://line.me/ti/g2/nipkjq2WoZPKX5dTn9tE9266aEOt6EOICFGa1g) ➡️ [DC 社群](https://discord.gg/fvCF2whZ9D)[小編精選 - 技術文章翻譯](https://codelove.tw/@tony) · [05月25日](https://codelove.tw/@tony/post/xNQDEq "2026-05-25 18:15:11")



## 手把手教你寫一個 AI Skill，讓 AI 真正學會你的工作流程

```
\`Skill\` 可以理解成 AI 的「專業能力模組」。
```

它不是臨時 prompt，也不一定要連網呼叫外部工具。更準確地說，它是一個資料夾，把某類任務裡的經驗、規則、操作流程和注意事項都收進去，後面反覆使用。

比如你經常讓 AI 寫週報，每次都要提醒它：

```
text 體驗AI程式碼助手 代碼解讀複製代碼不要寫得太空。
不要虛構資料。
要先整理工作內容。
要寫成果、問題和下週計畫。
語言正式一點。
```

像這種每次都會重複提醒、又很容易沉澱成規則的東西，就適合做成 Skill。

下面我用「工作總結寫作」舉例，建立一個叫 `work-summary-writing` 的 Skill。

## 一個 Skill 長什麼樣？

一個最簡單的 Skill，其實只需要一個 `SKILL.md` 檔案：

```
text 體驗AI程式碼助手 代碼解讀複製代碼work-summary-writing/
└── SKILL.md
```

更完整的 Skill 還可以包含這些目錄：

```
text 體驗AI程式碼助手 代碼解讀複製代碼work-summary-writing/
├── SKILL.md
├── scripts/
├── references/
└── assets/
```

不同目錄的作用不一樣：

目錄作用 `SKILL.md` 必需檔案，描述 Skill 什麼時候觸發，以及觸發後怎麼做 `scripts/` 放可重複執行的腳本 `references/` 放較長的規則、規範、介面文件或業務資料 `assets/` 放範本、圖片、字型、範例檔案等輸出素材一開始不用急著把目錄搭得很完整。先把 `SKILL.md` 寫清楚，讓它能觸發、能穩定完成任務，第一版就夠用了。

## 第 1 步：確定這個 Skill 解決什麼問題

別一上來就寫「萬能寫作 Skill」。

Skill 越泛，越容易失控。比如：

```
yaml 體驗AI程式碼助手 代碼解讀複製代碼description: "幫助使用者寫東西"
```

這個描述太寬。寫文章、寫郵件、寫程式註解、寫廣告文案、寫產品需求文件，都可能觸發它。模型最後反而不知道該按哪套規則走。

我們這次只解決一個具體場景：

> 當使用者需要寫工作總結、週報、月報、季報、年度總結、專案回顧或階段性彙報時，幫助使用者把零散資訊整理成一篇總結。這篇總結要結構清晰、重點明確、表達正式自然。

邊界說得越清楚，Skill 越穩定。

## 第 2 步：建立 Skill 目錄

不同 AI 工具放 Skill 的位置不一樣。

以 Codex 為例，個人全域 Skill 通常放在：

```
text 體驗AI程式碼助手 代碼解讀複製代碼~/.codex/skills/
```

如果我們要建立 `work-summary-writing` ，目錄結構就是：

```
text 體驗AI程式碼助手 代碼解讀複製代碼~/.codex/skills/work-summary-writing/
└── SKILL.md
```

可以用命令建立目錄：

```
bash 體驗AI程式碼助手 代碼解讀複製代碼mkdir -p ~/.codex/skills/work-summary-writing
```

如果你使用的是 Claude Code，目錄可能是：

```
text 體驗AI程式碼助手 代碼解讀複製代碼~/.claude/skills/work-summary-writing/
```

關鍵不是路徑本身，而是要放進對應 Agent 能識別的 Skill 目錄。

## 第 3 步：寫 FrontMatter

`SKILL.md` 開頭需要一段 YAML FrontMatter：

```
yaml 體驗AI程式碼助手 代碼解讀複製代碼---
name: work-summary-writing
description:  撰寫、潤飾或整理中文工作總結類文件時使用，包括週報、雙週報、月報、季報、半年/年度總結、專案回顧、階段性彙報、述職報告、工作小結等。當使用者說「幫我寫個週報」、「整理一份月度工作」、「年終總結」、「專案回顧」、「做彙報材料」、「述職」，或丟過來一堆零散工作紀錄要求「總結一下」、「整理成報告」時，都要主動啟用。該技能用於幫助使用者把零散工作內容整理成結構清晰、重點明確、表達正式自然的總結文字。
---
```

這裡最重要的是兩個欄位：

欄位作用 `name` Skill 的名字，建議使用小寫英文、數字和連字號 `description` Skill 的觸發條件和能力說明很多人會低估 `description` 。

它不是寫給人看的簡介，而是給 AI 判斷「什麼時候該載入這個 Skill」的依據。只有 Skill 被觸發後，AI 才會繼續讀正文。觸發條件如果只寫在正文裡，模型可能根本看不到。

所以 `description` 裡至少要說清楚三件事：

- 這個 Skill 能做什麼。
- 適合什麼場景。
- 使用者說哪些話時應該觸發。

不要寫成：

```
yaml 體驗AI程式碼助手 代碼解讀複製代碼description: 幫助寫總結
```

這太短，也太模糊。

## 第 4 步：把流程寫成可執行步驟

Skill 更像操作手冊，不是理念宣言。

不要只寫：

```
text 體驗AI程式碼助手 代碼解讀複製代碼寫得專業、自然、有邏輯。
```

這類話方向沒錯，但模型不好執行。更穩的寫法，是把任務拆成一步一步的動作。

下面是一個完整的 `SKILL.md` 範例：

```
markdown 體驗AI程式碼助手 代碼解讀複製代碼---
name: work-summary-writing
description:  撰寫、潤飾或整理中文工作總結類文件時使用，包括週報、雙週報、月報、季報、半年/年度總結、專案回顧、階段性彙報、述職報告、工作小結等。當使用者說「幫我寫個週報」、「整理一份月度工作」、「年終總結」、「專案回顧」、「做彙報材料」、「述職」，或丟過來一堆零散工作紀錄要求「總結一下」、「整理成報告」時，都要主動啟用。該技能用於幫助使用者把零散工作內容整理成結構清晰、重點明確、表達正式自然的總結文字。
---

# 工作總結寫作

## 寫作步驟

### 1. 明確總結範圍

先判斷總結的時間範圍、使用場景和目標字數，例如：

- 週報
- 月報
- 季度總結
- 年度總結
- 專案階段總結
- 個人工作回顧

如果使用者沒有說明時間範圍，可使用「本階段」作為預設表述。

如果使用者沒有說明字數，預設生成 800-1200 字左右的工作總結。

### 2. 提煉核心工作

從使用者提供的資訊中提取主要工作內容。

優先識別：

- 負責了什麼任務
- 推進了哪些專案
- 解決了哪些問題
- 參與了哪些協作
- 完成了哪些交付

### 3. 整理工作成果

將工作內容轉化為結果表達。

盡量補充：

- 完成情況
- 進展狀態
- 產生的價值
- 效率提升
- 問題解決效果
- 可量化資料

如果使用者沒有提供資料，不要編造，可使用「提升了工作效率」「推動了專案進展」等穩妥表達。

### 4. 分析問題與不足

客觀總結工作中存在的問題。

可以從以下角度整理：

- 進度是否受阻
- 溝通是否充分
- 流程是否順暢
- 方案是否還有優化空間
- 個人能力是否需要提升

表達要克制，不誇大問題，也不迴避問題。

### 5. 提煉經驗與反思

總結本階段獲得的經驗。

重點寫：

- 哪些方法有效
- 哪些流程可以複用
- 哪些協作方式值得保留
- 後續可以如何改進

### 6. 制定下一步計畫

根據前文內容，提出後續計畫。

計畫應具體、可執行，例如：

- 繼續推進某項任務
- 優化某個流程
- 提升某項能力
- 完成某個交付
- 加強溝通協作

## 字數要求

- 優先遵循使用者明確提出的字數要求。
- 如果使用者沒有提出字數要求，預設生成 800-1200 字。
- 如果使用者要求「簡短」「簡單」「概括」，控制在 300-600 字。
- 如果使用者要求「詳細」「完整」「正式」，控制在 1200-2000 字。
- 不要為了湊字數重複表達。
- 不要為了壓縮字數刪掉關鍵事實。

## 推薦結構

\`\`\`text
一、總體情況
簡要說明本階段的工作重點和整體完成情況。

二、主要工作
分條說明完成的重點事項。

三、工作成果
總結取得的結果、進展或價值。

四、問題與不足
客觀說明存在的問題和改進空間。

五、經驗總結
提煉本階段的收穫和方法。

六、下一步計畫
說明後續重點工作安排。
```

## 注意事項

- 不要虛構關鍵事實、資料、專案名稱或成果。
- 使用者提供的資訊不足時，可以使用通用表達，但應避免寫得過滿。
- 表達要正式、自然、簡潔，避免口號式語言。
- 多寫具體行動和實際結果，少寫空泛態度。
- 總結個人工作時，突出職責和貢獻，但不要過度誇大。
- 總結團隊工作時，注意使用「我們」「團隊」等表述。
- 問題與不足要真實可改進，不要寫成嚴重失誤。
- 下一步計畫要和前文問題、成果形成呼應。

## 輸出要求

預設輸出一篇完整工作總結。

如果使用者只要求框架，則只輸出結構和要點。

如果使用者提供了原始素材，應優先基於素材改寫，不隨意擴展事實。

```
寫到這裡，一個最小可用的 Skill 就完成了。

第 5 步：測試 Skill 是否能觸發
--------------------

儲存後，可以用接近真實場景的提示詞測試：
```

text 體驗AI程式碼助手 代碼解讀複製代碼幫我寫一份本週工作總結。

```
或者：
```

text 體驗AI程式碼助手 代碼解讀複製代碼把下面這些工作紀錄整理成月報，正式一點，控制在 1000 字左右。

```
測試時我一般看三件事：

1. 是否會按工作總結場景回應。
2. 是否遵守結構、字數和注意事項。
3. 是否在資訊不足時亂編資料。

經常不觸發，就先改 \`description\`。

能觸發，但輸出不穩定，就改正文裡的「寫作步驟」「注意事項」和「輸出要求」。

第 6 步：什麼時候需要 scripts、references 和 assets？
-----------------------------------------

不是每個 Skill 都需要額外目錄。

像「工作總結寫作」這種偏寫作和判斷的 Skill，通常一個 \`SKILL.md\` 就夠。

任務變複雜之後，再考慮拆資源。

### 需要 scripts 的情況

如果某個操作需要穩定執行，就別讓 AI 每次重新想辦法。

例如：
```

text 體驗AI程式碼助手 代碼解讀複製代碼markdown-mermaid-to-images/  
├── [SKILL.md](http://skill.md/)  
└── scripts/  
└── convert\_mermaid.py

```
這類任務涉及檔案掃描、圖片匯出、路徑替換。交給腳本，比寫一堆自然語言規則可靠。

### 需要 references 的情況

如果有大量規則、規範、介面文件或業務知識，就放到 \`references/\`。

例如：
```

text 體驗AI程式碼助手 代碼解讀複製代碼contract-review/  
├── [SKILL.md](http://skill.md/)  
└── references/  
└── [clause-checklist.md](http://clause-checklist.md/)

```
\`SKILL.md\` 只寫核心流程，並說明什麼時候讀取參考資料。

### 需要 assets 的情況

如果最終產物需要範本或素材，就放到 \`assets/\`。

例如：
```

text 體驗AI程式碼助手 代碼解讀複製代碼slides-builder/  
├── [SKILL.md](http://skill.md/)  
└── assets/  
└── company-template.pptx

```
這樣 AI 不用每次重新建立範本，也更容易保持統一樣式。

手寫 Skill 的常見問題
--------------

手寫 Skill 很適合理解原理，但也容易踩坑。

### 1. description 太寬泛

錯誤範例：
```

yaml 體驗AI程式碼助手 代碼解讀複製代碼description: 寫作助手

```
這個描述幾乎什麼都能觸發，邊界太寬。

更好的寫法：
```

yaml 體驗AI程式碼助手 代碼解讀複製代碼description: 撰寫、潤飾或整理中文工作總結類文件時使用，包括週報、雙週報、月報、季報、半年/年度總結、專案回顧、階段性彙報、述職報告、工作小結等。當使用者說「幫我寫個週報」、「整理一份月度工作」、「年終總結」、「專案回顧」、「做彙報材料」、「述職」，或丟過來一堆零散工作紀錄要求「總結一下」、「整理成報告」時，都要主動啟用。該技能用於幫助使用者把零散工作內容整理成結構清晰、重點明確、表達正式自然的總結文字。

```
### 2. 只寫原則，不寫步驟

錯誤範例：
```

markdown 體驗AI程式碼助手 代碼解讀複製代碼## 要求

寫得專業、自然、有深度。

```
更好的寫法，是把「專業」拆成動作：

- 先判斷總結範圍。
- 再提煉核心工作。
- 然後整理成果。
- 接著寫問題與不足。
- 最後寫經驗和計畫。

### 3. 把所有東西都塞進 SKILL.md

\`SKILL.md\` 最好保持精簡。

內容越來越長時，通常就該拆了：

- 長規範放 \`references/\`。
- 重複操作放 \`scripts/\`。
- 範本素材放 \`assets/\`。

### 4. 沒有寫反例和禁區

很多輸出問題，不是 AI 不知道該做什麼，而是不知道什麼不能做。

例如工作總結裡必須明確：

- 不要虛構資料。
- 不要寫口號。
- 不要把小問題寫成嚴重事故。
- 不要為了湊字數重複表達。

限制寫得越清楚，輸出越穩定。

接下來：用 skill-creator 自動建立
------------------------

手寫過一遍之後，再看 \`skill-creator\` 就順很多。

\`skill-creator\` 也是一個 Skill，只是它專門負責建立和更新 Skill。目錄結構、FrontMatter、資源該不該拆，它會幫你一起處理。你不用每次都從空白檔案開始。

github 地址：[github.com/anthropics/…](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fanthropics%2Fskills%2Ftree%2Fmain%2Fskills%2Fskill-creator)

安裝 skill-creator：
```

bash 體驗AI程式碼助手 代碼解讀複製代碼npx skills add [https://github.com/anthropics/skills](https://github.com/anthropics/skills) --skill skill-creator

```
你可以直接這樣要求 claude code:
```

text 體驗AI程式碼助手 代碼解讀複製代碼使用 skill-creator，幫我建立一個 work-summary-writing skill。

目標：  
當使用者需要寫工作總結、週報、月報、季度總結、年度總結、專案回顧或階段性彙報時觸發。

能力：

1. 判斷總結範圍和字數要求。
2. 從零散工作紀錄中提煉核心工作。
3. 整理工作成果、問題不足、經驗總結和下一步計畫。
4. 輸出正式、自然、結構清晰的工作總結。

注意事項：

1. 不要虛構關鍵事實、資料、專案名稱或成果。
2. 資訊不足時使用穩妥表達。
3. 避免口號式語言。
4. 下一步計畫要具體可執行。

位置：  
安裝到全域 Claude skills 目錄。

```
這種請求比「幫我建立一個寫週報 Skill」穩定得多。邊界給清楚了，\`skill-creator\` 才知道該往哪裡收、哪裡不能展開。

skill-creator 會幫你做什麼？
---------------------

\`skill-creator\` 不只是生成一個空資料夾。

它會從「另一個 AI 以後怎麼用這個 Skill」的角度，幫你把這些事情過一遍：

1. 理解這個 Skill 要解決什麼問題。
2. 整理觸發條件和真實使用場景。
3. 規劃是否需要 \`scripts/\`、\`references/\`、\`assets/\`。
4. 建立 Skill 目錄和 \`SKILL.md\`。
5. 寫好 \`name\`、\`description\` 和正文流程。
6. 必要時補充資源檔案。
7. 檢查 Skill 結構是否完整。
8. 根據真實使用結果繼續迭代。

換句話說，手寫時你要自己操心的細節，\`skill-creator\` 會替你檢查一輪。

為什麼要用 skill-creator？
--------------------

手寫適合學習原理。真要長期用，我更建議交給 \`skill-creator\` 起草和迭代。

原因主要有四個。

### 1. 它更懂 Skill 的結構

一個 Skill 不只是寫一段 prompt。

它需要考慮：

- \`name\` 是否規範。
- \`description\` 是否能準確觸發。
- 正文是否是可執行流程。
- 是否缺少注意事項。
- 是否應該拆分資源目錄。

這些細節手寫時很容易漏掉。

### 2. 它能減少觸發錯誤

低品質 Skill 最常見的問題，就是 \`description\` 寫得太寬，或者太窄。

太寬會誤觸發，太窄會不觸發。

\`skill-creator\` 會提醒你把能力、場景和觸發詞寫清楚。比如別只寫「處理合約」，要寫成「當使用者要求檢查合約、總結合約風險或整理關鍵條款時使用」。

### 3. 它會幫你判斷資源怎麼拆

很多人第一次寫 Skill，會把所有規則都塞進 \`SKILL.md\`。

短期能用，時間一長就會臃腫。

\`skill-creator\` 會根據任務判斷：

- 是否需要腳本保證穩定執行。
- 是否需要參考資料保存長規則。
- 是否需要範本或素材支援最終輸出。

這就是漸進載入：常用規則放正文，長資料按需讀取，重複操作交給腳本。

### 4. 它適合持續迭代

好的 Skill 通常不是一次寫完的。

真實使用後，你會發現它可能有這些問題：

- 某些提示詞沒有觸發。
- 輸出格式不穩定。
- 經常編造不存在的資料。
- 處理複雜輸入時漏步驟。
- 和其他 Skill 的邊界衝突。

這時可以繼續讓 \`skill-creator\` 檢查和更新：
```

text 體驗AI程式碼助手 代碼解讀複製代碼使用 skill-creator 檢查這個 skill 是否合格：  
~/.claude/skills/work-summary-writing

重點檢查：

1. description 是否能準確觸發。
2. [SKILL.md](http://skill.md/) 是否過長。
3. 寫作步驟是否可執行。
4. 注意事項是否涵蓋常見錯誤。
5. 是否需要拆分 references 或 assets。

這比臨時修一句 prompt 穩得多。

## 手寫和 skill-creator 怎麼選？

可以按這個規則判斷：

場景建議第一次學習 Skill手寫一遍只需要一個很小的個人規則手寫也可以要建立長期複用的 Skill用 `skill-creator` Skill 涉及腳本、範本、參考資料用 `skill-creator` 需要檢查觸發條件和目錄結構用 `skill-creator` 需要迭代已有 Skill用 `skill-creator` 我的建議是：先手寫一個最小版本，搞清楚 `name` 、 `description` 、正文流程和注意事項分別在做什麼。之後要建立正式 Skill，再交給 `skill-creator` 。

## 小結

寫 Skill 的重點，不是把 prompt 寫長。真正要做的，是把高頻任務沉澱成一套能觸發、能執行、後面還能維護的工作流程。

一個好 Skill，至少要回答四個問題：

1. 什麼時候觸發？
2. 觸發後按什麼步驟做？
3. 輸出應該長什麼樣？
4. 哪些事情不能做？

手寫能幫你理解底層結構。 `skill-creator` 則適合把這套結構做完整，並且在後續使用中持續修。

第一版 Skill 不需要完美。先讓它在真實任務裡跑起來。哪裡觸發錯了，就補觸發條件；哪裡輸出跑偏了，就補規則和反例；哪裡靠文字說不穩，就拆資源、加腳本。Skill 就是這樣一點點變強的。

[![](https://i.imgur.com/6ZzuCV4.jpeg)](https://i.imgur.com/6ZzuCV4.jpeg)

## 往期回顧

- [AI Skill 是什麼？一篇講清楚它和 Prompt、MCP 的差別](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FAj7mOtgJd2xxlNJg464wow)
- [如何尋找、安裝和管理 AI Skill？](https://link.juejin.cn/?target=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FlgKiloZ83TbB2iYaa-EX5g)

---

原文出處： [https://juejin.cn/post/7642171057679859775](https://juejin.cn/post/7642171057679859775)

---

## 小編精選 - 技術文章翻譯

精選技術文章翻譯，幫助開發者持續吸收新知。

[0留言](https://codelove.tw/@tony/post/xNQDEq) [21瀏覽](https://codelove.tw/@tony/post/xNQDEq)

---

**共有 0 則留言**

[登入之後發表留言](https://codelove.tw/login?to=http://codelove.tw/@tony/post/xNQDEq)

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

🔥 [50 多個必備 Telegram 機器人的終極清單](https://codelove.tw/@tony/post/37oZV3)

🔥 [Claude Code 進階：用 Superpowers 打造可靠的 AI 開發工作流程](https://codelove.tw/@tony/post/qv2Neq)

🔥 [【Claude Code】應加入 Agent 的 20 個 Skills｜skills.sh 活用指南](https://codelove.tw/@tony/post/am270q)

🔥 [Claude Code 與 Codex CLI：該選哪一個？根據我使用兩者的經驗，從不同角度解釋要點。](https://codelove.tw/@tony/post/qEl6Ma)

##### 🏆 本月排行榜

🥇

[站長阿川](https://codelove.tw/@howtomakeaturn)

📝20 💬11 ❤️1

599

🥈

[alicec](https://codelove.tw/@alicec)

📝1 ❤️2

84

🥉

[我愛JS](https://codelove.tw/@JsLover0018)

💬1

4

#4

[JCConf Taiwan](https://codelove.tw/@jcconftw)

4

評分標準：發文×10 + 留言×3 + 獲讚×5 + 點讚×1 + 瀏覽數÷10

本數據每小時更新一次

<iframe frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-1009112352200321&amp;output=html&amp;adk=1812271804&amp;adf=3025194257&amp;lmt=1779845659&amp;plaf=7%3A2&amp;plat=3%3A128%2C4%3A128%2C9%3A32776%2C16%3A8388608%2C17%3A32%2C24%3A32%2C25%3A32%2C30%3A1081344%2C32%3A32%2C41%3A32%2C42%3A32%2C43%3A32&amp;format=0x0&amp;url=https%3A%2F%2Fcodelove.tw%2F%40tony%2Fpost%2FxNQDEq&amp;pra=5&amp;asro=0&amp;aiactd=0&amp;aicctd=0&amp;ailctd=0&amp;aimartd=4&amp;aieuf=1&amp;aicrs=1&amp;uach=WyJtYWNPUyIsIjI2LjUuMCIsImFybSIsIiIsIjE0OC4wLjc3NzguMTc5IixudWxsLDAsbnVsbCwiNjQiLFtbIkNocm9taXVtIiwiMTQ4LjAuNzc3OC4xNzkiXSxbIkdvb2dsZSBDaHJvbWUiLCIxNDguMC43Nzc4LjE3OSJdLFsiTm90L0EpQnJhbmQiLCI5OS4wLjAuMCJdXSwwXQ..&amp;abgtt=6&amp;dt=1779845659319&amp;bpp=2&amp;bdt=81&amp;idt=79&amp;shv=r20260521&amp;mjsv=m202605210101&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D48a146798d0a7a7c%3AT%3D1774603059%3ART%3D1779845643%3AS%3DALNI_MYrF43qhfGkjVOOymA6AgdYvlutzg&amp;gpic=UID%3D0000122fd6a46a84%3AT%3D1774603059%3ART%3D1779845643%3AS%3DALNI_MYteeCikHWOXmunVL8LklHTdMIC2Q&amp;eo_id_str=ID%3D5d1198dc07f33d2e%3AT%3D1774603059%3ART%3D1779845643%3AS%3DAA-AfjZ6bhE9zxbu58DxGUKvaGbB&amp;nras=1&amp;correlator=5096358673555&amp;frm=20&amp;pv=2&amp;u_tz=480&amp;u_his=1&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1050&amp;u_aw=1920&amp;u_cd=24&amp;u_sd=1&amp;dmc=32&amp;adx=-12245933&amp;ady=-12245933&amp;biw=1680&amp;bih=938&amp;scr_x=0&amp;scr_y=0&amp;eid=31098654%2C31098751%2C95390668%2C31098762%2C42533294%2C95391807&amp;oid=2&amp;pvsid=7378565231613747&amp;tmod=941033422&amp;uas=0&amp;nvt=1&amp;fsapi=1&amp;ref=https%3A%2F%2Fcodelove.tw%2F%3Fsort%3Dlatest&amp;fc=1920&amp;brdim=0%2C0%2C0%2C0%2C1920%2C30%2C0%2C0%2C1680%2C938&amp;vis=2&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=32768&amp;bc=31&amp;bz=0&amp;ifi=1&amp;uci=a!1&amp;fsb=1&amp;dtd=107" title="Advertisement" aria-label="Advertisement"></iframe><iframe width="1005" height="124" frameborder="0" allow="attribution-reporting; run-ad-auction" src="https://googleads.g.doubleclick.net/pagead/html/r20260521/r20190131/zrt_lookup_fy2021.html#RS-1-&amp;adk=1812271801&amp;client=ca-pub-1009112352200321&amp;fa=1&amp;ifi=3&amp;uci=a!3&amp;btvi=1" title="Advertisement" aria-label="Advertisement"></iframe>